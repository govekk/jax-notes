# Short description
Convert GKE for images.jax.org from Public to Private network access

# Description
Our public OMERO instance is currently running on a GKE cluster with Public network access. We want to change this to a GKE cluster with Private network access, which will require creating a new GKE cluster with OMERO and cutting over the same IP.

# Justification
The goal is to improve security of the public OMERO instance by removing access from public networks to the Kubernetes control plane. 

# Implementation plan
A new Private GKE cluster will be created on the existing VPC and subnet. The only authorized network for its control plane will be the internal IP (172.20.200.2) of the existing Google Compute VM used for data imports (toad). This VM will be configured to apply kubectl commands to the cluster. We will initialize OMERO.server and OMERO.web running on the new cluster at 35.207.18.128, so that when we remove the OMERO.web service running on the current cluster, images.jax.org will point to the new instance. We have already performed a dry run of OMERO on a Private cluster at a different IP to test everything but the IP cut-over.

# Risk and impact analysis

1. Business impact assessment
Impact is entirely local, and should not be noticeable to end users. There will be no down time.

2. Environment Assessment
This impacts the production OMERO - Public Hosting service.

3. Outage Assessment
There will be no down time.

4. Pre-Change communications assessment
We will send an email to current JAX users with data hosted on images.jax.org, informing them that we are making some changes which should not affect user experience, though there may be some downtime if we need to backout.

5. Post-change communication assessment
No notification necessary

6. Regulatory impact
No regulatory impact

# Backout plan
Backout would involve stopping the new cluster and restarting the OMERO.web service of the current instance, which may involve a minute of downtime but will return to the current state.

# Test plan
Testing will involve accessing images.jax.org via web browser and trialling a small data import.






# Kiya's work notes
## Create Google cloud stuffs

Create koopa-v2 on same subnet
```
gcloud container clusters create-auto koopa-v2 \
    --region "us-east1" \
    --subnetwork jax-omero-pub-internal \
    --enable-master-authorized-networks \
    --network jax-omero-pub-vpc \
    --enable-private-nodes \
    --enable-private-endpoint \
    --project jax-omero-pub
```

Give Toad control plane access to koopa-v2
```
gcloud container clusters update koopa-v2 \
    --enable-master-authorized-networks \
    --region=us-east1 \
    --master-authorized-networks 172.20.200.2/32 \
    --project jax-omero-pub
```

### Set up kubectl
```
gcloud compute ssh toad --project jax-omero-pub
```

```
sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin kubectl
```

```
gcloud auth login
```

```
gcloud container clusters get-credentials koopa-v2 --zone=us-east1
```

### Get secrets from current kubernetes Omero
```
kubectl get secret <secret-name> -o yaml
```

### Get static IP
```
# Request a static IP
gcloud compute addresses create omero-web-us-east1-ip-v2 --region=us-east1 --network-tier=STANDARD

# Get info on it
gcloud compute addresses describe omero-web-us-east1-ip-v2 --region=us-east1

# Release static IP
gcloud compute addresses delete omero-web-us-east1-ip-v2 --region=us-east1
```

## Add logo
I have added an environment variable to the web yaml files to change the top logo:
```
- name: CONFIG_omero_web_top__logo
value: '/static/webgateway/img/jax_logo_23.png'
```
In order for this to work, the logo image file (jax_logo_23.png) must be copied into the running web pod:
```
kubectl cp jax_logo_23.png <omero-web-pod-name>:/opt/omero/web/OMERO.web/var/static/webgateway/img
```
Alternatives: host logo separately (e.g. GitHub) and change environment value to URL, or add to image (I didn't get this working, the static file creation might overwrite the folder I wanted)