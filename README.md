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

> docker save
> docker load

> docker commit

> docker pause
> docker unpause

> docker stop
> docker start

> docker rm 


## Understanding Docker Networking 


+ Default networking uses Linux bridges
  + Different containers on the same host can communicate;
  + communications with containers on other hosts is not possible
  + As the bridge uns NAT, containers are not directly accessible from the outside
+ Overlay networking: using overlay networking technologies allows containers to communicate if they're running on different hosts
+ Other technologies do exist, but are irrelevant if you're going to use Kubernetes.

### Exposing Container Ports

+ By default, container ports are not accessible from the outside
+ Use port mapping to make them accessible:
  + docker run -d --name httpd -p 8080:80 httpd
  + curl http://localhost:8080
+ Alternatively, you can specifiy an IP address for the port forward
  + docker run -d --name httpd -p 192.168.1.150:8090:80 httpd


> bridge

## Understanding Docker Storage

### Default Storage

+ Containers are based on different filesystem layers
+ You can modify contents of a running container, but storage by default is ephemeral, it's gone after a restart
+ All modifications in running containers are written to a read/writable filesystem layer that by default is removed when the container is stoppped
+ Notice that this R/W filesystem layer should not be used for persistent storage, as it doesn't perform well 
+ Also notice that Docker keeps the ephemeral storage around for some time after the container is stopped. making it easier to troubleshoot based on information in logs

### Host Directory Storage

+ Persistent storage can be offered by using a host directory
+ The container will bind-mount to this host directory to wirte and access persistent data
  + Notice that in RHEL, this host directory should have the svirt_sandbox_file_t context label
+ On the host directory, use chonw to set the appropriate UID and GID as owner
 + Notice that this refers to the UID and GID of a user in the container, which may not exists on the host, for instance, on a mariadb container, the user that mariadb is running are UID 27 and GID 27,Apply these using chown -R 27:27 /var/local/mysql

 > mkdir -p /var/local/mysql
 > setenforce 0
 > chown -R 27:27 /var/local/mysql
 > docker pull mariadb
 > docker run --name mariadb -d -v /var/local/mysql:/var/lib/mysql -e MYSQL_USER=user -e MYSQL_PASSWORD=password -e MYSQL_DATABASE=address -e MYSQL_ROOT_PASSWORD=password mariadb
      
# Setting up a Lab Environment

## Installing kubectl

### Understanding Kubectl

+ kubectl is the generic Kubernetes management command
+ It use the ~/.kube/config file to find information on what to connect to
+ It is available for all platforms to allow you to manage Kubernetes

### Installing kubectl

+ kubectl is the management command that needs to be available on the management workstation
+ You'll use it to interface the kubernetes cluster. no matter where it will be running
+ There are different ways to install it 
  + From a cloud client
  + Run from a cloud shell
  + Compile from source
  + Run from the release binaries

### Installing on Linux from the Release Binaries

There are different ways to install it 
  + From a cloud client
  + Run from a cloud shell
  + Compile from source
  + Run from the release binaries

#### Install on Linux

> curl -LO https://storage.googleapis.com/kubenertes-releases/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/amd64/kubectl
> chmod +x kubectl
> mv kubectl /usr/local/bin
> kubectl version
> kubectl get nodes(only if a working cluster can be found)

#### Install on MacOS

> install homebrew: /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
> brew install kubectl
> kubectl version to verify
> kubectl get nodes[-insecure-skip-tls-verify] (only if a working cluster can be found)

> firewall-cmd list-all
> firewall-cmd --permanent --zone=FedoraWorkstation --add-masquerade
> firewall-cmd --permanent --zone=FedoraWorkstation --add-forward-port=port=8443:proto=tcp:toport=8443:toaddress=192.168.90.100
> kubectl get nodes --insecure-tls-verify




## Installing minikube


### Understanding Minikube

+ Minikube is a single-VM demo environment that runs on top of VirtualBox
+ The minikube start command will run a VirtualBox VM with a single-node Kubernetes deployment as well as a Docker engine
> It's not for production, but it's excellent for learning Kubernetes!

### Minikube Requirements

+ Physical machine
+ 2 GB of RAM(4GB recommended)
+ Hardware assisted virtualization
+ No other virtualization stack

### Installing minikube

> installing VirtualBox: sudo yum install virtualbox
> Look up the current Minikube release, and use that in the command below
> curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.25/minikube-linux-amd64
> chmod +x minikube
+ sudo mv minikube /usr/local/bin
> minikube start
> lubectl cluster-info
> kubectl get nodes

> grep vmx /proc/cpuinfo

## Working with Minikube

> minikube --help
> minikube start
> minikube status
> minikube ssh
> top
> ps aux 
> ps aux  | grep localkube
> minikube ip
> minikube dashboard
> minikube logs


## Working with kubectl

> kubectl --help
> kubectl run --help
> kubectl run nginx --image=nginx
> kubectl get --help
> kubectl get endpoints
> kubectl get po
> kubectl get all
> kubectl config view

### Conecting to a Cluster

+ Kubernetes uses a config file in ~/.kube to specify details about the cluster you want to connect to
+ When setting up a cluster, this config is automatically created. you may modify it manually
+ Use kubectl config view to show current configuration


### Understanding the config File

+ Config file is used to define 3 different elements
  + Cluster - the kubernetes cluster
  + user - the authorized user
  + context the part of the cluster the user wants to access. typically a namespace
+ Multiple clusters, users and contexts can be referred to from the config file


### Connecting to a Remote Cluster

+ When setting up a cluster, the config file is created automatically
+ To connect to a remote cluster, you'll need to set up appropriate credentials, consisting of a client certificate and a client key
+ Use kubectl config set-cluster mycluster --server=http://ip:port --api=version=v1 to connect to the cluster
> next use kubectl use-context mycluster to  start using it.
+ When working with mulitple configuration files, set the KUBECONFIG variable with as the argument a list of config files where each file is separated by a :


> kubectl config access to multiple clusters

> google kubernetes configure access to multiple clusters



## Importing images to Kubernetes

### Workflow Overview

+ Use Docker images to created pods
+ Kubernetes uses Pods as the smallest managed items
+ Easily create basic Pods using the dashboard
+ Or use YAML files for increased scalability


### Importing Images into kubernetes

+ To start using Kubernetes, you'll need to pull container images into the pod
+ To use custom images, create your own Docker images and push them to the registry
+ Alternatively, pull images from public containers
  + Using Docker hub or any private container registry
  + Using Docker Hub images makes sense in a small private deployment
+ Use the Kubernetes Dashboard for an easy way to get started
  + Start through minikube dashboard
  + Or by address port 30000 on the Kubernetes Master node


## Working with YAML Files

### Creating Pods with YAML Files

+ Use kubectl with  a YAML file to make creating pods easy
+ kubectl create -f <name>.yaml --validate=false
+ kubectl get pods will show the new pod
+ kubectl describe pods show all details about pods, including the information about its containers
  + kubectl describe pods busybox
  + kubectl describe pods busybox -o yaml
  + kubectl describe pods busybox -o json


> kubectl describe pods busybox -o yaml


## Running an application from the Kubernetes Dashboard
## Lab: Running a Kubernetes Application

# Using Kubernetes Components

## Understanding Kubernetes Resource Types


## Understanding the Pod

What is a Pod?

+ A group of one or more containers, shared storage and networking, and a specification of how to run these containers
+ All containers in the pod are always co-located and co-scheduled
+ Everything within a pod runs within a shared context
+ You can see a pod as an application or logical host
+ Pod are considered  to be ephemeral, if a pod is stopped ,it will go away
+ If the node running a pod crashes, the pod will be shceduled to go somewhere else

Why Pods?

+ Pods implement the application you want to offer to end users
+ Everything within a pod needs to work together,so you don't want to work at a container level
+ Containers in the pod have the following requirements which make sense running them in the same pod
  + deployment: it all belongs together
  + co-location
  + shared fate, as containers depend on one another
  + Resource sharing
  + dependency management


Pod networking

+ Applications in a pod all use the same network namespaces
  + One IP address for the pod
  + One range of ports that needs to be shared by all containers in the pod
+ Therefore, containers in a pod must coordinate their usage of ports
+ Each pod has one IP address
+ The host name to that ip address is set to the pods name

## Starting Pods


Starting Pods

+ individual Pods can be started using kubectl create -f <name>.yaml
+ Alternatively, use the dashboard running on port 30000
+ After starting a pod, use kubectl get pods to get an overview of all running pods
+ kubectl describe pods shows all details about a pod
+ use kubectl delete pod podname to delete it


## Understading Namespaces

Understanding Namespaces

+ A namespace is a strict isolation that occurs on Linux Kernel level
+ Every kubelctl request uses namespaces to ensure that resources are strictly separated and names don't have to bbe unique
+ Namespaces can be added when creating a pod, thus ensuring that a pod is availiable in a specific namespace only
+ Before adding a pod to a namespace, ensre that the namespace exists
+ Use namespaces in cluster environments with many users that are spread across multiple teams and  projects


Default Namespaces

+ By default, Kubernetes has three namespaces
  + default
  + kube-public
  + kube-system
    
## Working with Namespaces

> kubectl get ns
> kubectl create ns secrect
> kubectl get ns secret
> kubectl get ns secret -o yaml
> kubectl describe ns secret



## Working with Replica Sets

Understanding Replica Sets

+ The pod is the moste basic entity in Kubernetes
+ Replica Sets come next and are used to determine the number of instances that are needed of a pod 
+ Replica Sets replace replication controllers that were used in previous versions of Kubernetes 
  + Replication controller-based commands like kubectl get rc won't work
+ Replica Sets can be created directly, but shouldn't ;use deployments instead
+ Applications that are lanuched throught a deployment automatically create replica sets

> kubectl run nginx --image=nginx
> kubectl get deployments
> kubectl get rs
> kubectl get rs nginx-76df748b9 -o yaml
> kubectl scale --help
> kubectl scale rs nginx-76df748b9 --replicas=3
> kubectl scale --replicas=3 deployment nginx




## Understading Deployments

Understanding Deployments

+ Deployments are used to create and automate replca sets
+ Deployments instruct the cluster how to create and scale aplications
+ Deployments controller will monitor instances of an application
+ Deployments allow for the creation of multiple replica sets for rolling updates or rollbacks

> kubectl get all
> kubectl get deployments
> kubectl scale --replicas=20 deployment nginx
> kubectl scale --replicas=10 deployment nginx
> kubectl delete rs nginx-76df748b9



## Lab: Running a Deployment

> kubectl run hazelcast --image=hazelcast --port=5701
> kubectl logs hazelcast-5c656cb674-72f9r
> kubectl describe deployments.apps hazelcast
> kubectl describe pod hazelcast-5c656cb674-72f9r
> kubectl logs hazelcast-5c656cb674-72f9r







# Scaling and Upgrades
# Kubernetes Networking
# Accessing Pods
# Using Volumes
# Setting up Kubernetes for Production
# Exploring the API

