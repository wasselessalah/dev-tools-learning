# npm vs npx

## 📌 Overview

`npm` and `npx` are command-line tools that come with the Node.js ecosystem.

They are related, but they have different purposes:

* **npm** → installs and manages packages.
* **npx** → executes packages and CLI tools.

Understanding the difference is important when working with Node.js, React, Next.js, TypeScript, Vite, and other JavaScript projects.

---

## 🟢 What is npm?

**npm** stands for **Node Package Manager**.

It is mainly used to:

* Install packages
* Remove packages
* Update packages
* Manage dependencies
* Manage project scripts
* Publish packages
* Manage `package.json`

### Check npm version

```bash
npm --version
```

or:

```bash
npm -v
```

Example:

```text
11.6.0
```

---

# 📦 Installing Packages with npm

## Install a package locally

```bash
npm install express
```

Short version:

```bash
npm i express
```

This adds the package to:

```json
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

and installs it inside:

```text
node_modules/
```

---

## Install a development dependency

Use `-D` or `--save-dev`:

```bash
npm install -D typescript
```

Example:

```json
{
  "devDependencies": {
    "typescript": "^5.9.0"
  }
}
```

Development dependencies are usually tools needed during development or building.

Examples:

* TypeScript
* ESLint
* Prettier
* Vitest
* Jest
* Tailwind CSS

---

# 🌍 Global Packages

You can install a package globally:

```bash
npm install -g typescript
```

Then you may be able to run:

```bash
tsc --version
```

However, global installation is often unnecessary for project-specific CLI tools.

For modern Node.js projects, prefer **local dependencies + `npx`** when appropriate.

---

# 📜 npm Scripts

One of the most important features of npm is running scripts from `package.json`.

Example:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint ."
  }
}
```

Run them with:

```bash
npm run dev
```

```bash
npm run build
```

```bash
npm run lint
```

Some common scripts have shortcuts:

```bash
npm start
```

```bash
npm test
```

---

# 🔵 What is npx?

`npx` is used to **execute a package from the command line**.

It is especially useful for CLI tools.

For example:

```bash
npx create-next-app@latest
```

This executes the `create-next-app` CLI without requiring you to manually install it globally first.

---

# 🚀 Example: Create a Next.js Project

Instead of installing the CLI globally:

```bash
npm install -g create-next-app
```

you can simply run:

```bash
npx create-next-app@latest my-app
```

Then:

```bash
cd my-app
npm run dev
```

---

# 🧩 npm vs npx

| Feature                       | npm       | npx |
| ----------------------------- | --------- | --- |
| Install packages              | ✅         | ❌   |
| Remove packages               | ✅         | ❌   |
| Update packages               | ✅         | ❌   |
| Manage dependencies           | ✅         | ❌   |
| Run package commands          | Sometimes | ✅   |
| Run CLI tools                 | Sometimes | ✅   |
| Manage `package.json`         | ✅         | ❌   |
| Install globally              | ✅         | ❌   |
| Execute a package temporarily | ❌         | ✅   |

### Simple rule

> **npm = manage packages**

> **npx = execute packages**

---

# 🔥 npm Install vs npx Execute

Consider the package:

```text
create-vite
```

### Using npm

```bash
npm install create-vite
```

This installs the package into your project.

You could then execute its CLI through the local installation.

### Using npx

```bash
npx create-vite@latest
```

This is convenient when you only need to execute the CLI.

---

# 📁 Local `node_modules/.bin`

When npm installs a package that provides a CLI, executable files are normally placed in:

```text
node_modules/.bin/
```

For example:

```text
node_modules/
└── .bin/
    ├── eslint
    ├── prisma
    ├── tsc
    └── vite
```

You can execute these tools through npm scripts or `npx`.

Example:

```bash
npx prisma generate
```

---

# 🛠️ Running Local CLI Tools

Suppose your project contains:

```json
{
  "devDependencies": {
    "typescript": "^5.9.0"
  }
}
```

You can run:

```bash
npx tsc --version
```

This uses the project's available TypeScript CLI.

Similarly:

```bash
npx eslint .
```

or:

```bash
npx prisma generate
```

---

# ⚡ npx with a Specific Version

You can specify exactly which package version you want to execute.

Example:

```bash
npx create-next-app@latest
```

Or:

```bash
npx create-vite@latest
```

You can also specify a particular version:

```bash
npx create-vite@7.0.0
```

This is useful when you want predictable CLI behavior.

---

# 📥 Does npx Install Packages?

This is an important point.

`npx` is primarily an **execution tool**, not a replacement for:

```bash
npm install
```

When you execute a package with `npx`, npm's underlying package tooling can obtain the package if it is not already available locally.

For example:

```bash
npx create-vite@latest
```

You do **not** need:

```bash
npm install -g create-vite
```

The package can be fetched and executed for that command.

---

# 🔐 Security Considerations

Be careful when running:

```bash
npx some-package
```

If the package is not already installed locally, the command may retrieve it from the npm registry.

Before executing an unfamiliar package:

1. Check the package name.
2. Verify the npm package.
3. Check the GitHub repository if available.
4. Review documentation.
5. Check download activity and package history.
6. Avoid executing unknown commands copied from random websites.

For example, do not blindly run:

```bash
npx unknown-package
```

---

# 🏠 npm Local vs Global

## Local installation

```bash
npm install eslint
```

Creates a project dependency.

Usually:

```text
my-project/
├── node_modules/
├── package.json
└── package-lock.json
```

The package version is recorded in the project.

This is generally preferred for project tooling.

---

## Global installation

```bash
npm install -g eslint
```

The package is installed for the user/system environment.

Global packages can be useful for certain standalone tools, but they can also create version-management problems.

For example:

```text
Project A → ESLint version X
Project B → ESLint version Y
Global → ESLint version Z
```

Using local dependencies helps each project control its own tool versions.

---

# 📌 npm Scripts vs npx

Suppose your `package.json` contains:

```json
{
  "scripts": {
    "lint": "eslint ."
  }
}
```

You can run:

```bash
npm run lint
```

You could also run:

```bash
npx eslint .
```

Both can execute the local ESLint CLI.

However, npm scripts are usually preferable for **project commands** because they document the commands inside `package.json`.

---

# 🧪 Practical Examples

## Create a Vite project

```bash
npm create vite@latest
```

You may also encounter:

```bash
npx create-vite@latest
```

Both are common ways to invoke the Vite project generator.

---

## Create a Next.js project

```bash
npx create-next-app@latest
```

Then:

```bash
cd my-app
```

Install dependencies if needed:

```bash
npm install
```

Start development:

```bash
npm run dev
```

---

## Run Prisma

If Prisma is installed in the project:

```bash
npm install -D prisma
```

Then:

```bash
npx prisma generate
```

```bash
npx prisma migrate dev
```

---

## Run TypeScript

Install TypeScript:

```bash
npm install -D typescript
```

Then:

```bash
npx tsc
```

---

## Run ESLint

Install:

```bash
npm install -D eslint
```

Then:

```bash
npx eslint .
```

---

# 🔄 Common Workflow

A typical Node.js project can look like this:

```bash
# Create project
mkdir my-app

# Enter project
cd my-app

# Initialize npm
npm init -y

# Install dependency
npm install express

# Install development dependency
npm install -D typescript

# Execute local CLI
npx tsc --init

# Run project script
npm run dev
```

---

# 🧠 Easy Way to Remember

Think about it like this:

```text
npm
│
├── install
├── uninstall
├── update
├── manage dependencies
├── package.json
└── run scripts

npx
│
└── execute CLI packages
```

### Example

```bash
npm install prisma
```

Means:

> "Install Prisma in my project."

```bash
npx prisma generate
```

Means:

> "Execute the Prisma CLI."

---

# ❌ Common Mistakes

## Mistake 1: Installing every CLI globally

Avoid unnecessarily doing:

```bash
npm install -g prisma
npm install -g typescript
npm install -g eslint
```

For project tooling, prefer local dependencies:

```bash
npm install -D prisma typescript eslint
```

Then:

```bash
npx prisma
npx tsc
npx eslint
```

---

## Mistake 2: Confusing npm with Node.js

`npm` is not Node.js.

```text
Node.js
    ↓
JavaScript runtime

npm
    ↓
Package manager

npx
    ↓
Package/CLI executor
```

---

## Mistake 3: Using `npx` for everything

You don't normally use:

```bash
npx install express
```

Instead:

```bash
npm install express
```

Then use the package in your application.

---

# 📊 Quick Comparison

### Install a dependency

```bash
npm install express
```

### Install a development dependency

```bash
npm install -D typescript
```

### Run a local CLI

```bash
npx prisma generate
```

### Run an npm script

```bash
npm run dev
```

### Create a Next.js application

```bash
npx create-next-app@latest
```

### Check npm version

```bash
npm --version
```

### Check Node.js version

```bash
node --version
```

---

# 🎯 Recommended Practice

For modern Node.js projects:

### Use `npm` for:

* Project dependencies
* Development dependencies
* Package management
* `package.json`
* `package-lock.json`
* npm scripts

### Use `npx` for:

* Running local CLI tools
* Running package generators
* Running a specific CLI version
* Executing tools without installing them globally

---

# 📝 Cheat Sheet

| Goal                     | Command                  |
| ------------------------ | ------------------------ |
| Initialize project       | `npm init`               |
| Initialize quickly       | `npm init -y`            |
| Install package          | `npm install package`    |
| Install dev package      | `npm install -D package` |
| Remove package           | `npm uninstall package`  |
| Update package           | `npm update package`     |
| Run script               | `npm run script`         |
| Install globally         | `npm install -g package` |
| Execute local CLI        | `npx package`            |
| Execute specific version | `npx package@version`    |
| Check npm version        | `npm -v`                 |
| Check Node version       | `node -v`                |

---

# 🔗 Useful Documentation

* [npm Documentation](https://docs.npmjs.com/)
* [npm CLI Documentation](https://docs.npmjs.com/cli/)
* [npx Documentation](https://docs.npmjs.com/cli/v/latest/commands/npx)
* [Node.js Documentation](https://nodejs.org/docs/)

---

# ✅ Final Summary

The easiest way to remember the difference is:

```text
npm → Manage packages
npx → Execute packages
```

For example:

```bash
npm install prisma
```

installs Prisma into your project.

Then:

```bash
npx prisma generate
```

executes the Prisma CLI.

For most modern projects, keep project tools installed locally and use `npm run ...` or `npx ...` rather than relying on global installations.
