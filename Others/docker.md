# Docker

Build and Ship any Application Anywhere

## What is Docker

Docker is a _containerization_ platform that packages your application and all its dependencies in the form of docker container.

## Why is Docker Needed

- To ensure that your application works seamlessly in any environment.
- No need to set up new environment on every server.
- Make sure the environmnent is consistent among servers.

## Docker Objects

### Image

An _Image_ is a read-only template with instructions for creating a Docker container.

- build image

```text
docker build [OPTIONS] PATH | URL | -
```

- list all images

```text
docker images
```

example:

```text
docker build -t <image-name> .
```

1. `-t` gives name to image
1. `.` path where to look for the `Dockerfile`

### Container

A container is a runnable instance of image.

- run container

```text
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

- list all containers

```text
docker ps
```

Example:

```text
docker run -dp 3001:3000 <image-name-or-id>
```

1. 3001: host's port
2. 3000: contaner's port

- Remove container

```text
docker stop <container-id>
docker rm <container-id>

or

docker rm -f <container-id>
```

> If source code is updated, just re-build the image , remove and re-run container.

## Persist the DB

The changes made in the container are lost when the container is removed. There are two ways to persist the data, `named volume` and `bind mounts`, which connect specific filesystem paths of the container back to the host machine.

### Named Volume

- Stored in host filesystem which is **managed by `Docker`** (`/var/lib/docker/volumes/` on Linux).

- Create voulme:

```text
docker volume create <volume-name>
```

- Inspect volume:

```text
docker volume inspect <volume-name>
```

- Start container with a volume

```text
docker run -d -v <volume-name>:<path-in-the-container> <image-name>
```

- Use volume with docker compose

```text
version: "3.9"
services:
  frontend:
    image: node:lts
    volumes:
      - myapp:/home/node/app
volumes:
  myapp:
```

### Bind Mounts

- Stored **anywhere** on the host machine.
- Often used to provide additional information data to containers.
- When working on an application, we can use a bind mount to mount our source cdoe in the container, to let it see code changes and respons right away.

- Start container with bind mount

```text
docker run -d -v <path-in-host-machine>:<path-in-the-container> <image-name>
```
