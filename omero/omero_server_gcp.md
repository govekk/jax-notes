# Check which project I am using
```
gcloud config get-value project
```

# Create private kubernetes
https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters

Create network
```
gcloud compute networks create test-omero-vpc \
    --subnet-mode custom
gcloud compute firewall-rules create --network=test-omero-vpc test-default-allow-ssh --allow=tcp:22
gcloud compute networks subnets create test-subnet --network=test-omero-vpc --range=10.131.2.0/24
```

Command line create private kubernetes
```
gcloud container clusters create-auto test-omero-cluster \
    --region "us-east1" \
    --subnetwork test-subnet \
    --enable-master-authorized-networks \
    --network test-omero-vpc \
    --enable-private-nodes \
    --enable-private-endpoint
```

Give VM control plane access (if VM address is 10.131.2.5)
```
gcloud container clusters update test-omero-cluster \
    --enable-master-authorized-networks \
    --region=us-east1 \
    --master-authorized-networks 10.131.2.5/32
    
```

"
Now these are the only IP addresses that have access to the control plane:
- The primary range of my-subnet-0.
- The secondary range used for Pods.
- Address ranges that you have authorized, for example, 10.131.2.5/32.
"

# Create VM
```
gcloud compute instances create control-vm --zone=us-east1-b --machine-type=e2-small --network-interface=network-tier=STANDARD,private-network-ip=10.131.2.5,subnet=test-subnet --metadata=enable-oslogin=true --maintenance-policy=MIGRATE --provisioning-model=STANDARD --no-service-account --no-scopes --create-disk=auto-delete=yes,boot=yes,device-name=instance-4,image=projects/debian-cloud/global/images/debian-11-bullseye-v20221206,mode=rw,size=10,type=projects/jax-ritci-sandbox02/zones/us-central1-a/diskTypes/pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any
```

```
gcloud compute ssh control-vm --zone=us-east1-b
```

## Set up kubectl
```
sudo apt-get install google-cloud-sdk-gke-gcloud-auth-plugin kubectl
```

```
gcloud auth login
```

https://cloud.google.com/blog/products/containers-kubernetes/kubectl-auth-changes-in-gke 
(Or else get errors about using the wrong auth plugin when try to use any kubectl command)
(annoying other people: https://github.com/actions/runner-images/issues/6778)
- They changed so they didn't remove the plugin before 1.26, so it warns but doesn't error
```
vi ~/.bashrc
i
<copy/paste> export USE_GKE_GCLOUD_AUTH_PLUGIN=True
:wq
source ~/.bashrc
```

```
gcloud container clusters get-credentials test-omero-cluster --zone=us-east1
```

```
kubectl version
```

v1.25.4

## Copy over files
```
gcloud compute scp --recurse ./k8_gcp control-vm:~/k8_gcp --zone=us-east1-b
```

# Might need to set up Cloud NAT to give private IP nodes internet access
- Needed to figure this out anyways for Google Load Balancer

# Questions:
- can I access control plane still if within authorized networks, even on Private cluster?
    - "With "Control plane access using its external IP address" disabled, only internal Network IP ranges are allowed for control plane authorized networks"
    - "cluster update failed" trying to add my external JAX IP
- What happens if I just try to get-credentials and use kubectl from home computer, without adding the authorized networks?
    - get-credentials works, but kubectl gets "timeout i/o" message (same as before w/ public omero)