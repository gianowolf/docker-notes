# Networking

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

```sh
docker network create --driver bridge alpine-net

docker network ls

docker run -dit --name alpineX --network alpine-net alpine ash
# do this four times



