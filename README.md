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

