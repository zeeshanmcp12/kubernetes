# Notes about Kubernetes

### What is Kubernetes
- Kubernets is a framework for building distributed platforms. It means that it has a capability to build distributed platform.

### Current version of k8s at the time of writing these notes (29th October 2022)
- version 1.25

#### Current version in Azure for kubernetes cluster
- version 1.23.8 (1st October 2022)

### History of k8s
- [History](./images/history-of-k8s.png) of kubernetes
- Why we need any [orchestration tool](./images/why-we-need-orchestration-tool.png)?

### Best thing in Kubernetes
- It provides resilience
  - If a pod dies, it will be recreated till kubernetes has resources and until we change our desire

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
- controller manager
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
- namespaces are an virtual isolated environment and name of one namespace is 'default' which we work in it.
  - kubectl get namespaces
- If we want to see what's going on in any namespace then we can execute below command:
  - kubectl -n kube-system get pods

### How to interect with Kubernetes
- cli with kubectl
- web ui
- fedora project like cockpit-project.org


### How to get output of pod in yaml format
- kubectl get pod nginx-55f494c486-jsqf9 -o yaml > ./app-nginx.yaml
  - This command will send the output of above command in yaml format that we can use later on to create a template.

### Some commands
- kubectl get nodes
  - To see the running nodes, for example master or worker etc. for example:
    - NAME       STATUS   ROLES           AGE   VERSION
    - minikube   Ready    control-plane   15h   v1.25.0
- kubectl get nodes -o wide
  - To see the nodes with details for example, external ip etc
- kubectl run nginx --image nginx:alpine --port 80 -----> 
  - This command will create a pod as well as the deployment in older version but in newer version it only creates a pod and not the deployment.
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
- kubectl get cs
  - This will show the cluster state, for example:
    - Warning: v1 ComponentStatus is deprecated in v1.19+
    - NAME                 STATUS    MESSAGE                         ERROR
    - controller-manager   Healthy   ok
    - scheduler            Healthy   ok
    - etcd-0               Healthy   {"health":"true","reason":""}
- kubectl config get-clusters
  - to see if any cluster is running
- kubectl scale deployment nginx --replicas=4
- kubectl get pods -o wide -w
- kubectl scale deployment nginx --replicas=2
  - This will change the desired state to running or in simple words, adds some replica sets.
- kubectl get svc
- kubectl logs POD_NAME
- kubectl config get-clusters
  - NAME
  - minikube
- kubectl config get-contexts
  - CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
  - *         minikube   minikube   minikube   default
- kubectl cluster-info
- kubectl describe <command>
  - kubectl describe services nginx
  - kubectl describe deployment nginx
  - kubectl describe pod nginx
- kubectl get pods -o wide --all-namespaces=true
  - This will show pod details like namespace, pod name, it's ip and which node this pod is running in etc.
- kubectl config view
  - This will show the cluster configurations that stored in ~/.kube/config file
- kubectl config view --minify=true
  - This will show the active cluster configurations
- kubectl cluster-info
  - Output:
  - Kubernetes control plane is running at https://<ip>:<port>
- kubectl describe node <node_name>
  - This will show all information about node
- kubectl get all
- kubectl get replicasets
  - This will show the replica sets
- kubectl exec --it <pod_name> bash
  - To start/execute the pod and log into that. Similar to docker exec
  - kubectl exec --it multitool-6dd498589f-t7cvs bash
- kubectl logs -f <pod_name>
  - -f -> in follow up mode -> or trail mode
  - We can see logs only for pod and not for deployment or service.
- kubectl describe svc nginx
  - This command will display service information in detail, for example ip, port, node ip, endpoint etc
- kubectl exec --it <pod_name> -c <container_name> bash
  - If we have multi-container pod running we can use '-c' switch to log into any container we provided in above command.


- These commands works for Google Cloud platform to authenticate/authorize with gcp:
gcloud container clusters get-credentials my-first-cluster-1 --zone asia-south1-a --project ans-dev
gcloud init
gcloud config list
gcloud auth login 
gcloud config set project PROJECT_ID

### minikube commands
- minikube node add
  - To add a node

### Some more notes
- CSPs does not give access on master/control nodes and keeps it with them and highly available. We only have access on worker node and have flexibility to play around worker nodes.


### Where it stores configuration for kubernetes
- ~/.kube/config


### Expose service in Kubernetes
- There are three types of service in kubernetes
- NodePort
  - minikube service <service_name> --url
  - run this command in separate terminal otherwise the session will be timedout hence no exposure of service.
- LoadBalancer
  - minikube tunnel
  - This will assign an IP to pod and expose the service.
- ClusterIP
  - with this service, we can access cluster using it's name for example curl nginx.
  - 

### Create deployments in Kubernetes
- kubectl create deployment nginx --image=nginx:alpine
  - kubectl expose deployment nginx --type=LoadBalancer --port=80
- We can create deployments using yaml (template) file
  - kubectl apply -f <template_file.yaml>
    - -f -> file
  - kubectl delete -f <template_file.yaml>

### Important
- kubectl run nginx --image=nginx:alpine --port=80
  - This command will create a pod as well as the deployment in older version but in newer version it only creates a pod and not the deployment. So, to create a deployment along with the pod, we need to execute below command:
- kubectl create deployment nginx --image=nginx:alpine --port=80
- minikube service nginx --url
  - This will expose the service of type "NodePort".
  - It must be run in a separate terminal window to keep the tunnel open.
  - Above is very important when:
    - minikube is running under wsl
- kubectl api-resources | grep deployment
  - to check which APIs support current kubernetes object

### How to verify the access to k8s cluster
- Execute this command and see the output, if no errors then we can interect with cluster whether it is managed or local cluster (minikube, kube-adm etc)
  - kubectl get cs (component status)

### Troubleshooting service from inside the cluster
- dig apache.default.svc.cluster.local
  - We can execute this command from network-multitool

- matallb -> local loadbalancer 


### Create kubernetes yaml manifests
- Create Service
  - kubectl expose deployment nginx --type=NodePort --dry-run=client -o yaml > support-files/nginx-service.yaml

### Rolling updates in Kubernetes
- curl -sI <url>:<port>(if any)
  - to include the headers in request, for example, HTTP status, Server, Date, Content-Type, Content-Length, Connection etc
  - add above command in loop to verify the rollout status
    - while true ; do curl -sI localhost:9080 | grep Server; sleep 1; done
- kubectl set image deployment nginx nginx=nginx:1.9.5 --record
  - this will rollout an update (i.e. change nginx version from 1.9.1 to 1.9.5)
- kubectl rollout status deployment nginx
  - This will show the status of rolling out an update for example:
    - Waiting for deployment "nginx" rollout to finish: 2 out of 4 new replicas have been updated...
- kubectl rollout history deployment nginx
  - We can rollout history using above command for example:
    - REVISION  CHANGE-CAUSE
    - 1         <none>
    - 2         kubectl set image deployment nginx nginx=nginx:1.9.5 --record=true
    - 3         kubectl set image deployment nginx nginx=nginx:1.12.2 --record=true
- kubectl rollout status deployment nginx
  - We can undo image changes (or image rollout) incase of any error for example, image not found etc


### Configmaps and Secrets
#### Secrets
- kubectl create secret tls nginx-certs --cert=./certs/tls.crt --key=./certs/tls.key
  - tls -> is a type of secret
  - nginx-certs -> name of secret
  - --cert -> requires .crt file
  - --key -> requires .key file
- kubectl get secrets
  - shows the secret we create
  - NAME          TYPE                DATA   AGE
  - nginx-certs   kubernetes.io/tls   2      3m19s
- kubectl describe secret
  - shows more information about secret
  
#### Configmaps
- kubectl create configmap nginx-config --from-file=support-files/nginx-connectors.conf
  - Create configmap
  - nginx-config -> name for the cm
  - --from-file -> switch to accept the file
- kubectl get configmaps ----> kubectl get cm
- kubectl describe configmap nginx-config
- kubectl get secrets,cm


#### curl useful commands
- curl -k https://localhost
  - ignore ssl warning
- curl -sI https://127.0.0.1
  - add headers in response