# Minikube

Minikube runs as a local k8s cluster but is not geared towards production use.
Its a playground for testing k8s commands, deployments, etc.

## Installation

Install Minikube on MacOSX using brew:
```
brew install minikube
```

## Run

To start your minikube local cluster using docker run:
```
minikube start --driver=docker
```

## Tunneling
To route traffic from the local ip to a pod ip tunneling must be enabled (in addition to having a LoadBalancer running of course - see example in /services):
```
minikube tunnel
```

## Cleanup
First cleanup any resources created (`kubectl delete -f <configuration-file>`)
Note: if you delete a namespace all the resources in that namespace will also be deleted.
Then run: 
```
minikube delete
```
