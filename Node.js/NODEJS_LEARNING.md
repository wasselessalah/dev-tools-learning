# Node.js Learning Guide

A practical guide to learning **Node.js** for backend development, APIs,
authentication, databases, real-time applications, and production.

------------------------------------------------------------------------

## 1. What is Node.js?

**Node.js** is a JavaScript runtime that allows you to run JavaScript
outside the browser.

Node.js is built on the **V8 JavaScript engine** used by Chromium.

### Why use Node.js?

-   Build backend applications with JavaScript/TypeScript
-   Create REST APIs
-   Build real-time applications
-   Work with databases
-   Create CLI tools
-   Build microservices
-   Create web servers
-   Use the same language on frontend and backend
-   Large npm ecosystem

Official website:

https://nodejs.org/

Official documentation:

https://nodejs.org/docs/latest/api/

------------------------------------------------------------------------

# 2. Node.js vs JavaScript

  JavaScript                  Node.js
  --------------------------- ------------------------------
  Programming language        JavaScript runtime
  Usually runs in a browser   Runs on your computer/server
  Has DOM APIs                No browser DOM by default
  Browser APIs                Node.js APIs
  Frontend                    Backend, CLI, scripts, tools

Example JavaScript:

``` js
const name = "Wassel";

console.log(`Hello ${name}`);
```

The same JavaScript syntax can run with Node.js.

------------------------------------------------------------------------

# 3. Install Node.js

Check if Node.js is installed:

``` bash
node --version
```

Check npm:

``` bash
npm --version
```

Run a JavaScript file:

``` bash
node app.js
```

Example:

``` js
console.log("Hello Node.js");
```

Run:

``` bash
node app.js
```

------------------------------------------------------------------------

# 4. Node.js Versions

Node.js releases include:

-   Current
-   LTS (Long-Term Support)
-   Maintenance releases

For production projects, prefer an **LTS version** unless your project
specifically requires another version.

Check your version:

``` bash
node -v
```

You can use a version manager such as **nvm** to switch between Node.js
versions.

nvm:

https://github.com/nvm-sh/nvm

Example:

``` bash
nvm install --lts
nvm use --lts
```

------------------------------------------------------------------------

# 5. Your First Node.js Project

Create a directory:

``` bash
mkdir node-learning
cd node-learning
```

Initialize npm:

``` bash
npm init -y
```

Create:

``` text
node-learning/
├── package.json
└── index.js
```

`index.js`:

``` js
console.log("Hello Node.js!");
```

Run:

``` bash
node index.js
```

------------------------------------------------------------------------

# 6. package.json

`package.json` describes your Node.js project.

Example:

``` json
{
  "name": "node-learning",
  "version": "1.0.0",
  "description": "Node.js learning project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "keywords": [
    "nodejs",
    "backend"
  ],
  "license": "MIT"
}
```

Run the script:

``` bash
npm start
```

------------------------------------------------------------------------

# 7. npm

**npm** is the package manager commonly used with Node.js.

Check npm:

``` bash
npm -v
```

Initialize a project:

``` bash
npm init
```

Quick initialization:

``` bash
npm init -y
```

Install a package:

``` bash
npm install express
```

Install a development dependency:

``` bash
npm install -D nodemon
```

Remove a package:

``` bash
npm uninstall express
```

Update packages:

``` bash
npm update
```

Check outdated packages:

``` bash
npm outdated
```

------------------------------------------------------------------------

# 8. Dependencies

After installing packages, you will usually have:

``` text
node_modules/
package.json
package-lock.json
```

### node_modules

Contains installed dependencies.

Do not normally commit it to Git.

`.gitignore`:

``` gitignore
node_modules/
.env
dist/
coverage/
```

### package-lock.json

Locks dependency versions so installations are more reproducible.

Commit `package-lock.json` to Git.

------------------------------------------------------------------------

# 9. Common npm Commands

``` bash
npm init
npm install
npm install package-name
npm install -D package-name
npm uninstall package-name
npm update
npm outdated
npm audit
npm audit fix
npm run script-name
npm start
npm test
```

------------------------------------------------------------------------

# 10. Node.js Modules

Node.js supports modules for organizing code.

## CommonJS

``` js
const fs = require("node:fs");
```

Export:

``` js
module.exports = {
  name: "Node.js"
};
```

## ES Modules

Modern Node.js projects can use:

``` js
import fs from "node:fs";
```

Export:

``` js
export const name = "Node.js";
```

For ES Modules, configure:

``` json
{
  "type": "module"
}
```

------------------------------------------------------------------------

# 11. Built-in Node.js Modules

Node.js provides many built-in modules.

Important modules:

``` text
fs
path
http
url
os
crypto
events
stream
buffer
util
process
```

You can use them without installing npm packages.

Example:

``` js
import os from "node:os";

console.log(os.platform());
console.log(os.cpus().length);
console.log(os.totalmem());
```

------------------------------------------------------------------------

# 12. File System

The `fs` module allows you to work with files.

Read a file:

``` js
import fs from "node:fs/promises";

const data = await fs.readFile("hello.txt", "utf8");

console.log(data);
```

Write a file:

``` js
await fs.writeFile("hello.txt", "Hello Node.js!");
```

Append:

``` js
await fs.appendFile("hello.txt", "\nNew line");
```

------------------------------------------------------------------------

# 13. Path Module

Use `path` to safely work with file paths.

``` js
import path from "node:path";

const filePath = path.join("src", "data", "users.json");

console.log(filePath);
```

Useful methods:

``` js
path.join()
path.resolve()
path.basename()
path.dirname()
path.extname()
```

------------------------------------------------------------------------

# 14. Process

The `process` object provides information about the running Node.js
application.

``` js
console.log(process.version);
console.log(process.platform);
console.log(process.cwd());
```

Environment variables:

``` js
console.log(process.env.NODE_ENV);
```

------------------------------------------------------------------------

# 15. Environment Variables

Never hard-code secrets in your source code.

Bad:

``` js
const password = "my-secret-password";
```

Better:

``` js
const password = process.env.DB_PASSWORD;
```

Example `.env`:

``` env
PORT=4000
DATABASE_URL=postgresql://localhost:5432/mydb
JWT_SECRET=change-me
```

Add `.env` to `.gitignore`:

``` gitignore
.env
.env.local
.env.production
```

------------------------------------------------------------------------

# 16. HTTP Server

Node.js can create an HTTP server without Express.

``` js
import http from "node:http";

const server = http.createServer((req, res) => {
  res.writeHead(200, {
    "Content-Type": "application/json"
  });

  res.end(
    JSON.stringify({
      message: "Hello from Node.js"
    })
  );
});

server.listen(4000, () => {
  console.log("Server running on port 4000");
});
```

Test:

``` bash
curl http://localhost:4000
```

------------------------------------------------------------------------

# 17. HTTP Methods

Important HTTP methods:

  Method   Typical purpose
  -------- ---------------------
  GET      Read data
  POST     Create data
  PUT      Replace data
  PATCH    Update part of data
  DELETE   Delete data

Example API:

``` text
GET    /api/users
GET    /api/users/:id
POST   /api/users
PATCH  /api/users/:id
DELETE /api/users/:id
```

------------------------------------------------------------------------

# 18. Express.js

Express is a popular web framework for Node.js.

Install:

``` bash
npm install express
```

Basic server:

``` js
import express from "express";

const app = express();

app.use(express.json());

app.get("/", (req, res) => {
  res.json({
    message: "Hello Express"
  });
});

app.listen(4000, () => {
  console.log("API running on port 4000");
});
```

Official website:

https://expressjs.com/

------------------------------------------------------------------------

# 19. REST API

A REST API exposes resources through HTTP.

Example:

``` text
GET    /api/users
GET    /api/users/123
POST   /api/users
PATCH  /api/users/123
DELETE /api/users/123
```

Example response:

``` json
{
  "id": 123,
  "name": "Wassel",
  "role": "developer"
}
```

------------------------------------------------------------------------

# 20. Express Router

Separate routes into files.

``` js
import { Router } from "express";

const router = Router();

router.get("/", (req, res) => {
  res.json([]);
});

router.post("/", (req, res) => {
  res.status(201).json(req.body);
});

export default router;
```

Register it:

``` js
import userRoutes from "./routes/user.routes.js";

app.use("/api/users", userRoutes);
```

------------------------------------------------------------------------

# 21. Middleware

Middleware runs during the request/response cycle.

Example:

``` js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);

  next();
});
```

Common middleware tasks:

-   Authentication
-   Authorization
-   Logging
-   Validation
-   Error handling
-   Rate limiting
-   Parsing request bodies

------------------------------------------------------------------------

# 22. Error Handling

Use centralized error handling.

``` js
app.use((err, req, res, next) => {
  console.error(err);

  res.status(500).json({
    message: "Internal server error"
  });
});
```

Avoid exposing sensitive internal errors to clients.

------------------------------------------------------------------------

# 23. Async / Await

Node.js applications frequently use asynchronous operations.

``` js
async function getUsers() {
  const users = await database.users.findMany();

  return users;
}
```

Handle errors:

``` js
try {
  const users = await getUsers();

  console.log(users);
} catch (error) {
  console.error(error);
}
```

------------------------------------------------------------------------

# 24. Promises

Example:

``` js
const promise = fetch("https://example.com");

promise
  .then((response) => response.text())
  .then((data) => console.log(data))
  .catch((error) => console.error(error));
```

Modern code commonly uses:

``` js
const response = await fetch("https://example.com");
```

------------------------------------------------------------------------

# 25. Event Loop

Node.js uses an event-driven architecture.

A simplified model:

``` text
JavaScript code
      ↓
Call Stack
      ↓
Async APIs
      ↓
Event Loop
      ↓
Callback / Promise
      ↓
Call Stack
```

The event loop allows Node.js to handle many I/O operations without
blocking the JavaScript thread while waiting for them.

Learn:

-   Call Stack
-   Event Loop
-   Microtasks
-   Timers
-   I/O callbacks
-   `process.nextTick()`

------------------------------------------------------------------------

# 26. Don't Block the Event Loop

Avoid expensive synchronous operations inside request handlers.

Avoid:

``` js
const data = fs.readFileSync("large-file.json");
```

Prefer asynchronous APIs:

``` js
const data = await fs.readFile("large-file.json");
```

CPU-heavy workloads may require:

-   Worker Threads
-   Child Processes
-   Separate services
-   Queues

------------------------------------------------------------------------

# 27. Node.js Fetch

Modern Node.js includes `fetch()`.

``` js
const response = await fetch("https://api.example.com/users");

const users = await response.json();

console.log(users);
```

POST example:

``` js
const response = await fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Wassel"
  })
});
```

------------------------------------------------------------------------

# 28. Authentication

Common authentication approaches:

-   Session-based authentication
-   JWT
-   OAuth 2.0
-   OpenID Connect
-   API keys

Important concepts:

``` text
Authentication = Who are you?
Authorization  = What can you access?
```

Never store passwords as plain text.

Use a strong password hashing algorithm such as:

-   Argon2
-   bcrypt

------------------------------------------------------------------------

# 29. Databases

Node.js can connect to many databases.

### SQL

``` text
PostgreSQL
MySQL
MariaDB
SQLite
```

### NoSQL

``` text
MongoDB
Redis
```

Common tools:

``` text
Prisma
Drizzle
Mongoose
node-postgres (pg)
```

Prisma:

https://www.prisma.io/

------------------------------------------------------------------------

# 30. Node.js + PostgreSQL

Typical architecture:

``` text
Client
  ↓
Node.js API
  ↓
Service Layer
  ↓
Repository / ORM
  ↓
PostgreSQL
```

Example with `pg`:

``` bash
npm install pg
```

------------------------------------------------------------------------

# 31. Validation

Always validate user input.

Examples:

-   Zod
-   Joi
-   Valibot

Example with Zod:

``` bash
npm install zod
```

``` js
import { z } from "zod";

const userSchema = z.object({
  name: z.string().min(2),
  email: z.email()
});

const result = userSchema.safeParse(req.body);

if (!result.success) {
  return res.status(400).json({
    message: "Invalid input"
  });
}
```

------------------------------------------------------------------------

# 32. Security

Important Node.js security practices:

-   Validate all user input
-   Hash passwords
-   Protect secrets
-   Use HTTPS in production
-   Configure CORS carefully
-   Use secure cookies
-   Add rate limiting
-   Keep dependencies updated
-   Avoid exposing stack traces
-   Prevent SQL/NoSQL injection
-   Prevent XSS where applicable
-   Use security headers
-   Limit request body size

Useful package:

``` bash
npm install helmet
```

------------------------------------------------------------------------

# 33. CORS

CORS controls which browser origins can access your API.

Install:

``` bash
npm install cors
```

Example:

``` js
import cors from "cors";

app.use(
  cors({
    origin: "http://localhost:3000"
  })
);
```

In production, configure allowed origins explicitly.

------------------------------------------------------------------------

# 34. Logging

Logging helps diagnose application problems.

Simple logging:

``` js
console.log("Server started");
console.error("Database connection failed");
```

For larger applications, consider structured logging tools such as:

-   Pino
-   Winston

Pino:

https://getpino.io/

------------------------------------------------------------------------

# 35. Testing

Important testing levels:

``` text
Unit tests
Integration tests
API tests
End-to-end tests
```

Popular tools:

``` text
Vitest
Jest
Node.js test runner
Supertest
Playwright
```

Example:

``` bash
npm test
```

------------------------------------------------------------------------

# 36. Debugging

Start Node.js with the inspector:

``` bash
node --inspect app.js
```

You can connect using Chrome DevTools or VS Code.

VS Code can also debug Node.js applications using a launch
configuration.

------------------------------------------------------------------------

# 37. Nodemon

Nodemon automatically restarts the application when files change.

Install:

``` bash
npm install -D nodemon
```

Add:

``` json
{
  "scripts": {
    "dev": "nodemon src/server.js"
  }
}
```

Run:

``` bash
npm run dev
```

For production, use a proper process/runtime strategy rather than
relying on nodemon.

------------------------------------------------------------------------

# 38. TypeScript + Node.js

TypeScript is widely used for larger Node.js backend projects.

Typical structure:

``` text
src/
├── controllers/
├── routes/
├── services/
├── repositories/
├── middlewares/
├── schemas/
├── config/
└── server.ts
```

Advantages:

-   Static typing
-   Better IDE support
-   Safer refactoring
-   Better maintainability
-   Clear API contracts

------------------------------------------------------------------------

# 39. Recommended Backend Architecture

For a Node.js API:

``` text
src/
├── config/
├── controllers/
├── middlewares/
├── routes/
├── services/
├── repositories/
├── schemas/
├── utils/
├── app.ts
└── server.ts
```

Responsibilities:

``` text
Routes        → Define endpoints
Controllers   → Handle HTTP requests
Services      → Business logic
Repositories  → Database operations
Schemas       → Validation
Middleware    → Cross-cutting request logic
Config        → Environment/application configuration
```

------------------------------------------------------------------------

# 40. Node.js Project Structure

Example:

``` text
my-api/
├── src/
│   ├── config/
│   ├── controllers/
│   ├── middlewares/
│   ├── routes/
│   ├── services/
│   ├── repositories/
│   ├── schemas/
│   ├── app.ts
│   └── server.ts
│
├── tests/
├── .env
├── .env.example
├── .gitignore
├── package.json
├── package-lock.json
├── tsconfig.json
└── README.md
```

------------------------------------------------------------------------

# 41. Graceful Shutdown

Production applications should handle shutdown signals.

Example:

``` js
process.on("SIGTERM", async () => {
  console.log("SIGTERM received");

  // Close database connections
  // Stop accepting new requests
  // Finish active operations

  process.exit(0);
});
```

------------------------------------------------------------------------

# 42. Health Check

Create a simple health endpoint:

``` js
app.get("/health", (req, res) => {
  res.json({
    status: "ok"
  });
});
```

For production systems, health checks can also verify important
dependencies such as the database.

------------------------------------------------------------------------

# 43. Docker + Node.js

Example `Dockerfile`:

``` dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

EXPOSE 4000

CMD ["npm", "start"]
```

Build:

``` bash
docker build -t node-api .
```

Run:

``` bash
docker run -p 4000:4000 node-api
```

For production, use a Docker strategy appropriate for your application,
including dependency installation and build stages when needed.

------------------------------------------------------------------------

# 44. Node.js + Docker Compose

Example:

``` yaml
services:
  api:
    build: .
    ports:
      - "4000:4000"
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/app
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: app
    ports:
      - "5432:5432"
```

Start:

``` bash
docker compose up -d
```

Stop:

``` bash
docker compose down
```

------------------------------------------------------------------------

# 45. Production Checklist

Before deploying a Node.js application:

``` text
[ ] Use a supported Node.js version
[ ] Set NODE_ENV appropriately
[ ] Protect environment variables
[ ] Use HTTPS
[ ] Configure CORS
[ ] Validate input
[ ] Add authentication/authorization where required
[ ] Add centralized error handling
[ ] Add logging
[ ] Add health checks
[ ] Add rate limiting where appropriate
[ ] Keep dependencies updated
[ ] Run tests
[ ] Build the application
[ ] Configure graceful shutdown
[ ] Monitor CPU and memory
[ ] Configure database connection limits
[ ] Create backups for persistent data
```

------------------------------------------------------------------------

# 46. Useful Node.js Ecosystem

## Backend frameworks

-   Express
-   Fastify
-   NestJS
-   Hono

## Databases / ORMs

-   PostgreSQL
-   MySQL
-   MongoDB
-   Prisma
-   Drizzle
-   Mongoose

## Validation

-   Zod
-   Joi
-   Valibot

## Authentication

-   Better Auth
-   Passport
-   Auth.js

## Testing

-   Vitest
-   Jest
-   Playwright
-   Supertest

## Logging

-   Pino
-   Winston

## DevOps

-   Docker
-   Docker Compose
-   GitHub Actions
-   Kubernetes
-   Terraform

------------------------------------------------------------------------

# 47. Useful VS Code Extensions

Recommended extensions for Node.js development:

``` text
ESLint
Prettier - Code formatter
Error Lens
GitLens
Docker
REST Client
Thunder Client
npm Intellisense
Path Intellisense
```

Use extensions selectively; too many extensions can make VS Code
heavier.

------------------------------------------------------------------------

# 48. Useful Commands Cheat Sheet

``` bash
# Node version
node -v

# npm version
npm -v

# Create project
npm init -y

# Install dependency
npm install express

# Install dev dependency
npm install -D nodemon

# Remove dependency
npm uninstall express

# Run package script
npm run dev

# Run application
node src/server.js

# List dependencies
npm list

# Check outdated dependencies
npm outdated

# Security audit
npm audit

# Fix supported audit issues
npm audit fix

# Debug
node --inspect src/server.js
```

------------------------------------------------------------------------

# 49. Learning Roadmap

## Beginner

Learn:

``` text
JavaScript fundamentals
↓
Node.js runtime
↓
npm
↓
package.json
↓
Modules
↓
fs
↓
path
↓
http
↓
async/await
↓
fetch
```

## Backend

Then learn:

``` text
Express
↓
REST APIs
↓
Middleware
↓
Error handling
↓
Validation
↓
Authentication
↓
PostgreSQL
↓
ORM
```

## Advanced

Then:

``` text
TypeScript
↓
Testing
↓
Security
↓
Caching
↓
Redis
↓
Queues
↓
WebSockets
↓
Docker
↓
CI/CD
↓
Monitoring
```

## Cloud / DevOps

Finally:

``` text
Linux
↓
Docker
↓
GitHub Actions
↓
AWS
↓
Terraform
↓
Kubernetes
↓
Observability
```

------------------------------------------------------------------------

# 50. Practice Projects

## Project 1 --- CLI Tool

Build a CLI that:

-   Reads files
-   Creates files
-   Accepts command-line arguments
-   Stores configuration

------------------------------------------------------------------------

## Project 2 --- REST API

Build:

``` text
Users API
Posts API
Authentication
PostgreSQL
Validation
Error handling
```

------------------------------------------------------------------------

## Project 3 --- Authentication API

Implement:

``` text
Register
Login
Logout
Password hashing
Email verification
Password reset
Sessions
Roles
```

------------------------------------------------------------------------

## Project 4 --- Real-Time Chat

Use:

``` text
Node.js
Express
WebSocket / Socket.IO
PostgreSQL
Redis
```

Features:

``` text
Private messages
Group messages
Online status
Typing indicator
Message history
```

------------------------------------------------------------------------

## Project 5 --- Production API

Build a complete backend with:

``` text
Node.js
TypeScript
Express/Fastify
PostgreSQL
Prisma/Drizzle
Redis
Docker
GitHub Actions
Testing
Logging
Monitoring
```

------------------------------------------------------------------------

# 51. Official Resources

Node.js:

https://nodejs.org/

Node.js Documentation:

https://nodejs.org/docs/latest/api/

npm:

https://www.npmjs.com/

Express:

https://expressjs.com/

TypeScript:

https://www.typescriptlang.org/

Node.js GitHub:

https://github.com/nodejs/node

------------------------------------------------------------------------

# 52. Key Concepts to Master

Before considering yourself comfortable with Node.js, make sure you
understand:

``` text
✓ JavaScript fundamentals
✓ Node.js runtime
✓ Event loop
✓ Async programming
✓ Promises
✓ async/await
✓ npm
✓ package.json
✓ Modules
✓ Environment variables
✓ HTTP
✓ REST APIs
✓ Middleware
✓ Error handling
✓ Authentication
✓ Authorization
✓ Validation
✓ Databases
✓ Security
✓ Testing
✓ Logging
✓ Docker
✓ Production deployment
```

------------------------------------------------------------------------

## Final Goal

The goal is not only to memorize Node.js APIs.

You should be able to design and build a backend such as:

``` text
Frontend
   ↓
REST / WebSocket API
   ↓
Node.js + TypeScript
   ↓
Controllers
   ↓
Services
   ↓
Repositories
   ↓
PostgreSQL
   ↓
Redis
   ↓
Docker
   ↓
CI/CD
   ↓
Cloud
```

That architecture gives you a strong foundation for modern full-stack,
backend, Cloud, and DevOps projects.
