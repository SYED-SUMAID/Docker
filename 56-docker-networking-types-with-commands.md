# 🐳 Docker Networking — Types & Commands

A simple hands-on lab to understand Docker networking types and how containers communicate.

---

## 📌 1. Check Docker Networks

First, check the networks Docker already provides.

    docker network ls

![alt text](<Screenshot 2026-09-19 083444(1).png>)

You will normally see:

- `bridge`
- `host`
- `none`

---

## 🌉 2. Bridge Network

The **bridge network** is Docker's default networking method.

### Run Apache containers

    docker run -dit --name apache30 --network bridge httpd

Check the network:

    docker network inspect bridge

![alt text](<Screenshot 2026-09-19 135957(1)(1)-1.png>)
![alt text](<Screenshot (804)(1)-1.png>)

### Simple idea

    Container → Docker Bridge → Network

---

## 🔗 3. Custom Bridge Network

A custom bridge network is a network that **we create ourselves**.

### Create the network

    docker network create my-network
    docker network ls

![alt text](<Screenshot 2026-09-19 083444(2).png>)

### Run Apache containers

    docker run -dit --name apache1 --network my-network httpd
    docker run -dit --name apache2 --network my-network httpd

Check the network:

    docker network inspect my-network

![alt text](<Screenshot 2026-09-19 083903.png>)




### Test communication

Enter `apache1`:

    docker exec -it apache1 bash

Install curl:

    apt update && apt install -y curl

Test `apache2`:

    curl http://apache20

Exit:

    exit

![alt text](<Screenshot 2026-09-19 131038.png>)



### Simple idea

    apache1 ──┐
              ├── my-network
    apache2 ──┘

Containers on the same custom bridge network can communicate using their container names.

---

## 🖥️ 4. Host Network

With the **host network**, the container directly uses the host's network.

Remove the previous containers:

    docker rm -f apache1 apache2

Run Apache using host networking:

    docker run -d --name apache-host --network host httpd

![alt text](<Screenshot 2026-09-19 155706.png>)



Remove the container:

    docker rm -f apache-host

### Simple idea

    Host Network
         │
         └── Apache Container

The container uses the host's network directly.

---

## 🚫 5. None Network

The `none` network gives the container **no normal network connection**.

Run an Ubuntu container:

    docker run -dit --name no-network --network none ubuntu bash

Enter the container:

    docker exec -it no-network bash

Check its network:

    ip addr

Exit:

    exit

![alt text](<Screenshot 2026-09-19 125532-1.png>)

### Simple idea

    Container
        X
      Network

The container has no external network connection.

---

## 🔌 6. Connect a Container to a Network

Create an Apache container:

    docker run -dit --name apache30 httpd

Connect it to the custom network:

    docker network connect my-network apache30

Check the networks:

    docker inspect apache30 --format '{{json .NetworkSettings.Networks}}'

![alt text](<Screenshot 2026-09-19 135957(1)(1).png>)

---

## ❌ 7. Disconnect a Container

Disconnect the container from `my-network`:

    docker network disconnect my-network apache1

Remove the container:

    docker rm -f apache1


---

## 📊 Docker Network Types — Quick Overview

| Network Type | Simple Meaning | Main Use |
|---|---|---|
| 🌉 Bridge | Docker gives the container its own network | Normal container networking |
| 🔗 Custom Bridge | Our own Docker network | Container-to-container communication |
| 🖥️ Host | Container uses the host's network | Direct host networking |
| 🚫 None | No network connection | Containers that don't need networking |

---

## 🧠 Easy Way to Remember

    Bridge → Docker's network

    Custom Bridge → Our own Docker network

    Host → Uses the host's network

    None → No network


---

## ✅ Lab Completed

In this lab, we learned:

- Docker default bridge networking
- Custom bridge networking
- Host networking
- None networking
- Container-to-container communication
- Connecting containers to networks
- Disconnecting containers from networks
- Removing Docker networks