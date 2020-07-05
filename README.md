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
# Setting up a Lab Environment
# Using Kubernetes Components
# Scaling and Upgrades
# Kubernetes Networking
# Accessing Pods
# Using Volumes
# Setting up Kubernetes for Production
# Exploring the API

