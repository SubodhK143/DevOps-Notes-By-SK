# Docker Command Cheat Sheet

> A practical Docker command reference for developers and DevOps engineers. This cheat sheet covers essential Docker commands, Dockerfile instructions, Docker Compose, networking, volumes, cleanup, best practices, and common interview concepts.

---

## Docker Concepts at a Glance

| Concept | Description |
|---------|-------------|
| **Image** | A read-only blueprint used to create containers. |
| **Container** | A running instance of a Docker image. |
| **Volume** | Persistent storage that survives container removal. |
| **Network** | Enables communication between containers. |
| **Dockerfile** | A file containing instructions to build Docker images. |
| **Docker Compose** | A tool for defining and managing multi-container applications. |
| **Registry** | Stores Docker images (Docker Hub, Amazon ECR, Google GCR, Azure ACR). |

---

## Table of Contents

- [Docker Concepts at a Glance](#docker-concepts-at-a-glance)
- [Docker Installation & Version](#docker-installation--version)
- [Container Commands](#container-commands)
- [Container Management](#container-management)
- [Logs & Inspection](#logs--inspection)
- [Execute Inside Container](#execute-inside-container)
- [Docker Images](#docker-images)
- [Docker Hub](#docker-hub)
- [Docker Volumes](#docker-volumes)
- [Docker Networks](#docker-networks)
- [Run Containers on a Custom Network](#run-containers-on-a-custom-network)
- [Docker Compose](#docker-compose)
- [Dockerfile Instructions](#dockerfile-instructions)
- [Copy Files](#copy-files)
- [Cleanup Commands](#cleanup-commands)
- [Useful One-Liners](#useful-one-liners)
- [Docker Workflow](#docker-workflow)
- [Docker Best Practices](#docker-best-practices)
- [Frequently Asked Interview Questions](#frequently-asked-interview-questions)
- [Daily Docker Commands](#daily-docker-commands)
- [Author](#author)
---

## Docker Installation & Version

| Task | Command |
|------|---------|
| Docker version | `docker --version` |
| Docker Compose version | `docker compose version` |
| Docker system information | `docker info` |

---

## Container Commands

| Task | Command |
|------|---------|
| Run a container | `docker run nginx` |
| Run in detached mode | `docker run -d nginx` |
| Run with custom name | `docker run --name web nginx` |
| Publish port | `docker run -p 8080:80 nginx` |
| Interactive terminal | `docker run -it ubuntu bash` |
| Auto-remove after exit | `docker run --rm ubuntu` |
| Set restart policy | `docker run --restart=always nginx` |
| Set environment variable | `docker run -e APP_ENV=production app` |
| Bind mount local directory | `docker run -v $(pwd):/app app` |
| Attach named volume | `docker run -v app-data:/data app` |

---

## Container Management

| Task | Command |
|------|---------|
| List running containers | `docker ps` |
| List all containers | `docker ps -a` |
| Show only container IDs | `docker ps -q` |
| Stop container | `docker stop <container>` |
| Start container | `docker start <container>` |
| Restart container | `docker restart <container>` |
| Pause container | `docker pause <container>` |
| Resume container | `docker unpause <container>` |
| Kill container | `docker kill <container>` |
| Remove container | `docker rm <container>` |
| Force remove container | `docker rm -f <container>` |

---

## Logs & Inspection

| Task | Command |
|------|---------|
| View logs | `docker logs <container>` |
| Follow logs | `docker logs -f <container>` |
| Last 100 log lines | `docker logs --tail 100 <container>` |
| Inspect container | `docker inspect <container>` |
| Running processes | `docker top <container>` |
| Resource usage | `docker stats` |
| View port mappings | `docker port <container>` |
| Filesystem changes | `docker diff <container>` |

---

## Execute Inside Container

| Task | Command |
|------|---------|
| Open Bash shell | `docker exec -it <container> bash` |
| Open sh shell | `docker exec -it <container> sh` |
| Execute a single command | `docker exec <container> ls -la` |

---

## Docker Images

| Task | Command |
|------|---------|
| Pull image | `docker pull nginx` |
| Build image | `docker build -t my-app .` |
| Build without cache | `docker build --no-cache -t my-app .` |
| Remove image | `docker rmi <image>` |
| Remove unused images | `docker image prune` |
| Remove all unused images | `docker image prune -a` |
| Inspect image | `docker image inspect <image>` |
| Show image history | `docker history <image>` |

---

## Docker Hub

| Task | Command |
|------|---------|
| Log in to Docker Hub | `docker login` |
| Log out from Docker Hub | `docker logout` |
| Search for an image | `docker search nginx` |
| Pull an image | `docker pull nginx:latest` |
| Tag an image | `docker tag my-app:latest username/my-app:v1` |
| Push an image | `docker push username/my-app:v1` |

---

## Docker Volumes

| Task | Command |
|------|---------|
| List volumes | `docker volume ls` |
| Create volume | `docker volume create app-data` |
| Inspect volume | `docker volume inspect app-data` |
| Remove volume | `docker volume rm app-data` |
| Remove unused volumes | `docker volume prune` |

---

## Docker Networks

| Task | Command |
|------|---------|
| List networks | `docker network ls` |
| Create network | `docker network create app-net` |
| Create bridge network | `docker network create --driver bridge app-net` |
| Inspect network | `docker network inspect app-net` |
| Connect container | `docker network connect app-net web` |
| Disconnect container | `docker network disconnect app-net web` |
| Remove network | `docker network rm app-net` |
| Remove unused networks | `docker network prune` |

---

## Run Containers on a Custom Network

| Task | Command |
|------|---------|
| Run container on network | `docker run --network app-net nginx` |

---

## Docker Compose

| Task | Command |
|------|---------|
| Start services | `docker compose up` |
| Start in detached mode | `docker compose up -d` |
| Build and start | `docker compose up --build` |
| Stop services | `docker compose stop` |
| Restart services | `docker compose restart` |
| Remove services | `docker compose down` |
| Remove services and volumes | `docker compose down -v` |
| List services | `docker compose ps` |
| View logs | `docker compose logs` |
| Follow logs | `docker compose logs -f` |
| View logs for one service | `docker compose logs backend` |

---

## Dockerfile Instructions

| Instruction | Purpose |
|------------|---------|
| `FROM` | Defines the base image. |
| `WORKDIR` | Sets the working directory. |
| `COPY` | Copies local files into the image. |
| `ADD` | Copies files or extracts archives (use only when required). |
| `RUN` | Executes commands during image build. |
| `ENV` | Defines environment variables. |
| `EXPOSE` | Documents the container port. |
| `USER` | Specifies the user for subsequent instructions. |
| `CMD` | Provides the default command executed at container startup. |
| `ENTRYPOINT` | Defines the main executable for the container. |
| `.dockerignore` | Excludes unnecessary files from the build context. |

---

## Copy Files

| Task | Command |
|------|---------|
| Copy from host to container | `docker cp app.py web:/app` |
| Copy from container to host | `docker cp web:/app/log.txt .` |

---

## Cleanup Commands

| Task | Command |
|------|---------|
| Remove stopped containers | `docker container prune` |
| Remove unused images | `docker image prune` |
| Remove unused volumes | `docker volume prune` |
| Remove unused networks | `docker network prune` |
| Remove unused resources | `docker system prune` |
| Remove all unused images | `docker system prune -a` |
| Remove all unused resources and volumes | `docker system prune -a --volumes` |
| View Docker disk usage | `docker system df` |

---

## Useful One-Liners

### Stop all running containers

```bash
docker stop $(docker ps -q)
```

### Remove all containers

```bash
docker rm $(docker ps -aq)
```

### Remove all images

```bash
docker rmi $(docker images -q)
```

### Remove dangling images

```bash
docker image prune
```

### Perform a complete cleanup

```bash
docker system prune -a --volumes
```

---

## Docker Workflow

```text
Dockerfile
      │
      ▼
docker build
      │
      ▼
Docker Image
      │
      ▼
docker run
      │
      ▼
Container
      │
      ├── docker exec
      ├── docker logs
      ├── docker inspect
      ├── docker stop
      └── docker rm
```

---

## Daily Docker Commands

```bash
docker ps
docker images
docker build -t my-app .
docker run -d -p 8080:80 my-app
docker logs -f <container>
docker exec -it <container> bash
docker stop <container>
docker rm <container>
docker system df
```
---

## Docker Best Practices

- Use lightweight base images (Alpine or Slim).
- Prefer multi-stage builds for production images.
- Use `.dockerignore` to reduce build context.
- Store persistent data in Docker Volumes.
- Avoid running containers as the root user.
- Prefer `COPY` over `ADD` unless additional features are required.

---

## Frequently Asked Interview Questions

### 1. Image vs Container
An image is a read-only template. A container is a running instance of that image.

### 2. CMD vs ENTRYPOINT
CMD provides default arguments, while ENTRYPOINT defines the main executable.

### 3. COPY vs ADD
COPY copies local files. ADD also supports extracting archives and downloading URLs.

### 4. docker compose down vs docker compose down -v
The `-v` flag removes associated volumes in addition to containers and networks.

### 5. What is the difference between a Volume and a Bind Mount?
- **Volume:** Managed by Docker and ideal for persistent application data.
- **Bind Mount:** Maps a host directory directly into the container and is commonly used during development.

### 6. Difference between EXPOSE and -p
- `EXPOSE` documents the container port.
- `-p` publishes the port to the host machine.

### 7. What is Docker Hub?
Docker Hub is a cloud-based container registry used to store, share, and distribute Docker images. It allows developers to push custom images and pull them on any machine.

---

## Quick Notes

- Containers are ephemeral.
- Volumes preserve data.
- Images are immutable.
- One container should ideally run one main process.
- Docker Compose is used for multi-container applications.

---

# 👩‍💻 Author

**Jaishree Chaure**

**AWS Certified Cloud Practitioner • Aspiring DevOps Engineer**

Documented as part of the **#90DaysOfDevOps** challenge (UDAAN Batch 11).

⭐ If you found this cheat sheet helpful, consider **starring the repository**.
