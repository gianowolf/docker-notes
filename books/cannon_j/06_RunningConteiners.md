 # Exercise: Running Containers

- START A CONTAINER: docker run -dit redis
- START WITH A NAME: docker run -dit --name=redis_container redis
- STOP A NAMED CONT: docker stop container_name
- AUTO DELETE STOPPED CONTAINER: docker run --rm container_name

## Logs

docker logs provides information directly from the STDOUT and STDERR of a container 

- docker logs CONTAINER_NAME