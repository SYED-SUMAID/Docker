# Dockerfile Usage

A short guide to what Docker is, why it's used, and how a Dockerfile works.

## What is Docker?

Docker packages an app and everything it needs (code, runtime, libraries, config) into a **container**. The container runs the same way on any machine.

## Why Use Docker?

- **Consistency:** "Works on my machine" becomes "works everywhere."
- **Easy setup:** One command starts the whole app, with no manual installs.
- **Isolation:** Each app runs separately, so versions never clash.
- **Lightweight:** Containers start in seconds and use fewer resources than virtual machines.
- **Portable:** The same container runs on a laptop, a server, or the cloud.
- **Team friendly:** Everyone gets the same environment.

## Key Terms

| Term | Meaning |
|------|---------|
| Dockerfile | A text file with instructions to build an image |
| Image | A read-only template built from a Dockerfile |
| Container | A running instance of an image |
| Volume | Storage that keeps data after a container stops |
| Docker Compose | A tool to run multiple containers together |

## What is a Dockerfile?

A Dockerfile is a recipe. Docker reads it top to bottom and builds an image step by step.

## Common Instructions

| Instruction | Purpose |
|-------------|---------|
| `FROM` | Sets the base image |
| `RUN` | Runs a command while building |
| `COPY` | Copies files into the image |
| `WORKDIR` | Sets the working directory |
| `ENV` | Sets environment variables |
| `EXPOSE` | Documents the port the app uses |
| `CMD` | Default command when the container starts |

## Example

A Dockerfile for a PHP app with PostgreSQL support:

    FROM php:8.2-apache

    RUN apt-get update && apt-get install -y libpq-dev \
        && docker-php-ext-install pgsql pdo_pgsql \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

    COPY ./src/ /var/www/html/

What each part does:

- `FROM` starts from an official PHP and Apache image.
- `RUN` installs the PostgreSQL libraries and PHP extensions.
- `COPY` puts the app code where Apache serves it.

## Build and Run

    docker build -t my-app .
    docker run -d -p 8080:80 --name my-app my-app

Then open http://localhost:8080

## Docker Compose

Compose runs several containers (for example a web app and a database) from one `docker-compose.yml` file.

    docker compose up --build     # Build and start everything
    docker compose up -d          # Run in background
    docker compose down           # Stop and remove containers
    docker compose down -v        # Also delete volumes (data)

## Useful Commands

    docker ps                     # List running containers
    docker ps -a                  # List all containers
    docker images                 # List images
    docker logs <container>       # View logs
    docker exec -it <container> bash   # Open a shell inside
    docker stop <container>       # Stop a container
    docker rm <container>         # Remove a container
    docker rmi <image>            # Remove an image

## Best Practices

- Use small, official base images.
- Combine `RUN` commands and clean up caches to keep images small.
- Add a `.dockerignore` file to skip unneeded files.
- Never put passwords in a Dockerfile; use environment variables or a `.env` file.
- Use one main process per container.

## Real-World Uses

- Running web apps with their databases locally
- Sharing identical dev environments across a team
- Testing apps in a clean environment
- Deploying to servers and cloud platforms
- Running CI/CD pipelines