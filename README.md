# Understanding Container Orchestration
## What are Containers

> kubectl run fedora --image=fedora
> kubectl get deployment
> kubectl delete deployment  fedora
> kubectl run fedora --image=fedora -- /usr/bin/sleep 3600


## Leading Container Solutions


+ Docker has become the de facto standard for containers
+ Docker was started in 2013
+ Docker is not the only solution though
  + LXC has existed for many years
  + Linux systemd has systemd-nspawn, which also is a solution to run containers
+ Docker comes in a community edition and a proprietary edition
+ The proprietary edition adds some enterprise-level features



## What is Kubernetes

+ Kubernetes also referred to as k8s, is Greek and means helmsman or pliot
  + so it's the captain of the ship with all the containers
+ Kubernetes is an extensible open-source platform for managing containerized workloads and services
+ It comes from Google Borg, a system that Google has beening using since 2002 to run its applications
+ Amongst the first big users of Kubernetes was Pokeman Go in 2016
+ It's open-source software for automating deployment, scaling, and Orchestration of containerized applications
  + instead of running containers with the docker command, you'll use the Kubernetes command to run them
+ It's about running multie connected containers, organized in Pods or deployment, running on different host, where continuity of service is guaranteed
+ It has become the de facto standard for archestration of containers
+ With a new release every 3 months, it is rapidly evolving.

## Alternatives to Kubernetes

+ Docker Swarm
+ Apache Mesos
+ Azure Container Service
  + Run Kubernetes or Docker Swarm in Azure
+ Amazon ECS
  + Kubernetes
+ Google Container Engine
  + Allows running Kubernetes in Google Cloud
+ Redhat OpenShift
  + Integrated Kubernetes and adds services on top of it


## Understanding Kubernetes Architectures


+ master
  + api server
  + etcd
  + scheduler
  + controller management
+ worker
  + container engine
  + kubelet
  + server porxy

## Kubernetes and the cloud Native Computing Fundation

+ Google donated Kubernetes to the Cloud Native Computing Foundation(CNCF), which is a foundation in Linux foundation
+ CNCF is a govering body that solves issues faced by any cloud native application(so not just Kubernetes)
+ CNCF owns the copyright of Kubernetes
+ Kubernetes itself uses an Apache license
+ Developers need to sign a Contributor License Agreement with CNCF

# Working with Containers
## Containers Solutions Overview

+ A container is a solution that bundles an application and all of its dependencies in a box
  + Kernel is not included
+ This box can run on virtually any platform, as long as that platform runs a container engine
+ Containers are based on an image, from the image, you can spin multiple containers.
  + When the container is created, it runs a process on the hots kernel.The host kernel isolates resource belonging to each container 
  + For isolation, namespaces are used.
  + To dedicate resources, cgroups are used.
+ Container images consist of different layers
+ These layers are joined in the union filesystem
+ By overlaying these layers, a new virtual filesystem is created.
+ the layers are joined in images, and by default are read-only
+ When a ontainer is started, ephemeral read-write layer is added.
+ The original containers are defined in LXC, which is a part of the Linux operation system.
+ Docker is currently the leading container solution.
  + Enterprise-level services are provided by Docker,inc
+ Rkt(rock-it) is another container solution

### Why containers are Successful

+ Very fastdeployment
+ Flexible, as they run anywhere.
+ Easy to scale up and down
+ Very resource-efficient
+ Little footprint
+ Portable


## Getting started with Docker

### Install Docker(Fecora)

+ Remove any distro-specific docker: dnf remove -y docker docker-common container-selinux docker-selinux docker-engine
+ Get the Docker-ce repository: curl -o /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/fedora/docker-ce.repo
+ Install the software: dnf install docker-ce 
+ Start and enable the Docker Engine: systemctl start docker
+ systemctl enable docker


> dnf repolist
> rpm -qa | grep docker
> systemctl status docker

### verify ths installation

+ Verify that it is running: docker info
+ Search for containers: docker search fedoar
+ Start the container: docker run fedora echo Hello World
+ Verify operations: docker ps; docker ps --all
+ Verify image and container: docker image list; docker container ls
+ Start a shell o a container: docker run --it fedora /bin/bash
+ to detach from a running container: Ctrl-P; Ctrl+Q
+ to Run Docker commands as non-root user: usermode -aG docker <user>
> docker kill container_name

## Understanding container Architecture

### Docker Engine

+ Docker engine runs containers and consists of the following parts:
  + A server implemented by the dockerd service
  + A REST API which specifies interface that can be used by client commands
  + the Docker command

### Docker Registry

+ Docker registries are used to store Docker images
+ Docker Hub ispublicly available and used as the default
+ Docker cloud is also publicly avaiable
+ administrators can create private registries
+ When using docker pull or docker run an image is pulled from the registry
+ Yuo can upload images using docker push

### Understanding Docker Objects


+ image: a read-only template with instructions to create a Docker container
  + iamges are often based on other images
  + Default images are pulled from Docker registry, you can build your own images using Dockerfile
    + Each instruction in Dockerfile creates a layer in the image
    + When rebuilding an image, only those layers that changed are rebuilt
+ Container: a runnable instance of an image
  + Containers need persistent storage to keep modifications
+ Services: used in Docker swarm to scale containers across multiple Docker daemons


  

## Operational Container Management

### Working with Containers

+ Show Docker version information: docker version
+ Show current usage information: docker info
+ Search containers:docker search wordpress
+ Download an image: docker pull wordpress
+ Verify: docker images
+ Remove images: docker rmi wordpress
+ Run a container in interactive mode: docker run -dit --name centos -hostname="centos" centos /bin/bash
+ Verify running container: docker ps -a
+ Connect to a container: docker attach centos
+ Run a command in a container: docker run centos /usr/bin/free -m 
  + After running the command, the container stops, but is still avaiable on the system
+ Run a command in a container and remvoe the container after doing so: docker run -rm centos /us/bin/df -h
+ Show information about a running container: docker top centos
+ Show top-like information about a container : docker stat centos
+ Copy someting from the container to localhost: docker cp centos:/etc/hosts/ /root
+ Run a command inside a container: docker exec centos cat /etc/hostsname
+ Kill a container: docker kill centos




## When Does a Container Stop

+ Typically, when a container is started, a specific command is executed. When this command stops, the container stops as well
+ docker run --name demo1 centos:latest dd if=/dev/zero of=/dev/null
  + This will start the container, and use the terminal to run the dd command
  + use docker ps to see the active container
  + user ps aux, yuu will notice the dd process shows in the current process tree
  + use docker exec -ti demo1 /bin/bash and type ps aux, you will see the dd processis running as pid 1
+ type docker stop demo-container
+ now type docker run --name demo 2 centos:latest sleep 30
+ From another terminal, type doce ps
+ Observe that the container will stop after executing its task
+ Type docker ps  -a, this will show a list of all containers that have been activated in the past
+ Type docker rm <containername> to remove these containers
  + Use docker rm $(docker ps -aq) to delete all containers


> docker inspect
> docker inspect -f '{{ .NetworkSettings.IPAddress }}'  docker_name

## Docker Command overview
## Understanding Docker Networking 
## Understanding Docker Storage

# Setting up a Lab Environment
# Using Kubernetes Components
# Scaling and Upgrades
# Kubernetes Networking
# Accessing Pods
# Using Volumes
# Setting up Kubernetes for Production
# Exploring the API

