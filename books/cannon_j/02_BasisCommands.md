# Docker Basics and Common Commands

## Running a Container

When you run a container, the image that you are using will be downloaded, if it has not been already. If the image already exists, then there is nothing to download. if the image is not on oyur host system, Docker will download it for you.

```sh
docker run -dit debian
```

After Docker downloads the image, it starts a container using that image. The last line of the output is the container ID.

You can use the container ID to manage this particular container. You can use also a unique human-friendly name or a unique portion of that ID.

#### Options

- d: run container in the background
- t Allocates paseudo TTY or a pseudo terminal.
- i option stands for interactive. This allows you to type commands in the container.

### Show Runing Containers

```sh
docker ps
```

```sh
docker stop 'id'
docker run nameimage
docker ps
```

```sh
# Display Images
docker images

# Inspecting Resources
docker inspect %%first thtree hash chars of CID%%
# Shows networking details, troubleshooting, settings, kernel settings, etc.

# Assistance
docker --help
doker image --help
odcker image prune --help
```
