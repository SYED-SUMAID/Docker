# Dockerized PHP + PostgreSQL Portfolio

> A simple two-container web application built with Docker, where Apache/PHP handles the website and PostgreSQL stores the application data.

---

## 🎯 Objective

The objective of this project is to understand how a web application can be divided into separate services and run using Docker.

In this setup, Apache and PHP run inside one container while PostgreSQL runs inside another. Both containers communicate through a custom Docker network, allowing the PHP application to access the PostgreSQL database using the container name.

The project also demonstrates database initialization, PHP PostgreSQL connectivity, port mapping, and displaying database records dynamically on a web page.

---

## 🏗️ Architecture

    Browser
       │
       │ localhost:8082
       ▼
    portfolioWEB
    Apache + PHP
       │
       │ portfolio-network
       ▼
    portfolioDB
    PostgreSQL
       │
       ▼
    portfolio_db
       │
       ▼
    sm_users

---

## 📁 Project Structure

    portfolio_repo/
    ├── index.php
    └── init.sql

---

## 1. Create Docker Network

    sudo docker network create portfolio-network

![alt text](<Screenshot 2026-09-16 161719.png>)
---

## 2. Create Project Directory

    mkdir -p ~/portfolio_repo

Place `index.php` and `init.sql` inside the directory.

    ls -l ~/portfolio_repo

![alt text](<Screenshot (776)(1).png>)

---

## 3. Create PostgreSQL Container

    sudo docker run -d \
      --name portfolioDB \
      --network portfolio-network \
      -e POSTGRES_USER=sum \
      -e POSTGRES_PASSWORD=12345 \
      -e POSTGRES_DB=portfolio_db \
      postgres:15

Check the running container:

    sudo docker ps

![alt text](<Screenshot (786).png>)

---

## 4. Create Database Table

The `init.sql` file creates the `sm_users` table and inserts sample records.

    CREATE TABLE IF NOT EXISTS sm_users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(100) NOT NULL,
        time TIME,
        job VARCHAR(100),
        course VARCHAR(100),
        email VARCHAR(150),
        phone VARCHAR(20),
        gender VARCHAR(20)
    );

    TRUNCATE TABLE sm_users RESTART IDENTITY;

    INSERT INTO sm_users
    (name, time, job, course, email, phone, gender)
    VALUES
    ('Rahul Sharma', '10:00', 'Software Developer', 'B.Tech', 'rahul@gmail.com', '9876543211', 'Male'),
    ('Sara Ahmed', '11:30', 'Data Analyst', 'MCA', 'sara@gmail.com', '9876543212', 'Female'),
    ('Ayaan Malik', '14:00', 'Business Analyst', 'BBA', 'ayaan@gmail.com', '9876543213', 'Male'),
    ('Aisha Riyaz', '16:30', 'HR Executive', 'BBA', 'aisha@gmail.com', '9876543214', 'Female');

Copy and execute the SQL file:

    sudo docker cp ~/portfolio_repo/init.sql portfolioDB:/tmp/init.sql

    sudo docker exec -it portfolioDB \
    psql -U sum -d portfolio_db -f /tmp/init.sql

![alt text](<Screenshot (775).png>)

![alt text](<Screenshot 2026-09-17 145439.png>)
---

## 5. Create Apache + PHP Container

    sudo docker run -d \
      --name portfolioWEB \
      --network portfolio-network \
      -p 8082:80 \
      php:8.3-apache

![alt text](<Screenshot 2026-09-17 153359(1).png>)

---

## 6. Install PostgreSQL Support for PHP

Install the required PostgreSQL library:

    sudo docker exec portfolioWEB bash -c \
    "apt-get update && apt-get install -y libpq-dev"

Install PHP PostgreSQL extensions:

    sudo docker exec portfolioWEB \
    docker-php-ext-install pgsql pdo_pgsql

Restart the container:

    sudo docker restart portfolioWEB

![alt text](<Screenshot 2026-09-17 145536(1)(1).png>)

---

## 7. Configure PHP

The PHP application connects to PostgreSQL using these values:

| Variable | Value |
|---|---|
| `DB_HOST` | `portfolioDB` |
| `DB_PORT` | `5432` |
| `DB_NAME` | `portfolio_db` |
| `DB_USER` | `sum` |
| `DB_PASSWORD` | `12345` |

The PHP application queries the `sm_users` table and displays its records.

![alt text](<Screenshot (774).png>)

![alt text](<Screenshot 2026-09-17 152503.png>)
---

## 8. Copy PHP Application

    sudo docker cp ~/portfolio_repo/index.php \
    portfolioWEB:/var/www/html/index.php

The PHP file is served by Apache from:

    /var/www/html/index.php

![alt text](<Screenshot 2026-09-17 152821.png>)

---

## 9. Verify Docker Network

    sudo docker network inspect portfolio-network

Both containers should be connected to:

    portfolio-network

Expected containers:

- `portfolioWEB`
- `portfolioDB`

![alt text](<Screenshot (781)(1).png>)

---

## 🌐 10. Run the Application

Open the application in your browser:

    http://localhost:8082

The request reaches Apache, PHP connects to PostgreSQL through the Docker network, retrieves data from `sm_users`, and displays it on the webpage.

![alt text](<Screenshot (785)(1).png>)

---

## 🧠 Key Concepts

### Docker Network

Allows `portfolioWEB` and `portfolioDB` to communicate with each other.

### Port Mapping

    8082:80

means:

    Host Port 8082 → Container Port 80

### Container Name

PHP uses:

    portfolioDB

as the PostgreSQL hostname because both containers are connected to the same Docker network.

### PostgreSQL

Stores the application data inside the `portfolio_db` database.

### PHP + PostgreSQL

The `pgsql` extension allows PHP to communicate with PostgreSQL.

---

## 📝 Note

This project uses two separate containers instead of installing Apache, PHP, and PostgreSQL directly on the host system.

The containers communicate internally through `portfolio-network`, while only the web application is exposed to the host through port `8082`.

The PostgreSQL container does not need a host port mapping because PHP accesses it directly through Docker's internal network.

---

## 🏁 Conclusion

This project demonstrates a complete small-scale containerized web application.

Apache and PHP handle the frontend and application logic, PostgreSQL stores the data, and Docker provides the isolated environment and network that connects everything together.

The complete flow is:

**Browser → Apache/PHP → Docker Network → PostgreSQL → Database → PHP → Webpage**

A simple setup, but a practical foundation for understanding how multi-container applications work in Docker.