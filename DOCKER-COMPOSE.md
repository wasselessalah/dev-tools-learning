# 🧩 Docker Compose Documentation

Docker Compose is a tool for defining and running **multi-container applications** using a YAML configuration file.

Instead of manually running multiple Docker containers, you define your services in a `compose.yaml` file and manage the entire application with simple commands.

---

# 📚 Table of Contents

1. [What is Docker Compose?](#-what-is-docker-compose)
2. [Why Use Docker Compose?](#-why-use-docker-compose)
3. [Docker Compose File](#-docker-compose-file)
4. [Basic Structure](#-basic-structure)
5. [Services](#-services)
6. [Images](#-images)
7. [Build](#-build)
8. [Ports](#-ports)
9. [Environment Variables](#-environment-variables)
10. [Volumes](#-volumes)
11. [Networks](#-networks)
12. [Depends On](#-depends-on)
13. [Health Checks](#-health-checks)
14. [Restart Policies](#-restart-policies)
15. [Docker Compose Commands](#-docker-compose-commands)
16. [Node.js Example](#-nodejs-example)
17. [Next.js + PostgreSQL Example](#-nextjs--postgresql-example)
18. [Full-Stack Example](#-full-stack-example)
19. [Development vs Production](#-development-vs-production)
20. [Common Problems](#-common-problems)
21. [Best Practices](#-best-practices)
22. [Learning Projects](#-learning-projects)

---

# 🐳 What is Docker Compose?

Docker Compose allows you to define multiple containers in a single YAML file.

For example:

```text
                 Docker Compose
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       Frontend      Backend     PostgreSQL
       Next.js       Node.js       Database
                        │
                        ↓
                      Redis
```

Instead of starting each container separately, you can run:

```bash
docker compose up
```

---

# 🚀 Why Use Docker Compose?

Without Compose, you might need:

```bash
docker run ...
docker run ...
docker run ...
docker network create ...
docker volume create ...
```

With Compose:

```bash
docker compose up
```

Compose automatically manages:

* Containers
* Networks
* Volumes
* Environment variables
* Service configuration
* Dependencies

---

# 📄 Docker Compose File

The recommended modern filename is:

```text
compose.yaml
```

You may also see:

```text
compose.yml
docker-compose.yml
docker-compose.yaml
```

Example project:

```text
my-project/
│
├── compose.yaml
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│
└── .env
```

---

# 🧱 Basic Structure

A simple Compose file:

```yaml
services:

  backend:
    image: node:22-alpine
    ports:
      - "3000:3000"

  database:
    image: postgres:17
```

The main section is:

```yaml
services:
```

Each service represents a container.

---

# 📦 Services

A service is a container configuration.

Example:

```yaml
services:

  backend:
    image: node:22-alpine

  database:
    image: postgres:17

  redis:
    image: redis:8-alpine
```

This creates three services:

```text
backend
database
redis
```

Compose creates containers for these services.

---

# 🖼️ Images

You can use an existing Docker image:

```yaml
services:

  database:
    image: postgres:17
```

Another example:

```yaml
services:

  redis:
    image: redis:8-alpine
```

You can use images from Docker registries such as Docker Hub.

---

# 🏗️ Build

If you have your own Dockerfile:

```yaml
services:

  backend:
    build: ./backend
```

Project:

```text
project/
│
├── compose.yaml
│
└── backend/
    ├── Dockerfile
    ├── package.json
    └── src/
```

Docker Compose will use:

```text
backend/Dockerfile
```

to build the backend image.

---

# 🔌 Ports

Ports connect the host machine to the container.

```yaml
services:

  backend:
    ports:
      - "3000:3000"
```

Format:

```text
HOST_PORT:CONTAINER_PORT
```

Example:

```yaml
ports:
  - "8080:3000"
```

This means:

```text
Browser
   │
   ↓
localhost:8080
   │
   ↓
Container:3000
```

---

# ⚙️ Environment Variables

You can define environment variables directly:

```yaml
services:

  backend:
    environment:
      NODE_ENV: production
      PORT: 3000
```

Inside Node.js:

```javascript
console.log(process.env.NODE_ENV);
```

---

# 🔐 Using `.env`

Create:

```text
.env
```

Example:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_DB=myapp
```

Compose:

```yaml
services:

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
```

This keeps configuration separate from the Compose file.

> Do not commit production secrets to Git.

---

# 💾 Volumes

Volumes allow data to survive container recreation.

Example PostgreSQL:

```yaml
services:

  database:
    image: postgres:17
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

Architecture:

```text
PostgreSQL Container
        │
        ↓
postgres-data
        │
        ↓
Persistent Data
```

If the container is removed:

```bash
docker compose down
```

the named volume remains unless you explicitly remove it.

---

# 🌐 Networks

Compose automatically creates a network for your application.

Example:

```yaml
services:

  backend:
    image: my-backend

  database:
    image: postgres:17
```

The backend can connect to PostgreSQL using:

```text
database
```

For example:

```env
DATABASE_URL=postgresql://postgres:password@database:5432/myapp
```

Do **not** use:

```text
localhost
```

for container-to-container communication.

Remember:

```text
localhost
    ↓
Current container

database
    ↓
PostgreSQL service
```

---

# 🔗 Custom Networks

You can create a custom network:

```yaml
services:

  backend:
    build: ./backend
    networks:
      - app-network

  database:
    image: postgres:17
    networks:
      - app-network

networks:
  app-network:
```

Now:

```text
backend
   │
   │ app-network
   ↓
database
```

---

# ⛓️ Depends On

`depends_on` controls service startup order.

Example:

```yaml
services:

  backend:
    build: ./backend
    depends_on:
      - database

  database:
    image: postgres:17
```

Compose starts:

```text
database
   ↓
backend
```

### Important

`depends_on` controls startup order, but it does not necessarily mean the database is **ready to accept connections**.

For that, use a health check.

---

# 🏥 Health Checks

Example:

```yaml
services:

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
```

Backend:

```yaml
services:

  backend:
    build: ./backend

    depends_on:
      database:
        condition: service_healthy
```

Now:

```text
PostgreSQL
    ↓
Health Check
    ↓
Healthy
    ↓
Backend starts
```

---

# 🔄 Restart Policies

You can configure automatic restarts.

```yaml
services:

  backend:
    build: ./backend
    restart: unless-stopped
```

Common values:

```text
no
always
on-failure
unless-stopped
```

Example:

```yaml
restart: unless-stopped
```

This is useful for long-running services.

---

# 🟢 Docker Compose Commands

## Start services

```bash
docker compose up
```

---

## Start in background

```bash
docker compose up -d
```

The `-d` means detached mode.

---

## Build and start

```bash
docker compose up --build
```

Useful after changing a Dockerfile.

---

## Stop services

```bash
docker compose stop
```

---

## Stop and remove containers

```bash
docker compose down
```

---

## Remove containers and volumes

```bash
docker compose down -v
```

⚠️ Be careful.

This can delete persistent database volumes.

---

# 📋 List Services

```bash
docker compose ps
```

Example:

```text
NAME
my-project-backend
my-project-database
my-project-redis
```

---

# 🪵 View Logs

All services:

```bash
docker compose logs
```

Follow logs:

```bash
docker compose logs -f
```

Specific service:

```bash
docker compose logs backend
```

Follow backend logs:

```bash
docker compose logs -f backend
```

---

# 🔧 Execute Commands

Execute a command inside a service:

```bash
docker compose exec backend sh
```

For PostgreSQL:

```bash
docker compose exec database psql -U postgres
```

---

# 🔄 Restart Services

Restart everything:

```bash
docker compose restart
```

Restart only backend:

```bash
docker compose restart backend
```

---

# 🗑️ Rebuild

Rebuild a specific service:

```bash
docker compose build backend
```

Then start it:

```bash
docker compose up backend
```

Or:

```bash
docker compose up --build backend
```

---

# 🟢 Node.js Example

Project:

```text
node-compose/
│
├── compose.yaml
│
└── backend/
    ├── Dockerfile
    ├── package.json
    └── server.js
```

## Dockerfile

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## compose.yaml

```yaml
services:

  backend:
    build: ./backend
    ports:
      - "3000:3000"
```

Start:

```bash
docker compose up --build
```

Application:

```text
http://localhost:3000
```

---

# ▲ Next.js + PostgreSQL Example

A common modern stack:

```text
Next.js
   │
   ↓
PostgreSQL
```

`compose.yaml`:

```yaml
services:

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgresql://postgres:password@database:5432/myapp
    depends_on:
      database:
        condition: service_healthy

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp

    volumes:
      - postgres-data:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres-data:
```

Architecture:

```text
                Docker Compose
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       Next.js              PostgreSQL
       :3000                   :5432
          │                     │
          └─────────┬───────────┘
                    │
                 Network
```

---

# 🚀 Full-Stack Example

A more realistic application:

```text
fullstack-app/
│
├── compose.yaml
│
├── frontend/
│   ├── Dockerfile
│   └── package.json
│
├── backend/
│   ├── Dockerfile
│   └── package.json
│
└── .env
```

`compose.yaml`:

```yaml
services:

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

  backend:
    build: ./backend
    ports:
      - "4000:4000"
    environment:
      DATABASE_URL: postgresql://postgres:password@database:5432/myapp
      REDIS_URL: redis://redis:6379
    depends_on:
      database:
        condition: service_healthy
      redis:
        condition: service_started

  database:
    image: postgres:17
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:8-alpine

volumes:
  postgres-data:
```

Architecture:

```text
                    Docker Compose
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
     Frontend          Backend         PostgreSQL
     Next.js           Node.js          Database
        │                │
        │                ↓
        │              Redis
        │
        └────────────────┘
```

---

# 🔥 Development Environment

During development, you often want hot reload.

Example:

```yaml
services:

  backend:
    build: ./backend
    ports:
      - "4000:4000"
    volumes:
      - ./backend:/app
      - /app/node_modules
    command: npm run dev
```

The source code is mounted:

```text
Your Computer
      │
      ↓
./backend
      │
      ↓
Container
/app
```

When you edit code locally, the container can see the changes.

---

# 🏭 Development vs Production

## Development

Typical setup:

```text
Docker Compose
      │
      ├── Hot Reload
      ├── Source Volumes
      ├── Debugging
      └── Local Database
```

Example:

```yaml
command: npm run dev
```

---

## Production

Typical setup:

```text
Docker Image
      │
      ├── Multi-stage Build
      ├── Production Dependencies
      ├── Non-root User
      ├── Health Check
      └── Optimized Image
```

Example:

```yaml
command: npm start
```

---

# 🧪 Testing Compose Configuration

Before starting the application, validate the Compose file:

```bash
docker compose config
```

This is useful for finding YAML or configuration problems.

---

# 🔍 Check Running Containers

```bash
docker compose ps
```

Then:

```bash
docker ps
```

You can inspect individual containers with:

```bash
docker inspect container-name
```

---

# 🧹 Cleanup

Remove stopped containers:

```bash
docker container prune
```

Remove unused images:

```bash
docker image prune
```

Remove unused Docker resources:

```bash
docker system prune
```

For Compose:

```bash
docker compose down
```

Remove volumes too:

```bash
docker compose down -v
```

---

# ❌ Common Problems

## Problem 1 — Port already in use

Error:

```text
port is already allocated
```

Check:

```bash
docker ps
```

Change:

```yaml
ports:
  - "3001:3000"
```

Now access:

```text
localhost:3001
```

---

# ❌ Problem 2 — Backend cannot connect to database

Incorrect:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/myapp
```

Correct inside Compose:

```env
DATABASE_URL=postgresql://postgres:password@database:5432/myapp
```

Because:

```text
database
```

is the Compose service name.

---

# ❌ Problem 3 — Database data disappears

Make sure you have a volume:

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data
```

And define:

```yaml
volumes:
  postgres-data:
```

---

# ❌ Problem 4 — Backend starts before PostgreSQL is ready

Use:

```yaml
depends_on:
  database:
    condition: service_healthy
```

with a PostgreSQL health check.

---

# ❌ Problem 5 — Changes are not visible

Rebuild:

```bash
docker compose up --build
```

For development, consider source-code volumes and a development command such as:

```yaml
command: npm run dev
```

---

# 🔐 Best Practices

## 1. Use versioned images

Prefer:

```yaml
image: postgres:17
```

instead of:

```yaml
image: postgres:latest
```

---

## 2. Use `.env` for configuration

Example:

```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_DB=myapp
```

---

## 3. Don't commit secrets

Add:

```text
.env
```

to `.gitignore`.

---

## 4. Use health checks

Especially for:

* PostgreSQL
* MySQL
* Redis
* APIs

---

## 5. Use named volumes for databases

Example:

```yaml
volumes:
  postgres-data:/var/lib/postgresql/data
```

---

## 6. Use service names

Use:

```text
database
redis
backend
```

instead of container IP addresses.

---

## 7. Keep services focused

Prefer:

```text
frontend
backend
database
redis
```

instead of putting everything inside one container.

---

# 🧠 Important Concepts to Remember

### `image`

Use an existing Docker image:

```yaml
image: postgres:17
```

### `build`

Build your own image:

```yaml
build: ./backend
```

### `ports`

Expose a container port:

```yaml
ports:
  - "3000:3000"
```

### `environment`

Set environment variables:

```yaml
environment:
  NODE_ENV: production
```

### `volumes`

Persist data:

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data
```

### `depends_on`

Define service dependencies:

```yaml
depends_on:
  - database
```

### `networks`

Control service communication:

```yaml
networks:
  - app-network
```

### `healthcheck`

Check service health:

```yaml
healthcheck:
  test: ["CMD-SHELL", "pg_isready -U postgres"]
```

---

# 📋 Docker Compose Cheat Sheet

| Task                        | Command                          |
| --------------------------- | -------------------------------- |
| Start                       | `docker compose up`              |
| Start background            | `docker compose up -d`           |
| Build + start               | `docker compose up --build`      |
| Stop                        | `docker compose stop`            |
| Remove containers           | `docker compose down`            |
| Remove containers + volumes | `docker compose down -v`         |
| List services               | `docker compose ps`              |
| Logs                        | `docker compose logs`            |
| Follow logs                 | `docker compose logs -f`         |
| Service logs                | `docker compose logs backend`    |
| Execute command             | `docker compose exec backend sh` |
| Restart                     | `docker compose restart`         |
| Build                       | `docker compose build`           |
| Validate config             | `docker compose config`          |
| Pull images                 | `docker compose pull`            |

---

# 🗺️ Docker Compose Learning Roadmap

Follow this order:

```text
1. Basic compose.yaml
        ↓
2. Services
        ↓
3. Images
        ↓
4. Build
        ↓
5. Ports
        ↓
6. Environment Variables
        ↓
7. .env
        ↓
8. Volumes
        ↓
9. Networks
        ↓
10. depends_on
        ↓
11. Health Checks
        ↓
12. Development Volumes
        ↓
13. Multi-stage Builds
        ↓
14. Security
        ↓
15. CI/CD
        ↓
16. Cloud Deployment
```

---

# 🎯 Recommended Practice Projects

## Beginner

### Project 1

```text
Node.js
+
Docker Compose
```

Learn:

* `services`
* `build`
* `ports`

---

## Intermediate

### Project 2

```text
Node.js
+
PostgreSQL
```

Learn:

* Environment variables
* Volumes
* Networks
* Database connection

---

## Intermediate+

### Project 3

```text
Next.js
+
Node.js
+
PostgreSQL
```

Learn:

* Multiple services
* Dockerfiles
* Networks
* Dependencies

---

## Advanced

### Project 4

```text
Next.js
+
Node.js
+
PostgreSQL
+
Redis
+
Docker Compose
+
GitHub Actions
```

Learn:

* Full-stack containerization
* Caching
* Health checks
* CI/CD
* Production builds

---

# 🏆 Recommended Full-Stack Architecture

For a modern project:

```text
                         GitHub
                            │
                            ↓
                       CI / CD
                            │
                            ↓
                    Docker Registry
                            │
                            ↓
                    Docker Compose
                            │
       ┌────────────────────┼────────────────────┐
       ↓                    ↓                    ↓
   Next.js              Node.js              PostgreSQL
   Frontend              API                    DB
                            │
                            ↓
                          Redis
                            │
                            ↓
                         Cache
```

This architecture is a strong foundation before moving to:

```text
Docker
   ↓
CI/CD
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

# ✅ Final Checklist

Before moving to advanced Docker topics, you should be able to:

* [ ] Create `compose.yaml`
* [ ] Define services
* [ ] Use Docker images
* [ ] Build custom images
* [ ] Map ports
* [ ] Configure environment variables
* [ ] Use `.env`
* [ ] Create volumes
* [ ] Configure networks
* [ ] Use `depends_on`
* [ ] Configure health checks
* [ ] Read Compose logs
* [ ] Execute commands inside services
* [ ] Rebuild services
* [ ] Run PostgreSQL with Compose
* [ ] Run Redis with Compose
* [ ] Dockerize a Node.js application
* [ ] Dockerize a Next.js application
* [ ] Build a full-stack Compose project
* [ ] Use Compose in CI/CD
* [ ] Understand development vs production Compose

---

# 🎯 Final Goal

You should eventually be comfortable running an entire application with:

```bash
docker compose up -d
```

and managing:

```text
┌─────────────────────────────────────┐
│          Docker Compose             │
│                                     │
│  ┌──────────┐    ┌──────────────┐  │
│  │ Next.js  │───→│   Backend    │  │
│  └──────────┘    └──────┬───────┘  │
│                         │           │
│                 ┌───────┴───────┐   │
│                 ↓               ↓   │
│            PostgreSQL         Redis │
│                 │               │   │
│                 └───────────────┘   │
└─────────────────────────────────────┘
```

Then continue your learning toward **CI/CD → Cloud → Kubernetes → Terraform → Observability**.
