# example container workflow

## Outcome
Enablement instructions for creating a reference container lifecycle workflow that supports
- developing
- testing
- building
- deploying
- running
- changing


## Pre-req Steps taken

1. log into [GCP](https://console.cloud.google.com) and create a container cluster
1. install google-cloud-sdk
1. install kubectl
2. log into [docker hub](https://hub.docker.com/)
   - take not of your docker organization, it will be referred to below as `<docker_org>`

# Step 1 - Manually push a single container to Kubernetes

* clone this repo
* `cd example-container-workflow/teir_web`
* Build the web container `docker build -t <docker_org>/teir_web .`
* Test to ensure it works
  `docker run --name some-nginx -d -p 8080:80 <docker_org>/teir_web`
  point browser to: http://localhost:8080
* `docker login` [You will need a dockerhub account](cloud.docker.com)
* publish container image `docker push <docker_org>/teir_web`

## initialize GCP/k8s

* `gcloud init`
* create k8s cluster (look for Kubernetes Engine in GCP)
* connect to the cluster & run proxy (see connection string in web ui, something like gcloud container..)
* List the nodes in the cluster `kubectl get nodes`
* Run the container `kubectl run web --image <docker_org>/teir_web`
* Expose the container `kubectl expose deployment web --type=LoadBalancer --name=web --port=80`
* The ip will be exposed under EXTERNAL-IP here `kubectl get services`

There should now be a static web page exposed on the IP above.  Yay!  Obviously, there are a lot of real world things that are not considered.  Some things to note:
- this only addresses initial deployment
- source control is outside of the dev to prod path
- testing/quality is not enforced
- there are a lot of manual steps

# Step 2 - Push a change to that container
* make a change to teir_web/static-html-directory/index.html
* Build the web container with version info `docker build --no-cache -t <docker_org>/teir_web:1.0.1 -t <docker_org>/teir_web:latest .`
* publish container image `docker push <docker_org>/teir_web`
* get deployment name `kubectl get deployments` will return NAME
* get history `kubectl rollout history deployment/<NAME>`
* ## increase replicas to 3 `kubectl scale deployment <NAME> --replicas=3`
* deploy container `kubectl set image deployment/web web=<docker_org>/teir_web:1.0.1`

Now we can make changes, but all the other things still need to be addressed

# Step 3 - Introduce source control and pipeline

# Step 4 - Manage multiple tiers
