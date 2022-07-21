# Making a Container Publicly Available

- How to make an app that is running inside a container accesible from outside the Docker host machine.
- How to share data with containers running on your machine

Start up a container and open a specific netowrk port so that you can view the container web server in a web browser on your local machine.
Pick an arbitrary TCP port number on your host machine to expose our container’s network port on.

```sh
docker run  - -name our_nginx -d -p 8080:80 nginx
```

- ```-p 8080:80``` means open up TCP port 8080 on the docker host machine and redirect traffict to and from it, to TCP port 80 inside the container.
- ``` outside:inside```
- check with docker ps
- output in PORTS column
 0.0.0.0:8080 -> 80/tcp
:::8080 -> 80/tcp 

- La direccion IP 0.0.0.0 es conocida como direccion no especifica. El nombre tecnico original es “source asddress for this host on this network” *direccion de origen para este host en esta red*

- La IP 0.0.0.0 significa que todas las direcciones IPv4 se encuentran en el equipo local. 

8. curl http://localhost:8080

- If docker hostm achine is remote, and you want to connect it from another system over the network, you will need the ip address of your docker host machine

- If you are accessing the docker host via SSH from your laptop you will need to determinate the IP address of the docker host system.
on linux systems run ```ip a```

you can get the logs running ```docker logs container_name```

## making webpages

```sh
mkdir webpages
cd webpages
echo ‘hi from inside the container! > index.html
cd ..
```

```
docker run -p 8080:80 –name another_nginx -v ${PWD}/webpages:/usr/share/nginx/html:ro -d nginx
```

- -v option creates a volume for sharing files. 
- -v followd by the path on the host machine followed by a : and where you want that data to be accessed in the container.
after second semicolon can add options as ```ro``` wich mounts the volume in read only mode inside the container. This means that files can be changed from the docker host machine, but the container cannot alter the files. 

# Exercise: Making a container publicly available

```sh
# Start a container using Apache HTTP Server Image 
# Image for apache http server is httpd
docker run –name apache_welcome -d -p 9900:80 httpd:latest 
docker ps
curl http://localhosts:9900
```



