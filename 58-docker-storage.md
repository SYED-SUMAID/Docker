# 🐳 Docker Storage

When working with Docker, containers often need to create and store data.

This could be application files, database data, logs, uploaded files, configuration files, or anything else an application needs while running.

The important thing to understand is that **containers are temporary by nature**. If we store important data directly inside a container, that data can disappear when the container is removed.

Docker provides different storage options to handle this problem.

---

## 📦 What is Docker Storage?

Docker storage is the way Docker manages data used and created by containers.

A container may need to store things like:

- 📄 Application files
- 🗄️ Database data
- ⚙️ Configuration files
- 📝 Logs
- 📤 Uploaded files
- 🧾 Generated files

By default, files created inside a container are stored in the container's **writable layer**.

---

## ✏️ Container Writable Layer

When Docker creates a container from an image, the image itself remains read-only.

Docker adds a writable layer on top of the image where the container can create or modify files.

    Docker Image
         │
         ▼
    Read-only Layers
         │
         ▼
    Writable Container Layer
         │
         ▼
    Running Container

So, when we create or modify a file inside a container, Docker normally stores that change in the container's writable layer.

---

## 🧪 Simple Example

Let's see what happens when we store a file directly inside a container.

Create an Ubuntu container:

    docker run -it --name storage-test ubuntu bash

Create a file inside the container:

    echo "Docker Storage Lab" > /tmp/test.txt

Check the file:

    cat /tmp/test.txt

You should see:

    Docker Storage Lab

Now exit the container:

    exit

At this point, the container still exists.

Start it again:

    docker exec -it storage-test bash

Check the file again:

    cat /tmp/test.txt

The file is still there because we only stopped and started the same container.

![alt text](<Screenshot 2026-09-17 160234.png>)

---

## 🗑️ What Happens When the Container is Removed?

Now remove the container:

    docker rm storage-test

Create a new container with the same name:

    docker run -it --name storage-test ubuntu bash

Try to access the file:

    cat /tmp/test.txt

The file or directory is no longer there.

![alt text](<Screenshot 2026-09-17 160639.png>)

### Why?

Because the file was stored in the writable layer of the **old container**.

Once that container was removed, its writable layer was removed as well.

This gives us an important rule:

> ⚠️ Data stored only inside a container should not be considered persistent storage.

---

## 🤔 Why Do We Need Persistent Storage?

Imagine we have a PostgreSQL database running inside a Docker container.

The database might contain:

- 👤 User accounts
- 📊 Application data
- 📝 Records
- ⚙️ Database configuration

If all of this data is stored only inside the container and we remove that container, the data can be lost.

We therefore need a way to store important data **outside the container's temporary writable layer**.

This is where Docker's persistent storage options become useful.

---

## 💾 Docker Storage Options

Docker provides different ways to handle container data.

The three important concepts for this lab series are:

1. 📦 Container Writable Layer
2. 💽 Docker Volumes
3. 📁 Bind Mounts

| Storage Type | Where Data is Stored | Survives Container Removal |
|---|---|---|
| Writable Layer | Inside the container | ❌ No |
| Volume | Docker-managed storage | ✅ Yes |
| Bind Mount | Host filesystem | ✅ Yes |

---

## 💽 Docker Volumes

A Docker volume is a storage location that is **managed by Docker**.

Unlike the container's writable layer, a volume exists independently from the container.

For example:

    docker volume create myvolume

The volume can then be attached to a container.

The main idea is:

    Container
        │
        │ uses
        ▼
      Volume
        │
        ▼
    Persistent Data

Even if the container is removed, the volume can continue to exist.

Volumes are commonly used for things such as:

- 🗄️ Database storage
- 📦 Application data
- 💾 Persistent files

---

## 📁 Bind Mounts

A bind mount allows us to connect a specific file or directory on the Docker host to a location inside a container.

For example:

    Host
    /home/user/project
            │
            │ Bind Mount
            ▼
    Container
    /app

The container can access the files from the host directory.

This is especially useful during development.

For example, we can keep our source code on the host and allow the application running inside the container to use that code.

---

## ⚖️ Volumes vs Bind Mounts

Both volumes and bind mounts allow data to exist outside the container, but they work differently.

| Feature | Volume | Bind Mount |
|---|---|---|
| Managed by Docker | ✅ Yes | ❌ No |
| Uses Docker-managed storage | ✅ Yes | ❌ No |
| Uses a specific host path | ❌ No | ✅ Yes |
| Data survives container removal | ✅ Yes | ✅ Yes |
| Useful for databases | ✅ Yes | ✅ Possible |
| Useful for development files | Sometimes | ✅ Very common |

---

## 🧠 The Main Idea


    Docker Storage
           │
    ┌──────┼──────┐
    │      │      │
    ▼      ▼      ▼
    Writable  Volume  Bind Mount
    Layer
      │       │       │
      ▼       ▼       ▼
    Temporary Persistent Host Files

The **writable layer belongs to the container**.

A **volume exists independently of the container and is managed by Docker**.

A **bind mount connects the container directly to a file or directory on the host**.

---

## 🔄 Container Lifecycle vs Data Lifecycle

This is one of the most important concepts in Docker storage.

    Container
        │
        ├── Created
        │
        ├── Running
        │
        ├── Stopped
        │
        └── Removed
               │
               ▼
        Writable Layer Removed

    Volume / Bind Mount
               │
               ▼
        Data Can Remain

This separation allows us to replace or recreate containers without necessarily losing the data they use.


---

## 🎯 Key Takeaways

- 🐳 Containers have a writable layer where changes can be stored.
- ⚠️ Data stored only in the writable layer is tied to the container.
- 🗑️ Removing the container also removes its writable layer.
- 💾 Persistent storage allows data to survive container removal.
- 💽 Docker volumes are managed by Docker.
- 📁 Bind mounts connect host files or directories to containers.
- 🔄 Containers can be recreated without losing externally stored data.

---

