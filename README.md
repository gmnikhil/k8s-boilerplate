# k8s-boilerplate

This repo aims to get one started with the basics of `kubernetes`.

## What is kubernetes?
Kubernetes is an open-source container-orchestration system for automating computer application deployment, scaling, and management. It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation.

### Advantages
- High availability or no downtime
- Scalability or high performance
- Disaster recovery

## Basic Terms:

### Container
- way to package application with all necessary dependencies and configuration
- package is portable, easily shared, moved around
- Each container that you run is repeatable. the standardization from having dependencies included means that you get the same behavior wherever you run it.
- Containers decouple applications from underlying host infrastructure. This makes deployment easier in different cloud or OS environments.
- it is a running environment for image.

### Images
- A container image represents binary data that encapsulates an application and all its software dependencies. 
- Container images are executable software bundles that can run standalone and that make very well defined assumptions about their runtime environment.

### Pod
- pod is the smallest unit of k8s
- it is an abstraction over container (layer ontop of container) - only interacts with kubernetes layer
- Usually 1 application per pod
- Each pod gets its own IP address
- New IP address on recreation

### Service
- permanent IP address
- lifecycle of pod and service are not connected - so even if pod dies, service is available
- load balancer => transfers load to less busy pod

### Ingress
- does forwarding
- routes traffic into cluster

### ConfigMap
- contains external configuration of applications like database url, service enedpoint etc

### Secret
- just like ConfigMap but used for secret data
- base64 encoded

`Data from ConfigMap/secret can be used in application via environment variables/properties file`

### Volume
- Attaches a physical storage (like an exernal hard-drive) to node. 
- storage can be 
    - local on local machine (in k8s cluster) 
    - remote outside of k8s cluster (cloud storage, my own pc)
- Volumes are necessary because data will be lost if something bad happens to k8s cluster.

### Deployment
- Deployments represent a set of multiple, identical Pods with no unique identities. 
- A Deployment runs multiple replicas of your application and automatically replaces any instances that fail or become unresponsive.

### StatefulSet
- takes care of databsaes and data.
- should use Statefulset and not Deployment when using databases.
- StatefulSets are not easy to work with.


## Architecture

### Each node has multiple pods running on it

### 3 processes must be installed on every Node

- `Kubelet`
   - interacts with both container and node
   - starts pod with container inside
   - communication via services

- `Kube proxy`
   - forwards the requests
   - it is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept. 
   - maintains network rules on nodes. 
   - These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

- `Container runtime`
   - software responsible for running containers
   - need to install a container runtime into each node in the cluster so that Pods can run there.

## Master nodes:

### 4 processes run on every master node:

- `API server`
   - cluster gateway
   - acts as gatekeeper for authentication(for creating new nodes, services etc)
   - validates request and forwards to other process

- `Scheduler`
   - API Server passes info to Scheduler
   - if we need to create a new pod, scheduler looks at the nodes, compares utilisation and cretes new pod on less busy one
   - Scheduler just decides on which Node new Pod should be scheduled.
   - process execution is done by kubelet

- `Controller Manager`
   - watches shared state of cluster through API server
   - attempts to move current state to desired state

- `etcd`
   - cluster brain
   - it is a key value store
   - everytime node changes, it gets updated and stores on etcd
   - controller manager, scheduler, api server gets info from etcd- hence called cluster brain
   - app data is not stored in etcd
