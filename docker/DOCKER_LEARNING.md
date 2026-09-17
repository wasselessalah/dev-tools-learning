# Docker Documentation

A practical beginner-friendly guide to learning and using Docker.

---

## 📚 Table of Contents

1. [What is Docker?](#-what-is-docker)
2. [Why Use Docker?](#-why-use-docker)
3. [Docker Architecture](#-docker-architecture)
4. [Docker vs Virtual Machine](#-docker-vs-virtual-machine)
5. [Installing Docker](#-installing-docker)
6. [Verify Docker Installation](#-verify-docker-installation)
7. [Docker Terminology](#-docker-terminology)
8. [Docker Images](#-docker-images)
9. [Docker Containers](#-docker-containers)
10. [Basic Docker Commands](#-basic-docker-commands)
11. [Working with Containers](#-working-with-containers)
12. [Dockerfile](#-dockerfile)
13. [Build a Docker Image](#-build-a-docker-image)
14. [Docker Ports](#-docker-ports)
15. [Docker Volumes](#-docker-volumes)
16. [Docker Networks](#-docker-networks)
17. [Docker Compose](#-docker-compose)
18. [Node.js + Docker Example](#-nodejs--docker-example)
19. [Environment Variables](#-environment-variables)
20. [Docker Registry](#-docker-registry)
21. [Docker Hub](#-docker-hub)
22. [Useful Commands](#-useful-commands)
23. [Debugging](#-debugging)
24. [Best Practices](#-docker-best-practices)
25. [Common Problems](#-common-problems)
26. [Learning Roadmap](#-docker-learning-roadmap)

---

# 🐳 What is Docker?

Docker is a platform used to **build, package, distribute, and run applications in containers**.

A container packages an application together with the dependencies it needs to run.

For example, instead of installing:

* Node.js
* npm packages
* PostgreSQL
* Redis
* system dependencies

directly on your computer, Docker can run these services in isolated containers.

### Simple idea

Without Docker:

```text
Your Computer
│
├── Node.js
├── npm
├── PostgreSQL
├── Redis
└── Application
```

With Docker:

```text
Your Computer
│
└── Docker
    │
    ├── Node.js Container
    ├── PostgreSQL Container
    ├── Redis Container
    └── Application Container
```

---

# 🚀 Why Use Docker?

Docker helps developers create consistent environments.

## Main advantages

### 1. Consistency

The application runs in the same environment on:

```text
Developer PC
      ↓
Testing
      ↓
CI/CD
      ↓
Production
```

### 2. Isolation

Each container can have its own:

* dependencies
* environment variables
* filesystem
* network
* processes

### 3. Easy setup

Instead of manually installing many services:

```bash
npm install
```

```bash
sudo apt install postgresql
```

```bash
sudo apt install redis
```

You can start the environment with:

```bash
docker compose up
```

### 4. Reproducibility

A Dockerfile describes how an application environment should be created.

### 5. Deployment

Docker makes it easier to move applications between environments.

---

# 🏗️ Docker Architecture

Docker uses a client-server architecture.

```text
Docker CLI
    │
    │ docker build
    │ docker run
    │ docker ps
    ↓
Docker Engine
    │
    ├── Images
    ├── Containers
    ├── Networks
    └── Volumes
```

## Docker CLI

The CLI is the command-line interface.

Example:

```bash
docker ps
```

## Docker Engine

The Docker Engine manages:

* containers
* images
* networks
* volumes

---

# 🆚 Docker vs Virtual Machine

Docker containers and virtual machines are different.

## Virtual Machine

```text
Computer
│
├── Host OS
│
├── VM
│   ├── Guest OS
│   └── Application
│
└── VM
    ├── Guest OS
    └── Application
```

Each VM contains a complete operating system.

## Docker

```text
Computer
│
├── Host OS
│
└── Docker Engine
    ├── Container
    ├── Container
    └── Container
```

Containers share the host operating system kernel.

### General difference

| Docker Container       | Virtual Machine            |
| ---------------------- | -------------------------- |
| Lightweight            | Heavier                    |
| Starts quickly         | Usually slower             |
| Shares host kernel     | Has its own guest OS       |
| Uses fewer resources   | Uses more resources        |
| Good for microservices | Good for full OS isolation |

---

# 💻 Installing Docker

## Ubuntu

Update packages:

```bash
sudo apt update
```

Install Docker:

```bash
sudo apt install docker.io docker-compose-v2
```

Start Docker:

```bash
sudo systemctl start docker
```

Enable Docker at startup:

```bash
sudo systemctl enable docker
```

Check status:

```bash
sudo systemctl status docker
```

---

# ✅ Verify Docker Installation

Check Docker version:

```bash
docker --version
```

Example:

```text
Docker version 28.x.x
```

Check Docker information:

```bash
docker info
```

Run the official test container:

```bash
docker run hello-world
```

If Docker is working correctly, Docker will download the image and run the container.

---

# 📦 Docker Terminology

Understanding these terms is important.

## Image

An image is a **template** used to create containers.

Example:

```text
node:22
postgres:17
redis:8
nginx:latest
```

## Container

A container is a **running instance of an image**.

```text
Image
  ↓
Container
```

One image can create multiple containers.

```text
Node Image
   │
   ├── Container 1
   ├── Container 2
   └── Container 3
```

## Dockerfile

A Dockerfile contains instructions for creating an image.

Example:

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["npm", "start"]
```

## Volume

A volume stores persistent data outside the container lifecycle.

Useful for:

* databases
* uploaded files
* application data

## Network

A Docker network allows containers to communicate with each other.

---

# 🖼️ Docker Images

List local images:

```bash
docker images
```

Or:

```bash
docker image ls
```

Pull an image:

```bash
docker pull nginx
```

Pull a specific version:

```bash
docker pull node:22
```

Remove an image:

```bash
docker rmi nginx
```

Inspect an image:

```bash
docker image inspect nginx
```

---

# 📦 Docker Containers

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Create and run a container:

```bash
docker run nginx
```

Run in the background:

```bash
docker run -d nginx
```

Give a container a name:

```bash
docker run -d --name my-nginx nginx
```

---

# 🛠️ Basic Docker Commands

## Start a container

```bash
docker start my-nginx
```

## Stop a container

```bash
docker stop my-nginx
```

## Restart a container

```bash
docker restart my-nginx
```

## Remove a container

```bash
docker rm my-nginx
```

## Force remove a running container

```bash
docker rm -f my-nginx
```

## View container logs

```bash
docker logs my-nginx
```

Follow logs:

```bash
docker logs -f my-nginx
```

## Inspect a container

```bash
docker inspect my-nginx
```

---

# 🖥️ Working with Containers

## Execute a command inside a container

```bash
docker exec my-nginx ls
```

Open a shell:

```bash
docker exec -it my-nginx sh
```

For Ubuntu-based containers:

```bash
docker exec -it my-container bash
```

The `-it` option gives you an interactive terminal.

---

# 📄 Dockerfile

A Dockerfile describes how to build a Docker image.

Example:

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## Explanation

### FROM

Defines the base image.

```dockerfile
FROM node:22
```

### WORKDIR

Defines the working directory.

```dockerfile
WORKDIR /app
```

### COPY

Copies files into the image.

```dockerfile
COPY package*.json ./
```

### RUN

Runs a command while building the image.

```dockerfile
RUN npm install
```

### EXPOSE

Documents the port used by the application.

```dockerfile
EXPOSE 3000
```

### CMD

Defines the default command when the container starts.

```dockerfile
CMD ["npm", "start"]
```

---

# 🏗️ Build a Docker Image

Suppose your project contains:

```text
my-app/
│
├── Dockerfile
├── package.json
├── package-lock.json
└── src/
```

Build the image:

```bash
docker build -t my-app .
```

Explanation:

```text
docker build
    │
    ├── -t my-app → image name
    │
    └── . → current directory
```

List images:

```bash
docker images
```

Run the image:

```bash
docker run my-app
```

---

# 🔌 Docker Ports

Containers have their own network environment.

If your application listens on:

```text
3000
```

inside the container, you can expose it to your computer:

```bash
docker run -p 3000:3000 my-app
```

Format:

```text
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
docker run -p 8080:3000 my-app
```

Now:

```text
localhost:8080
      ↓
Container:3000
```

---

# 💾 Docker Volumes

Containers are temporary by default.

If a container is deleted, data stored inside it can disappear.

Volumes provide persistent storage.

Create a volume:

```bash
docker volume create my-data
```

List volumes:

```bash
docker volume ls
```

Inspect:

```bash
docker volume inspect my-data
```

Run PostgreSQL with a volume:

```bash
docker run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=password \
  -v postgres-data:/var/lib/postgresql/data \
  postgres
```

The database data is stored in:

```text
postgres-data
```

instead of only inside the container.

---

# 🌐 Docker Networks

Containers can communicate using Docker networks.

Create a network:

```bash
docker network create app-network
```

Run a container on the network:

```bash
docker run -d \
  --name database \
  --network app-network \
  postgres
```

Run another container:

```bash
docker run -d \
  --name backend \
  --network app-network \
  my-backend
```

The backend can communicate with PostgreSQL using:

```text
database
```

instead of:

```text
localhost
```

### Important

Inside a Docker container:

```text
localhost
```

means **the current container**, not another container.

---

# 🧩 Docker Compose

Docker Compose allows you to define multiple services in one file.

Instead of running:

```bash
docker run ...
docker run ...
docker run ...
```

you can define everything in:

```text
compose.yaml
```

Example:

```yaml
services:

  backend:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - database

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

Start the project:

```bash
docker compose up
```

Start in background:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down
```

Rebuild:

```bash
docker compose up --build
```

View services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs
```

Follow logs:

```bash
docker compose logs -f
```

---

# 🟢 Node.js + Docker Example

Let's create a simple Node.js application.

## Project

```text
node-docker/
│
├── Dockerfile
├── package.json
├── package-lock.json
└── server.js
```

## server.js

```javascript
const http = require("http");

const PORT = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, {
    "Content-Type": "text/plain",
  });

  res.end("Hello from Docker!");
});

server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Dockerfile

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

Build:

```bash
docker build -t node-docker .
```

Run:

```bash
docker run -p 3000:3000 node-docker
```

Open:

```text
http://localhost:3000
```

You should see:

```text
Hello from Docker!
```

---

# ⚙️ Environment Variables

Environment variables are commonly used for configuration.

Example:

```bash
docker run \
  -e NODE_ENV=production \
  -e PORT=3000 \
  my-app
```

Inside Node.js:

```javascript
console.log(process.env.NODE_ENV);
console.log(process.env.PORT);
```

You can also use an `.env` file.

Example:

```text
NODE_ENV=production
PORT=3000
DATABASE_URL=postgresql://...
```

With Compose:

```yaml
services:

  backend:
    build: .
    env_file:
      - .env
```

### Security

Do not commit secrets such as:

```text
DATABASE_PASSWORD
JWT_SECRET
API_KEY
AWS_SECRET_ACCESS_KEY
```

to Git.

---

# 🐳 Docker Registry

A Docker registry stores Docker images.

Common workflow:

```text
Developer
   │
   │ docker build
   ↓
Docker Image
   │
   │ docker push
   ↓
Registry
   │
   │ docker pull
   ↓
Production Server
```

Examples of registries:

* Docker Hub
* GitHub Container Registry
* Amazon ECR
* Google Artifact Registry
* Azure Container Registry

---

# 🌐 Docker Hub

Docker Hub is a public registry for Docker images.

Login:

```bash
docker login
```

Tag an image:

```bash
docker tag my-app username/my-app:latest
```

Push:

```bash
docker push username/my-app:latest
```

Pull:

```bash
docker pull username/my-app:latest
```

Run:

```bash
docker run username/my-app:latest
```

---

# 🧹 Useful Docker Commands

## Remove stopped containers

```bash
docker container prune
```

## Remove unused images

```bash
docker image prune
```

## Remove unused resources

```bash
docker system prune
```

Be careful with cleanup commands.

## Show disk usage

```bash
docker system df
```

## List networks

```bash
docker network ls
```

## List volumes

```bash
docker volume ls
```

---

# 🔍 Debugging Docker

## Check container status

```bash
docker ps -a
```

## Check logs

```bash
docker logs container-name
```

## Follow logs

```bash
docker logs -f container-name
```

## Inspect container

```bash
docker inspect container-name
```

## Enter container

```bash
docker exec -it container-name sh
```

## Check processes

```bash
docker top container-name
```

## Check resource usage

```bash
docker stats
```

---

# ❌ Common Problems

## Problem 1: Port already in use

Error:

```text
Bind for 0.0.0.0:3000 failed:
port is already allocated
```

Check:

```bash
docker ps
```

You can use another host port:

```bash
docker run -p 3001:3000 my-app
```

---

## Problem 2: Container exits immediately

Check:

```bash
docker ps -a
```

Then:

```bash
docker logs container-name
```

The application may have crashed.

---

## Problem 3: Cannot connect to PostgreSQL

Do not use:

```text
localhost
```

from the backend container.

Use the Compose service name:

```text
database
```

Example:

```text
postgresql://postgres:password@database:5432/myapp
```

---

## Problem 4: Changes are not visible

If you changed the Dockerfile or application dependencies, rebuild:

```bash
docker compose up --build
```

---

# 🔐 Docker Best Practices

## 1. Use specific image versions

Instead of:

```dockerfile
FROM node:latest
```

prefer:

```dockerfile
FROM node:22-alpine
```

This makes builds more predictable.

## 2. Use `.dockerignore`

Create:

```text
.dockerignore
```

Example:

```text
node_modules
.next
.git
.env
npm-debug.log
Dockerfile
docker-compose.yml
```

This prevents unnecessary files from being copied into the build context.

## 3. Don't run everything as root

For production applications, consider using a non-root user.

## 4. Keep images small

Use lightweight base images when appropriate.

Example:

```dockerfile
FROM node:22-alpine
```

## 5. Don't put secrets in Dockerfiles

Avoid:

```dockerfile
ENV API_KEY=secret123
```

Use environment variables or a proper secret-management system.

## 6. Use multi-stage builds

Example:

```dockerfile
FROM node:22 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build


FROM node:22-alpine

WORKDIR /app

COPY --from=builder /app ./

CMD ["npm", "start"]
```

Multi-stage builds can reduce the final image size and keep build-only dependencies out of the runtime image.

---

# 🧠 Docker Mental Model

Remember this relationship:

```text
Dockerfile
    │
    │ docker build
    ↓
Docker Image
    │
    │ docker run
    ↓
Docker Container
```

For multiple services:

```text
                 Docker Compose
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Backend     Database      Redis
       Container   Container     Container
```

---

# 🔄 Typical Docker Development Workflow

A common workflow is:

```bash
# 1. Create Dockerfile

# 2. Build image
docker build -t my-app .

# 3. Run container
docker run -p 3000:3000 my-app

# 4. Check containers
docker ps

# 5. Check logs
docker logs my-app

# 6. Stop
docker stop my-app

# 7. Remove
docker rm my-app
```

With Compose:

```bash
docker compose up --build
```

Then:

```bash
docker compose logs -f
```

Stop:

```bash
docker compose down
```

---

# 🏢 Example Full-Stack Architecture

A modern application might use:

```text
                    Internet
                       │
                       ↓
                    Nginx
                       │
              ┌────────┴────────┐
              ↓                 ↓
           Frontend           Backend
          Next.js             Node.js
              │                 │
              │          ┌──────┴──────┐
              │          ↓             ↓
              │      PostgreSQL      Redis
              │
              └─────────────────────────
```

Docker Compose can manage:

```text
frontend
backend
postgres
redis
nginx
```

---

# 📋 Important Commands Cheat Sheet

| Task                    | Command                     |
| ----------------------- | --------------------------- |
| Docker version          | `docker --version`          |
| Docker info             | `docker info`               |
| List images             | `docker images`             |
| Pull image              | `docker pull IMAGE`         |
| Build image             | `docker build -t NAME .`    |
| Run container           | `docker run IMAGE`          |
| Run background          | `docker run -d IMAGE`       |
| List running containers | `docker ps`                 |
| List all containers     | `docker ps -a`              |
| Stop container          | `docker stop NAME`          |
| Start container         | `docker start NAME`         |
| Restart container       | `docker restart NAME`       |
| Remove container        | `docker rm NAME`            |
| Remove image            | `docker rmi IMAGE`          |
| View logs               | `docker logs NAME`          |
| Enter container         | `docker exec -it NAME sh`   |
| Container stats         | `docker stats`              |
| List networks           | `docker network ls`         |
| List volumes            | `docker volume ls`          |
| Compose start           | `docker compose up`         |
| Compose background      | `docker compose up -d`      |
| Compose rebuild         | `docker compose up --build` |
| Compose stop            | `docker compose down`       |
| Compose logs            | `docker compose logs`       |

---

# 🗺️ Docker Learning Roadmap

## Level 1 — Fundamentals

Learn:

* What is Docker?
* Images
* Containers
* Docker Engine
* Docker CLI
* Docker Hub

Practice:

```bash
docker pull
docker run
docker ps
docker stop
docker rm
docker logs
```

---

## Level 2 — Dockerfile

Learn:

* `FROM`
* `WORKDIR`
* `COPY`
* `RUN`
* `EXPOSE`
* `CMD`
* `ENTRYPOINT`
* `.dockerignore`

Practice by Dockerizing:

```text
Node.js
Python
Next.js
Express
FastAPI
```

---

## Level 3 — Docker Compose

Learn:

* Services
* Ports
* Volumes
* Networks
* Environment variables
* `depends_on`
* Health checks

Build:

```text
Next.js
+
Node.js
+
PostgreSQL
+
Redis
```

---

## Level 4 — Production

Learn:

* Multi-stage builds
* Image optimization
* Security
* Non-root containers
* Health checks
* Logging
* Resource limits
* Docker registry
* CI/CD

---

## Level 5 — DevOps

After Docker, continue with:

```text
Docker
   ↓
Linux
   ↓
CI/CD
   ↓
GitHub Actions
   ↓
Docker Registry
   ↓
Cloud
   ↓
Kubernetes
   ↓
Terraform
   ↓
Observability
```

---

# 🎯 Practice Project

Build a full-stack application using:

```text
Frontend
Next.js

Backend
Node.js + Express

Database
PostgreSQL

Cache
Redis

Containerization
Docker

Orchestration
Docker Compose
```

Expected project:

```text
fullstack-docker/
│
├── frontend/
│   └── Dockerfile
│
├── backend/
│   └── Dockerfile
│
├── compose.yaml
│
├── .env
├── .dockerignore
└── README.md
```

Start everything with:

```bash
docker compose up --build
```

Your final architecture:

```text
                    Docker Compose
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
    Next.js            Express        PostgreSQL
    frontend            backend          database
                         │
                         ↓
                       Redis
```

---

# ✅ Final Checklist

Before considering yourself comfortable with Docker, you should be able to:

* [ ] Explain what Docker is
* [ ] Explain image vs container
* [ ] Install Docker
* [ ] Pull an image
* [ ] Run a container
* [ ] Stop/remove a container
* [ ] Read container logs
* [ ] Enter a container
* [ ] Write a Dockerfile
* [ ] Build an image
* [ ] Map ports
* [ ] Use environment variables
* [ ] Create volumes
* [ ] Create networks
* [ ] Use Docker Compose
* [ ] Dockerize a Node.js application
* [ ] Dockerize a Next.js application
* [ ] Run PostgreSQL with Docker
* [ ] Run Redis with Docker
* [ ] Push an image to a registry
* [ ] Debug common Docker problems
* [ ] Use Docker in CI/CD

---

# 📚 Summary

Docker provides a standardized way to package and run applications.

The most important concept is:

```text
Dockerfile
    ↓
Image
    ↓
Container
```

For multiple services:

```text
compose.yaml
      ↓
Docker Compose
      ↓
┌───────────────┐
│   Frontend    │
├───────────────┤
│   Backend     │
├───────────────┤
│   PostgreSQL  │
├───────────────┤
│   Redis       │
└───────────────┘
```

The best way to learn Docker is to **use it in real projects**, not only memorize commands.
