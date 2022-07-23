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

```yml
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
