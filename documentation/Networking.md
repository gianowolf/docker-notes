

## Overview

### docker network

The docker network directive manages networks

```sh
docker network COMMAND
```

- **docker network connect**: connect a container to a network
- **docker network create**
- **docker network disconnect**
- **docker network inspect**
- **docker network ls**
- **docker network prune**: remove all unused netowrks
- **docker network rm **

### Network Drivers

Docker's networking subsystem is pluggable

- **bridge**: **default network driver**. Usually used when apps run in standalone containers that need to communicate. BEst when you need multiple containers to communicate on the same Docker host.
- **host**: remove network isolation between the container and the Docker host, and use the host's networking directly. Best option when the network stack should no be isolated from the Docker host.
- **overlay**: Connect multiple Docker daemons together and enable swarm services to communicate with each other. Remove the need to do OS-level routing between these containers. Best when you need containers running on different Docker hosts to communicate, or when multiple apps work together using swarm services.
- **ipvlan**: IPvlan networks give users total control over IP addressing. The VLAN driver builds on top 
- **macvlan**: Allow to assign MAC address to a container, making it appear as a physical device in the network.
- **none**: Disable all networking. Usually used in conjuction with a custom network driver.

## Standalone Containers

### Default bridge network 

In this example you will start two different ```alpine``` containers on the same Docker host and do some tests.

```sh
docker network ls
```

The default bridge network is listed. This tutorial will connect two containers to the brige network.

Start two alpine containers running ash.

```sh
docker run -dit --name alpine1 alpine ash
docker run -dit --name alpine2 alpine ash
docker container ls
```

Inspect the bridge network to see what containers are connected to it.

```sh
docker network inspect bridge
```

Under the Containers key, each connected container is listed, along information about its  IP address.

Use the ```docker attach``` command to connect to alpine1

```sh
docker attach alpine1
ping -c 2 google.com # success
ping -c 2 172.17.0.3 # success
ping -c 2 alpine2 # badd address
```

detach from alpine1 without stopping it Ctrl+p+q

```sh
docker container stop alpine1 alpine2
docker container rm alpine1 alpine2
```

## User defined bridge networks

- provide automatic DNS resolution between containers
  - default bridge containers can only access by IP addresses
  - In user-defined network, containers can resolve each other by alias or name
- better isolation
- each user-defined network creates a configurable bridge

```sh
# create a user-defined bridge
docker network create my-net
# docker network rm my-net

## create a new container 
docker create \
    --name my-nginx \
    --network my-net \
    --publish 8080:80 \
    nginx:latest

## or connect a existing container
docker network connect    my-net my-nginx
docker network disconnect my-net my-nginx
```

## Enabling forwardming from Docker containers to the outside world

By deffault, traffic from containers connected to the default bridge network is not forwared to the outside world. To enable forwarding, you need to change two settings.

```sh
sysctl net.ipv4.conf.all.forwarding=1
sudo iptables -P FORWARD ACCEPT
```

These settings do not persist across a reboot, so you need to add them to a start-up script