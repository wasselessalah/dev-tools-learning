# ⚙️ GitHub Actions Learning Guide

A practical guide to learning **GitHub Actions**, CI/CD, automated testing, builds, deployments, secrets, environments, Docker, and DevOps workflows.

---

## 📚 Table of Contents

* [What is GitHub Actions?](#-what-is-github-actions)
* [Why GitHub Actions?](#-why-github-actions)
* [CI/CD](#-cicd)
* [GitHub Actions Architecture](#-github-actions-architecture)
* [Workflow Files](#-workflow-files)
* [Your First Workflow](#-your-first-workflow)
* [Events](#-events)
* [Jobs](#-jobs)
* [Steps](#-steps)
* [Actions](#-actions)
* [Runners](#-runners)
* [Commands](#-commands)
* [Node.js CI](#-nodejs-ci)
* [npm CI](#-npm-ci)
* [Environment Variables](#-environment-variables)
* [Secrets](#-secrets)
* [Contexts](#-contexts)
* [Expressions](#-expressions)
* [Conditions](#-conditions)
* [Artifacts](#-artifacts)
* [Caching](#-caching)
* [Matrix Builds](#-matrix-builds)
* [Dependencies Between Jobs](#-dependencies-between-jobs)
* [Environments](#-environments)
* [Deployment](#-deployment)
* [GitHub Token](#-github-token)
* [Permissions](#-permissions)
* [Concurrency](#-concurrency)
* [Reusable Workflows](#-reusable-workflows)
* [Docker with GitHub Actions](#-docker-with-github-actions)
* [Next.js CI/CD](#-nextjs-cicd)
* [Express CI/CD](#-express-cicd)
* [AWS Deployment](#-aws-deployment)
* [Security](#-security)
* [Debugging](#-debugging)
* [Practice Projects](#-practice-projects)
* [Learning Roadmap](#-learning-roadmap)
* [Checklist](#-learning-checklist)
* [Useful Resources](#-useful-resources)

---

# ⚙️ What is GitHub Actions?

**GitHub Actions** is a platform for automating software development workflows.

It can automatically:

* Run tests
* Install dependencies
* Build applications
* Run linters
* Build Docker images
* Publish packages
* Deploy applications
* Run scheduled jobs
* Check pull requests
* Automate DevOps tasks

Example:

```text
Developer
    │
    ▼
git push
    │
    ▼
GitHub
    │
    ▼
GitHub Actions
    │
    ├── Install dependencies
    ├── Lint
    ├── Test
    ├── Build
    └── Deploy
```

GitHub describes Actions as a CI/CD platform that can automate build, test, and deployment workflows.

---

# 🔄 CI/CD

## CI — Continuous Integration

CI automatically checks your code when changes are pushed.

Example:

```text
git push
   ↓
Install dependencies
   ↓
Lint
   ↓
Test
   ↓
Build
```

---

## CD — Continuous Delivery / Deployment

CD automates releasing or deploying your application.

Example:

```text
git push
   ↓
Tests
   ↓
Build
   ↓
Docker image
   ↓
Deploy
   ↓
Production
```

---

# 🏗️ GitHub Actions Architecture

The main concepts are:

```text
Workflow
   │
   ├── Event
   │
   └── Jobs
        │
        ├── Job 1
        │    ├── Step
        │    ├── Step
        │    └── Step
        │
        └── Job 2
             ├── Step
             └── Step
```

Important concepts:

| Concept     | Meaning                         |
| ----------- | ------------------------------- |
| Workflow    | Complete automation process     |
| Event       | What starts the workflow        |
| Job         | Group of steps                  |
| Step        | Individual command/action       |
| Action      | Reusable automation             |
| Runner      | Machine executing the job       |
| Secret      | Sensitive configuration         |
| Artifact    | Files produced by a workflow    |
| Environment | Deployment target/configuration |

GitHub's current Actions documentation organizes workflows around events, jobs, steps, actions, variables, contexts, expressions, artifacts, caching, environments, and runners.

---

# 📁 Workflow Files

GitHub Actions workflow files must be stored in:

```text
.github/workflows/
```

Example:

```text
project/
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── cd.yml
│       └── docker.yml
│
├── src/
├── package.json
└── README.md
```

Workflow files use YAML:

```text
.yml
```

or:

```text
.yaml
```

---

# 🚀 Your First Workflow

Create:

```text
.github/workflows/hello.yml
```

```yaml
name: Hello World

on:
  push:

jobs:
  hello:
    runs-on: ubuntu-latest

    steps:
      - name: Say hello
        run: echo "Hello from GitHub Actions!"
```

Push the file:

```bash
git add .github/workflows/hello.yml
git commit -m "ci: add first GitHub Actions workflow"
git push
```

Then open:

```text
GitHub Repository
→ Actions
```

---

# 🔔 Events

The `on` section determines when a workflow runs.

## Push

```yaml
on:
  push:
```

---

## Pull Request

```yaml
on:
  pull_request:
```

---

## Push to a specific branch

```yaml
on:
  push:
    branches:
      - main
```

---

## Multiple branches

```yaml
on:
  push:
    branches:
      - main
      - develop
```

---

## Pull Requests to main

```yaml
on:
  pull_request:
    branches:
      - main
```

---

## Manual execution

```yaml
on:
  workflow_dispatch:
```

This allows you to manually start the workflow from GitHub.

---

## Schedule

You can use cron:

```yaml
on:
  schedule:
    - cron: "0 8 * * *"
```

The workflow syntax supports events, schedules, matrices, conditions, variables, and other workflow configuration features.

---

# 💼 Jobs

A workflow can contain multiple jobs.

```yaml
jobs:

  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Testing"

  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building"
```

By default, jobs can run independently.

---

# 🔗 Job Dependencies

Use `needs`:

```yaml
jobs:

  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Testing"

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building"
```

Flow:

```text
test
 ↓
build
```

---

# 🪜 Steps

Steps execute commands or actions.

```yaml
steps:

  - name: Print message
    run: echo "Hello"

  - name: Show Node version
    run: node --version

  - name: Show npm version
    run: npm --version
```

---

# 🧩 Actions

An action is reusable automation.

Example:

```yaml
- uses: actions/checkout@v6
```

This checks out your repository.

Another common action:

```yaml
- uses: actions/setup-node@v7
```

This configures Node.js.

GitHub maintains official actions such as `checkout` and `setup-node`; the current Node.js documentation recommends `setup-node` for selecting the Node.js version in workflows.

---

# 🖥️ Runners

A runner is the machine that executes your workflow.

Common GitHub-hosted runners include:

```yaml
runs-on: ubuntu-latest
```

```yaml
runs-on: windows-latest
```

```yaml
runs-on: macos-latest
```

You can also use self-hosted runners.

Architecture:

```text
GitHub
   │
   ▼
Workflow
   │
   ▼
Runner
   │
   ├── Git
   ├── Node.js
   ├── npm
   ├── Docker
   └── Your commands
```

GitHub provides hosted virtual machines and also supports self-hosted runners that you manage yourself.

---

# 🖥️ Commands

You can execute shell commands using `run`.

```yaml
- name: Show files
  run: ls -la
```

Multiple commands:

```yaml
- name: System information
  run: |
    pwd
    whoami
    node --version
    npm --version
```

---

# 🟢 Node.js CI

For your Node.js projects, a basic CI workflow can be:

```yaml
name: Node.js CI

on:
  push:
    branches:
      - main
      - develop

  pull_request:
    branches:
      - main

jobs:

  test:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout repository
        uses: actions/checkout@v6

      - name: Setup Node.js
        uses: actions/setup-node@v7
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build
```

GitHub's Node.js CI documentation recommends `actions/setup-node` for consistent Node.js versions and demonstrates `npm ci` for installing dependencies from the lockfile.

---

# 📦 npm CI

For projects with:

```text
package-lock.json
```

prefer:

```bash
npm ci
```

instead of:

```bash
npm install
```

Typical workflow:

```yaml
- uses: actions/checkout@v6

- uses: actions/setup-node@v7
  with:
    node-version: 20

- run: npm ci
- run: npm test
- run: npm run build
```

---

# 🌍 Environment Variables

You can define variables using `env`.

## Workflow level

```yaml
env:
  NODE_ENV: production
```

## Job level

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    env:
      NODE_ENV: production
```

## Step level

```yaml
steps:
  - name: Build
    env:
      NODE_ENV: production
    run: npm run build
```

Variables are intended for non-sensitive configuration. Sensitive values should be stored as secrets instead.

---

# 🔐 Secrets

Never put passwords or API keys directly inside your workflow.

❌ Bad:

```yaml
env:
  DATABASE_PASSWORD: "my-password"
```

Use GitHub Secrets instead:

```yaml
env:
  DATABASE_PASSWORD: ${{ secrets.DATABASE_PASSWORD }}
```

Common secrets:

```text
DATABASE_URL
JWT_SECRET
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
DOCKER_USERNAME
DOCKER_PASSWORD
DEPLOY_TOKEN
```

Repository secrets can be configured from:

```text
Repository
→ Settings
→ Secrets and variables
→ Actions
```

GitHub also supports organization-level and environment-level secrets.

---

# 🔑 Secrets Example

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:

  deploy:

    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v6

      - name: Deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: |
          echo "Deploying application..."
```

Do not print secrets:

```yaml
# ❌ Don't do this
run: echo "${{ secrets.DEPLOY_TOKEN }}"
```

---

# 🧠 Contexts

GitHub Actions provides contexts containing workflow information.

Example:

```yaml
${{ github.repository }}
```

Repository:

```yaml
${{ github.repository }}
```

Branch:

```yaml
${{ github.ref }}
```

Commit:

```yaml
${{ github.sha }}
```

Actor:

```yaml
${{ github.actor }}
```

Job status:

```yaml
${{ job.status }}
```

Common contexts:

```text
github
env
vars
job
jobs
steps
runner
secrets
strategy
matrix
needs
inputs
```

---

# 🧮 Expressions

Expressions use:

```text
${{ ... }}
```

Example:

```yaml
name: ${{ github.repository }}
```

Conditional:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

Another example:

```yaml
if: ${{ success() }}
```

Functions include:

```text
success()
failure()
cancelled()
always()
```

---

# 🔀 Conditions

Run a step only on main:

```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: echo "Deploying"
```

Run only after failure:

```yaml
- name: Upload logs
  if: failure()
  run: echo "Uploading logs"
```

Run when previous steps succeed:

```yaml
- name: Deploy
  if: success()
  run: echo "Deploy"
```

---

# 📦 Artifacts

Artifacts allow workflow outputs to be stored and downloaded after a job completes.

Example:

```yaml
- name: Upload build
  uses: actions/upload-artifact@v5
  with:
    name: build
    path: dist/
```

Download later:

```yaml
- name: Download build
  uses: actions/download-artifact@v6
  with:
    name: build
```

Useful artifacts:

```text
dist/
build/
coverage/
logs/
test-results/
screenshots/
```

---

# ⚡ Caching

Caching dependencies can make workflows faster.

Example with npm:

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v7
  with:
    node-version: 20
    cache: npm
```

Then:

```yaml
- run: npm ci
```

---

# 🔢 Matrix Builds

A matrix allows the same job to run with multiple configurations.

Example:

```yaml
jobs:

  test:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version:
          - 20
          - 22
          - 24

    steps:

      - uses: actions/checkout@v6

      - uses: actions/setup-node@v7
        with:
          node-version: ${{ matrix.node-version }}

      - run: npm ci

      - run: npm test
```

This creates multiple jobs:

```text
Node 20 → Test
Node 22 → Test
Node 24 → Test
```

GitHub's workflow syntax supports matrix strategies for testing multiple operating systems, versions, and configurations.

---

# 🔗 Dependencies Between Jobs

Example:

```yaml
jobs:

  test:
    runs-on: ubuntu-latest

    steps:
      - run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: npm run build

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

Architecture:

```text
test
 ↓
build
 ↓
deploy
```

---

# 🌎 Environments

GitHub Actions environments can represent:

```text
development
staging
production
```

Example:

```yaml
jobs:

  deploy:

    runs-on: ubuntu-latest

    environment:
      name: production

    steps:
      - run: echo "Deploying to production"
```

Environments can provide:

* Environment secrets
* Environment variables
* Deployment protection rules
* Manual approvals
* Branch restrictions

GitHub documents environments as deployment targets such as `production`, `staging`, and `development`.

---

# 🚀 Deployment Workflow

A typical deployment pipeline:

```text
Push to main
     ↓
Install
     ↓
Lint
     ↓
Test
     ↓
Build
     ↓
Docker build
     ↓
Push image
     ↓
Deploy
```

Example:

```yaml
name: Production Deployment

on:
  push:
    branches:
      - main

jobs:

  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v6

      - uses: actions/setup-node@v7
        with:
          node-version: 20
          cache: npm

      - run: npm ci
      - run: npm test
      - run: npm run build

  deploy:
    needs: test
    runs-on: ubuntu-latest

    environment:
      name: production

    steps:
      - uses: actions/checkout@v6

      - name: Deploy
        run: echo "Deploy application"
```

---

# 🔑 GITHUB_TOKEN

GitHub automatically provides a token that workflows can use to interact with the repository.

Example:

```yaml
permissions:
  contents: read
```

You can access it using:

```yaml
${{ secrets.GITHUB_TOKEN }}
```

or:

```yaml
${{ github.token }}
```

Use the minimum permissions required by the workflow.

---

# 🔒 Permissions

Example:

```yaml
permissions:
  contents: read
```

For specific operations:

```yaml
permissions:
  contents: write
```

Avoid giving workflows more permissions than necessary.

A useful principle is:

```text
Minimum permissions
       ↓
Smaller attack surface
```

---

# 🔄 Concurrency

Concurrency can prevent multiple deployments from running at the same time.

Example:

```yaml
concurrency:
  group: production
  cancel-in-progress: true
```

Useful for:

```text
main branch
     ↓
Deployment 1
     ↓
Deployment 2
     ↓
Deployment 3
```

You can configure GitHub Actions so obsolete runs are cancelled when a newer run supersedes them.

---

# ♻️ Reusable Workflows

Instead of duplicating CI logic, create reusable workflows.

Example:

```text
.github/
└── workflows/
    ├── ci.yml
    ├── deploy.yml
    └── reusable-node-ci.yml
```

Reusable workflows are useful when several repositories use the same CI/CD process.

---

# 🐳 Docker with GitHub Actions

Typical Docker pipeline:

```text
Git Push
   ↓
GitHub Actions
   ↓
Test
   ↓
Docker Build
   ↓
Docker Push
   ↓
Docker Registry
   ↓
Server
```

Example:

```yaml
name: Docker Build

on:
  push:
    branches:
      - main

jobs:

  docker:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout
        uses: actions/checkout@v6

      - name: Build Docker image
        run: |
          docker build \
            -t my-app:latest \
            .
```

---

# 🐳 Docker Hub Example

A production workflow can:

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

Credentials should be stored as secrets.

Example:

```yaml
- name: Login to Docker Hub
  uses: docker/login-action@v4
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
```

---

# ▲ Next.js CI/CD

Example:

```yaml
name: Next.js CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

jobs:

  build:

    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v6

      - uses: actions/setup-node@v7
        with:
          node-version: 20
          cache: npm

      - run: npm ci

      - run: npm run lint

      - run: npm run build
```

---

# 🟢 Express CI/CD

Example:

```yaml
name: Express CI

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

      - uses: actions/checkout@v6

      - uses: actions/setup-node@v7
        with:
          node-version: 20
          cache: npm

      - run: npm ci

      - run: npm run lint

      - run: npm test
```

---

# ☁️ AWS Deployment

GitHub Actions can be used with AWS.

Typical architecture:

```text
GitHub
   ↓
GitHub Actions
   ↓
AWS Authentication
   ↓
Build
   ↓
Deploy
   ↓
AWS
```

Possible AWS services:

```text
EC2
ECS
ECR
S3
Lambda
Elastic Beanstalk
CloudFront
```

For cloud authentication, GitHub Actions supports OpenID Connect (OIDC), which can avoid storing long-lived cloud credentials as GitHub secrets when the cloud provider is configured for OIDC.

---

# 🔐 Security Best Practices

## 1. Never hard-code secrets

❌

```yaml
DATABASE_URL: "postgres://user:password@..."
```

✅

```yaml
DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

---

## 2. Use minimum permissions

```yaml
permissions:
  contents: read
```

Only add permissions when necessary.

---

## 3. Be careful with pull requests

Do not blindly expose sensitive credentials to untrusted workflow code.

Review workflows that execute code from pull requests, especially when using powerful tokens or secrets.

---

## 4. Pin important actions

Instead of blindly using:

```yaml
uses: some-action/action@main
```

prefer a reviewed release or immutable reference appropriate for your security requirements.

---

## 5. Don't print secrets

Avoid:

```yaml
run: echo "${{ secrets.API_KEY }}"
```

---

## 6. Use environment protection

For production:

```text
GitHub
   ↓
Tests
   ↓
Build
   ↓
Production approval
   ↓
Deploy
```

---

# 🐛 Debugging Workflows

When a workflow fails:

```text
GitHub
  ↓
Actions
  ↓
Workflow
  ↓
Failed Job
  ↓
Failed Step
```

Read the logs from the failed step first.

---

## Check versions

```yaml
- name: Versions
  run: |
    node --version
    npm --version
    git --version
```

---

## Debug environment

```yaml
- name: Environment
  run: |
    pwd
    ls -la
    env | sort
```

Be careful with environment output because sensitive information must not be exposed.

---

## Common problems

### `npm ci` fails

Check:

```text
package.json
package-lock.json
Node.js version
```

---

### Build works locally but fails on GitHub

Check:

```text
Node version
Environment variables
Operating system
Case-sensitive paths
Missing dependencies
Build scripts
```

---

### Secret is empty

Check:

```text
Secret name
Repository settings
Environment
Workflow trigger
Secret availability
```

---

### Permission denied

Check:

```yaml
permissions:
  contents: read
```

or the specific permission required by the operation.

---

# 📁 Recommended Project Structure

For a Node.js project:

```text
my-project/
│
├── .github/
│   └── workflows/
│       ├── ci.yml
│       ├── docker.yml
│       └── deploy.yml
│
├── src/
├── tests/
├── Dockerfile
├── docker-compose.yml
├── package.json
├── package-lock.json
└── README.md
```

---

# 🧪 Practice Projects

## 🟢 Project 1 — Hello Actions

Create:

```text
.github/workflows/hello.yml
```

Practice:

* Workflow
* Event
* Job
* Step
* Runner

---

## 🟢 Project 2 — Node.js CI

Build:

```text
Push
 ↓
npm ci
 ↓
npm test
 ↓
npm run build
```

---

## 🟡 Project 3 — Matrix Testing

Test:

```text
Node 20
Node 22
Node 24
```

---

## 🟡 Project 4 — Docker CI

Build:

```text
Git Push
 ↓
Test
 ↓
Docker Build
 ↓
Docker Image
```

---

## 🟡 Project 5 — Docker Hub

Build:

```text
GitHub
 ↓
Actions
 ↓
Docker Build
 ↓
Docker Hub
```

---

## 🔴 Project 6 — Full CI/CD

Build:

```text
Developer
    ↓
Git Push
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Lint
    ↓
Test
    ↓
Build
    ↓
Docker Build
    ↓
Docker Registry
    ↓
Production Server
```

---

# 🛣️ Learning Roadmap

```text
Git & GitHub
      ↓
YAML
      ↓
GitHub Actions Basics
      ↓
Workflow
      ↓
Events
      ↓
Jobs
      ↓
Steps
      ↓
Actions
      ↓
Runners
      ↓
Node.js CI
      ↓
Secrets
      ↓
Environment Variables
      ↓
Artifacts
      ↓
Caching
      ↓
Matrix
      ↓
Docker
      ↓
Environments
      ↓
Deployment
      ↓
AWS
      ↓
Advanced CI/CD
```

---

# 🎯 Recommended Learning Order

For your Node.js / Next.js / Docker / AWS path:

```text
1. Git & GitHub
       ↓
2. YAML
       ↓
3. GitHub Actions
       ↓
4. Node.js CI
       ↓
5. Testing
       ↓
6. Docker
       ↓
7. Docker + Actions
       ↓
8. Secrets
       ↓
9. Production Environments
       ↓
10. Deployment
       ↓
11. AWS
       ↓
12. Advanced DevOps
```

---

# ✅ Learning Checklist

## Fundamentals

* [ ] Understand GitHub Actions
* [ ] Understand CI/CD
* [ ] Create a workflow
* [ ] Understand YAML
* [ ] Understand events
* [ ] Understand jobs
* [ ] Understand steps
* [ ] Understand actions

## Runners

* [ ] GitHub-hosted runners
* [ ] Ubuntu runner
* [ ] Windows runner
* [ ] macOS runner
* [ ] Understand self-hosted runners

## Node.js

* [ ] Setup Node.js
* [ ] npm ci
* [ ] npm test
* [ ] npm run build
* [ ] npm run lint
* [ ] Dependency caching

## Advanced

* [ ] Secrets
* [ ] Variables
* [ ] Contexts
* [ ] Expressions
* [ ] Conditions
* [ ] Artifacts
* [ ] Matrix
* [ ] Job dependencies
* [ ] Environments
* [ ] Concurrency
* [ ] Reusable workflows

## DevOps

* [ ] Docker
* [ ] Docker Hub
* [ ] CI pipeline
* [ ] CD pipeline
* [ ] Production deployment
* [ ] AWS
* [ ] OIDC
* [ ] Security

---

# 📌 GitHub Actions Cheat Sheet

## Workflow

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v6

      - uses: actions/setup-node@v7
        with:
          node-version: 20
          cache: npm

      - run: npm ci
      - run: npm test
      - run: npm run build
```

## Useful expressions

```yaml
${{ github.repository }}
${{ github.ref }}
${{ github.sha }}
${{ github.actor }}
${{ github.event_name }}
${{ secrets.MY_SECRET }}
${{ vars.MY_VARIABLE }}
${{ matrix.node-version }}
```

## Useful conditions

```yaml
if: success()
```

```yaml
if: failure()
```

```yaml
if: github.ref == 'refs/heads/main'
```

---

# 🔗 Useful Resources

### GitHub Actions

* [GitHub Actions](https://github.com/features/actions)
* [GitHub Actions Documentation](https://docs.github.com/en/actions)
* [GitHub Actions Concepts](https://docs.github.com/en/actions/concepts)
* [GitHub Actions Reference](https://docs.github.com/en/actions/reference)
* [Workflow Syntax](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)
* [Events That Trigger Workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)
* [GitHub Actions Runners](https://docs.github.com/en/actions/concepts/runners)

### Node.js

* [Building and Testing Node.js](https://docs.github.com/en/actions/tutorials/build-and-test-code/nodejs)
* [actions/setup-node](https://github.com/actions/setup-node)
* [actions/checkout](https://github.com/actions/checkout)

### Security

* [GitHub Actions Security](https://docs.github.com/en/actions/security-for-github-actions)
* [Secrets](https://docs.github.com/en/actions/reference/security/secrets)
* [Using Secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
* [OpenID Connect](https://docs.github.com/en/actions/security-for-github-actions/security-hardening-your-deployments/about-security-hardening-with-openid-connect)

### Deployment

* [Deployments and Environments](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments)
* [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)

---

# 🚀 Final Architecture

After learning GitHub Actions, your development workflow should look like:

```text
                    GitHub
                       │
                       │ git push
                       ▼
              ┌─────────────────┐
              │ GitHub Actions  │
              └────────┬────────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
            Tests              Lint
              │                 │
              └────────┬────────┘
                       ▼
                    Build
                       │
                       ▼
                  Docker Build
                       │
                       ▼
                Docker Registry
                       │
                       ▼
                  Deployment
                       │
                       ▼
                   AWS / VPS
                       │
                       ▼
                  Production
```

The goal is to move from:

```text
Manual deployment
```

to:

```text
Git Push
   ↓
Automated CI
   ↓
Automated Tests
   ↓
Automated Build
   ↓
Automated Deployment
```

That is the foundation of a modern **CI/CD + DevOps workflow**.
