# 🐳 Docker Volume with PostgreSQL

> A hands-on lab to understand how Docker Volumes provide persistent storage for containerized PostgreSQL databases.

---

## 🎯 Objective

In this lab, we will:

- Create a Docker Volume
- Attach it to a PostgreSQL container
- Store database data inside the volume
- Remove the PostgreSQL container
- Create a new container using the same volume
- Verify that the data is still available

### 🧠 Main Idea

**Container lifecycle ≠ Data lifecycle**

The PostgreSQL container can be removed while the Docker Volume continues to store the database data.

---

## 📦 1. Create the Docker Volume

Create a named volume:

    sudo docker volume create postgres-data

Verify it:

    sudo docker volume ls

![alt text](<Screenshot 2026-09-17 162637(1).png>)

---

## 🐘 2. Run PostgreSQL with the Volume

Start a PostgreSQL container and attach the volume:

    sudo docker run -d --name postgres-volume -e POSTGRES_PASSWORD=postgres -v postgres-data:/var/lib/postgresql postgres

![alt text](<Screenshot 2026-09-17 163612(1).png>)

### Command Breakdown

| Option | Meaning |
|---|---|
| `-d` | Runs the container in detached mode |
| `--name postgres-volume` | Names the container |
| `-e POSTGRES_PASSWORD=postgres` | Sets the PostgreSQL password |
| `-v postgres-data:/var/lib/postgresql` | Mounts the Docker Volume |
| `postgres` | Uses the PostgreSQL Docker image |

> The current PostgreSQL Docker image uses `/var/lib/postgresql` for this volume setup.

---

## 🔐 3. Enter PostgreSQL

Use `docker exec` to access PostgreSQL inside the running container:

    sudo docker exec -it postgres-volume psql -U postgres


---

## 🗃️ 4. Create a Table

Inside PostgreSQL, create a `students` table:

    CREATE TABLE students (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100)
    );

---

## ✍️ 5. Insert Data

Add some sample records:

    INSERT INTO students (name)
    VALUES ('Sumaid'), ('Ahmed'), ('Zaid');

Verify the data:

    SELECT * FROM students;

Expected result:

| id | name |
|---:|------|
| 1 | Sumaid |
| 2 | Ahmed |
| 3 | Zaid |

![alt text](<Screenshot 2026-09-17 173336.png>)

> Add a screenshot here showing the `students` table with the inserted records.

Exit PostgreSQL:

    \q

---

## 🗑️ 6. Remove the Container

Remove the PostgreSQL container:

    sudo docker rm -f postgres-volume

The container has now been removed.

However, the `postgres-data` volume still exists.

This is the important part of the demonstration.

---

## 🔄 7. Create a New PostgreSQL Container

Create another PostgreSQL container and attach the same volume:

    sudo docker run -d --name postgres-volume-new -e POSTGRES_PASSWORD=postgres -v postgres-data:/var/lib/postgresql postgres

The new container is different from the original container, but both containers use the same Docker Volume.

---

## 🔐 8. Enter the New Container

Access PostgreSQL in the new container:

    sudo docker exec -it postgres-volume-new psql -U postgres

---

## ✅ 9. Verify Data Persistence

Run:

    SELECT * FROM students;

The original data should still be available:

| id | name |
|---:|------|
| 1 | Sumaid |
| 2 | Ahmed |
| 3 | Zaid |


![alt text](<Screenshot 2026-09-17 173456.png>)

---

## 🔁 What Happened?

    PostgreSQL Container
            │
            ▼
      postgres-data
       Docker Volume
            │
            ▼
      Database Data

The original container was removed:

    Container → Removed
    Volume    → Still exists
    Data      → Still exists

A new PostgreSQL container was then connected to the same volume:

    New Container
          │
          ▼
    postgres-data
          │
          ▼
    Existing Database Data

---

## 🧠 Key Takeaways

- Docker containers have their own writable storage.
- Container storage is tied to the container lifecycle.
- Docker Volumes provide persistent storage outside the container.
- Named volumes can be reused by different containers.
- Removing a container does not automatically remove its named volume.
- PostgreSQL database data can persist even when the original container is removed.

> **Containers can come and go. Your data doesn't have to.**