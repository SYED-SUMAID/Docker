# Docker Basics

## 1. List Docker Images

    sudo docker image ls

Lists all Docker images available locally.

Screenshot: Add image list output here.

## 2. List Running Containers

    sudo docker container ls

Shows only running containers.

Screenshot: Add running containers output here.

## 3. List All Containers

    sudo docker container ls -a

Shows both running and stopped containers.

Screenshot: Add all containers output here.

## 4. Run Ubuntu Interactively

    sudo docker run -it ubuntu /bin/bash

Creates and starts an Ubuntu container with an interactive Bash terminal.

- `-i` → Interactive input
- `-t` → Allocate a terminal
- `ubuntu` → Docker image
- `/bin/bash` → Bash shell

Screenshot: Add Ubuntu container terminal here.

## 5. Create a Named Container

    sudo docker run -it --name my_ubuntu ubuntu

Creates an Ubuntu container with the name `my_ubuntu`.

Screenshot: Add container list showing `my_ubuntu` here.

## 6. Enter a Running Container

    sudo docker exec -it my_ubuntu /bin/bash

Opens a Bash shell inside an already-running container.

Screenshot: Add container terminal here.

## 7. Stop a Container

    sudo docker stop my_ubuntu

Stops a running container.

You can use either the container name or ID:

    sudo docker stop <container_name>
    sudo docker stop <container_id>

## 8. Run a Container in Detached Mode

    sudo docker run -d -t --name my_ubuntu ubuntu

Runs the Ubuntu container in the background.

- `-d` → Detached/background mode
- `-t` → Allocate a terminal
- `--name` → Set container name

Check running containers:

    sudo docker container ls

Screenshot: Add running container output here.

## Quick Reference

| Command | Purpose |
|---|---|
| `docker image ls` | List images |
| `docker container ls` | List running containers |
| `docker container ls -a` | List all containers |
| `docker run` | Create and start a container |
| `docker exec` | Execute a command inside a running container |
| `docker stop` | Stop a container |