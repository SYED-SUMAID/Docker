# 🐳 Docker Portfolio Website - Volumes & Bind Mounts

A small portfolio website setup using Docker.

The web application runs with Apache + PHP, while PostgreSQL runs in a separate container. Both containers use the existing `portfolio` Docker network.

This lab demonstrates:

- 🔗 Bind mounts
- 💾 Named volumes
- 🌐 Docker networking
- 🗄️ PostgreSQL persistence
- 🔌 PHP + PostgreSQL communication

---

## 📁 Project Folder

    portfolio_repo/
    ├── index.php
    └── init.sql

Move into the project folder:

    cd ~/portfolio_repo

Check the files:

    ls

| File | Purpose |
|---|---|
| `index.php` | PHP website |
| `init.sql` | Database SQL file |


---

## 🌐 1. Check the Docker Network

The `portfolio` network already exists.

    docker network ls

Both containers will use this network.

| Container | Network |
|---|---|
| `portfolio-web` | `portfolio-network` |
| `portfolio-db` | `portfolio-network` |

![alt text](<Screenshot 2026-09-18 075608.png>)

---

## 💾 2. Create a PostgreSQL Volume

    docker volume create portfolio-db-data

Check it:

    docker volume ls

| Volume | Used For |
|---|---|
| `portfolio-db-data` | PostgreSQL data |

![alt text](<Screenshot 2026-09-18 080222.png>)

---

## 🗄️ 3. Start the PostgreSQL Container

    docker run -d \
      --name portfolio-db \
      --network portfolio-network\
      -e POSTGRES_USER=portfolio_user \
      -e POSTGRES_PASSWORD=portfolio_pass \
      -e POSTGRES_DB=portfolio_db \
      -v portfolio-db-data:/var/lib/postgresql/data \
      postgres:15

Check it:

    docker ps

| Setting | Value |
|---|---|
| Container | `portfolio-db` |
| Image | `postgres:16` |
| Network | `portfolio` |
| Database | `portfolio_db` |
| User | `portfolio_user` |
| Volume | `portfolio-db-data` |

![alt text](<Screenshot 2026-09-18 080811.png>)

---

## 📄 4. Copy `init.sql` to the Database Container

    docker cp init.sql portfolio-db:/tmp/init.sql

Enter the container:

    docker exec -it portfolio-db bash

Start PostgreSQL:

    psql -U portfolio_user -d portfolio_db


Check the tables:

    \dt

Check the data:

    SELECT * FROM your_table_name;

Replace `your_table_name` with the actual table created by `init.sql`.

Exit:

    \q
    exit

![alt text](<Screenshot 2026-09-18 081355.png>)

---

## 🌐 5. Start the Apache + PHP Container

    docker run -d \
      --name portfolio-web \
      --network portfolio0-network\
      -p 8084:80 \
      -v "$(pwd):/var/www/html" \
      php:8.2-apache

The bind mount is:

    -v "$(pwd):/var/www/html"

| Setting | Value |
|---|---|
| Container | `portfolio-web` |
| Image | `php:8.2-apache` |
| Network | `portfolio` |
| Port | `8080:80` |
| Mount | `$(pwd):/var/www/html` |

![alt text](<Screenshot 2026-09-18 082550.png>)

---

## 🔌 6. Install PostgreSQL Support for PHP

Enter the web container:

    docker exec -it portfolio-web bash

Install the package:

    apt update
    apt install -y libpq-dev

Install the PHP extensions:

    docker-php-ext-install pdo_pgsql pgsql

Exit:

    exit

Restart:

    docker restart portfolio-web

| Extension | Purpose |
|---|---|
| `pdo_pgsql` | PHP PDO support for PostgreSQL |
| `pgsql` | PostgreSQL support for PHP |

![alt text](<Screenshot 2026-09-18 082704.png>)
---

## ⚙️ 7. Configure `index.php`

Edit the file:

    nano index.php

Use:

    $host = "portfolio-db";
    $dbname = "portfolio_db";
    $user = "portfolio_user";
    $password = "portfolio_pass";

Use `portfolio-db` instead of `localhost` because PostgreSQL is in another container.

| Setting | Value |
|---|---|
| Host | `portfolio-db` |
| Database | `portfolio_db` |
| User | `portfolio_user` |
| Password | `portfolio_pass` |

![alt text](<Screenshot (803).png>)

---

## 🌍 8. Test the Website

Open:

    http://localhost:8080

| Host | Container |
|---|---|
| `8080` | `80` |

![alt text](<Screenshot (795).png>)

---

## 🔄 9. Test the Bind Mount

Edit the website on the host:

    nano index.php

Make a visible change and save it.

Enter the web container:

    docker exec -it portfolio-web bash

Check the file:

    cat /var/www/html/index.php

The host-side change should be visible inside the container.

Exit:

    exit

Refresh:

    http://localhost:8080

![alt text](<Screenshot (796).png>)

![alt text](<Screenshot (801).png>)

---

## 💾 10. Test PostgreSQL Volume Persistence

Stop PostgreSQL:

    docker stop portfolio-db

Remove the container:

    docker rm portfolio-db

Check the volume:

    docker volume ls

`portfolio-db-data` should still exist.

Recreate PostgreSQL using the same volume:

    docker run -d \
      --name portfolio-db-new-container \
      --network portfolio-network \
      -e POSTGRES_USER=portfolio_user \
      -e POSTGRES_PASSWORD=portfolio_pass \
      -e POSTGRES_DB=portfolio_db \
      -v portfolio-db-data:/var/lib/postgresql/data \
      postgres:15

Enter the container:

    docker exec -it portfolio-db bash

Open PostgreSQL:

    psql -U portfolio_user -d portfolio_db

Check the tables:

    \dt

Check the data:

    SELECT * FROM your_table_name;

Exit:

    \q
    exit

![alt text](<Screenshot (800).png>)

![alt text](<Screenshot 2026-09-18 080222(1).png>)

![alt text](<Screenshot 2026-09-18 194350.png>)


---


## 📦 Bind Mount vs Named Volume

| Feature | Bind Mount | Named Volume |
|---|---|---|
| Example | `$(pwd):/var/www/html` | `portfolio-db-data:/var/lib/postgresql/data` |
| Used For | Website files | PostgreSQL data |
| Storage | Host directory | Docker-managed |
| Host editing | Easy | Not directly |
| Survives container removal | ✅ | ✅ |

---

## 🔗 Bind Mount

    -v "$(pwd):/var/www/html"

Used for the website files.

Changes made to `index.php` on the host appear inside the container immediately.

---

## 💾 Named Volume

    -v portfolio-db-data:/var/lib/postgresql/data

Used for PostgreSQL data.

The data remains after the PostgreSQL container is removed.

---

## 🌐 Docker Network

Both containers use:

    portfolio

Communication:

    portfolio-web
         |
         | portfolio network
         v
    portfolio-db

PHP uses:

    $host = "portfolio-db";

---

## 🧩 Complete Architecture

| Component | Container | Storage | Network | Port |
|---|---|---|---|---|
| Web | `portfolio-web` | Bind Mount | `portfolio` | `8080:80` |
| Database | `portfolio-db` | Named Volume | `portfolio` | `5432` |

---

## 📝 Main Commands Used

    docker network ls
    docker volume create portfolio-db-data
    docker volume ls
    docker run
    docker ps
    docker cp
    docker exec -it
    docker stop
    docker rm
    docker restart

---

## 🎯 What This Lab Demonstrates

- 🐳 Apache + PHP in one container
- 🗄️ PostgreSQL in another container
- 🌐 Communication through a Docker network
- 🔗 Bind mounts for website files
- 💾 Named volumes for database persistence
- 📄 Loading data with `init.sql`
- 🔌 PHP connecting to PostgreSQL
- 🔄 Host changes appearing inside the container
- 💾 Database data surviving container removal

## Key Takeaway

    index.php
        ↓
    Bind Mount
        ↓
    portfolio-web
        ↓
    Docker Network
        ↓
    portfolio-db
        ↓
    Named Volume
        ↓
    PostgreSQL Data