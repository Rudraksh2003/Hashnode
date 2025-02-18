---
title: "Docker Cheat Sheet 🐳"
datePublished: Tue Feb 18 2025 17:48:16 GMT+0000 (Coordinated Universal Time)
cuid: cm7as2jho000009jjahfu1hs2
slug: docker-cheat-sheet
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1739900768356/9a7750e7-120f-48b9-9f4d-ed7f1279ff7c.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1739900881265/689d9463-037e-44d5-b91a-2721e0f31cad.jpeg
tags: docker, development, developer, devops, mern, cheatsheet, dockerfile, docker-compose, docker-images, devops-articles, techwithrudraksh

---

🔹 **Docker Basics**

| Command | Description |
| --- | --- |
| `docker --version` | Check Docker version |
| `docker info` | Display system-wide information |
| `docker help` | Show help for Docker commands |

---

## 🔹 **Working with Containers**

| Command | Description |
| --- | --- |
| `docker run <image>` | Run a container from an image |
| `docker run -d <image>` | Run a container in detached mode (background) |
| `docker run -it <image> bash` | Run a container interactively with a shell |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped ones) |
| `docker start <container>` | Start a stopped container |
| `docker stop <container>` | Stop a running container |
| `docker restart <container>` | Restart a container |
| `docker kill <container>` | Kill a running container immediately |
| `docker rm <container>` | Remove a stopped container |
| `docker rm -f <container>` | Force remove a running container |
| `docker logs <container>` | Show logs of a container |
| `docker exec -it <container> bash` | Access a running container's shell |
| `docker inspect <container>` | Get detailed information about a container |
| `docker stats` | Show resource usage of running containers |
| `docker rename <old> <new>` | Rename a container |
| `docker pause <container>` | Pause a running container |
| `docker unpause <container>` | Unpause a paused container |

---

## 🔹 **Working with Images**

| Command | Description |
| --- | --- |
| `docker images` | List all local images |
| `docker pull <image>` | Download an image from Docker Hub |
| `docker push <image>` | Upload an image to Docker Hub |
| `docker rmi <image>` | Remove an image |
| `docker tag <image> <new_image>` | Rename/tag an image |
| `docker history <image>` | Show history of an image |
| `docker save -o image.tar <image>` | Save an image to a tar file |
| `docker load -i image.tar` | Load an image from a tar file |
| `docker commit <container> <new_image>` | Create an image from a container |

---

## 🔹 **Docker Volumes (Storage Management)**

| Command | Description |
| --- | --- |
| `docker volume create <volume>` | Create a new volume |
| `docker volume ls` | List all volumes |
| `docker volume inspect <volume>` | Get details of a volume |
| `docker volume rm <volume>` | Remove a volume |
| `docker run -v <volume>:/path <image>` | Mount a volume inside a container |

---

## 🔹 **Docker Networks**

| Command | Description |
| --- | --- |
| `docker network ls` | List all networks |
| `docker network create <network>` | Create a new network |
| `docker network inspect <network>` | Get details of a network |
| `docker network connect <network> <container>` | Connect a container to a network |
| `docker network disconnect <network> <container>` | Disconnect a container from a network |
| `docker network rm <network>` | Remove a network |

---

## 🔹 **Building Custom Images (Dockerfile)**

| Command | Description |
| --- | --- |
| `docker build -t <image_name> .` | Build an image from a Dockerfile in the current directory |
| `docker build -t <image_name> -f <Dockerfile>` | Build an image using a specific Dockerfile |
| `docker run --rm <image>` | Run a container and remove it after exiting |

**Basic Dockerfile Example:**

```dockerfile
# Use an official image as a base
FROM node:16

# Set the working directory
WORKDIR /app

# Copy files and install dependencies
COPY package.json .
RUN npm install

# Copy the rest of the app
COPY . .

# Expose a port and run the app
EXPOSE 3000
CMD ["node", "server.js"]
```

---

## 🔹 **Docker Compose**

| Command | Description |
| --- | --- |
| `docker-compose up` | Start services in the `docker-compose.yml` file |
| `docker-compose up -d` | Start services in detached mode |
| `docker-compose down` | Stop and remove all containers from `docker-compose.yml` |
| `docker-compose ps` | List running services |
| `docker-compose restart` | Restart all services |
| `docker-compose logs` | View logs of all services |
| `docker-compose exec <service> bash` | Open a shell inside a service container |

**Basic** `docker-compose.yml` Example:

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
```

---

## 🔹 **Managing Docker System Resources**

| Command | Description |
| --- | --- |
| `docker system df` | Show Docker disk usage |
| `docker system prune` | Remove unused data (stopped containers, networks, images) |
| `docker container prune` | Remove all stopped containers |
| `docker image prune` | Remove unused images |
| `docker volume prune` | Remove unused volumes |
| `docker network prune` | Remove unused networks |

---

## 🔹 **Security & User Management**

| Command | Description |
| --- | --- |
| `docker login` | Log in to Docker Hub |
| `docker logout` | Log out from Docker Hub |
| `docker secret create <name> <file>` | Create a secret from a file |
| `docker secret ls` | List all secrets |
| `docker secret rm <name>` | Remove a secret |

---

## 🔹 **Docker Swarm (Orchestration)**

| Command | Description |
| --- | --- |
| `docker swarm init` | Initialize a Swarm cluster |
| `docker swarm join --token <token> <manager-IP>` | Join a Swarm cluster |
| `docker swarm leave` | Leave a Swarm cluster |
| `docker service create --name web -p 80:80 nginx` | Create a service in Swarm |
| `docker service ls` | List all services |
| `docker service rm <service>` | Remove a service |
| `docker node ls` | List all nodes in the Swarm |

---

## 🔹 **Kubernetes with Docker**

| Command | Description |
| --- | --- |
| `kubectl get pods` | List all running pods |
| `kubectl get services` | List all services |
| `kubectl logs <pod>` | View logs of a pod |
| `kubectl exec -it <pod> -- bash` | Access a pod’s shell |
| `kubectl apply -f <file>.yaml` | Apply a Kubernetes configuration |

---

[Linux Cheatsheet](https://rudrakshladdha.hashnode.dev/linux-command-cheat-sheet)