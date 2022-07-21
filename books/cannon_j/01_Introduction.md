# Introduccion

## Containers

- Might be described as creating a small software package that can ru na particular app and its associated processes

Container becomes portable and can run on all types of computers in the same way without compataibility issues and dependencies. This is why Docker refers to a container as a __Standarized unit of Software__.

### Benefits 

- Not disruptive to a system 
- Easy clean up afterwards
- No conflicting versions of software or libraries
- Lightweight: Optimized for a medium-powered laptop.
- Can run multiple containers on one server
- Orchestrators can help organize and run multiple containers, as Kubernetes or Openshift

### VM vs Containers

 VMs might have different apps present on a single VM. There might be a web server and a database along their backup scripts.

 The container is using the underlying host computer OS, and not starting its own whenever it launches

 ## Keys 

 - Isolate containers from one another
 - control groups wich provide resource kernel quotas. Limit onecontainre from using up all the resources, such as the CPU or disk reads and wirtes on the host machine.

### Dockerfile Basic Sample

In order to create a Docker image, a Dockerfile is required. This is a simple script that determines how an image is built frmo scratch.

One you use docker to build an image from a Dockerfile, you can run as many identical containers from it as you wish.

## Base Images and Layering

Base images are important part of Docker and involve an image which you build your own image upon. Base images usually come with a tiny but functional OS.

Docker uses a layered filesystem that allows to add a base image without making your image much larger. 

Whenever you want to create a container image, will be used. Docker will kown wisch base image to use when you specify a FROM line in the Dockerfile.

## Microservices

Docker containers are better suited to a microservice architectura than a traditional monolithic architectura because a container can run autonomously without relying on the other centrlized processes.

```dockerfile
FROM debian:stable
LABEL author=LinuxTrainingAcademy
LABEL Version=dockerbook

RUN apt update &&
    apt install -y nano &&
    apt clean

CMD ["/bin/bash"]