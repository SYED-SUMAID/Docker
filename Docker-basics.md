# Docker Basics

## What is Docker?

Docker is a platform used to build, run, and manage applications inside isolated environments called **containers**.

### Why Docker?

- Same environment on different machines
- Lightweight compared to virtual machines
- Easy application deployment
- Fast startup
- Isolated applications

## Core Concepts

### Image

An **image** is a ready-made template used to create containers.

Examples:

    ubuntu
    nginx
    postgres

### Container

A **container** is a running instance of an image.

    Image → Container

### Dockerfile

A **Dockerfile** contains instructions for creating a Docker image.

    Dockerfile → Image → Container

## Basic Commands

Check Docker:

    docker --version

Download an image:

    docker pull ubuntu

List images:

    docker images

Run a container:

    docker run ubuntu

Run an interactive Ubuntu container:

    docker run -it ubuntu bash

Here:

- `-i` = interactive
- `-t` = terminal
- `bash` = starts the Bash shell inside the container

List running containers:

    docker ps

List all containers:

    docker ps -a

Start a stopped container:

    docker start CONTAINER_ID

Stop a container:

    docker stop CONTAINER_ID

Restart a container:

    docker restart CONTAINER_ID

Remove a container:

    docker rm CONTAINER_ID

Remove an image:

    docker rmi IMAGE_ID

View container logs:

    docker logs CONTAINER_ID

Enter a running container:

    docker exec -it CONTAINER_ID bash

## Useful Docker Run Options

Run in background:

    docker run -d nginx

Give a container a name:

    docker run --name mycontainer nginx

Automatically remove container after it stops:

    docker run --rm ubuntu

Map a port:

    docker run -d -p 8080:80 nginx

`8080` = host port

`80` = container port

You can then access Nginx using:

    http://localhost:8080

## Dockerfile Example

Create a file named `Dockerfile`:

    FROM ubuntu:24.04

    RUN apt update && apt install -y curl

    WORKDIR /app

    CMD ["bash"]

Build the image:

    docker build -t myubuntu .

Run the image:

    docker run -it myubuntu

## Volumes

Volumes are used to persist data outside the container.

Create a volume:

    docker volume create myvolume

List volumes:

    docker volume ls

Use a volume:

    docker run -it -v myvolume:/data ubuntu bash

Anything stored in `/data` is stored in the Docker volume.

## Docker Compose

Docker Compose is used to run multiple containers together.

Example `compose.yaml`:

    services:
      web:
        image: nginx
        ports:
          - "8080:80"

      database:
        image: postgres
        environment:
          POSTGRES_PASSWORD: password

Start services:

    docker compose up -d

Stop services:

    docker compose down

## Docker Workflow

    Dockerfile
         ↓
    docker build
         ↓
       Image
         ↓
     docker run
         ↓
     Container

## Simple Mental Model

**Dockerfile** = Instructions

**Image** = Template

**Container** = Running instance

    Dockerfile → Image → Container