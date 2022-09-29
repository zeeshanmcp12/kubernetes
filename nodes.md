# Notes about Kubernetes

### What is Kubernetes
- Kubernets is a framework for building distributed platforms. It means that it has a capability to build distributed platform.

### Current version of k8s at the time of writing these notes (29th October 2022)
- version 1.25

### History of k8s
- [History](./images/history-of-k8s.png) of kubernetes
- Why we need any [orchestration tool](./images/why-we-need-orchestration-tool.png)?

### Play with K8s
- minikube -> if we want to play around kubernetes on our local computer. mikikube basically works as a small vm. It works as cluster. All the processes runs similar to cluster.
- kops -> old tool on AWS
- aks (Azure Kubernetes Services)
- gks (Google Kubernetes Services)
- ...etc

### Main components of k8s
- etcd cluster
  - This stores cluster information
  - It should be setup as highly available because it contains all information about cluster so if it corrupted then whole cluster will corrupt.
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

#### Worker node
- following components run in worker node
  - kube-proxy
  - kubelet
  - docker

#### Tool to work with k8s
- kubectl
  - command line utility to work with kubernetes


#### Pod
- is a collection of container which lives together and die together
- pod is a work unit in kubernetes similar to container which is a work unit in docker
- if one container inside a pod is die then the whole pod will die and recreated.
- if a pod is died then this pod will never be live/up again but recreated with new id etc.

#### How pod works
- tomcat(8080)
- Following are helper containers
  - initContainer
  - pause
  - proxy (80/443)

#### How many containers usually pod contains
- Every pod has atleast 2 containers. One is hidden than main image. For example:
  - tomcat
  - pause -> this container is hidden because it only reserve the ip and keep it till the pod is died.


#### Common objects in kubernetes
- ingress
  - We use this if we want to expose any service to external network or outside of our network.
  - Here comes Application gateway, Nginx (as reverse proxy) etc
- service
  - for example nginx, apache etc
- deployment
  - Deployment can be part of multiple pod to make them highly available
- pods
  - Every pod can contain one to many containers
- containers
  - these are the containers
- nodes
  - This could be a vm/host or any physical machine

#### Storage objects in kubernetes