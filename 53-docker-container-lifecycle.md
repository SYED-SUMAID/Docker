# 🐳 Docker Container Lifecycle

A practical lab demonstrating the complete lifecycle of a Docker container — from creation to removal.

## 🎯 Objective

To understand and practice the different stages of a Docker container lifecycle:

**Create → Start → Run → Stop → Restart → Pause → Unpause → Remove**

---

## 🧰 Prerequisites

- Docker installed
- Ubuntu/Linux terminal
- Internet connection for pulling the Ubuntu image

---

## 1. 📥 Pull the Ubuntu Image

Make sure the Ubuntu image is available locally.

    sudo docker pull ubuntu

This downloads the Ubuntu image from Docker Hub if it is not already available locally.

![alt text](<Screenshot (761)(1).png>)

---

## 2. 🏗️ Create the Container

Create a container named `lifecycle-test`:

    sudo docker create --name lifecycle-test ubuntu sleep infinity

### What this does

- `docker create` → Creates a container without starting it.
- `--name lifecycle-test` → Gives the container a name.
- `ubuntu` → The image used to create the container.
- `sleep infinity` → Keeps the container running once started.

Check the container:

    sudo docker ps -a

The container should show:

    STATUS: Created

![alt text](<Screenshot 2026-09-16 080921.png>)

---

## 3. ▶️ Start the Container

Start the container:

    sudo docker start lifecycle-test

Check the running container:

    sudo docker ps

The container should show a status similar to:

    Up ...

The lifecycle is now:

**Created → Running**

![alt text](<Screenshot 2026-09-16 081109.png>)

---

## 4. 💻 Enter the Running Container

Use `docker exec` to open a Bash shell inside the running container:

    sudo docker exec -it lifecycle-test /bin/bash

Once inside the container, run:

    ls

You should see directories such as:

    bin
    boot
    dev
    etc
    home
    lib

To leave the container:

    exit

### Important

`docker exec` does **not** create or start another container.

It simply executes a command inside an already-running container.

![alt text](<Screenshot 2026-09-16 081421.png>)

---

## 5. ⏹️ Stop the Container

Stop the running container:

    sudo docker stop lifecycle-test

Check all containers:

    sudo docker ps -a

The status should show:

    Exited (0)

### Important

Stopping a container does **not** delete it.

The container still exists and can be started again.

**Running → Stopped**
![alt text](<Screenshot 2026-09-16 082033.png>)
![alt text](<Screenshot 2026-09-16 081830(1).png>)

---

## 6. ▶️ Start the Stopped Container Again

Start the same container:

    sudo docker start lifecycle-test

Verify:

    sudo docker ps

The container should be running again.

**Stopped → Running**

### Start vs Restart

| Command | Action |
|---|---|
| `docker start` | Starts a stopped container |
| `docker restart` | Stops and starts a container |

![alt text](<Screenshot 2026-09-16 082033-1.png>)

---

## 7. 🔄 Restart the Container

Restart the running container:

    sudo docker restart lifecycle-test

This performs:

    Running → Stop → Start → Running

Verify:

    sudo docker ps

### Remember

**Start = Turn it on**

**Restart = Turn it off and on again**

![alt text](<Screenshot (762)(1)(1).png>)

---

## 8. ⏸️ Pause the Container

Pause the processes inside the running container:

    sudo docker pause lifecycle-test

Check the status:

    sudo docker ps

The status should show something similar to:

    Up ... (Paused)

### What does Pause do?

`docker pause` freezes the processes inside the container.

    Running
       ↓
    Pause
       ↓
    Processes Frozen

The container is **not deleted**, and pausing is different from stopping it.

![alt text](<Screenshot 2026-09-16 082943(1).png>)
![alt text](<Screenshot 2026-09-16 082958(1).png>)

---

## 9. ▶️ Unpause the Container

Resume the paused processes:

    sudo docker unpause lifecycle-test

Verify:

    sudo docker ps

The container should return to its normal running state.

**Paused → Running**

![alt text](<Screenshot 2026-09-16 083050.png>)
![alt text](<Screenshot 2026-09-16 083054(1).png>)
---

## 10. ⏹️ Stop the Container

Stop the container before removing it:

    sudo docker stop lifecycle-test

Verify:

    sudo docker ps -a

The container should show:

    Exited (0)

![alt text](<Screenshot 2026-09-16 081812.png>)

---

## 11. 🗑️ Remove the Container

Remove the stopped container:

    sudo docker rm lifecycle-test

Verify that it has been removed:

    sudo docker ps -a

The `lifecycle-test` container should no longer appear.

### Stop vs Remove

| Command | What happens |
|---|---|
| `docker stop` | Stops the container |
| `docker rm` | Deletes the container |

![alt text](<Screenshot 2026-09-16 083424.png>)
![alt text](<Screenshot 2026-09-16 083440(1).png>)

---

# 🔄 Complete Container Lifecycle

    🏗️ Create
         ↓
    ▶️ Start
         ↓
    🟢 Running
         ↓
    ⏹️ Stop
         ↓
    ▶️ Start
         ↓
    🔄 Restart
         ↓
    ⏸️ Pause
         ↓
    ▶️ Unpause
         ↓
    ⏹️ Stop
         ↓
    🗑️ Remove

---

# 📌 Command Summary

| Command | Purpose |
|---|---|
| `docker pull` | Downloads an image |
| `docker create` | Creates a container without starting it |
| `docker start` | Starts a stopped container |
| `docker ps` | Shows running containers |
| `docker ps -a` | Shows all containers |
| `docker exec` | Executes a command inside a running container |
| `docker stop` | Stops a running container |
| `docker restart` | Stops and starts a container |
| `docker pause` | Freezes container processes |
| `docker unpause` | Resumes paused processes |
| `docker rm` | Removes a container |

---

## 🧠 Key Takeaways

- A **container can exist without running**.
- `docker start` starts an existing stopped container.
- `docker exec` lets you work inside a running container.
- `docker stop` stops a container but does not delete it.
- `docker restart` performs a stop followed by a start.
- `docker pause` freezes the processes inside a container.
- `docker unpause` resumes those processes.
- `docker rm` permanently removes the container.

> **Container lifecycle:** Create → Start → Run → Stop/Restart/Pause → Remove