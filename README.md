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

# TODO

- local Dockerfile workflow
* cd teir_web
* docker build -t teir_web .
* docker run --name some-nginx -d -p 8080:80 [docker_org]/teir_web
* http://localhost:8080
* docker login [YAK: create dockerhub account]
* docker push [docker_org]/teir_web

- initialize GCP/k8s
* install google-cloud-sdk
* install kubectl
* gcloud init
* create k8s cluster
* connect to the cluster (see connection string in web ui)


* kubectl expose deployment pig-farm --type=LoadBalancer --name=pig-farm --port=80

- publish Dockerfile
