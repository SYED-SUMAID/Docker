# Lab 52: Docker Port Mapping

## 📌 Objective

Understand Docker port mapping and learn how to make an application running inside a container accessible from the host machine.

---

## 📋 Quick Reference

| **Command / Concept** | **Example** | **Purpose** |
|---|---|---|
| Port Mapping | `-p 8080:80` | Maps host port `8080` to container port `80` |
| Syntax | `HOST:CONTAINER` | Defines the port mapping order |
| `-p` | `-p 8080:80` | Manually publishes a port |
| `-P` | `-P` | Automatically assigns host ports |
| `EXPOSE` | `EXPOSE 80` | Documents the container port |
| Check Ports | `docker ps` | Shows published ports |
| Check Mapping | `docker port <container>` | Shows a container's port mapping |
| Browser | `http://localhost:8080` | Accesses the application |

---

## 🖼️ Port Mapping Overview



Docker connects a **host port** to a **container port**.

    Host Machine                    Docker Container
    ┌───────────────┐              ┌─────────────────┐
    │               │              │                 │
    │ localhost     │   8080 → 80  │     Apache      │
    │    :8080      │ ───────────→ │      :80        │
    │               │              │                 │
    └───────────────┘              └─────────────────┘

---

## Part 1 — Understanding Port Mapping

### 📖 What is Port Mapping?

Port mapping connects a **port on the host machine** to a **port inside the container**.

The basic syntax is:

    -p HOST_PORT:CONTAINER_PORT

For example:

    -p 8080:80

This means:

    Host port 8080 → Container port 80

### 🧠 Remember

> **Left side = Host**  
> **Right side = Container**

    -p 8080:80
       │    │
       │    └── Container Port
       └─────── Host Port

---

## Part 2 — Run Apache with Port Mapping

### 💻 Command

    docker run -d -p 8080:80 httpd

This starts an Apache container and maps port `8080` on the host to port `80` inside the container.

![alt text](<Screenshot 2026-09-15 123446.png>)

### 🔍 Flag Breakdown

| **Flag** | **Full Form / Syntax** | **Purpose** |
|---|---|---|
| `-d` | `--detach` | Runs the container in the background |
| `-p` | `--publish` | Publishes a container port to the host |
| `8080:80` | `HOST:CONTAINER` | Maps host `8080` to container `80` |
| `httpd` | Docker Image | Apache HTTP Server image |

###  Result

The Apache container was successfully started and accessed through the mapped host port `8080`.

    Host Port 8080 → Container Port 80

The application is successfully reachable at:

    http://localhost:8080


## Part 3 — Check the Running Container

### 💻 Command

    docker ps

Look at the **PORTS** column.

Example:

    0.0.0.0:8080->80/tcp

This means:

    Host:8080
          ↓
    Container:80
          ↓
       Apache

### 📸 Output

![alt text](<Screenshot 2026-09-15 123650.png>)

---

## Part 4 — Access Apache from the Browser

### 🌐 Open

    http://localhost:8080

The browser sends the request to port `8080` on the host.

Docker forwards the request to port `80` inside the container.

    Browser
       │
       │ localhost:8080
       ↓
    Host Port 8080
       │
       │ Docker Port Mapping
       ↓
    Container Port 80
       │
       ↓
    Apache

### 📸 Apache Page

![alt text](<Screenshot 2026-09-15 123650-1.png>)

---

## Part 5 — Check Port Mapping

### 💻 Command

    docker port <container_name>

Example:

    docker port myapache

Possible output:

    80/tcp -> 0.0.0.0:8080

This confirms that:

    Container 80 → Host 8080

---

## Part 6 — Use a Different Host Port

The host port can be changed while keeping the container port the same.

### 💻 Command

    docker run -d -p 9090:80 httpd

Now:

    Host port 9090 → Container port 80

Access Apache using:

    http://localhost:9090

### 🖼️ Port Comparison

| **Host Port** | **Container Port** | **Browser URL** |
|---:|---:|---|
| `8080` | `80` | `http://localhost:8080` |
| `9090` | `80` | `http://localhost:9090` |
| `8888` | `80` | `http://localhost:8888` |

The Apache container continues listening on port `80`.

---

## Part 7 — `EXPOSE` vs `-p`

| **Feature** | **`EXPOSE`** | **`-p`** |
|---|---|---|
| Purpose | Documents a container port | Publishes a port |
| Host access | No | Yes |
| Example | `EXPOSE 80` | `-p 8080:80` |
| Usually used in | Dockerfile | `docker run` |

### Example

    EXPOSE 80

This tells Docker that the application uses port `80`.

But it does **not** make the application available through the host.

To publish the port:

    docker run -p 8080:80 httpd

---

## Part 8 — `-p` vs `-P`

| **Option** | **Meaning** | **Example** |
|---|---|---|
| `-p` | Manually choose the host port | `-p 8080:80` |
| `-P` | Automatically choose host ports | `-P` |

### Manual Mapping

    docker run -d -p 8080:80 httpd

You choose the host port.

### Automatic Mapping

    docker run -d -P httpd

Docker automatically assigns an available host port to the exposed container port.

---

## 📊 Port Mapping Flow


    ┌──────────┐
    │ Browser  │
    └────┬─────┘
         │
         │ localhost:8080
         ↓
    ┌──────────────┐
    │ Host :8080   │
    └──────┬───────┘
           │
           │ Docker maps 8080 → 80
           ↓
    ┌──────────────────┐
    │ Container :80    │
    │                  │
    │     Apache       │
    └──────────────────┘

---

## 🛠️ Useful Commands

### Run Apache with Port Mapping

    docker run -d -p 8080:80 httpd

### List Running Containers

    docker ps

### List All Containers

    docker ps -a

### Check Port Mapping

    docker port <container_name>

### Stop Container

    docker stop <container_name>

### Remove Container

    docker rm <container_name>

---

## ⚠️ Common Mistake

Do not reverse the port order.

### ❌ Wrong Understanding

    -p 80:8080

This means:

    Host 80 → Container 8080

### ✅ Correct

    -p 8080:80

This means:

    Host 8080 → Container 80

Always remember:

    -p HOST:CONTAINER

---

## 🧠 Key Points

- `-p` is used to **publish container ports**.
- The format is **`HOST_PORT:CONTAINER_PORT`**.
- `-p 8080:80` means **Host `8080` → Container `80`**.
- The host port and container port do not have to be the same.
- `EXPOSE` documents a port but does not publish it.
- `-P` automatically assigns host ports.
- `docker ps` shows published ports.
- `docker port` shows a specific container's port mapping.

---

## ⭐ Most Important Example

    docker run -d -p 8080:80 httpd

Remember:

    8080 = Host Port
    80   = Container Port

So the complete flow is:

    Browser
       ↓
    localhost:8080
       ↓
    Host Port 8080
       ↓
    Docker Port Mapping
       ↓
    Container Port 80
       ↓
    Apache

---

## ✅ Lab Complete

- Docker port mapping
- `-p` and `-P`
- `HOST:CONTAINER` syntax
- `EXPOSE`
- Checking published ports
- Accessing a containerized application from the browser