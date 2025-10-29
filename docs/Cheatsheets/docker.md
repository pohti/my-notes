# Docker

A complete reference for working with Docker — images, containers, networks, volumes, and best practices.

---

## ⚙️ Basics

### What is Docker?
- Docker is a **containerization platform** that packages applications and dependencies into **containers**.
- Containers are **lightweight, isolated environments** that run the same regardless of host OS.

---

## 📦 Common Commands

| Command | Description |
|----------|--------------|
| `docker version` | Show Docker version info |
| `docker info` | Show system-wide information |
| `docker help` | List commands or get help for a command |

---

## 🧱 Images

| Command | Description |
|----------|--------------|
| `docker images` | List local images |
| `docker pull <image>` | Download image from Docker Hub |
| `docker rmi <image>` | Remove image |
| `docker build -t <name>:<tag> .` | Build image from Dockerfile |
| `docker tag <image> <repo>:<tag>` | Tag image |
| `docker push <repo>:<tag>` | Push image to registry |

### Example:
```bash
docker build -t myapp:latest .
docker push myusername/myapp:latest
````

---

## 🧩 Containers

| Command                            | Description                             |
| ---------------------------------- | --------------------------------------- |
| `docker ps`                        | List running containers                 |
| `docker ps -a`                     | List all containers (including stopped) |
| `docker run <image>`               | Run a new container                     |
| `docker run -it <image> bash`      | Run interactively                       |
| `docker start <container>`         | Start existing container                |
| `docker stop <container>`          | Stop container                          |
| `docker restart <container>`       | Restart container                       |
| `docker rm <container>`            | Remove container                        |
| `docker logs <container>`          | Show logs                               |
| `docker exec -it <container> bash` | Run command inside running container    |
| `docker inspect <container>`       | Detailed info (JSON)                    |

### Example:

```bash
docker run -d -p 8080:80 --name web nginx
docker exec -it web bash
```

---

## 🗂️ Volumes (Data Persistence)

| Command                        | Description    |
| ------------------------------ | -------------- |
| `docker volume create <name>`  | Create volume  |
| `docker volume ls`             | List volumes   |
| `docker volume inspect <name>` | Inspect volume |
| `docker volume rm <name>`      | Remove volume  |

### Mounting volumes:

```bash
docker run -v mydata:/var/lib/mysql mysql
```

### Bind mount (host ↔ container):

```bash
docker run -v $(pwd)/app:/usr/src/app node
```

---

## 🌐 Networks

| Command                         | Description                 |
| ------------------------------- | --------------------------- |
| `docker network ls`             | List networks               |
| `docker network create <name>`  | Create network              |
| `docker network inspect <name>` | Inspect network             |
| `docker network rm <name>`      | Remove network              |
| `docker run --network <name>`   | Attach container to network |

### Example:

```bash
docker network create mynet
docker run -d --network mynet --name db postgres
docker run -d --network mynet --name api myapp
```

---

## 🧰 Dockerfile

A **Dockerfile** defines how an image is built.

### Example

```dockerfile
# Use base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy files
COPY package*.json ./
RUN npm install

COPY . .

# Expose port and run command
EXPOSE 3000
CMD ["npm", "start"]
```

### Build and run:

```bash
docker build -t myapp .
docker run -p 3000:3000 myapp
```

---

## ⚡ Multi-Stage Builds

Reduce image size by building and running in separate stages.

```dockerfile
# Stage 1 - Build
FROM node:18 as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Stage 2 - Run
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
```

---

## 🧩 Environment Variables

```bash
docker run -e NODE_ENV=production myapp
```

### From `.env` file

```bash
docker run --env-file .env myapp
```

---

## 🧍 Docker Compose

`docker-compose.yml` allows managing multiple containers easily.

### Example

```yaml
version: "3.9"
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```

### Commands

| Command                | Description                |
| ---------------------- | -------------------------- |
| `docker compose up`    | Start all services         |
| `docker compose up -d` | Detached mode              |
| `docker compose down`  | Stop and remove containers |
| `docker compose logs`  | Show logs                  |
| `docker compose ps`    | Show running services      |

---

## 🧼 Cleaning Up

| Command                  | Description               |
| ------------------------ | ------------------------- |
| `docker system prune`    | Remove unused data        |
| `docker container prune` | Remove stopped containers |
| `docker image prune`     | Remove unused images      |
| `docker volume prune`    | Remove unused volumes     |

### Example (aggressive cleanup)

```bash
docker system prune -a --volumes
```

---

## 🧠 Useful Tips

* Always use **.dockerignore** to speed up builds.
* Use **multi-stage builds** to keep images small.
* Prefer **named volumes** over anonymous ones for persistent data.
* Run containers as **non-root** where possible.
* Use `docker logs -f` for real-time log streaming.

---

## 🧾 Dockerfile Best Practices

✅ Use official base images
✅ Leverage caching (copy only needed files before `npm install`)
✅ Combine related `RUN` commands
✅ Clean up cache (`apt-get clean`, `rm -rf /var/lib/apt/lists/*`)
✅ Use lightweight images (e.g., `alpine`)

---

## 🧱 Example .dockerignore

```
node_modules
.git
.gitignore
Dockerfile
*.log
.env
```

---

## 📦 Pushing to Docker Hub

```bash
docker login
docker tag myapp username/myapp:latest
docker push username/myapp:latest
```

---

## 🧠 Troubleshooting

| Problem                     | Fix                                                     |
| --------------------------- | ------------------------------------------------------- |
| Container exits immediately | Add `-it` or correct entrypoint                         |
| Port already in use         | Stop existing service or use another port               |
| Permission denied           | Check file permissions or use `--user` flag             |
| DNS/network issues          | Restart Docker daemon (`sudo systemctl restart docker`) |

---

## 🧰 Useful Commands Summary

```bash
# Containers
docker ps -a
docker start|stop|rm <id>
docker exec -it <id> bash

# Images
docker build -t app .
docker rmi <id>

# Volumes
docker volume ls
docker volume rm <name>

# Networks
docker network ls
docker network create mynet

# Cleanup
docker system prune -a
```

---

## 🧾 References

* [Docker Docs](https://docs.docker.com/)
* [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
* [Docker Hub](https://hub.docker.com/)
* [Compose Reference](https://docs.docker.com/compose/)
