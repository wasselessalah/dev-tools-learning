# 📦 npm Learning Guide

A practical guide to learning **npm (Node Package Manager)** for modern JavaScript and Node.js development.

This documentation covers npm fundamentals, packages, dependencies, scripts, `package.json`, `package-lock.json`, workspaces, security, publishing, and practical workflows.

---

## 📚 Table of Contents

- [What is npm?](#-what-is-npm)
- [npm vs Node.js](#-npm-vs-nodejs)
- [Installing npm](#-installing-npm)
- [Check Versions](#-check-versions)
- [npm Configuration](#-npm-configuration)
- [Creating a Project](#-creating-a-project)
- [package.json](#-packagejson)
- [Installing Packages](#-installing-packages)
- [Dependencies](#-dependencies)
- [package-lock.json](#-package-lockjson)
- [node_modules](#-node_modules)
- [npm Scripts](#-npm-scripts)
- [Updating Packages](#-updating-packages)
- [Removing Packages](#-removing-packages)
- [Global Packages](#-global-packages)
- [npm Cache](#-npm-cache)
- [npx](#-npx)
- [npm Workspaces](#-npm-workspaces)
- [Semantic Versioning](#-semantic-versioning)
- [Peer Dependencies](#-peer-dependencies)
- [Optional Dependencies](#-optional-dependencies)
- [Environment Variables](#-environment-variables)
- [npm Registry](#-npm-registry)
- [Publishing Packages](#-publishing-packages)
- [Security](#-security)
- [npm Audit](#-npm-audit)
- [npm CI](#-npm-ci)
- [npm + Docker](#-npm--docker)
- [npm + Git](#-npm--git)
- [Common Problems](#-common-problems)
- [Useful Commands](#-useful-commands)
- [Practice Projects](#-practice-projects)
- [Learning Roadmap](#-learning-roadmap)
- [Learning Checklist](#-learning-checklist)
- [Official Resources](#-official-resources)

---

# 📦 What is npm?

**npm** stands for **Node Package Manager**.

It is the standard package manager distributed with Node.js and is used to:

- Install JavaScript packages
- Manage project dependencies
- Run project scripts
- Execute command-line tools
- Publish packages
- Manage package versions
- Work with the npm registry

Example:

```bash
npm install express
```

This installs the `express` package into your project.

---

# 🟢 npm vs Node.js

| Tool | Purpose |
|---|---|
| Node.js | JavaScript runtime |
| npm | Package manager |
| npm Registry | Online package registry |
| npx | Execute package commands |

```bash
node app.js
```

Runs a Node.js application.

```bash
npm install express
```

Installs a package.

```bash
npx create-next-app@latest
```

Executes a package command without requiring a permanent global installation.

---

# 💻 Installing npm

npm is normally installed together with Node.js.

Download Node.js:

https://nodejs.org/

Check Node.js:

```bash
node --version
```

Check npm:

```bash
npm --version
```

---

# 🔎 Check Versions

```bash
node -v
npm -v
npm config list
npm prefix
npm list -g --depth=0
```

---

# ⚙️ npm Configuration

View configuration:

```bash
npm config list
```

Get the registry:

```bash
npm config get registry
```

Set the registry:

```bash
npm config set registry https://registry.npmjs.org/
```

Delete a configuration:

```bash
npm config delete registry
```

---

# 🚀 Creating a Project

```bash
mkdir my-app
cd my-app
npm init
```

For automatic initialization:

```bash
npm init -y
```

This creates:

```text
package.json
```

---

# 📄 package.json

`package.json` contains important information about your project.

Example:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My Node.js application",
  "type": "module",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

Important fields:

```text
name
version
description
type
main
scripts
dependencies
devDependencies
engines
```

---

# 📦 Installing Packages

Install a package:

```bash
npm install express
```

Short version:

```bash
npm i express
```

Install multiple packages:

```bash
npm install express cors dotenv
```

Install a specific version:

```bash
npm install express@5.1.0
```

Install the latest version:

```bash
npm install express@latest
```

---

# 🧩 Dependencies

Dependencies are packages required by your application at runtime.

Example:

```json
{
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^17.0.0",
    "express": "^5.1.0"
  }
}
```

Install dependencies from `package.json`:

```bash
npm install
```

---

# 🛠️ Development Dependencies

Install tools used mainly during development:

```bash
npm install -D typescript
```

or:

```bash
npm install --save-dev typescript
```

Common examples:

- ESLint
- Prettier
- TypeScript
- Vitest
- Jest
- Nodemon
- tsx

---

# 🔒 package-lock.json

npm normally creates:

```text
package-lock.json
```

The lockfile records the resolved dependency tree and helps make installations reproducible.

Example:

```text
my-app/
├── node_modules/
├── package.json
└── package-lock.json
```

For applications, normally commit it to Git:

```bash
git add package.json package-lock.json
git commit -m "chore: add dependencies"
```

---

# 📁 node_modules

Installed packages are stored in:

```text
node_modules/
```

Do not normally commit it to Git.

`.gitignore`:

```gitignore
node_modules/
```

---

# 📜 npm Scripts

Example:

```json
{
  "scripts": {
    "dev": "node --watch index.js",
    "start": "node index.js",
    "test": "vitest run",
    "lint": "eslint ."
  }
}
```

Run scripts:

```bash
npm run dev
npm run lint
npm run test
npm start
npm test
```

---

# 🔄 Updating Packages

Check outdated packages:

```bash
npm outdated
```

Update packages according to the ranges in `package.json`:

```bash
npm update
```

Update one package:

```bash
npm update express
```

Install a newer explicit version:

```bash
npm install express@latest
```

Review major version upgrades before applying them.

---

# 🗑️ Removing Packages

```bash
npm uninstall express
```

or:

```bash
npm remove express
```

Remove a development dependency:

```bash
npm uninstall -D eslint
```

---

# 🌍 Global Packages

Install a CLI globally:

```bash
npm install -g npm
```

List global packages:

```bash
npm list -g --depth=0
```

Find global prefix:

```bash
npm prefix -g
```

Prefer local project dependencies for project-specific tools:

```bash
npm install -D eslint
```

Then use:

```bash
npx eslint .
```

---

# 🧹 npm Cache

Verify cache:

```bash
npm cache verify
```

Clear cache when troubleshooting a specific cache problem:

```bash
npm cache clean --force
```

Do not clear the cache routinely without a reason.

---

# ⚡ npx

`npx` executes package commands.

Examples:

```bash
npx create-next-app@latest
npx prisma init
npx eslint .
```

It is useful for project-specific CLI tools without requiring global installation.

---

# 🏗️ npm Workspaces

npm supports monorepositories using workspaces.

Example:

```text
my-project/
├── package.json
├── apps/
│   ├── frontend/
│   └── backend/
└── packages/
    └── shared/
```

Root `package.json`:

```json
{
  "name": "my-project",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}
```

Install:

```bash
npm install
```

Run a script in a workspace:

```bash
npm run dev --workspace=frontend
```

---

# 🔢 Semantic Versioning

Format:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
5.2.3
```

- **MAJOR** — breaking changes
- **MINOR** — backward-compatible features
- **PATCH** — bug fixes

Common version ranges:

```text
5.1.0
^5.1.0
~5.1.0
>=5.1.0
```

View package versions:

```bash
npm view express versions
```

---

# 👥 Peer Dependencies

Peer dependencies describe packages expected to be provided by the consuming project.

Example:

```json
{
  "peerDependencies": {
    "react": "^19.0.0"
  }
}
```

Common in:

- React libraries
- UI component libraries
- Plugins
- Framework integrations

---

# 🔧 Optional Dependencies

Example:

```json
{
  "optionalDependencies": {
    "some-package": "^1.0.0"
  }
}
```

Useful when a package is optional for certain environments.

---

# 🌱 Environment Variables

Linux/macOS:

```bash
NODE_ENV=production npm start
```

Windows PowerShell:

```powershell
$env:NODE_ENV="production"
npm start
```

Example `.gitignore`:

```gitignore
.env
.env.local
.env.production
```

Never commit secrets such as passwords, tokens, or API keys.

---

# 🌐 npm Registry

Official registry:

https://registry.npmjs.org/

Search:

```bash
npm search express
```

View package information:

```bash
npm view express
```

View dependencies:

```bash
npm view express dependencies
```

View repository:

```bash
npm view express repository
```

---

# 📤 Publishing Packages

Login:

```bash
npm login
```

Create an archive to inspect package contents:

```bash
npm pack
```

Publish:

```bash
npm publish
```

Scoped public package:

```bash
npm publish --access public
```

Before publishing, check:

- Package name
- Version
- README
- License
- Repository
- Included files
- Sensitive information

---

# 🔐 Private Packages

Private packages can be used for:

- Company libraries
- Internal tools
- Shared components
- Private SDKs

Never commit:

- npm access tokens
- passwords
- API keys
- private credentials

---

# 🛡️ Security

Check dependencies:

```bash
npm audit
```

Try automatic fixes:

```bash
npm audit fix
```

Be careful with:

```bash
npm audit fix --force
```

Security practices:

- Keep dependencies updated
- Review dependency changes
- Use `npm audit`
- Avoid unknown packages
- Use lockfiles
- Remove unused dependencies
- Never commit secrets

---

# 🚀 npm ci

`npm ci` is designed for clean, reproducible installations.

```bash
npm ci
```

Commonly used in:

- CI/CD
- Docker
- Deployment environments

Typical workflow:

```bash
npm ci
npm test
npm run build
```

### npm install vs npm ci

| Command | Typical Use |
|---|---|
| `npm install` | Local development |
| `npm ci` | CI/CD and clean installations |

---

# 🏭 Production Installation

For production:

```bash
npm ci --omit=dev
```

Then:

```bash
npm start
```

---

# 🐳 npm + Docker

Example `Dockerfile`:

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Production:

```dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Copying `package*.json` before the source code helps Docker reuse dependency layers.

---

# 🔗 npm + Git

Typical project:

```text
my-project/
├── node_modules/
├── src/
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

`.gitignore`:

```gitignore
node_modules/
.env
.env.local
dist/
coverage/
```

Commit dependencies:

```bash
git add package.json package-lock.json
git commit -m "chore: add npm dependencies"
```

Do not commit:

```text
node_modules/
.env
```

---

# 🐧 npm on Linux

Find npm:

```bash
which npm
```

Find Node.js:

```bash
which node
```

Check prefix:

```bash
npm prefix
```

Check global prefix:

```bash
npm prefix -g
```

Check registry:

```bash
npm config get registry
```

Test registry:

```bash
npm ping
```

Useful for troubleshooting:

- `command not found`
- permission errors
- PATH problems
- registry problems
- slow installations
- global package problems

---

# 🛠️ Common Problems

## `npm: command not found`

Check:

```bash
node -v
npm -v
```

If Node.js is not installed, install it from:

https://nodejs.org/

---

## Permission errors

Example:

```text
EACCES: permission denied
```

Avoid randomly using:

```bash
sudo npm install -g ...
```

A Node.js version manager such as `nvm` can help keep Node.js and npm installations under your user account.

---

## Broken `node_modules`

Remove dependencies:

```bash
rm -rf node_modules
```

Reinstall:

```bash
npm install
```

For a clean lockfile-based installation:

```bash
npm ci
```

---

## Slow package installation

Check registry:

```bash
npm config get registry
```

Reset:

```bash
npm config set registry https://registry.npmjs.org/
```

Test:

```bash
npm ping
```

---

## Dependency conflict

Example:

```text
ERESOLVE unable to resolve dependency tree
```

Inspect:

```bash
npm ls
npm outdated
```

Review peer-dependency requirements before using:

```bash
npm install --legacy-peer-deps
```

Do not use `--force` automatically.

---

# 📊 Useful npm Commands

| Command | Purpose |
|---|---|
| `npm init` | Create `package.json` |
| `npm init -y` | Create `package.json` automatically |
| `npm install` | Install dependencies |
| `npm i package` | Install a package |
| `npm install -D package` | Install dev dependency |
| `npm uninstall package` | Remove a package |
| `npm update` | Update dependencies |
| `npm outdated` | Show outdated packages |
| `npm list` | Show installed packages |
| `npm list --depth=0` | Show direct dependencies |
| `npm audit` | Check vulnerabilities |
| `npm audit fix` | Attempt security fixes |
| `npm ci` | Clean lockfile installation |
| `npm run script` | Run an npm script |
| `npm start` | Run start script |
| `npm test` | Run test script |
| `npm search package` | Search packages |
| `npm view package` | View package information |
| `npm pack` | Create package archive |
| `npm publish` | Publish a package |
| `npm login` | Log in to npm |
| `npm cache verify` | Verify cache |
| `npm config list` | Show configuration |
| `npm ping` | Test registry connectivity |

---

# 🧪 Practical Projects

## Project 1 — npm Basics

```text
npm-basics/
├── package.json
├── package-lock.json
└── index.js
```

Practice:

- `npm init`
- `npm install`
- `npm uninstall`
- npm scripts
- dependencies
- devDependencies

---

## Project 2 — Express API

```text
npm-express-api/
├── src/
│   └── server.js
├── package.json
└── package-lock.json
```

Install:

```bash
npm install express
```

Scripts:

```json
{
  "scripts": {
    "dev": "node --watch src/server.js",
    "start": "node src/server.js"
  }
}
```

---

## Project 3 — TypeScript API

Install:

```bash
npm install express
npm install -D typescript tsx @types/node @types/express
```

Create:

```text
src/
└── server.ts
```

Scripts:

```json
{
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

---

## Project 4 — npm + Docker

Build:

```text
Node.js
   ↓
npm
   ↓
Docker
   ↓
PostgreSQL
```

Practice:

- `package.json`
- `package-lock.json`
- `npm ci`
- Dockerfile
- Docker Compose
- Environment variables

---

# 🛣️ npm Learning Roadmap

## Beginner

- [ ] Understand npm
- [ ] Install Node.js
- [ ] Check npm version
- [ ] `npm init`
- [ ] `package.json`
- [ ] Install packages
- [ ] Remove packages
- [ ] `node_modules`
- [ ] `package-lock.json`
- [ ] npm scripts

## Intermediate

- [ ] dependencies
- [ ] devDependencies
- [ ] peerDependencies
- [ ] optionalDependencies
- [ ] Semantic Versioning
- [ ] `npm update`
- [ ] `npm outdated`
- [ ] `npm audit`
- [ ] `npm ci`
- [ ] `npx`
- [ ] npm configuration
- [ ] npm registry
- [ ] Environment variables
- [ ] npm workspaces

## Advanced

- [ ] Publishing packages
- [ ] Scoped packages
- [ ] Private packages
- [ ] Package maintenance
- [ ] Monorepos
- [ ] npm + CI/CD
- [ ] npm + Docker
- [ ] Dependency security
- [ ] Package automation

---

# ✅ Learning Checklist

## npm Fundamentals

- [ ] Understand npm
- [ ] Install npm
- [ ] Understand `package.json`
- [ ] Install packages
- [ ] Remove packages
- [ ] Understand `node_modules`
- [ ] Understand `package-lock.json`

## Dependencies

- [ ] dependencies
- [ ] devDependencies
- [ ] peerDependencies
- [ ] optionalDependencies
- [ ] Version ranges
- [ ] Semantic Versioning

## Scripts

- [ ] Create npm scripts
- [ ] Run npm scripts
- [ ] Chain scripts
- [ ] Development scripts
- [ ] Build scripts
- [ ] Test scripts

## Production

- [ ] `npm ci`
- [ ] `npm ci --omit=dev`
- [ ] `npm audit`
- [ ] Docker
- [ ] CI/CD
- [ ] Dependency management

## Advanced

- [ ] npx
- [ ] npm workspaces
- [ ] npm registry
- [ ] Package publishing
- [ ] Scoped packages
- [ ] Private packages

---

# 🔗 npm Ecosystem

```text
Node.js
   │
   ├── npm
   │    ├── package.json
   │    ├── package-lock.json
   │    └── node_modules
   │
   ├── npx
   │
   ├── npm Registry
   │
   ├── Express
   ├── Prisma
   ├── TypeScript
   ├── ESLint
   ├── Prettier
   └── Vitest
```

---

# 🌐 Official Resources

- [npm Official Website](https://www.npmjs.com/)
- [npm Documentation](https://docs.npmjs.com/)
- [npm CLI Documentation](https://docs.npmjs.com/cli/)
- [npm Registry](https://registry.npmjs.org/)
- [Node.js Official Website](https://nodejs.org/)
- [Node.js Documentation](https://nodejs.org/docs/latest/api/)
- [Semantic Versioning](https://semver.org/)

---

# 🎯 Final Goal

After completing this guide, you should be comfortable with:

```text
npm
│
├── package.json
├── package-lock.json
├── node_modules
├── dependencies
├── devDependencies
├── peerDependencies
├── npm scripts
├── npm install
├── npm ci
├── npm update
├── npm audit
├── npx
├── npm workspaces
├── npm registry
└── package publishing
```

The goal is to use npm confidently in real-world:

```text
Node.js
Express
Next.js
TypeScript
React
Docker
CI/CD
Backend
Full-Stack
DevOps
```

projects.
