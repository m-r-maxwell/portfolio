+++ 
draft = false
date = 2024-05-15T14:35:26-04:00
title = "Kubernetes Cheat Sheet"
description = ""
slug = ""
authors = []
tags = ["docker", "kubernetes"]
categories = []
externalLink = ""
series = ["Dev tools"]
+++

One of the things I have been learning recently is kubernetes, but it's kind of hard to learn without setting up kubernetes in a public cloud. That's where [minikube](https://minikube.sigs.k8s.io/docs/) comes in!

Minekube let's you set up a cluster running on your machine! Making it much easier to learn or play around with kubernetes.
Here is also a cheatsheet for kubectl commands.

### Using minikube
```shell
# Start a Minikube cluster
minikube start
# Verify if the cluster is running
kubectl cluster-info
```

### Cluster Management
```shell
# Get information about nodes in the cluster
kubectl get nodes
# Get a list of namespaces (e.g., dev, prod)
kubectl get namespaces
# Get all pods across all namespaces
kubectl get pods -A
# Get all services across all namespaces
kubectl get services -A
# Apply a namespace definition from a YAML file
kubectl apply -f namespaces.yml
# Delete a namespace from a YAML file
kubectl delete -f namespaces.yml
```

### Pods Management
- Pods are the smallest deployable units in kuberneters and represent a running instance of a container
- Deployements manage pods

```shell
# Get information about nodes in the cluster
kubectl get nodes
# Get a list of namespaces (e.g., dev, prod)
kubectl get namespaces
# Get all pods across all namespaces
kubectl get pods -A
# Get all services across all namespaces
kubectl get services -A
# Apply a namespace definition from a YAML file
kubectl apply -f namespaces.yml
# Delete a namespace from a YAML file
kubectl delete -f namespaces.yml
```

### Services
- Services provide a way to expose pods to other services or clusters
- Handles load balancing as well

```shell
# Get services in the "development" namespace
kubectl get services -n development
```

### Tearing down
- You can delete resources by provding the YAML file.
- Deleting a namespace deletes everything within it

```shell
# Delete resources defined in namespaces.yml
kubectl delete -f namespaces.yml
```