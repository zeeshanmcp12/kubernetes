# Notes about Kubernetes

### What is Kubernetes
- Kubernets is a framework for building distributed platforms. It means that it has a capability to build distributed platform.

### Current version of k8s at the time of writing these notes
- version 1.25

### History of k8s
- Refer to images

### Play with K8s
- minikube -> if we want to play around kubernetes on our local computer. mikikube basically works as a small vm. It works as cluster. All the processes runs similar to cluster.
- kops -> old tool on AWS
- aks (Azure Kubernetes Services)
- gks (Google Kubernetes Services)
- ...etc

### Main components of k8s
- etcd cluster
  - This stores cluster information
  - It should be setup as highly available
  - It works as database to store configuration in key-value pair format
  - Only Api server can talk with this etcd cluster
- api server
  - this api server talks with scheduler and controller manager
- scheduler
- contoller manager
- worker nodes
  - kube-proxy
    - to manage networking inside cluster
  - kubelet
    - to manage pods
  - docker
    - to manager containers

#### Master node
- following components run in master node or contol node.
  - api server
  - scheduler
  - controller manager

### Tool to work with k8s
- kubectl
  - command line utility to work with kubernetes
