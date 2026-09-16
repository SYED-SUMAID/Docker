# 🚀 Run Verventech Website Container with Port Forwarding

## 🎯 Objective

In this lab, we will take a Docker image available on Docker Hub, download it to our system, and use it to create a running container.

We will also configure port forwarding so that the website running inside the container can be accessed from our browser through the host machine.

### What we will do

- Pull the `verventech-website` image from Docker Hub
- Create a container from the image
- Forward a host port to the container
- Verify that the container is running
- Access the website from a browser
- Check the container logs and port mapping

---

## 1️⃣ Pull the Docker Image

Before creating a container, we need the image on our system. Since the image is stored on Docker Hub, we can download it using `docker pull`.

    docker pull verventech-website

The image contains everything Docker needs to create the container, such as the application files, web server, and required configuration.

- `docker pull` → Downloads an image from a container registry
- `verventech-website` → The Docker image we want to download

![alt text](<Screenshot (763)(1).png>)

---

## 2️⃣ Run the Container

Once the image is available locally, we can create a container from it.

    docker run -d -p 8080:80 --name verventech-website-container verventech-website

Here, Docker creates a new container from the `verventech-website` image and starts it immediately.

The important part of this command is the port mapping:

    -p 8080:80

This connects port `8080` on the host machine to port `80` inside the container.

The web server inside the container listens on port `80`, while we will access it through port `8080` on our machine.

- `verventech-website` → Docker image
- `--name verventech-website-container` → Gives the container a name
- `-d` → Runs the container in the background
- `-p 8080:80` → Maps host port `8080` to container port `80`

![alt text](<Screenshot 2026-09-16 152854.png>)

---

## 3️⃣ 🔍 Verify the Container

Now we need to make sure the container is actually running.

    docker ps

The output should show the container with a port mapping similar to:

    0.0.0.0:8080->80/tcp

This tells us that Docker has successfully connected port `8080` on the host to port `80` inside the container.

![alt text](<Screenshot 2026-09-16 152911(2)-1.png>)

---

## 4️⃣ 🌐 Access the Website

The container is running and the port has been forwarded, so we can now access the website from a browser.

Open:

    http://localhost:8080

When we visit this address, the request goes to port `8080` on the host. Docker forwards that request to port `80` inside the container, where the web server serves the website.

The flow is:

    Browser → localhost:8080 → Docker → Container:80 → Website

![alt text](<Screenshot (764)(1).png>)

---

## 5️⃣ 📋 Check Container Logs

Docker containers generate logs that can help us understand what the application or web server is doing.

    docker logs verventech-website-container

This is especially useful when troubleshooting. If the website does not load or the application produces an error, the container logs are one of the first places to check.

![alt text](<Screenshot 2026-09-16 153237.png>)

---

## 6️⃣ 🔌 Check Port Mapping

We can also check the port mapping directly using:

    docker port verventech-website-container

Expected output:

    80/tcp -> 0.0.0.0:8080

This confirms that traffic coming to port `8080` on the host is being forwarded to port `80` inside the container.

### Port Mapping

    Host Machine                 Container
    ───────────                  ─────────
    Port 8080  ───────────────→  Port 80

![alt text](<Screenshot 2026-09-16 153256.png>)

---

# ✅ Lab Completed

The `verventech-website` image was pulled from Docker Hub and used to create a running container.

Port forwarding was configured so that the website inside the container could be accessed through:

    http://localhost:8080

### Key Concepts

- **Image** → A packaged template used to create containers
- **Container** → A running instance of an image
- **Docker Hub** → A registry where Docker images can be stored and shared
- **Port Forwarding** → Connects a port on the host machine to a port inside the container