# Run Ubuntu Interactive Container

## 1. Pull Ubuntu Image

    sudo docker pull ubuntu

Downloads the Ubuntu image from Docker Hub.

![alt text](<Screenshot 2026-09-14 194922.png>)

## 2. Run Ubuntu Interactive Container

    sudo docker run -it ubuntu /bin/bash

Creates and starts an Ubuntu container with an interactive Bash terminal.

- `-i` → Keeps standard input open
- `-t` → Allocates a terminal
- `ubuntu` → Ubuntu image
- `/bin/bash` → Starts the Bash shell

![alt text](<Screenshot 2026-09-14 195210.png>)

## 3. Check Current User

    whoami

Shows the current user inside the container.

![alt text](<Screenshot 2026-09-14 195001.png>)

## 4. Check Files

    ls

Lists the files and directories inside the Ubuntu container.

![alt text](<Screenshot 2026-09-14 195008.png>)

## 5. Check Ubuntu Version

    cat /etc/os-release

Displays information about the Ubuntu version running inside the container.

![alt text](<Screenshot 2026-09-14 195032.png>)

## 6. Exit the Container

    exit

Exits the interactive container.

![alt text](<Screenshot 2026-09-14 195044(1).png>)