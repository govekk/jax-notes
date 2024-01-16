# Miscellaneous notes
Cloud Run vs Batch vs GKE??

Google supports Terraform: "equivalent command line" button on VMs can give terraform command instead of gcloud
"Terraform is easily the best way to set up things in Google Cloud" - use cloud storage to save state
- Mike says he uses Terraform to set up cluster, then kubectl to deploy apps (you caaaan theoretically use Terraform to deploy apps - Sean/Alex does)

Use Cloud Spanner over Cloud SQL? Not out for postgres yet, but coming?
- spanner can get expensive

https://googlecloud.qwiklabs.com/ in incognito tab
kiya.govek@jax.org

9am-5pm, ending each day with lab
1 hr lunch (1pm-2pm?), breaks throughout day
Forced break Tues for fire alarm sometime between 10am-12pm

# Networking in Google Cloud
## Setting up VPC
- Everything on Google Cloud on one big physical network. VPC is software-defined network
- Default VPC in GCP - don't use - firewall rules are too permissive, allows ssh/rdp/http from anywhere
    - Delete it in new projects (already deleted in mine)
- Don't define IP range at VPC level, just defined for subnets
- IPv4 vs IPv6 - IPv6 meant to solve problem of running out of IPv4 addresses. Not really a concern anymore, so most people still use IPv4.
    - Google Cloud VPC is either IPv4 or both IPv4/IPv6
- Google can create Global VPC with regional subnets (which is a benefit over AWS and Azure that only do regional VPC and zonal subnets)
- Select custom subnet creation when creating VPC
    - Can pick IP ranges for subnets in same VPC that are non-consecutive, but maybe shouldn't?
- Must configure firewall rules to allow traffic into VPC network (don't use defaults)
- IPv4 - job of VM to give itself external IP address. IPv6 - job of subnet to give external IP address.
    - internal = private; external = public
- Even if VMs are in different regions, if part of same VPC can talk internally

Q: How choose IPv4 ranges?? What is "best practice"??

## Routing
- System-generated routes are created when subnet is created: enable VMs on same network to communicate
- 0.0.0.0/0 = "internet" - last option in VM routing table
- Can delete routes or replace (i.e. if don't want to keep 0.0.0.0/0 default)
- By default, Google routes traffic through global load balancer based on closest to user
- Goal to reduce footprint of public IP addresses
- Static vs Dynamic routes
    - Static is quicker and more secure, but require more maintenance because not dynamically updated 
- Dynamic routes set up by Cloud Router

## IP addresses
- Bring your own IP: user owns IP range and can assign those IPs to cloud resources
    - managed in the same way as Google-provided
- Higher charge for unused IP addresses to prevent hoarding IP addresses
- IP address cannot be assigned to GKE node or pod, autoscaling MIG, or classic VPN gateway

## Multiple VPCs
- nic: Network interface controllers
    - each is attached to separate VPC network, uses internal IP to communicate across networks
- can only be configured when you create VM instance, not after

## Cloud DNS
- DNS = Domain Name System
- this service never goes down. "Built on edges of network"
- private vs public DNS zones
- routing policies 
    - weighted round robin: different weights per DNS target
    - geolocation: map traffic originates from google cloud regions to specific DNS targets
    - nesting or combining policies not supported
1. Create VMs, serving to external IP (e.g. Apache). 
2. Create Cloud DNS separately with host name, set routing policy that sends traffic from specific zones to the external IPs of the VMs

## Controlling access to VPC networks
- IAM (Identity and Access Management)
    - most useful for service accounts - giving VMs etc access to other services
    - resources inherit policies from parent, less restrictive parent policy overrides more restrictive child policy??
    - custom roles user is responsible for managing - i.e. updating if Google makes changes to permissions included in a list of permissions, you need to mimic those changes in custom - pre-defined roles get those changes automatically
        - better to use pre-defined roles
            -e.g. network viewer (read-only access to all networking resources), security admin (create, modify, delete firewall rules and SSL)
- Service account
    - VM, Cloud Run, etc use default service account unless otherwise specified - bad idea, too many privileges
    - Vishal says default service accounts should be turned off? or just they don't use them
    - Can edit VM's service account if stopped
- Policy constraints: block people at org/ project level from doing things
- Firewall rules
    - default is all ingress denied, all egress allowed. so really only makes sense to make ingress allow and egress deny rules.
    - 0.0.0.0/0 is usually an awful idea (except if single web server, though better to do load balancer on top)
    - icmp = ping
    - tcp port 80 is http
    - "only networking team should edit firewall rules"?
    - always blocks port 25 (SMTP) on external IP
- Google doesn't do ACL (access control list) on subnet like AWS does
- Can share VPC subnets across projects (Shared VPC)
    - need to set a host project
    - Requires high amount of permissions - Shared VPC Admin role, Service Project Admin role
- Peer connection between two VPC networks either in same project or between projects. Need to create two peer connections (net1 -> net2 and net2 -> net1)
    - allow private traffic between vpcs
    - since each direction is set up independently, doesn't require high permissions and can be done between teams
    - requires non-overlaping CIDR ranges in subnets to be peered
    - need to use IPs between peered networks, can't use instance names

- Cloud Interconnect
    - share custom routes so that peered networks can reach your on-premises network

## Load balancing
- Global: http(s), SSL proxy, TCP proxy
    - proxy load balancer - terminating connection at load balancer. SSL or HTTPS certs go on load balancer, not application
- Regional: Internal TCP, Network TCP, Internal HTTP(s), External HTTP(s), Internal TCP proxy
- can balance public traffic or private traffic (e.g. between parts of application)
- backend services define 1. how traffic is distributed, 2. which health check to use, 3. if session affinity is used
    - backend service can be many things - VM, Cloud Run, Cloud Storage, on-prem, AWS, etc
        - e.g. cloud storage bucket can store static web page - send traffic there if other apps are down
- network endpoint groups (NEG) - specifies group of backend endpoints
- load balancer can absorb denial of service attacks (and mitigation techniques built into Google infrastructure, Cloud Armor)

### Hybrid load balancing
- Hybrid NEG (network endpoint group) connects to on-prem or other cloud
- Add IP address:port for each backend service, specify google cloud zone close as possible, add health check to NEG

### Traffic management
- direct traffic based on https parameters, should specify where "everything else" goes
- path rules evaluated on "longest path matches first" basis
- wildcards supported only after forward slash (e.g. /video/*, not /video*)

### Internal TCP/UDP load balancers
- fast, routes connections directly from clients to healthy backends
- distribute traffic to VMs or k8s pods
- enable global access VPC so routing works from any region

## Cloud CDN
- "content delivery network"
- trying to cache content closer to users
- only works with HTTP(s) Global Load Balancers
    - so if using with cloud storage, still need to put load balancer in front
- 3 cache modes: use origin headers, cache all static, force cache all
- can do manual invalidation of cache if update cloud storage
- just a checkbox option when creating  global external application load balancer

### Hybrid connectivity
- Cloud Interconnect
    - VPN tunnel: encrypted tunnel to VPC networks through public internet (requires remote VPN gateway) (1-3 Gbps)
    - Dedicated interconnect - requires connection to colocation facility (closest NYC) (10-100 Gbps)
    - Partner interconnect - requires service provider partner (1-50Gbps, depending on provider)
        - JAX has partner through Connecticut Education Network for Layer 2 connection (idk what layer 2 is)
- Cloud VPN
    - slower speed than Cloud Interconnect
    - encrypted data in transit

## Private connection options
- Way to connect with APIs and services that aren't in your network without needing an external IP
    - e.g. a separate Google Cloud API is not running in your VPC
- Private Google Access
    - connect to public IP addresses of Google APIs through the default internet gateway of the VPC network
    - lets you use Google services without giving your Google CLoud resources external IP addresses
    - radio button on subnet
- Private Service Connect & Serverless VPC Access
    - connect to any Google, third-party, or your services using internal IP addresses
    - serverless examples: cloud run, cloud functions, app engine
- Private services access
    - Connect Google and third-party services with VPC network peering
    - only use it for certain services: Google Cloud VMware engine, Cloud TPU

### Cloud NAT
- What is NAT?
    - Network Address Translation
    - Analogy: house
        - Lots of things in house (Laptop, Switch, Smart TV, etc) connect to internet but don't have public IP address - all have private IP address
        - Something to get to outside world: Router/Modem - that device is acting as house's NAT
- Makes requests for services from things with private IP addresses and delivers responses back
- Pass through, not proxy (traffic doesn't terminate at NAT, continues through)
    - autoscales
- Only thing with public IP address is NAT and blocks external ingress traffic
- Benefits
    - reduces need for individual VMs to have external IP
    - not dependent on single physical device (autoscales)
- so Private Google Access gives internal VM access to Google services, Cloud NAT gives internal VM access to internet (e.g. for apt-get update)

## Network spend
- price calculator
- Ingress/egress
    - No charge: ingress, egress to same zone (internal), egress to google products (youtube, maps, drive), egress to different Google Cloud storage (same region, exceptions (e.g. VM to cloud storage))
    - $0.01 per GB: egress between zones in same region, egress to same zone (external), egress between regions in US
    - higher internet egress rates to regions outside US
- IP address
    - external IP address: static IP unused $0.01 per hour, used (static or ephemeral) is $0.004 on standard VM (cheaper)
    - static and ephemeral IP addresses attached to forwarding rules are not charged
- premium vs standard tier
    - premium: Global Load Balancing, Cloud CDN
        - your traffic goes to closest PoP to you, travels "cold potato routing" on Google fiber
    - standard: Regional Load Balancing
        - your traffic travels public internet ("hot potato"?) to closest PoP to target
    - can specify premium vs standard on a network interface level (e.g. per VM)

## Monitoring & logging
- traditionally would have one project for monitoring / logging (scope project) to keep track of others
- if want to monitor/log VMs, need to install agents on VMs - best way to install is during VM creation
    - extra cost of metric and log collection
- logs automatically deleted after 30 days - best to archive in cloud storage or push into big query
- Monitoring > Dashboards
    - some pre-built dashboards e.g. for kubernetes
- Firewall Insights
    - tells if rules are too permissive, or redundant, or contradictory
- Flow logs (VPC logging) can turn on per subnet
    - get source/destination IP and port
    - produces many logs, best for troubleshooting but not always

# Cloud run / Cloud functions
- Cloud Function: load piece of code into function - activated on some trigger (click link, upload file to storage, etc)
- Cloud Run: deploy application without worrying about infrastructure (compared to GKE where need to know Kubernetes?)

# Security
## Overview
- "defense in depth" approach: many layers of security on top of services, so if one layer comprimised there's another under
    - low-level infrastructure (obscure data centers, up to date hardware), service deployment (access management, transfer encryption), data storage (encryption at rest, hardware shredders), internet communication (DoS protection, user authentication), operations (monitoring/logging)
- by default, everything is encrypting in/out/between services
- regulatory compliance (HIPAA, CLIA, etc)
- Google does not require notification when you perform penetration testing - AWS does
    - Security assessment services: Google Security Scanner
- DoS (Denial of Service), DDoS (Distributed Denial of Service)
    - Google HTTP(s) global load balancer provides built-in defense
- Application attacks
    - Google Cloud Armor (web application firewall) - customize defenses
- All data at rest is chunked and encrypted
    - Other options: customer supplied keys (CSEK), customer managed keys (CMEK), external key manager
    - Can encrypt data before it goes into cloud storage as well

## Data access
- all APIs are done via REST

## Best practices:
- when possible, put managed service in front of your resource so if attacked, attacks service not resource
- fewer public IPs
- to give VM access to cloud storage, give service account (don't use default)
    - toad needs kubernetes, cloud storage

## IAM
- "who can do what on which resource"
- types of roles: custom, pre-defined, basic
    - basic: project only roles. owner, editor, viewer, billing administrator on project
        - Mike says we shouldn't use these for most users but I think this is all JAX uses mainly lol
        - Owner: invite & remove members, turn on/off APIs; editor: deploy apps, modify code, configure services; viewer: read-only access
    - pre-defined: map to job functions (Network Admin, Security Viewer) - specific "what" on specific resource, hundreds of options

### Policy intelligence
- troubleshoot and query access policies
- recommender: suggest remove excess permissions

## Service accounts
- used to authenticate from one service to another
- can be used to link rules
    - either to object or to JSON key
- two types of service accounts
    - Google-managed service accounts - google managed keys, public key for maximum of two weeks, private key never shown to user
    - user-managed service accounts - user responsible for private key security (should rotate frequently)
- hardest thing in service accounts is managing keys
    - gcloud iam service-accounts keys list
- used to not be the case, but now if you edit roles in a service account that will propogate to VMs with that account
- **default service account has EDITOR role on project - that's everything! we can create service accounts but can't add any roles because missing setIAMPolicy**

## Workload identity federation
- Application running outside google cloud access to data without service account keys
- grant external identities IAM roles 
- create workload identity pool in google cloud project, app authenticates with identity provider and receives account credentials with short-lived google cloud token
- Sean/Alex says use a lot with GKE "application developers never see a service account"???

## SSL
- 3 pre-configured policies: compatible, modern, restricted
- load balancer can hold up to 15 certificates

## VPC service controls / service perimeters
- Can limited what services / locations can access service even with correct credentials
- requires extra permissions to set up?
- Access context manager reduces size of privileged network

## Cloud IDS (intrusion detection system)
- threat detection
- meets threat detection compliance requirements, including PCI 11.4

## Securing Compute Engine (VMs)
- scopes:
    - only used by default service account - usually better to define access based on service account roles
    - allow default access, allow full access to All Cloud APIs (usually bad idea, unless you are running a "deployment server" from which you run Terraform etc)
    - custom scopes per API
        - storage read-only by default
         
### connecting to VMs
- Linux machines are accessed using SSH - requires SSH key
- Windows machines accessed using RDP - requires username and password
- SSH
    - requires VM to have external IP?
    - by default, using project-wide SSH key for VMs - maybe something for security team to care about
    - can also add SSH keysto instances specifically and tell to not use project-wide key
- connect to VMs without external IPs
    - Bastion host / jumpbox is different VM with external IP
    - Can set up IAP (identity aware proxy) 

### OS Login
- Can manage SSH access and consistent Linux user identity to instances using IAM
- recommended way to manage linux instances esp w/ many users across many instances + projects
- Supports 2 factor auth
- Automatic linux account lifecycle management and permission updates
- can import existing linux accounts

### Certificate Authority Service
- I think JAX has a CA, so maybe not applicable
- simpler management and taylorable than standard CA

### Best practices
- Lock things down using IAM
- Isolate machines using multiple networks
- Connect using VPNs or Cloud Interconnect
- Monitor and audit logs regularly
- Keep deployed VM instances updated (updating OS - patch management system built into google cloud)
- Run VMs using custom service accounts with appropriate roles, not default service account

## Securing Cloud Data
- Cloud Storage, Cloud SQL, BigQuery

### Cloud storage
- ACL (access control lists) can grant access to objects in buckets - uniform (all objects use bucket-level IAM) or Fine-grained
- to make bucket public, grant allUsers the Storage Object Viewer role
    - for hosting static website or sharing data
    - same for object with fine grained ACL
- Cloud Storage bucket data access must be turned on manually
    - make a bucket to hold the logs, allow write access to the bucket, set logging on and specify log bucket
    - or make BigQuery dataset, use load job to copy log data into BigQuery tables

### Signed URLs
- Share data with someone not in google cloud
    - can only be created for object, not bucket or group of objects
- Created with timer (e.g. 10 minutes)
- while link is alive, anyone can use it
- create service account with rights to storage, create service account key, use key with signurl command (with -d param to specify duration)
    - maximum duration 7 days
- Sean/Alex says we use this to share Object Storage data (because sharing via application is slow)
    - gotcha: if using workload identity, workload identity service account doesn't have key - need to provision private key to do url signing

### Encryption
- Google managed-keys - rotated every 90 days
- CMEK (customer managed encryption keys) - you manage KEKs (generate keys, rotation perios, expire keys), KEKs still stored on Cloud KMS
- Customer supplied keys - JAX-provided keys we can bring to Cloud - Google doesn't store them, so don't lose them!
- Cloud EKM (external key manager) - keys you manage within supported external key management partner
- if change default encryption key for bucket, objects already in bucket are not changed (by default, they use Google-managed keys)
- can also use different keys for different objects using gsutil -o
- after rotation, new primary is created for encryption but old keys are kept around for decryption - data encrypted with old version is not automatically re-encrypted
    - If you are rotating because of unauthorized access, should manually re-encrypt and schedule destruction of old key

### Cloud HSM
- hardware security module (physical device that generates digital keys)
- sometimes hardware keys are required by government

### BigQuery IAM and Authorized Views
- Authorized View is way to grab data from different tables, make joined table, and give different user authorized access to the merged virtual table rather than the origin data tables

## Securing GKE (Google Kubernetes Engine)
- Kubernetes secrets: local/secret partition loaded in confines of pod. Don't really need - can use Terraform secrets or Google Secret Manager
    - in Kubernetes would usually need to be encrypted, Google you don't really need to cause everything already encrypted
- Kubernetes supports two types of authentication: user accounts, service accounts
    - Kubernetes service accounts are part of clusters, used within cluster
        - Role-based access control (RBAC)
    - Google service accounts part of GCP, grant permissions to clusters and other resources, use IAM
- Hardening cluster
    - upgrade GKE infrastructure or let Google automatically update for you
    - **monitor configurations**
    - restrict direct access to control planes and nodes
        - disable public endpoint access, or enable but use control plane authorized networks
    - use shielded GKE nodes
    - **restrict traffic between pods - can set up firewalls at pod level**
        - do not modify or delete firewall rules created by GKE
    - don't give nodes public IP addresses, can give pods public IP addresses
- Secure workflows
    - limit pod container process privileges
        - **if exec into pod and escape, can enter into root mode in pod - by default user 0 turned on, can turn off in config**
    - **use Workload Identity to allow applications on pods to authenticate to google cloud API**
        - configure a Kubernetes service account to act as Google service account
    - **how are we authenticating the gcsfuse command?? by user on kubectl or by default service account on pod??**
    - Binary Authorization - cluster admin / image attestor can sign images to allow on cluster
- Monitoring and logging
    - don't have to install ops agent like compute engine
    - infrasctructure: cluster - node - pod - container
    - workload: cluster - namespace - service - pod - container
    - services: cluster - namespace - workload - pod - container
    - integrates with Cloud Audit Logs
    - *create log-based metrics & alerts
- **default service account has EDITOR role on project - that's everything! we can create service accounts but can't add any roles because missing setIAMPolicy**

## Application security
- applications are most common target of attackers
- injection: sql injection, ldap injection, html injection, XSS (cross site scription), week authentication, components with known vulnerabilities
- Web Security Scanner checks applications for common vulnerabilities
    - generates real load against application and can change state data - so don't run in production!
    - run in as-close-to-prod test environment, block specific UI elements or URLs, use backup data
- OAuth phishing - gain access to backend user accounts
- zero trust security model
    - identity and context-aware access
    - BeyondCorp Enterprise
    - DLP (data level protection)
    - enforcement points: Cloud Identity, VPC service controls, IAP
- IAP - identity aware proxy
    - reverse proxy sitis in front of every data request, central authentication for applications over HTTPs
- Secret Manager
    - can use instead of Terraform or GKE secrets
    - e.g. Cloud SQL credentials
    - access control using IAM (by default, only project owners can create and access secrets within their project)
    - should rotate secrets - Secret Manager supports adding new version or rotating on schedule

### Protecting against DDoS (distributed denial of service)
- make online application unavailable by overwhelming it with traffic
- Cloud Armor
    - works with Global load balancers only - less helpful with Regional Load balancer cause traffic still gets partway there
    - allows blocklist/ allowlist, 
- load balancing, reduce attack surface (fewer external resources), restrict access to internal traffic, offload static content to a CDN
    - make traffic/attack hit a managed service (load balancing) before your application
- Google Load Balancer automatically configured with DoS mitigation
    - **if autoscale, set up alerts when bring up new machines**
    - anything will take 10-15 min to respond: load balancer, DDoS tools

### Content related vulnerabilities
- ransomware
- DLP API (Data loss prevention)
    - scans documents for sensitive data before publication
- Google tries prevents ransomware automatically - not going to say how cause less secure
- Make regular backsups, use IAM best practices, use Cloud Data Lost Prevention API
    - **make separate project with less access with cloud storage bucket backup**

## Monitoring / auditing / logging
- Security Command Center - for org admins
    - centralized view for cloud resources
    - uncovers machines that are being used for malicious purposes (bitcoin mining)
    - standard version is free, enterprise is not
    - need organization admin and security center admin to run
- effective incident response: **monitoring dashboard**, **alerting regimen**, plans for responding to issues
- Monitoring
    - built in monitoring: app engine flexible and standard environment, **google kubernetes engine** , cloud run & functions, **?istio**
    - Ops Agent needed on VM / Compute Engine - not built-in
    - Monitoring third party open-source apps (apache, nginx, mysql, postgresql)
- Trace: latency reporting
- Error Reporting: error notifications, error dashboard - anytime application crashes, errors logged - errors stay forever unless accepted
- Logging
    - platform, system, and application logs
    - **logs-based metrics**
    - Log Explorer is nice but takes practice
    - **Cloud Logging stores logs for limited number of days - move to Cloud Storage, Big Query, or Pub/Sub**
        - Pub/Sub acts a queue - can push log data into splunk etc
        - Audit logs stay around longer - who did what where and when? - more for org admins
- Security automation risks
    - ensure you have highly descriptive playbooks
    - peer reviews, QA & testing

# Testing
- Prisma IP ranges
    - Gateway East: 208.127.91.211

# Future learning

## Courses
- Architecting with Google Kubernetes Engine: Production: https://www.cloudskillsboost.google/course_templates/33?catalog_rank=%7B%22rank%22%3A13%2C%22num_filters%22%3A1%2C%22has_search%22%3Atrue%7D&search_id=28426771
    - 30 credits, "40" hours (lots of videos though)
    - **Securing Google Kubernetes Engine with Cloud IAM and Pod Security Admission**, **Implementing Role-Based Access Control with Google Kubernetes Engine**, **Configuring GKE-Native Monitoring and Logging**, **Configuring Liveness and Readiness Probes**, Using Cloud SQL with Google Kubernetes Engine, **CI/CD for Google Kubernetes Engine using Cloud Build**
- Google Kubernetes Engine Best Practices: Security: https://www.cloudskillsboost.google/course_templates/732?catalog_rank=%7B%22rank%22%3A19%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&search_id=28375626
    - 18 credits, 5 hours
    - How to Use a Network Policy on Google Kubernetes Engine, **Using Role-based Access Control in Kubernetes Engine**, **Google Kubernetes Engine Security: Binary Authorization**, Hardening Default GKE Cluster Configurations
- CI/CD on Google Cloud: https://www.cloudskillsboost.google/course_templates/691?catalog_rank=%7B%22rank%22%3A5%2C%22num_filters%22%3A1%2C%22has_search%22%3Atrue%7D&search_id=28426787
    - 21 credits, 5 hours
    - Working with Artifact Registry, Google Kubernetes Engine Pipeline using Cloud Build, Cloud Run Canary Deployments, Continuous Delivery with Google Cloud Deploy, CI/CD on Google Cloud: Challenge Lab
- 

## Labs
- Understanding and Combining GKE Autoscaling Strategies: https://www.cloudskillsboost.google/focuses/15636?catalog_rank=%7B%22rank%22%3A34%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=28375674
- Deploy Kubernetes Load Balancer Service with Terraform: https://www.cloudskillsboost.google/focuses/1205?catalog_rank=%7B%22rank%22%3A11%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=28375626
- Implementing Canary Releases of TensorFlow Model Deployments with Kubernetes and Anthos Service Mesh: https://www.cloudskillsboost.google/focuses/18471?catalog_rank=%7B%22rank%22%3A27%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=28375657
- Google Kubernetes Engine Pipeline using Cloud Build: https://www.cloudskillsboost.google/focuses/52829?catalog_rank=%7B%22rank%22%3A5%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=28375614
    - "In this lab, you create a CI/CD pipeline that automatically builds a container image from committed code, stores the image in Artifact Registry, updates a Kubernetes manifest in a Git repository, and deploys the application to Google Kubernetes Engine using that manifest."
- Using Role-based Access Control in Kubernetes Engine: https://www.cloudskillsboost.google/focuses/5156?catalog_rank=%7B%22rank%22%3A26%2C%22num_filters%22%3A2%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=28375657 

# Note on Eric's conversions
- Currently using Batch
- Alternative is Workflows or Cloud Runs (server-less, but less configurable - can't define VM types?)
- run for all *filesets* in project or dataset ID
    - connect to omero server via toad currently
    - `gcloud ssh` port-forward to run local something through bowser/toad to omero-readonly-server - so don't need to actually run on bowser/toad env
        - `gcloud compute ssh --zone "us-east1-b" "bowser" --project "jax-omero-pub" -- -L4064:172.20.200.243:4064`
    - there is a /webclient/ way to get all map annotations
- not invoking gcsfuse explicitly, mounting gcs volumes in batch uses gcsfuse according to https://cloud.google.com/batch/docs/create-run-job-storage#use-bucket
- single fileset becomes zarr currently
    - diff. images inside fileset (e.g. ndpi is 3 images) are diff "datasets" in zarr
- recommended path for one-off workloads - json files in directory in cloud bucket
    - not enterprise scalable solution
    - invoke worker script with task index and where to find task jsons
- `gcloud batch jobs submit`
    - pretty good at showing things that go to sterr in batch job
- small script at end to add map annotations to images.jax.org with links to zarr (need read-write omero.server running) & versions used to produce
    - don't need to run at same time, since batch just saves json files at end, can run this later
    - don't want something easily scrapable - so maybe don't use consecutive IDs
- HCS is currently skipped
- produces zarr version based on bf2raw in container - so need new image if update version
- ideally would create service account for this and grant permissions - for both the user running the scripts and the service accounts on the VMs

# Architecting with Google Kubernetes Engine: Production

## RBAC
- For the recent versions of kubernetes, default kub service accounts don't have any permissions or access tokens
- Not relevant for gcp I think, but maybe for local: Kubernetes API server uses OpenID tokens, x509 certs, or passwords for authentication - OpenID best of those options
- RBAC controls what operations (get, create, etc) certain users or processes can perform on Kubernetes API objects (Pod, Deployment, Service, etc)
    - Roles: connect operations to objects; Role Bindings: connect users/processes with roles
    - can apply at cluster or namespace level (Role: namespace level, ClusterRole: cluster level)
        - common to give "get" "list" and "watch" together
        - cluster level: nodes, storage classes (PersistentVolumes); namespace level: pods, deployments
    - users/accounts: Google account, Google service account, or Kubernetes service account
- probably only useful if we want a google service account on toad/other that does or doesn't have admin abilities on koopa - not useful for users or kubernetes service accounts in our case - because users get full viewer/editor and our pods aren't running other pods

## Node security
- Each cluster has own certificate authority (CA) - used to secure communications between control plane and nodes
    - Kubernetes API server and kubelets use secure protocols (TLS, SSH) - supported by certificates of cluster's root CA
    - whenever you rotate credentials, API server is unavailable for short time
    - in GKE can manually rotate credentials
        - new IP address for control plane along with existing, new credentials created by control plane (API server unavailable but pods still run), nodes updated with new IP and credentials
            - because nodes recreated, any pod without control becomes unavailable (e.g. pod vs deployment)
            - must also update kubeconfig file on any other system that uses kubectl before completing rotation (i.e. re-run gcloud clusters get-credentials)
    - pods can access the metadata of the nodes they run on, including Node secret from Node configuration
        - configure cloud IAM service account for *node* with minimal permissions (we are currently using default - change that)
            - esp. don't give compute.instances.get permission, which allows reading metadata on Node w/ direct compute engine API calls
            - use GKE version 1.12
            - temporary solution will be deprecated eventually: enable metadata concealment - firewall preventing Pods from accessing kube-env (which contains kubelet credentials)

## Pod security
- security context: security measures defined in pod specification
    - e.g. runAsUser: 1000 and fsGroup: 2000
        - important runAsUser not 0 (root)
    - whether privileged containers can run, and whether code can escalate to root privileges
    - can define PodSecurityPolicies to create reusable security contexts - admission controler that updates pods to security policies - only occurs on pod creation or update
        - need to also create PodSecurityPolicy controller (after creating policies)
- apply labels to define behavior on violating policy:
    ```
      kubectl label --overwrite ns baseline-ns pod-security.kubernetes.io/warn=baseline # apply baseline policy in warn mode
      kubectl label --overwrite ns restricted-ns pod-security.kubernetes.io/enforce=restricted # apply restricted policy in enforce mode
    ```
    - warn: allow, but give warning; enforce: reject, and add entry to audit logs

## GKE Logging & Monitoring
- Setup cluster with Logging and Monitoring settings set to System
- Metrics explorer -> pick metric -> save chart -> create new dashboard -> now have custom dashboard in Dashboards tab