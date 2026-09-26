# 🔄 CI/CD Learning Guide

A practical guide to learning **Continuous Integration (CI)** and **Continuous Delivery / Deployment (CD)** for modern software development, DevOps, and cloud engineering.

---

# 📚 Table of Contents

* [What is CI/CD?](#-what-is-cicd)
* [Why CI/CD?](#-why-cicd)
* [CI vs CD](#-ci-vs-cd)
* [Typical CI/CD Pipeline](#-typical-cicd-pipeline)
* [CI/CD Workflow](#-cicd-workflow)
* [Pipeline Stages](#-pipeline-stages)
* [Continuous Integration](#-continuous-integration)
* [Continuous Delivery](#-continuous-delivery)
* [Continuous Deployment](#-continuous-deployment)
* [Git and CI/CD](#-git-and-cicd)
* [Testing in CI/CD](#-testing-in-cicd)
* [Build Stage](#-build-stage)
* [Environment Variables](#-environment-variables)
* [Secrets](#-secrets)
* [Artifacts](#-artifacts)
* [Docker and CI/CD](#-docker-and-cicd)
* [Database Migrations](#-database-migrations)
* [Deployment Strategies](#-deployment-strategies)
* [Rollback](#-rollback)
* [CI/CD Tools](#-cicd-tools)
* [GitHub Actions Example](#-github-actions-example)
* [Node.js CI/CD Example](#-nodejs-cicd-example)
* [Next.js CI/CD Example](#-nextjs-cicd-example)
* [Docker CI/CD Example](#-docker-cicd-example)
* [Production Pipeline](#-production-pipeline)
* [Security](#-security)
* [Best Practices](#-best-practices)
* [Common Problems](#-common-problems)
* [Practice Projects](#-practice-projects)
* [Learning Roadmap](#-learning-roadmap)
* [Checklist](#-checklist)
* [Useful Resources](#-useful-resources)

---

# 🚀 What is CI/CD?

**CI/CD** stands for:

* **CI** → Continuous Integration
* **CD** → Continuous Delivery
* **CD** → Continuous Deployment

CI/CD automates the process of:

```text
Code
 ↓
Push
 ↓
Build
 ↓
Test
 ↓
Security Checks
 ↓
Package
 ↓
Deploy
 ↓
Monitor
```

Instead of manually doing:

```text
git pull
npm install
npm test
npm run build
docker build
docker push
ssh server
deploy application
```

a CI/CD pipeline can perform these steps automatically.

---

# 🎯 Why CI/CD?

Without CI/CD:

```text
Developer
   ↓
Write code
   ↓
Push code
   ↓
Manually test
   ↓
Manually build
   ↓
Manually deploy
```

With CI/CD:

```text
Developer
   ↓
git push
   ↓
CI/CD Pipeline
   ├── Install
   ├── Lint
   ├── Test
   ├── Build
   ├── Security
   ├── Package
   └── Deploy
```

Benefits include:

* Faster development
* Automated testing
* Fewer manual deployment steps
* Consistent builds
* Faster feedback
* Easier rollback
* Better collaboration
* Repeatable deployments

---

# 🔄 CI vs CD

## Continuous Integration

CI means developers frequently integrate code into a shared repository.

A CI pipeline may:

```text
Push code
   ↓
Install dependencies
   ↓
Lint
   ↓
Run tests
   ↓
Build
```

Example:

```bash
git add .
git commit -m "add authentication"
git push origin main
```

The CI system automatically starts.

---

# 📦 Continuous Delivery

Continuous Delivery means the application is automatically prepared for release.

Example:

```text
git push
   ↓
Build
   ↓
Test
   ↓
Docker image
   ↓
Staging
   ↓
Ready for production
```

Production deployment may still require manual approval.

---

# 🚀 Continuous Deployment

Continuous Deployment automatically deploys successful changes to production.

```text
git push
   ↓
Build
   ↓
Test
   ↓
Security
   ↓
Deploy Production
```

No manual production approval is required.

---

# 🔀 CI/CD Pipeline Types

## CI only

```text
Git Push
   ↓
Install
   ↓
Test
   ↓
Build
```

## CI + Continuous Delivery

```text
Git Push
   ↓
Build
   ↓
Test
   ↓
Deploy Staging
   ↓
Manual Approval
   ↓
Production
```

## CI + Continuous Deployment

```text
Git Push
   ↓
Build
   ↓
Test
   ↓
Deploy Production
```

---

# 🏗️ Typical CI/CD Pipeline

A common production pipeline:

```text
Developer
    ↓
Git
    ↓
GitHub
    ↓
CI
    ↓
Lint
    ↓
Test
    ↓
Build
    ↓
Security Scan
    ↓
Docker Build
    ↓
Docker Registry
    ↓
Staging
    ↓
Approval
    ↓
Production
    ↓
Monitoring
```

---

# 🔁 CI/CD Workflow

A typical workflow:

```text
1. Developer writes code
        ↓
2. Developer creates commit
        ↓
3. Developer pushes code
        ↓
4. CI pipeline starts
        ↓
5. Dependencies installed
        ↓
6. Code quality checks
        ↓
7. Tests executed
        ↓
8. Application built
        ↓
9. Docker image created
        ↓
10. Image pushed to registry
        ↓
11. Application deployed
        ↓
12. Health check
        ↓
13. Monitoring
```

---

# 🧩 Pipeline Stages

A production pipeline commonly contains:

```text
Checkout
   ↓
Install
   ↓
Lint
   ↓
Test
   ↓
Build
   ↓
Security Scan
   ↓
Package
   ↓
Deploy
   ↓
Verify
```

---

# 🔍 1. Checkout

The CI server downloads the repository.

Example:

```bash
git clone https://github.com/example/project.git
```

CI platforms normally provide a built-in action/plugin for this.

---

# 📦 2. Install Dependencies

Node.js:

```bash
npm ci
```

Python:

```bash
pip install -r requirements.txt
```

Docker:

```bash
docker build .
```

For Node.js projects, prefer:

```bash
npm ci
```

for reproducible CI installations when a lockfile is available.

---

# 🧹 3. Lint

Linting checks code quality and common mistakes.

Example:

```bash
npm run lint
```

Possible tools:

* ESLint
* Prettier
* Ruff
* Flake8

---

# 🧪 4. Testing

Example:

```bash
npm test
```

Types of tests:

```text
Unit Tests
    ↓
Integration Tests
    ↓
End-to-End Tests
```

Examples:

* Jest
* Vitest
* Playwright
* Cypress

---

# 🏗️ 5. Build

Example:

```bash
npm run build
```

For Next.js:

```bash
npm run build
```

For Docker:

```bash
docker build -t my-app:latest .
```

---

# 🔐 6. Security Scanning

Security checks can include:

```text
Dependency scanning
       ↓
Secret scanning
       ↓
Container scanning
       ↓
SAST
```

Example:

```bash
npm audit
```

Container scanning tools include:

* Trivy
* Docker Scout

---

# 📦 7. Package

Applications can be packaged as:

```text
Docker Image
       ↓
Container Registry
```

Example:

```bash
docker build -t my-app:1.0.0 .
```

Then:

```bash
docker push my-registry/my-app:1.0.0
```

---

# 🚀 8. Deploy

Deployment can target:

* VPS
* AWS
* Azure
* Google Cloud
* Kubernetes
* Docker servers
* Managed platforms

Example:

```text
CI/CD
  ↓
Docker Registry
  ↓
Server
  ↓
Docker
  ↓
Application
```

---

# ❤️ 9. Health Check

After deployment, verify that the application works.

Example:

```bash
curl https://example.com/health
```

Possible endpoint:

```text
GET /health
```

Response:

```json
{
  "status": "ok"
}
```

---

# 🔗 Git and CI/CD

Git is the foundation of most CI/CD systems.

Typical workflow:

```bash
git checkout -b feature/auth

git add .

git commit -m "add authentication"

git push origin feature/auth
```

Then:

```text
Pull Request
     ↓
CI
     ↓
Lint
     ↓
Tests
     ↓
Build
     ↓
Code Review
     ↓
Merge
     ↓
Deployment
```

---

# 🌿 Branch Strategy

Example:

```text
main
 │
 ├── develop
 │    │
 │    ├── feature/auth
 │    ├── feature/profile
 │    └── feature/payment
 │
 └── release
```

A simpler strategy:

```text
main
 │
 ├── feature/auth
 ├── feature/profile
 └── fix/login
```

Pull requests trigger CI.

---

# 🧪 Testing in CI/CD

Testing should happen before deployment.

Example:

```text
Code
 ↓
Unit Tests
 ↓
Integration Tests
 ↓
E2E Tests
 ↓
Build
 ↓
Deploy
```

If tests fail:

```text
Test ❌
  ↓
Pipeline stops
  ↓
No deployment
```

This protects production.

---

# 📦 Build Artifacts

An artifact is a file produced by a pipeline.

Examples:

```text
dist/
build/
.next/
Docker image
ZIP archive
```

Example:

```text
Build
 ↓
app.zip
 ↓
Upload artifact
 ↓
Deploy artifact
```

---

# 🌍 Environment Variables

Applications usually have different environments:

```text
Development
Staging
Production
```

Example:

```env
NODE_ENV=production
DATABASE_URL=...
API_URL=...
```

Do not hard-code production configuration.

---

# 🔐 Secrets

Secrets include:

```text
DATABASE_PASSWORD
JWT_SECRET
API_KEY
AWS_ACCESS_KEY
PRIVATE_KEY
```

Never commit:

```env
DATABASE_PASSWORD=myPassword123
```

to Git.

Instead use your CI/CD platform's secret management.

Example:

```text
GitHub Secrets
      ↓
CI/CD Workflow
      ↓
Environment Variable
      ↓
Application
```

---

# 🐳 Docker and CI/CD

Docker works very well with CI/CD.

Example:

```text
Git Push
   ↓
CI
   ↓
Test
   ↓
Docker Build
   ↓
Docker Image
   ↓
Docker Registry
   ↓
Production Server
```

Dockerfile:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

Build:

```bash
docker build -t my-app:latest .
```

---

# 🗄️ Database Migrations

Database changes must be handled carefully.

Example:

```text
Application Update
       ↓
Database Migration
       ↓
Application Deployment
```

For Prisma:

```bash
npx prisma migrate deploy
```

Example pipeline:

```text
Build
 ↓
Test
 ↓
Migration
 ↓
Deploy
```

Always consider backward compatibility for production database changes.

---

# 🚦 Deployment Strategies

## 1. Recreate

Stop the old version:

```text
Version 1
   ↓
STOP
   ↓
Version 2
```

Simple but can cause downtime.

---

## 2. Rolling Deployment

Gradually replace old instances:

```text
V1 V1 V1
 ↓
V2 V1 V1
 ↓
V2 V2 V1
 ↓
V2 V2 V2
```

Useful with multiple servers or Kubernetes.

---

## 3. Blue-Green Deployment

Two environments:

```text
Blue
Production V1

Green
New V2
```

Test Green.

Then switch traffic:

```text
Users
  ↓
Green V2
```

Rollback:

```text
Users
  ↓
Blue V1
```

---

## 4. Canary Deployment

Release to a small percentage of users first.

```text
Users
  ↓
95% → V1
5%  → V2
```

If everything works:

```text
80% → V2
20% → V1
```

Eventually:

```text
100% → V2
```

---

# ↩️ Rollback

A good CI/CD system must support rollback.

Example:

```text
Version 1
    ↓
Version 2
    ↓
Production Problem
    ↓
Rollback
    ↓
Version 1
```

Docker example:

```bash
docker pull my-app:1.0.0
```

Instead of relying only on:

```text
latest
```

use versioned tags:

```text
my-app:1.0.0
my-app:1.1.0
my-app:1.2.0
```

---

# 🛠️ CI/CD Tools

## GitHub Actions

Integrated with GitHub.

```text
GitHub
  ↓
GitHub Actions
  ↓
CI/CD
```

---

## GitLab CI/CD

GitLab provides integrated CI/CD pipelines.

---

## Jenkins

Jenkins is a popular automation server.

```text
Git
 ↓
Jenkins
 ↓
Build
 ↓
Test
 ↓
Deploy
```

---

## Other Tools

```text
GitHub Actions
GitLab CI/CD
Jenkins
CircleCI
Azure Pipelines
AWS CodePipeline
Argo CD
Tekton
```

---

# ⚙️ GitHub Actions Example

Create:

```text
.github/workflows/ci.yml
```

Example:

```yaml
name: CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

jobs:
  test:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build
```

Pipeline:

```text
Push
 ↓
Checkout
 ↓
Node.js
 ↓
npm ci
 ↓
Lint
 ↓
Test
 ↓
Build
```

---

# 🟢 Node.js CI/CD Example

For a Node.js application:

```text
GitHub
   ↓
GitHub Actions
   ↓
npm ci
   ↓
npm run lint
   ↓
npm test
   ↓
npm run build
   ↓
Docker build
   ↓
Docker push
   ↓
Deploy
```

---

# ▲ Next.js CI/CD Example

Example:

```yaml
name: Next.js CI

on:
  push:
    branches:
      - main

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Build
        run: npm run build
```

---

# 🐳 Docker CI/CD Example

Example:

```yaml
name: Docker CI

on:
  push:
    branches:
      - main

jobs:
  docker:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build image
        run: |
          docker build \
            -t myusername/my-app:latest .

      - name: Push image
        run: |
          docker push myusername/my-app:latest
```

Production flow:

```text
GitHub
   ↓
GitHub Actions
   ↓
Docker Build
   ↓
Docker Hub
   ↓
Production Server
```

---

# 🏭 Production Pipeline

A more complete architecture:

```text
                 Developer
                     │
                     ▼
                  GitHub
                     │
                     ▼
              GitHub Actions
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
       Lint/Test           Security Scan
          │                     │
          └──────────┬──────────┘
                     ▼
                   Build
                     │
                     ▼
                Docker Image
                     │
                     ▼
              Container Registry
                     │
                     ▼
                  Staging
                     │
                  Tests
                     │
                     ▼
                Production
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
      Monitoring             Logging
```

---

# 🔒 CI/CD Security

Important security practices:

### 1. Never commit secrets

Bad:

```env
AWS_SECRET_KEY=xxxxxxxx
```

Good:

```text
CI/CD Secret Store
```

---

### 2. Use least privilege

Give workflows only the permissions they need.

---

### 3. Protect production

Use:

```text
Pull Request
     ↓
Tests
     ↓
Review
     ↓
Approval
     ↓
Production
```

---

### 4. Scan dependencies

Example:

```bash
npm audit
```

---

### 5. Scan Docker images

Example:

```bash
trivy image my-app:latest
```

---

# 📊 Monitoring

CI/CD does not end after deployment.

Production should be monitored.

```text
Deploy
  ↓
Monitor
  ↓
Detect Problem
  ↓
Alert
  ↓
Investigate
  ↓
Rollback/Fix
```

Useful tools:

* Prometheus
* Grafana
* Loki
* OpenTelemetry
* CloudWatch

---

# 🧰 Best Practices

## 1. Keep pipelines fast

Avoid unnecessary steps.

---

## 2. Fail fast

If tests fail:

```text
STOP PIPELINE
```

---

## 3. Use reproducible builds

Use lockfiles:

```text
package-lock.json
```

and:

```bash
npm ci
```

---

## 4. Version releases

Use:

```text
1.0.0
1.1.0
1.2.0
```

instead of only:

```text
latest
```

---

## 5. Separate environments

```text
Development
     ↓
Staging
     ↓
Production
```

---

## 6. Automate everything possible

Instead of:

```text
Manual build
Manual tests
Manual deployment
```

use:

```text
Automated build
Automated tests
Automated deployment
```

---

## 7. Always have rollback

Every production deployment should have a recovery strategy.

---

# ⚠️ Common Problems

## Pipeline fails during npm install

Check:

```bash
npm ci
```

Check:

```text
package-lock.json
Node.js version
npm version
```

---

## Build works locally but fails in CI

Check:

```text
Node.js version
Environment variables
Operating system
Case-sensitive file paths
Missing dependencies
```

---

## Environment variable missing

Verify the CI/CD secret/environment configuration.

---

## Docker build fails

Check:

```bash
docker build .
```

locally.

Then inspect:

```text
Dockerfile
.dockerignore
Node version
Dependencies
Build command
```

---

## Deployment succeeds but application is down

Check:

```text
Application logs
Container logs
Port
Reverse proxy
DNS
Environment variables
Database connection
Health endpoint
```

---

# 🧪 Practice Projects

## Project 1 — Node.js CI

Create:

```text
Node.js
 ↓
GitHub
 ↓
GitHub Actions
```

Pipeline:

```text
npm ci
 ↓
npm run lint
 ↓
npm test
```

---

## Project 2 — Next.js CI

Create:

```text
Next.js
 ↓
GitHub Actions
 ↓
Lint
 ↓
Build
```

---

## Project 3 — Docker CI

Create:

```text
GitHub
 ↓
GitHub Actions
 ↓
Docker Build
 ↓
Docker Hub
```

---

## Project 4 — Full CI/CD

Architecture:

```text
Next.js
    +
Express
    +
PostgreSQL
    ↓
Docker Compose
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Tests
    ↓
Docker Build
    ↓
Docker Registry
    ↓
VPS
    ↓
Nginx
    ↓
Production
```

---

# 🗺️ CI/CD Learning Roadmap

## Level 1 — Fundamentals

* [ ] Understand CI
* [ ] Understand Continuous Delivery
* [ ] Understand Continuous Deployment
* [ ] Understand pipelines
* [ ] Understand jobs and steps
* [ ] Understand artifacts
* [ ] Understand environments

---

## Level 2 — Git

* [ ] Git branches
* [ ] Pull Requests
* [ ] Merge
* [ ] Tags
* [ ] Releases
* [ ] Branch protection

---

## Level 3 — Testing

* [ ] Unit testing
* [ ] Integration testing
* [ ] E2E testing
* [ ] Test reports
* [ ] Coverage

---

## Level 4 — GitHub Actions

* [ ] Workflows
* [ ] Events
* [ ] Jobs
* [ ] Steps
* [ ] Actions
* [ ] Runners
* [ ] Secrets
* [ ] Environments
* [ ] Artifacts
* [ ] Caching
* [ ] Matrix builds
* [ ] Reusable workflows

---

## Level 5 — Docker

* [ ] Dockerfile
* [ ] Docker build
* [ ] Docker images
* [ ] Docker Registry
* [ ] Docker Compose
* [ ] Container deployment

---

## Level 6 — Deployment

* [ ] VPS deployment
* [ ] Nginx
* [ ] Reverse Proxy
* [ ] HTTPS
* [ ] DNS
* [ ] Health checks
* [ ] Rollback

---

## Level 7 — Cloud

* [ ] AWS
* [ ] EC2
* [ ] IAM
* [ ] VPC
* [ ] RDS
* [ ] S3
* [ ] CloudWatch

---

## Level 8 — Advanced DevOps

* [ ] Terraform
* [ ] Kubernetes
* [ ] Helm
* [ ] Prometheus
* [ ] Grafana
* [ ] Observability
* [ ] GitOps
* [ ] Argo CD

---

# ✅ CI/CD Checklist

### Fundamentals

* [ ] Understand CI
* [ ] Understand CD
* [ ] Understand pipeline stages
* [ ] Understand environments
* [ ] Understand artifacts

### Git

* [ ] Branches
* [ ] Pull Requests
* [ ] Tags
* [ ] Releases

### Automation

* [ ] GitHub Actions
* [ ] GitLab CI/CD
* [ ] Jenkins

### Testing

* [ ] Lint
* [ ] Unit tests
* [ ] Integration tests
* [ ] E2E tests

### Docker

* [ ] Dockerfile
* [ ] Docker Build
* [ ] Docker Registry
* [ ] Docker Compose

### Deployment

* [ ] VPS
* [ ] Nginx
* [ ] HTTPS
* [ ] Health checks
* [ ] Rollback

### Security

* [ ] Secrets
* [ ] Permissions
* [ ] Dependency scanning
* [ ] Container scanning

### Advanced

* [ ] Blue-Green deployment
* [ ] Canary deployment
* [ ] Rolling deployment
* [ ] Kubernetes
* [ ] Terraform
* [ ] Monitoring
* [ ] Observability

---

# 🏆 Recommended Learning Architecture

For a full-stack developer learning DevOps:

```text
Git & GitHub
      ↓
Linux
      ↓
SSH
      ↓
Node.js
      ↓
Databases
      ↓
Docker
      ↓
Docker Compose
      ↓
Nginx
      ↓
Reverse Proxy
      ↓
GitHub Actions
      ↓
CI/CD
      ↓
AWS
      ↓
Terraform
      ↓
Kubernetes
      ↓
Monitoring
      ↓
Observability
```

---

# 🔗 Useful Resources

## GitHub Actions

🌐 https://github.com/features/actions

🌐 https://docs.github.com/en/actions

🌐 https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax

---

## GitLab CI/CD

🌐 https://docs.gitlab.com/ci/

---

## Jenkins

🌐 https://www.jenkins.io/doc/

---

## Docker

🌐 https://docs.docker.com/

---

## Kubernetes

🌐 https://kubernetes.io/docs/

---

## Terraform

🌐 https://developer.hashicorp.com/terraform/docs

---

## AWS

🌐 https://docs.aws.amazon.com/

---

# 🎯 Final Goal

The final objective is to be able to build and maintain a complete automated deployment pipeline:

```text
Developer
    ↓
Git
    ↓
GitHub
    ↓
Pull Request
    ↓
GitHub Actions
    ↓
Lint
    ↓
Tests
    ↓
Security
    ↓
Build
    ↓
Docker
    ↓
Container Registry
    ↓
Staging
    ↓
Production
    ↓
Nginx
    ↓
Application
    ↓
Monitoring
```

The goal is not simply to know CI/CD commands.

The goal is to understand **how code moves from development to production safely and automatically**.

---

# 🧠 Key Concepts to Remember

```text
CI
→ Automatically validate code changes.

Continuous Delivery
→ Automatically prepare software for release.

Continuous Deployment
→ Automatically release validated changes.

Pipeline
→ Automated sequence of development and deployment steps.

Artifact
→ Output produced by a pipeline.

Runner
→ Machine that executes CI/CD jobs.

Secret
→ Sensitive configuration stored securely.

Rollback
→ Return to a previous working version.

Deployment Strategy
→ Method used to release a new version.

Observability
→ Understanding what is happening inside production.
```

---

# 📌 Recommended Next Steps

After completing this document:

```text
1. GitHub Actions
       ↓
2. CI Pipeline
       ↓
3. Automated Testing
       ↓
4. Docker CI
       ↓
5. Docker Registry
       ↓
6. VPS Deployment
       ↓
7. Nginx
       ↓
8. HTTPS
       ↓
9. AWS
       ↓
10. Terraform
       ↓
11. Kubernetes
       ↓
12. Monitoring
```

This gives you a practical foundation for **DevOps and Cloud Engineering**.
