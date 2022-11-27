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