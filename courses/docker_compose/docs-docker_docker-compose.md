# Docker Compose

## Overview 

A tool for defining and runnign multi-container Docker apps

- Uses a YAML file to configure your application's services
- With a single command you create and start all the services from your config
- Works in all environments: prod, test, dev, etc.

1. Define environment with a ```Dockerfile``` 
2. Define services that make up your up in ```docker-compose.yml```
3. Run ```docker compose up``` 

### Example

```yaml
version: "3.9"  #optional
services:
    web:
        build: .
        ports:
            - "8000:5000"
        volumes:
            - ./code
            - logvolume01:/var/log
        links:
            - redis
        redis:
            image: redis   
volumes:
    logvolume01: {}
```

## Create the Compose file

- create a ```docker-compose.yml``` file

Example app to create

```sh
docker run -dp 3000:3000    \
  -w /app -v "$(pwd):/app"  \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root  \
  -e MYSQL_PASSWORD=secret  \
  -e MYSQL_DB=todos \
  -e node:12-alpine
  sh -c "yarn install && yarn run dev"
```

### Services or Containers

#### App Service 

1. First define the service entry and the image for the container.

- We can pick any name for the container.
- Name will automatically become a network alias

2. The command to close the image definition 

3. Migrates ```-p 3000:3000``` by defining ports for the service.

4. Migrate working directory ```-w /app``` and the volume mapping ```-v "$(pwd):/app" by using the working_dir and volumes definitions

5. Finally migrates the environment variable definitions using the
   ```enviroonment``` key

```yml
services:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yurn run dev"      
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
```

#### DB Service

add to docker-file.yaml

```yaml
services:
  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

### Run the App

```sh
docker compose up -d
```


## Compose Specification

Applications are designed as a set of containers which have to both run together with adequate shared resources and communication channels

### Services

Computing components of an application. A service is an abstract concept
implemented on platforms by running the same container image one or more times.

### Networks

Services communicate with each other through Networks. A Network is a platform capability abstraction to establish an IP route between containers within
services connected together.

### Volumes

Services store and share persistent data into Volumes. 

### Configs

Some services require configuration data that is dependent on the runtime or platform. For this the specification defines the Config concept.

### Secret

A Secret is a flavor of configuration data for sensitive data that should not be exposed without security considerations. Secrets are made available to services as files mounted into their containers.

### Project

A pproject is an individual deployment of an application specification on a platform. A project's name is used to gorup resources together and isolate the mfrom other applications or other installation of the same Compose specified application with distinct parameters. 

Project name can be set explicitly by top-level ```name``` attribute. 

## Example

- Compose applicatoin split into frontend web app and backend service
- Frontend
  - HTTP Configuration file managed by infrastructure
  - Providing external domain name
  - HTTPS Server certificate injected 
- Backend
  - Stores data in persistent volume
- Both services communicate with each other on a network
  - External users connects to frontend service via port 443
  - Backend network links frontend webapp and backend database

```yaml
services:
  frontend:
    image: awesome/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier
    configs:
      - httpd-config
    secrets:
      - server-certificate

  backend:
    image: awesome/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

  volumes: # Read/Write
    db-data:
      driver: flocker
      driver_opteX
        size: "10GiB"
  
  # Configs and Secrets are Read Only
  configs:
    httpd-config:
      external: true
    
  secrets:
    server-certificate:
      external: true
  
  networks:
    front-tier: {}
    back-tier: {}
```

## Top Level Element

the ```name``` property is defined as project name to be used if user does not set one expolicitly.

```yaml
services:
  foo:
    image: busybox
    environment:
      - COMPOSE_PROJECT_NAME
    command: echo "I'm running ${COMPOSE_PROJECT_NAME}
