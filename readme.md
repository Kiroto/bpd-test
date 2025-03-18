# BPD Test

This repository holds the dockerization and kubernetes files to create a pod for the adminapi and clientapi projects. The purpose of this collection of projects is to display my skills making a RESTful API in typescript, securing with TLS 1.2, using a database (in this case, MongoDB), dockerizing and creating kubernetes pods.

## Using this repository

This repository must be the root of the adminapi and clientapi repositories. You may use the following commands to create docker images and subsequently kubernetes pods for them.

Note that if you're not going to use my docker images, you must modify the kubernetes configurations accordingly, specifically, the container images used.

> ⚠️ You will need to supply your own web certificate files: `/server.crt`, `/server.csr` and `/server.key`. For the docker-compose, these must be inside a `/certs/` directory within the root of the project.

### Docker

`docker-compose build` to build the API images.

`docker-compose push` to push the API images to your dockerhub repository. You will need to be logged in to docker.

You may want to run the instances locally. In that case you may use `docker-compose up --build`. By default, adminapi runs on port 443, clientapi runs on port 5001, and the mongo database on port 27017. If you use self-signed certificates, you may need to accept self-signed certificates from your endpoint consumer's requests.

### Kubernetes

Make sure you have a kubernetes context up and running. You may use `minikube` for a local instance of kubernetes, for testing/development purposes.

`kubectl apply -f k8s/` to apply the configurations to a new series of pods and services.

`kubectl get pods` to get the current pods. adminapi, clientapi, and mongo live here.

`kubectl get services` to get the current services. adminapi-service and the mongo database service live here.

#### Required secrets

For the kubernetes pods, you will need to have configured the following secrets:

* `mongo-secret` Information about mongo and connections to it.
    * `mongo-host`: The host and port of the mongo database service.
    * `mongo-database`: The name of the mongo database to be used.
    * `mongo-root-username`: The root username of the mongo service to the used.
    * `mongo-root-password`: The root password of the mongo service to be used.
    * `mongo-auth-source`: The authentication source inside mongo that will use this information.

* `ssl-certificates` For Https connections.
    * `server.crt`: The certificate.
    * `server.key`: The certificate key.

### Troubleshooting

If the docker service isn't on by default on your computer, remember to run it.
If you're using minikube, remember to start it with `minikube start`.
