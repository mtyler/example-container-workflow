# example container workflow

## Outcome
Enablement instructions for creating a reference container lifecycle workflow that supports
- developing
- testing
- building
- deploying
- running
- changing


## Steps taken

1. log into GCP and create a container cluster
2. log into docker hub

# Step 1 - Manually push a single container to Kubernetes

* clone this repo
* cd example-container-workflow/teir_web
* docker build -t teir_web .
* Test to ensure it works
  docker run --name some-nginx -d -p 8080:80 [docker_org]/teir_web
  http://localhost:8080
* docker login [You will need a dockerhub account](cloud.docker.com)
* docker push <docker_org>/teir_web

- initialize GCP/k8s
* install google-cloud-sdk
* install kubectl
* gcloud init
* create k8s cluster
* connect to the cluster (see connection string in web ui)
* kubectl get nodes
* kubectl run web --image <docker_org>/teir_web
* kubectl expose deployment web --type=LoadBalancer --name=web --port=80
* kubectl get services

# TODO

- publish Dockerfile
