# Docker Basics

**Status:** 🔲 Not started
**Started:** —
**Completed:** —

---

## Overview

Docker is a platform for packaging and running applications in **containers** — lightweight, isolated environments that bundle the app and all its dependencies.

- **Image** — read-only blueprint (built from a Dockerfile)
- **Container** — running instance of an image
- **Registry** — stores and distributes images (e.g. Docker Hub)

Key benefit: "works on my machine" becomes "works everywhere" — same image runs identically in dev, staging, and prod.

---

## Sections

### 1. Introduction to Containers

- Containers package application code, runtime, libraries, and configuration into isolated units.
- They share the host OS kernel, so they are usually lighter and faster to start than virtual machines.
- Container images make environments repeatable across developer laptops, CI, staging, and production.
- Common workflow:
  - Pull or build an image.
  - Run a container from that image.
  - Inspect logs, exec into the container, stop/remove it when done.
- Containers should be treated as disposable. Store persistent state outside the container.

Useful commands:

```bash
docker version
docker info
docker run hello-world
docker ps
docker ps -a
```

### 2. Docker Architecture

- Docker uses a client-server model.
- The Docker client (`docker`) sends commands to the Docker daemon (`dockerd`).
- The daemon manages images, containers, networks, and volumes.
- Registries such as Docker Hub store and distribute images.
- Images are pulled from a registry, then used to create containers on a Docker host.

Core components:

- **Docker client** - command-line interface used by users and scripts.
- **Docker daemon** - background service that performs Docker operations.
- **Docker host** - machine running the daemon.
- **Registry** - remote or local image store.
- **Image** - immutable template.
- **Container** - running process created from an image.

Useful commands:

```bash
docker system info
docker system df
docker login
docker pull nginx
```

### 3. Images & Containers

- Images are layered, read-only templates.
- Containers add a writable layer on top of an image.
- Multiple containers can run from the same image.
- Container names must be unique on the same Docker host.
- Tags identify image versions, for example `nginx:1.27` or `redis:alpine`.
- Avoid relying on `latest` in production because it can change unexpectedly.

Useful commands:

```bash
docker images
docker pull nginx:alpine
docker run --name web -d -p 8080:80 nginx:alpine
docker logs web
docker exec -it web sh
docker stop web
docker rm web
docker rmi nginx:alpine
```

### 4. Dockerfile

- A Dockerfile defines how an image is built.
- Each instruction creates an image layer.
- Put slow-changing instructions near the top to make better use of build cache.
- Use `.dockerignore` to keep unnecessary files out of the build context.
- Prefer small base images when possible, but keep debugging and compatibility needs in mind.
- Use `CMD` for the default command; use `ENTRYPOINT` when the container should behave like a fixed executable.

Common instructions:

- `FROM` - base image.
- `WORKDIR` - default directory for following instructions.
- `COPY` - copy local files into the image.
- `RUN` - execute commands during image build.
- `ENV` - set environment variables.
- `EXPOSE` - document intended port.
- `CMD` - default command at container start.

Example:

```Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "app.py"]
```

Useful commands:

```bash
docker build -t my-app:1.0 .
docker run --rm my-app:1.0
docker history my-app:1.0
```

### 5. Volumes & Storage

- Container writable layers are temporary and disappear when the container is removed.
- Volumes persist data outside the container lifecycle.
- Bind mounts map a host path directly into a container.
- Volumes are managed by Docker and are the usual choice for persistent application data.
- Bind mounts are useful for local development because source files can be mounted into the container.

Storage options:

- **Writable layer** - temporary container filesystem changes.
- **Volume** - Docker-managed persistent storage.
- **Bind mount** - host path mounted into a container.
- **tmpfs mount** - memory-backed temporary storage.

Useful commands:

```bash
docker volume ls
docker volume create app-data
docker run -d --name db -v app-data:/var/lib/postgresql/data postgres:16
docker inspect app-data
docker volume rm app-data
```

### 6. Networking

- Docker creates networks so containers can communicate with each other and with the host.
- The default `bridge` network supports basic container networking.
- User-defined bridge networks provide built-in DNS by container name.
- Port publishing maps a host port to a container port, for example `-p 8080:80`.
- Containers on the same user-defined network can reach each other by container name.

Common network types:

- **bridge** - default single-host container networking.
- **host** - container shares the host network namespace.
- **none** - container has no network access.
- **overlay** - multi-host networking, mainly with Swarm.

Useful commands:

```bash
docker network ls
docker network create app-net
docker run -d --name api --network app-net my-api
docker run --rm --network app-net curlimages/curl http://api:8000
docker port api
docker network inspect app-net
```

### 7. Docker Compose

- Docker Compose defines multi-container applications in a YAML file.
- Services describe containers, images/build settings, ports, volumes, networks, and environment variables.
- Compose automatically creates a project network and DNS entries for services.
- Use Compose for local stacks such as app + database + cache.
- Compose file changes often require recreating containers with `docker compose up -d`.

Example:

```yaml
services:
  web:
    build: .
    ports:
      - "8080:8000"
    environment:
      DATABASE_URL: postgres://app:secret@db:5432/app
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: app
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

Useful commands:

```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose exec web sh
docker compose down
docker compose down -v
```

---

## Key Takeaways

- Images are immutable templates; containers are running instances.
- Containers are disposable, so persistent data belongs in volumes or external services.
- Dockerfiles should be cache-friendly and should keep images as small as practical.
- User-defined bridge networks make service-to-service communication easier.
- Docker Compose is the standard local workflow for multi-container applications.

## Commands Reference

```bash
# Images
docker pull IMAGE[:TAG]
docker images
docker build -t NAME:TAG .
docker rmi IMAGE

# Containers
docker run [OPTIONS] IMAGE [COMMAND]
docker ps
docker ps -a
docker logs CONTAINER
docker exec -it CONTAINER sh
docker stop CONTAINER
docker rm CONTAINER

# Volumes
docker volume ls
docker volume create NAME
docker volume inspect NAME
docker volume rm NAME

# Networks
docker network ls
docker network create NAME
docker network inspect NAME

# Compose
docker compose up -d
docker compose ps
docker compose logs -f
docker compose exec SERVICE sh
docker compose down
docker compose down -v
```

## Questions / Things to revisit

- Difference between `CMD` and `ENTRYPOINT` in real examples.
- When to choose a volume instead of a bind mount.
- How Docker layer caching changes build performance.
- Security basics: non-root users, secrets, image scanning, and trusted base images.
