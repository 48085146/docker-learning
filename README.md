# Docker Learning Project

This is the docker learning project, I publish it directly from Visual Studio Code.

## Hands-On Setup

A laptop/PC with Windows 10, Mac OS or Linux
Visual Studio Code
Docker Desktop with Kubernetes enabled (Or another way to run Kubernetes locally)
A Docker Hub account
Tools

## Lab Files
Clone or donwload he repo from GitHub
https://github.com/K8sAcademy/Fundamentals-HandsOn

## From Monolith to Microservices

Break your application/system in small units
Use the strangler pattern
https://martinfowler.com/bliki/StranglerFigApplication.html

## Microsservices - Benefits

Improved faut isolation
Eliminate vendor or technology lock-in
Ease of Understanding
Smaller and faster deployments
Scalability

## Microsservices - Drawbacks

Complexity is added to resolve complexity issues
Testing may appear simpler but is it ?
Deployment may appear simpler but ist it ?
Multiple databases
Latency issues.
Transient errors
Multiple point of failures
How about security?

## Docker CLI Management Commands
Docker CLI Cheet Sheet - Management
docker info     Display system information
docker version  Display the system's version
docker login    Log in to a Docker registry

## Docker Running Container

### Docker CLI Cheat sheet - Running and stopping
docker pull [impageName]        Pull an image from a registry
docker run [imageName]          Run containers
docker run -d [imageName]       Detached mode
docker start [containerName]    Start stopped containers
docker ps                       List running containers
docker ps -a                    List running and stopped containers
docker stop [containerName]     Stop containers
docker kill [containerName]         Kill containers
docker image inspect [imageName]    Get image info  

imageName: the name of the image as you find it in the container registry
containerName: the name of the running container

### Running a container
#pull and urn an nginx server (nginx: Container image in the Docker register; webserver: Container local Name; --publish Maps the host port to the container listening port)
docker run --publish 80:80 --name webserver nginx

#list the running containers
docker ps

#stop the container
docker stop webserver

#remove the container
docker rm webserver

### Docker CLI Cheat Sheet - Cleaning up
docker rm [containerName]       Removes stopped containers
docker rm $(docker ps -a -q)    Removes all stopped containers
docker images                   Lists images
docker rmi [imageName]          Deletes the image
docker system prune -a          Removes all images not in used by any containers

### Let's run an Nginx container
#Type the following Docker command
Pull and run a Nginx server
docker run -d -p 8080:80 --name webserver nginx

List the running containers
docker ps

List the images
docker images

Attach to the container
docker container exec -it webserver bash

Stop the container
docker stop webserver

Remove the container from memory
docker rm webserver

Remove the image
docker rmi nginx

## Docker CLI Building Containers
### Docker CLI Cheat Sheet - Building

docker build -t [name:tag] .
#Builds an image using a Dockerfile located in the same folder

docker build -t [name:tag] -f [fileName]
#Builds and image using a Dockerfile located in a different folder

docker tag [imageName] [name:tag]
#Tag an existing image

### Dockerfile - static HTML site
----------------------------------
FROM nginx:alpine
COPY . /usr/share/nginx/html

#build
docker build -t webserver-image:v1 .

#run
docker run -d -p 8080:80 webserver-image:v1

#display
curl localhost:8080

----------------------------------
FROM alpine
RUN apk add -update nodejs nodejs-npm
COPY . /src
WORKDIR /SRC
RUN npm install
EXPOSE 8080
ENTRYPOINT ["Nnode", "./app.js"]

### Docker CLI - Tagging
docer tag => Create a target image
    name:tag
        myimage:v1
    repository/name:tag
        myacr.azureecr.io/myimage:v1

## Visual Studio Code

### Let's containerize an Express Website


## Persisting Data
### Containers are ephemerous and sateless
You usually don't store data in containers
Non-persistent data
    Locally on a writable layer
    It's the default, just write to the filesystem
    When containers are destroyed, so the data inside them
Persistent data
    Stored outside the container in a Volume
    A volume is mapped to a logical folder
## Volumes
Maps a folder on the host to a logical folder in the container
### Volumes Cheat Sheet
docker create volume [volumeName]
#Creates a new volume

docker volume ls
#list the volumes

docker volume inspect [volumeName]
#display the volume info

docker volume rm [volumeName]
#Deletes a volume

docker volume prune
#Deletes all volumes not mounted (Caveat)

## Mapping a volume
#create a volume
docker volume create myvol

#inspect the volume
docker volume inspect myvol

#list the volumes
docker volume ls

#run a container with a volume
docker run -d  --name devtest -v myvol:/app nginx:latest

#run a container with a volume
docker run -d --name devtest -v d:/test:/app nginx:latest'

#inspect the container
docker inspect devtest

## Docker Volumes Hand On
#Open a terminal and create a volume
docker volume create myvol

#List the volumes
docker volume ls

#Run a Nginx container that will use the volume
docker run -d --name voltest -v myvol:/app nginx:latest

#Connect to the instance
docker exec -it voltest bash

#Let's create a file in the volume using Nano
apt-get update
apt-get install nano

#Create a file in the app folder
cd app
nano test.text

#Type something, save the file and exit Nano using:
CTRL-O (O for Orange)
CTRL-X (to exit)

#Detach from the instance (responding to docker exec)
exit 

#Stop and remove the container
docker stop voltest
docker rm voltest

#Run it again and see if the file still exits
docker run -d --name voltest -v myvol:/app nginx:latest
docker exec -it voltest bash
cd app
cat test.txt

#Clean up
docker volume rm myvol (error coz the myvol is in using)
docker stop voltest
docker rm voltest
docker volume rm myvol

## YAML
YAML: YAML Ain't Markup Language
Human friendly data serialization standard
Used by Docker-Conpose and Kubernetes

www.yamllint.com

## Docker Compose Conceptes
Docker Compose
Define and run multi-containers applications
Define using YAML files
Run using the docker CLI with the compose plubin
    Docker compose
Compose Specs
    https://compose-spec.io

Compse V2
GA ata DockerCon Live 2022
Incorporates the docker compose command into the Docker CLI
Installed with Docker Desktop
    Linux:apt-get intall docker compose-plugin
Writen in Go
    Docker compose is written in Python

In summary, it's simply a faster version of the good old docker compose tool that is shipped as a plugin insead of a Python app

## Docker Compose - Use Cases
Workloads that don't require a full orchestrator
Development and tests
Use of a service that can run Docker Compose files
    Azure App Service
    AWS ECS
    Virtual machines

## Docker Compose Commands
** Docker Compose Cheat Sheet **
docker compose build        Build the images
docker compose start        Start the containers
docker compose stop         Stop the containers
docker compose up -d        Bild and start
docker compose ps           List what's running
docker compose rm           Remove from memory
docker compose down         Stop and remove
docker compose logs         Get the logs
docker compose exec [container] bash    Run a command in a container

** Compose V2 - New Commands **
docker compose --project-name test1 up -d
#Run an instance as a project

docker compose -p test2 up -d
#shortcut

docker compose ls
#list running projects

docker compose cp
[containerID]:[SRC_PATH] [DEST_PATH]
#Copy files from the container

docker compose cp
[SRC_PATH] [containerID]:[DEST_PATH]
#Copy files to the container

** Docker Compose **
#build the service
docker compose build

#build, (re)recates, starts, attaches to containers for a service
docker compose up

#list the services
docker compose ps

#bring down what was created by UP
docker compose down

## Docker Compose Lab
** Build the app **
docker compose build

** Run the app **
docker compose up -d

When the app will run, lanuch the voting app in your browser http://localhost:5000

** Look at the db container logs **
docker compose logs -f web-fe

** Compose V2 commands **
LS will list the current projects
docker compose ls

** Let's try to do deploy a second version
docker compose up -d
This fails because we can only run an app a single time

** Deploy a second version using a different project name **
Let's now use a project name to see if we can deploy a second version
docker compose -p test up -d
This fails because the lcoalhost port 5000 is already assigned.
If need to execute it successfully, change the 5000 in YAML to 5001

** Cleanup **
docker compse down
docker compose ls

#docker compose -p <project_name> down
#this will bring down the one created by sepcifying the project name in the upper section

## Docker Compose Sample App ##
Sample App: React -> Node -> MariaDB

## Docker Compose Features ##
** Resource Limits **
** Environment variables **
** Networking **
** Dependence **
** Volumes - Named **
** Restart Policy **
no
    The default restart policy
    Does not restart a container under any circumstances
always
    Always  restart the container until its removal.
on-failure
    Restart a container if the exit code indicates an error.
unless-stopped
    Restarts a container irrespective of the exit code but will stop restarting when the service is stopped or removed.

## Container Registry"
** What are Container Registries? **
Central repositories for container images
Private or public
Docker Hub - hub.docker.com
Microsoft - Azure Container Registry; Microsfot container Registry (public images)
Amazon Elastic Contaner Registry
Google Container Registry

## Container Registries Docker Hub ##
** Publish to Docker Hub **
#login to Docker Hub
docker login -u <username> -p <password>

#tag the image previously built
docker tag my_imaege k8sacademy/my_image:latest

#push the image
dokcker push k8sacademy/my_image:latest

#pull the image
docker pull k8sacademy/my_image:latest

## Kubernetes ##
** Kubernetes a..a. K8s **
Originated from Google
v1.0 released on July 2015
3rd gen container scheduler from Google
Donated to the Cloud Native Computing Foundation (CNCF: www.cncf.io)

** What is Kubernetes ? **
K8s is the leading container orchestration tool
Designed as a loosely coupled collection of components cnetered around deploying, maintaining and scaling workloads.
Vendor-neutral (Runs on all cloud providers)
Backed by a huge community

** What K8s can do **
Service discovery and load balancing
Storage orchestration (local or Cloud based)
Automated rollouts and rollbacks
Self-healing
Secret and configuration management
Use the same API across on-premise and every cloud providers

** What K8s can't do **
Does not deploy source code
Does not build your application
Does not provide application level services (Message buses, databases, caches, etc)

https://kubernetes.io/docs/concepts/overview/components/

## Running Kubernetes locally ##
** Local K8s **
Requires virtualization
    Docker Desktop
    MicroK8s
    Minikube
Runs over Docker Desktop
    Kind
Limited to 1 node
    Docker Desktop
Multiple nodes
    MicroK8s
    Kind
    Minikube
** Local K8s - Windows **
Docker Desktop is currently the only way to run both Linux and Windows containers
Docker Desktop can run on Hyper-V or WSL (Win 10 2004)
If Hyper-V is enabled, you can't run another hypervisor at the same time
You can install Minikube on Hyper-V or VirtualBox

** MiniKube **
Does not require Docker Desktop
Install on Linux, macOS and Windows
    https://kubernetes.io/docs/tasks/tools/install-minikube/
An Hypervisor like VirtualBox is required

** Kind **
Kubernetes IN Docker
    https://kind.sigs.k8s.io/docs/user/quick-start/
Runs on macOS, Linux and Windows
Only requires Docker installed
    No need for another VM installation
    Installs the nodes as containers
Windows installation via Chocolatey
    choco install kind
macOS installaiton via Brew
    brew install kind

Multi nodes
High Availability Control Plane
Define in YAML

** Kubernetes local quick lab **
Let's test our local Kubernetes installation.
kubectl cluster-info

if the installation is correct, you should get something like this:
Kubernetes control plan is running at htps://kubernetes.docker.internal:6443
CoreDNS is running at http://kubernetes.docker.internal:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debut and diagnose cluster problems, use 'kubectl custer-info dump'

## K8s CLI & Context ##
** K8s API **
** CLI **
kubectl 
Communicates with the api server
Configuration stored locally
    ${HOME}/.kube/config
    C:\Users\{USER}\.kube\config
** K8s Conext **
A context is a group of access parameters to a K8s cluster
Contains a Kubernetes cluster, a user, and a namespace
The current context is the cluster that is currently the default for kubectl
    All kubectl commands run against that cluster

** kubctl - Context cheat sheet **
kubectl config current-context
#Get he current context

kubectl config get-contexts
#list all conext

kubectl config use-context [contextName]
#Set the current context

kubectl config delete-context [contextName]
#delete a context from the config file

** Kubectx - Quickly switch context **
Instead of typing
    kubectl config use-context minikube
Simply type
    kubectx [contextName]
Windows
    choco install kubectx-ps

https://github.com/ahmetb/kubectx

## Kubernetex context quick lab ##
** Kubernetes context **
Current context
get the current context
kubectl config curren-context

List all contexts:
List all the contexts
ku ectl config get-contexts

Change context
Use a context:
kubectl config use-context [contextName]

Using kubectx
What's great about Kubernetes is the incredible amount of tools created by the community and available for free. Kubectx is a simple tool that provides an easy way to list and change context.

You can install it on:
Windows (if you have Chocolatey installed):
choco install kubectx-ps

Rename context
Rename context:
kubectl config rename-context [old-name] [new-name]

Delete context
Delete context:
kubectl config delete-context [contextName]

## The declarative way vs the imperative way ##
** Imperative **
Using kubectl commands, issue a series of commands to create resources
Great for learning, testing and troubleshooting
It's like code

** Declaraive **
Using kubectl and YAML manifests defining the resources that you need
Reproducible, repeatable
Can be saved in source control
It's like data that can be parsed and modified

** YAML **
Root level requied prperties
apiVersion
    Api version of the object
kind
    type of object
metadata.name
    unique name for the object
metadata.namespace
    scoped environment name (will default to current)
spec
    object specifications or desired state

Create an object using YAML
    kubectl create-f [YAML file]

** Creating new manifests **
Referring to kubernetes.io/docs for an example, copy

** Creating new manifests - VS Code **

** Creating new manifests - kubectl**
kubectl create deply mynginx --image=nginx --port=80 --replicas=3 --dry-run=client -o yaml > deploy.yaml

** Declarative vs Imperaive **

Let's deploy an Nginx ocntainer using both methods.

Imperative
kubectl create deployment mynginx1 --image=nginx

Delclarative 
kubectl creeate -f deploy-example.yaml

Cleanup
kubectl delete deployment mynginx1
kubectl delete deploy mynginx2

## Namespaces ##
Allow to group resources
    Example: Dev, Test, Prod
K8s creates a default workspace
Objects in one namespace can access objects in a different one
    Example: objecname.prod.svc.cluster.local
Deleting a namespace will delete all its child objects

kubectl - Namespace Cheat Sheet
kubectl get namespace
#List all namespaces

kubectl get ns
#shortcut

kubectl config set-context --current --namespace=[namespaceName]
#Set the current context to use a namespace

kubectl create ns [namespaceName]
#cretate a namespace

kubectl delete ns [namespaceName]
#delete a namespace

kubectl get pods --all-namespaces
#List all pods in all namespaces

## Namespace Quick Lab ##
Get namespaces
Open a terminal and get the currently configured namespaces
kubectl get namespace
kubectl get ns

Get the pod list
Get a list of all the installed pods
kubectl get pods

You get the pods from the defaul names. Try getting the pods from the docker namespace. You will get a differnt list.
kubectl get pods --namespace=kube-system
kubectl get pods -n=kube-system

Change namespace
Change the namespace to the docker one and get the pods lists.
kubectl config set-context --current --namespace=kube-system

Get the pods
kubectl get pods

Now change back to the default namespace
kubectl config set-context --current --namespace=default
kubectl get pods

Create and delete a namespace
kubectl create ns [name]
kubectl get ns
kubectl delete ns [name]

## Master Node ##
** kube-apiserver **
REST interface
Save the state to the datastore (etcd)
All clients interact with it, never directly to the datastore

** etcd **
Act as the cluster datastore for storing state
key-value store
Not a database or a datastore for application to use
The single source of truth

** kube-control-manager **
The controller of controllers!
It runs controllers
    Node controller
    Replication controller
    Endpoints controller
    Service account & Token controllers

** cloud-control-manager **
Interact with the cloud providers controllers
    Node
        For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
    Route
        For setting up routes in the underlying cloud infrastructure
    Service
        For creating, updating and deleting cloud provider load balancers
    Volume
        For creating, attaching, and mounting volumes, and interacting with the cloud provider to orchestrate volumes

** kube-scheduler **
Watches newly created pods that have no node assigned, and selects a node for them to run on
Factors taken into account for scheduling decisions inlcude
    Individual and collective resource requirements
    Hardware/software/policy constraints
    Affinity and anti-affinity specifications
    Data locality

** Addons **
DNS
Web UI (dashboard)
Cluster-level logging
Container resource monitoring

## Worker Node ##
** kubelet **
Manage the pods lifecycle
Ensures that the containers described in the Pod specs are running and healthy

** kube-proxy **
A network proxy
Manages network rules on nodes

** Container runtime **
K8s supports serveral container runtimes
Must implement the Kubernetes Container Runtime Interface
    Moby
    Containerd
    Cri-0
    Rkt
    Kata
    Virtlet

** Container runtime - k8s V1.9+ **
Docker images run as is. It's business as usual!
What's changed is what you can do inside the cluster
    You can no longer access the Docker engine inside the cluster
    Docker commands won't run if you ssh into a node
    Use crictl instead

** Node pool **
A node pool is a group virtual machines, all with the same size
A cluster can have multiple node pools
    These pools can host different sizes of VMs
    Each pool can be autoscaled independently from the other pools
Docker desktop is liminted to 1 node

** Node Quick Lab **
Get nodes information
Get a list of all the installed nodes. Using Docker Desktop, there should be only one.
kubectl get nodes

Get some info about the node.
kubectl describe node

** Pods **
Atomic unit of the smallest unit of work of K8s
Encapsulates an applicaiton's container
Represents a unit of deployment
Pods can run one or multiple containers
Containers within a pod share
    IP address space, mounted volumes
Containers within a pod can communicate via
    Localhost, IPS
Pods are ephemeral
Deploying a pod is an atomic operation, it succeed or not
If a pod fails, it is replaced with a new one with a shiny new IP address
You don't update a pod, you replace it with an updated version
you scale by adding more pods, not more containers in a pod

## Pod Lifecycle ##
** Pod Creation **
** Pod Deletion **
** Pod State **
Pending
    Accepted but not yet creaed
Running
    Bound to a node
Succeeded
    Exited with status 0
Failed
    All containers exit and at least one exited with non-zero status
Unknown
    Communication issues with the pod
CrashLoopBackOff
    Started, crashed, started again, and then crashed again

## Defining and running pods ##
YAML
** kubectl - Pod Cheat Sheet **
kubectl crete -f [pod-definitionl.yml]
#Create a pod

kubectl run [podname] --image=busybox -- /bin/sh -c "sleep 3600"
#Run a pod

kubectl get pods
#List the running pods

kubectl get pods -o wide
#Same but with more info

kubectl describe pod [podname]
#Show pod info

kubectl get pod [podname] -o yaml > file.yaml
#Extract the pod definition in YAML and save it to a file

kubectl exec -it [podname] -- sh
#Interactive mode

kubectl delete -f [pod-definition.yml]
#Deleet a pod

kubectl delete pod [podname]
#Same using the pod's name

** Pods Quick Lab **
Let's first create a node running Nginx by using the imperative way.
Create the pod
kubectl run mynginx --image=nginx

Get a list of running pods
kubectl get pods

Get more info
kubectl get pods -o wide
kubectl describe pod mynginx

Delete the pod
kubectl delete pod mynginx

Create a pod running BusyBox
Let's now create a node running BusyBox, this time attaching bash to our terminal.
(If you don't see a command prompt, try pressing enter.)
kubectl run mybox --image=busybox -it -- /bin/sh

List the folders and use command
ls
echo -n 'A Secret' | base64
exit

Clearnup
kubectl delete pod mybox --wait=false
or
kubectl delete pod mybox --grace-period=0 --force

Create a pod using the declarative way
Let's now create a node using a YAML file

kubectl create -f myapp.yaml

Get some info
kubectl get pods -o widekubectl describe pod myapp-pod

Attach our terminal
kubectl exec -it myapp-pod -- bash

Print the DBCON environment variable that was set in the YAML file.
echo $DBCON

Detatch from the instance
exit
Cleanup
kubectl delete -f myappp.yaml

## Init Containers ##
Always run to completion
Each init container must complete successfully before the enxt one startsIf it fails, the kubelet repeatedly restarts it untl it successdes.
    Unless it's restartPolicy is set to Never
Probes are not supported
    livenessProbe, readinessProbe, or startupProbe

## Selectors ##
** Labels **
key-value pairs used to identify, describe and group related sets of objects or resources
Selectors use labels to filter or select objects

** Selectors Quick Lab **
Deploy the app
kubectl apply -f myapp.yaml

Deploy the service
kubectl apply -f myservice.yaml

Is the service connected to the pod ?
If so, the endpoint will point to the pod IP address. Get the IP address of the pod:
kubectl get po -o wide

Then get the service endpoint. The IP address should match.
kubectl get ep myservice

Port forward to the service
kubectl port-forward service/myervice 8080:80

Open a browser and point to http://localhost:8080
Stop the port forward by typing Ctrl+C

Edit the app YAML file
Change the app label to myapp2 and save the file
Deploy the change
kubectl apply -f myapp.yaml

Check the endpoint again
kubectl get ep myservice

Port forward to the service again
kubectl port-forward service/myervice 8080:80

The screen will display below error coz the label not matched:
Error from server (NotFound): services "myervice" not found

Cleanup
kubectl delete -f myservice.yaml
kubectl delete -f myapp.yaml

## Multi-containers pods ##
** kubectl 0 Pod Cheat Sheet **
kubectl create -f [pod-definition.yml]
#create a pod

kubectl exec -it [podname] -c [containername] --sh
#Exec into a pod

kubectl logs [podname] -c [containername]
#Get the logs for a container

## Networking Concepts ##
All containers wpods can communicate with each other
All pods can communicate with each other
All nodes can communicate with all pods
Pods are given an IP address (ephemeral)
Services are given a persistent IP

** Multi-container POds Quick Lab **
Let's create multiple containers in a Pod using a YAML file. We'll use the Busybox container to get the default page served by the Nginx container.
Create the pod
kubectl create -f two-containers.yaml

Get some info
kubectl get pods -o wide
kubectl describe pod two-containers

Connect to the BusyBox container
kubectl exec -it two-containers --container mybox -- /bin/sh

Fetch the HTML page served by the Nginx container
This will output the content of the Web page in the terminal.
wget -qO- localhost

Quit
exit

Cleanup
kubectl delete -f two-containers.yaml --force --grace-period=0

## Workloads ##
A workload is an application running on Kubernetes
    Pod
        Represents a set of running containers
    ReplicaSet
    Deployment
    StatefulSet
    DeamonSet
        Provide node-local facilities, such as a strage driver or network plugin
    Tasks that run to completion
        Job
        CronJob

## ReplicaSet ##
Primary method of managing pod replicas and their lifecycle to provide self-healing capabilities
Their job is to always ensure the desired number of pods are running
While you can create ReplicaSets, the recommended way is to crete Deployments

** kubectl - ReplicaSets Cheat Sheet **
kubectl apply -f [definition.yaml]
#Create a ReplicaSet

kubectl get rs
#List ReplicaSets

kubectl describe rs [rsName]
#Get info

kubectl delete -f [definition.yaml]
#Delete a ReplicaSet

kubectl delete rs [rsName]
#Same but using the ReplicaSet name

** ReplicaSets Quick Lab **
Let's now use the ReplicaSet template instead of the Pod template
Create the ReplicaSet
kubectl apply -f rs-example.yaml

Get the pods list
kubectl get pods -o wide

Get the ReplicaSet name
kubectl get rs

Describe he ReplicaSet
kubectl describe rs rs-example

Cleanup
kubectl delete -f rs-example.yaml

## Deployments ##
** Pods vs Deployments **
Pods don't
    Self-heal
    Scale
    Updates     
    Rollback
Deployments Can

** Deployments **
A deployment manage a single pod template
You crete a deployment for each microsservice  
    front-end
    back-end
    image-processor
    creditcard-processor
Deployments create Replicaets in the background
Don't interact with the ReplicaSets directly

Deployment: {ReplicaSet: {pod1, pod2, pod3...}}

replicas
    Number of pod instances
revisionHistoryLimit
    Number of previous iterations to keep
strategy
    RollingUpdate
        Cycle through updating pods
    Recreate
        All existing pods are killed before new ones are created

** kubectl - Deployments Cheat Sheet ** 
kubectl create deploy [deploymentName] --image=busybox --replicas=3 --port=80
#The imperative way

kubectl apply -f [definition.yaml]
#Create a deployment

kubectl get deploy
#List deployments

kubectl describe deploy [deploymentName]
#Get info

kubectl get rs
#List replicasets

kubectl delete -f [definition.yaml]
#Delete a deployment

kubectl delete deploy [deploymentName]
#Same but using the deployment name

** Deployments Quick Lab **
Let's now use the Deployment template instead of the Pod template.
Create the deployment
kubectl apply -f deploy-example.yaml

Get the pods list
kubectl get pods -o wide

Describe the pod
kubectl describe pod deployment-example

Get the Deployment info
kubectl get deploy
kubectl describe deploy deployment-example

Get the ReplicaSet name
kubectl get rs

Describe the ReplicaSet
kubectl describe rs

Cleanup
kubectl delete -f deployment-example.yaml

## DaemonSet ##
Ensure all NOdes (or a subset) run an instance of a Pod
Scheduled by the scheduler controller and run by the daemon controller
As nodes are added to the cluster, Pods are added to them.
Typical uses
    Runnig a cluster storage daemon
    Running a logs collection daemon on every node
    Running a node monitoring daemon on every node

** kubectl - DaemonSets Cheat Sheet **
kubectl apply -f [definiiton.yaml]
#Create a DaemonSet

kubectl get ds
#List DaemonSets

kubectl describe ds [rsName]
#Get info

kubectl delete -f [definition.yaml]
#Delete a DaemonSet

kubectl delete ds [rsName]
#Same but using the DaemonSet name

** DaemonSet Quick Lab **
Let's deploy a Busybox as a Daemonet.
Create the Deploymen
kubectl apply -f daemonset.yaml

Get the pods list
There should be one for each worker node.
kubectl get pods --Selector=app=daemonset-example -o wide

Cleanup
kubectl delete -f daemonset.yaml

## StatefulSet ##
** StatefulSet **
For Pods that must persist or maintain state
Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods
Each has a persistent identifier (name-x)
If a pod dies, it is replaced with another one using the identifier
Creates a series of pods in sequence from 0 to X and delees them from X to 0
Typical uses
    Stable, unique network identifiers
    Stable, databases using persistent storage

Caveat:
Containers are stateless by design
StatefulSets offers a solution for stateful scenarios
A better approach could be to use the Cloud provider database services
Deleting a StafulSet will not delete the PVCs
    You have to do this manually

** kubectl - StatefulSets Cheat Sheet **
kubectl apply -f [definition.yaml]
#Create a SatefulSet

kubectl get sts
#List StatefulSets

kubectl describe sts [rsName]
#Get info

kubectl delete -f [definition.yaml]
#Delete a StatefulSet

kubectl delete sts [rsName]
#Same but using the StatefulSet name

** StatefulSet Quick Lab **
Let's now create a StafulSet.
Create the Depoyment
kubectl apply -f statefulset.yaml

Get the pods list
kubectl get pods -o wide

Get a list of the PersistentVolumens Claims
kubectl get pvc

Create a file in nginx-sts-2
Open a session in nginx-sts-2 and create a file in the folder mapped to the volume.
kubectl exec nginx-sts-2 -it -- /bin/sh
cd var/www
echo Hello > hello.txt

Modify the default Web page
cd /usr/share/nginx/html
cat > index.html
Hello
Ctrl-D
exit

Open a session in nginx-sts-0 and reach nginx-sts-2
kubectl exec nginx-sts-0 -it -- /bin/sh
curl http://nginx-sts-2.nginx-headless
exit

Delete pod 2
Delete a pod and watch as it is recreated with the same name.
kubectl delete pod nginx-sts-2

Is the file still there ?
Open a session in nginx-sts-2 and see if the file is still present.
kubectl exec nginx-sts-2 -it -- /bin/sh
ls var/www
exit

Cleanup
kubectl delete -f statefulset.yaml
kubectl delete pvc www-nginx-sts-0
kubectl delete pvc www-nginx-sts-1
kubectl delete pvc www-nginx-sts-2

## Job ##
Workload for short lived tasks
Create one or more Pods and ensures that a speccified number of them successfully terminate
As pods successfully complete, the Job tracks the successful completions.
When a specified number of successful completions is reached, the Job completes
By default, job with more than 1 pod will create them one after the other. To create them at the same time, add parallelism.

** kubectl - Jobs Cheat Sheet **
kubectl create job [jobName] --image=busybox
#The imperative way

kubectl apply -f [definition.yaml]
#create a job

kubectl get job
#List jobs

kubectl describe job [jobName]
#get info

kubectl delete -f [definition.yaml]
#delete a job

kubectl delete job [jobName]
#Same but usint the job name

** Job Quick Lab **
let's now use the job template.
Create the Job
kubectl apply -f job.yaml

Get the jobs list
kubectl get jobs

Get more info
kubectl describe job

Get the pod name
Get the pod's log. Something starting with hello-
kubectl get pods
