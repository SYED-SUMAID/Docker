# Install Docker

## 1. Update Packages and Install Dependencies

    sudo apt update
    sudo apt install ca-certificates curl

Installs the required packages for securely downloading Docker's repository key.

![alt text](<Screenshot (735)(1).png>)

## 2. Add Docker's Official GPG Key

    sudo install -m 0755 -d /etc/apt/keyrings
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    sudo chmod a+r /etc/apt/keyrings/docker.asc
    sudo tee /etc/apt/sources.list.d/docker.sources <<EOF

Adds Docker's official GPG key so APT can verify packages from the Docker repository.

![alt text](<Screenshot (734)(1).png>)

## 3. Add Docker Repository

    Types: deb
    URIs: https://download.docker.com/linux/ubuntu
    Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
    Components: stable
    Architectures: $(dpkg --print-architecture)
    Signed-By: /etc/apt/keyrings/docker.asc
    EOF

Adds Docker's official repository to the system's APT sources.


## 4. Install Docker Engine

    sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Installs Docker Engine, Docker CLI, containerd, Buildx, and Docker Compose.

![alt text](<Screenshot (737).png>)

## 5. Check Docker Service

    sudo systemctl status docker

Verifies that the Docker service is running.

![alt text](<Screenshot (737)-1.png>)