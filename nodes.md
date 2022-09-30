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

### Objects in Kubernetes
#### Common objects in kubernetes
- ingress
  - We use this if we want to expose any service to external network or outside of our network.
  - Here comes Application gateway, Nginx (as reverse proxy) etc
- service
  - for example nginx, apache etc
- deployment
  - Deployment can be part of multiple pod to make them highly available
  - pods are normally part of any deployment (object in k8s).
  - pods aren't directly part of deployment but a replica set.
  - Deployment manages replica set and replica sets manages pods.
- pods
  - Every pod can contain one to many containers
- containers
  - these are the containers
- nodes
  - This could be a vm/host or any physical machine

#### Storage objects in kubernetes
- PVC -> persistent volume claim
- PVC is attached with pod. In this case, it could be mysql db with persistent volume claim.
- PVC (persistent volume claims) takes some slice of space from PV (persistent volume)
- ...this pv takes storage from any physical or remote storage for example, NFS, Storage account, etc

#### Configmaps and Secrets
- Configmaps
  - For example: db name, url, ip address
  - For exmaple: apache web server needs virtual box configuration.
  - ...So, we use this configmaps object and ask kubernetes to pass these configurations to containers.
  - It could be a complete file, username, db url etc
  - Once the configuration has been added in configmap, we define in pod that this configuration will come from configmaps.
- Secrets
  - These are secrets that we don't want to expose publically and store in any public repository etc
  - These comes from secrets' object in kubernetes.

#### Different controllers in Kubernetes
- Deployments
  - Rolling updates
  - Canary deployments
  - Rollbacks
    - pods are normally part of any deployment (object in k8s).
    - pods aren't directly part of deployment but a replica set.
    - Deployment manages replica set and replica sets manages pods.
- StatefulSets
  - Rolling updates
- CronJobs
  - Cron job
- Daemon Sets

### Namespaces in Kubernetes
- Kubernets support multiple virtual clusters backed by same physical k8s cluster.
- In simple words, k8s makes our life easier by creating multiple virtual clusters (namespaces) to support our different environment i.e. development, test and production.
- These virtual clusters are called namespaces.
- It's a very big advantage to have sandbox environment in same cluster.

### How to interect with Kubernetes
- cli with kubectl
- web ui
- fedora project like cockpit-project.org


### Some commands
- kubectl get nodes -> to see the nodes
- kubectl get nodes -o wide
  - To see the nodes with details for example, external ip etc
- kubectl run nginx --image nginx:alpine --port 80
  - This will create a deployment.
  - In this deployment in will create a replica set.
  - In this replica set it will create a pod.
- kubectl get deployments
  - To see how many deployments we have currenly running
- kubectl get pods
  - To see how many pods are running
- kubectl expose deployment nginx --type=LoadBalancer
  - We will create a service to expose the deployment
- kubectl get services
  - To see which services are running
- kubectl logs <pod_name>
  - To see the logs for running pod
- kubectl delete pod POD_NAME
- kubectl scale deployment nginx --replicas=4
- kubectl get pods -o wide -w
- kubectl scale deployment nginx --replicas=2
- kubectl get cs
- kubectl expose deployment nginx --type=LoadBalancer
- kubectl get services
- kubectl get svc
- kubectl logs POD_NAME
gcloud container clusters get-credentials my-first-cluster-1 --zone asia-south1-a --project ans-dev

gcloud init

- These commands works for Google Cloud platform to authenticate/authorize with gcp:
gcloud config list
gcloud auth login 
gcloud config set project PROJECT_ID

### Some more notes
- CSPs does not give access on master/control nodes and keeps it with them and highly available. We only have access on worker node and have flexibility to play around worker nodes.