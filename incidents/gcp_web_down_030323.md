# Incident
On March 3, 2023, we got a ticket saying that images.jax.org was unavailable - we think it had been down since maybe 4pm the previous day.

Web pods crashed and then couldn't restart because of a failed nginx image pull.

It appears this image pull only doesn't work on private kubernetes.

# Solution
Pull nginx image from docker to local, push to our artifact registry, change yml to point to that image in our registry instead of general nginx image

```
docker pull --platform linux/amd64 nginx:1.21.4
docker tag <image_id> us-east1-docker.pkg.dev/jax-omero-pub/jax-omero-repo/nginx:1.21.4
docker push us-east1-docker.pkg.dev/jax-omero-pub/jax-omero-repo/nginx:1.21.4
```

# Testing: On private kubernetes

## Postgres succeeds
```
15s         Normal    Pulling                pod/database   Pulling image "postgres:11"
9s          Normal    Pulled                 pod/database   Successfully pulled image "postgres:11" in 6.383537619s
```

## Nginx 1.21.4 fails
```
56s         Normal    Pulling                pod/jax-omero-admin-web-68df44c97c-mlzv4   Pulling image "nginx:1.21.4"
2m40s       Warning   Failed                 pod/jax-omero-admin-web-68df44c97c-mlzv4   Failed to pull image "nginx:1.21.4": rpc error: code = Unknown desc = failed to pull and unpack image "docker.io/library/nginx:1.21.4": failed to resolve reference "docker.io/library/nginx:1.21.4": failed to do request: Head "https://registry-1.docker.io/v2/library/nginx/manifests/1.21.4": dial tcp 34.205.13.154:443: i/o timeout
86s         Warning   Failed                 pod/jax-omero-admin-web-68df44c97c-mlzv4   Error: ErrImagePull
71s         Normal    BackOff                pod/jax-omero-admin-web-68df44c97c-mlzv4   Back-off pulling image "nginx:1.21.4"
71s         Warning   Failed                 pod/jax-omero-admin-web-68df44c97c-mlzv4   Error: ImagePullBackOff
86s         Warning   Failed                 pod/jax-omero-admin-web-68df44c97c-mlzv4   Failed to pull image "nginx:1.21.4": rpc error: code = Unknown desc = failed to pull and unpack image "docker.io/library/nginx:1.21.4": failed to resolve reference "docker.io/library/nginx:1.21.4": failed to do request: Head "https://registry-1.docker.io/v2/library/nginx/manifests/1.21.4": dial tcp 44.205.64.79:443: i/o timeout
```

## Nginx 1.21.6 succeeds
```
12s         Normal    Pulling                  pod/jax-omero-admin-web-74f87b49bc-jtrj2   Pulling image "nginx:1.21.6"
8s          Normal    Pulled                   pod/jax-omero-admin-web-74f87b49bc-jtrj2   Successfully pulled image "nginx:1.21.6" in 3.878238561s
```

# Testing: On public kubernetes
## Nginx 1.21.4 succeeds
```
4m44s       Normal   Pulling                pod/jax-omero-web-c55748845-82p72           Pulling image "nginx:1.21.4"
4m37s       Normal   Pulled                 pod/jax-omero-web-c55748845-82p72           Successfully pulled image "nginx:1.21.4" in 6.834041666s
```