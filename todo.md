# Committed tasks
- Update OMERO images
    - Need omero-certificates version > 0.3.1 with read-only fixes (PR approved Dec 12, not yet merged)
    - OMERO.server 3.6.10+ w/ bioformats 7.1.0+
    - Upgrade to debian bullseye or rocky 9? Need Python 3.8
    - https://www.glencoesoftware.com/blog/2023/12/08/ice-binaries-for-omero.html 
    - Upgrade to Python 3.8 for omero-web and ezomero
        - Kubernetes templates currently debian buster because no zeroc-ice whl for debian bullseye, but debian buster has only 3.7 - bullseye has 3.9, next version has 3.11
- Fix OMERO blitz logs accumulating on public OMERO pods and evicting them every 40 days or so

# Future tasks
- Knowledge Article todo
    - Edit import article to mention permissions issues that can arise from using a project and dataset you don't own
    - Change all to Research IT Services category
    - Make all parent "Getting started"
    - New article on creating annotations / ROIs (some of this info in [video](https://jax.service-now.com/jax?id=kb_article&sysparm_article=KB0010469))
- Plan for other workshop? OMERO and zarrs? or something?? carpentries?

# User support
- Watch for big data transfer from Mark Krebs / Nishini lab - if we don't hear from them by July 2023, maybe reach out?

# Continual
- Tickets
- Omero imports
- Sharepoint pages
- Knowledge articles
- Workshops
- Attend conferences

# 2024 goals (to be updated)
- Lead at least two workshops or tutorials on image analysis or data management topics (Q2 and Q4)
    - Intro to OMERO, Image Processing with Python (Carpentries)?
    - Recreate Intro to OMERO materials as "my version" with more written down about what to cover as the intro / on the side - more ROI shapes - address FAQs
- Attend at least one in-person and one virtual conference (Q2-4)
- Establish the research OMERO instance on the new servers (Q1-2)
    - something serviceable by June 2024, ideally run using kubernetes to align with the public GKE OMERO instance, working with HPC and network RIT teams as needed
- Keep prod, dev, and public OMEROs updated with recent versions (Q2-4)
    - Update versions of OMERO.server, OMERO.web, omero-py, ezomero, cli-transfer, and Python
    - Regularly back up public OMERO database and bucket
    - Monitor image.sc for new versions and extensions
- Maintenance and administration of OMERO instances (Q1-4)
    - Investigate any incidents or bugs in OMERO clients reported by JAX users, via testing and image.sc community
    - Monitor data imports to OMERO and perform data transfers to public instance on demand
    - Keep documentation up to date
- Meet with researchers and scientific services from each campus at least once this year (in-person or virtually) to keep up to date on their relevant projects and what image processing software they use [as part of Imaging Applications team goals]

# Main projects
- Run Research Omero using kubernetes on prem
    - Connect to new Cloudian object storage (Instructions in email from David McKenzie)
    - [Project notes](omero/on-prem_kubernetes.md)
    - Monitoring: prometheus?
- GCP Omero hardening - updates and backups
    - Update server image with https://github.com/TheJacksonLaboratory/jax-gcp-omero/pull/26
    - Backup storage bucket
    - Dump & backup database & filestore on different GCP project
    - Delete Filestore & koopa v2 - early 2024
    - is storage bucket publically accessible for data download?
- GCP Omero hardening - security and stability
    - Monitoring for images.jax.org - alert if down, warning for expired certs
        - Use https://github.com/kubernetes/kube-state-metrics to monitor kubernetes states
    - Block SSH access on VPC except when manually add own IP to access toad - firewall currently 0.0.0.0/0 for ssh and nfs
    - service accounts on GKE - workload identity or RBAC (currently default account on nodes)
    - look into OS Login for toad - suggested way to manage linux w/ multiple users?
    - Limit access to Filestore and Database to just control plane IP of GKE
    - Google Cloud regional Load Balancer
    - Block IPs with too high rate of request
    - [GCP network security training notes](gcp/gcp_network_security_training.md)

# Nice-to-have projects
- Figure out why vips converted pyramid ome.tiffs cannot be read by OMERO or newer qupath?
    - "Good morning, when I was converting my tiffs to pyramidal ome-tiffs with libvips, then omero was not recognizing the format... though qupath did okay. I found out the C++ libraries need to be from OME: https://gitlab.com/codelibre/ome/ome-files-cpp. Libvips is super fast, memory efficient doesn't need java, I might try these C++ libraries in the future."
    - "btw, the older qupath 0.3.0 could open vips pyramids, but I tried newer qupath 0.4.3 and it was not recognizing the vips pyramids"
- OMERO with object storage follow-up
    - timing on toy gcsfuse vs toy nfs
        - zarr on nfs vs same zarr on gcsfuse - per number of files
        - zarr vs same original file on nfs and gcsfuse - per number of files in zarr
        - gke tweaks: memory per pod, pods on diff or same node, etc
- Single-cell data in OMERO
    - OMERO.tables and masks over ROIs
    - Help Bill Flynn set up cloud instance to deliver images to UConn labs?
        - Use my simplified cloud instance as a foundation, except only need read/write server, not read-only

# Small task days
- Clean physical desk
- Clean up desktop and documents
- Deal with all unread emails
- Look through any PRs
- Clean up test environments & commit repos
- Read posts on image.sc
- Read about research from JAX labs we work with
- Do an intro course on imaging software (QuPath, Napari, ilastik, Fiji, CellProfiler, pytorch, tensorflow)
- Play with OMERO or JAX OMERO packages (ezomero, omero-cli-transfer)
- Play with Cloudian object storage
- Do an intro course on sysad concept (POSIX, etc)
- Check omero imports and microscopy MOD AD group
- Check microscopy ticket filter
- Health Advocate incentives & virtual fitness classes (bookmark)
- Get on Mastadon??

# To learn
- Helm
- QuPath
    - https://qupath.readthedocs.io/en/0.4/docs/tutorials/index.html - especially: measuring areas, cell detection, cell classification
- Napari
- ilastik
- popen python
- RabbitMQ & Celery with kubernetes
- SSL
- Istio
- Terraform

# 20% todo
- Read https://opensource.guide/how-to-contribute/
    - https://www.reddit.com/media?url=https%3A%2F%2Fi.redd.it%2Fnm1w0gnf2zh11.png
- Look through community calls at https://scientific-python.org/calendars/ 

# Ideas for 20% time
- Analysis project to learn viewing and analysis software
    - Work with/for a service or lab on analysis or processing?
- Benchmarking project
    - Timing / quantifying various software for a specific purpose
    - Need to keep purpose reasonably limited
- Become keyboard wizard
    - Learn vi/vim key shortcuts
    - Learn terminal/shell key shortcuts
    - Learn tmux commands and shortcuts
    - Maccy clipboard manager: https://maccy.app/
- Contribute to open source projects
    - Start with small bug request issues
    - ome
    - carpentries
    - budgeting app? - going to be javascript or c or something
    - https://github.com/ome/omero-cli-transfer/issues/50#issuecomment-1881140971 
- Automation
    - CI/CD
    - Google cloud logging/metrics/alerts
    - OMERO dashboard
- GCP training
    - credits?
- Kubernetes certificate (CKA?)
- Just following curiosity questions