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

example:

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

> If source code is updated, just re-build the image , remove container and re-run container.
