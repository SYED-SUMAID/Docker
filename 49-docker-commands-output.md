# Docker Commands - Output

## 1. Docker Version

    sudo docker --version

Displays the installed Docker version.

![alt text](<Screenshot 2026-09-14 175246.png>)

## 2. Docker Information

    sudo docker info

Displays information about the Docker installation and Docker Engine.

![alt text](<Screenshot (753).png>)

## 3. List Docker Images

    sudo docker image ls

Lists Docker images available locally.

![alt text](<Screenshot 2026-09-14 175731.png>)

## 4. List Running Containers

    sudo docker container ls

Lists currently running containers.

![alt text](<Screenshot 2026-09-14 175746.png>)

## 5. List All Containers

    sudo docker container ls -a

Lists all containers, including stopped containers.

![alt text](<Screenshot 2026-09-14 175805.png>)

## 6. Pull Ubuntu Image

    sudo docker pull ubuntu

Downloads the Ubuntu image from Docker Hub.

![alt text](<Screenshot 2026-09-14 175831.png>)

## 7. Run Ubuntu Container

    sudo docker run ubuntu


## 8. Check Containers

    sudo docker container ls -a

Verifies the Ubuntu container and its status.

![alt text](<Screenshot 2026-09-14 175920(1).png>)

## 9. Stop a Container

    sudo docker stop <container_name_or_id>

Stops a running container.


## 10. Remove a Container

    sudo docker rm <container_name_or_id>

Removes a stopped container.

![alt text](<Screenshot 2026-09-14 180011-1.png>)
![alt text](<Screenshot 2026-09-14 180046.png>)

## Quick Reference

| Command | Purpose |
|---|---|
| `docker --version` | Check Docker version |
| `docker info` | View Docker information |
| `docker image ls` | List images |
| `docker container ls` | List running containers |
| `docker container ls -a` | List all containers |
| `docker pull` | Download an image |
| `docker run` | Create and start a container |
| `docker stop` | Stop a container |
| `docker rm` | Remove a container |