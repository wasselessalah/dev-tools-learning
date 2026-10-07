# 🚀 Express.js Learning

A practical guide to learning **Express.js**, a lightweight and flexible web framework for **Node.js**.

This documentation covers Express.js from beginner concepts to building REST APIs, middleware, authentication, error handling, database integration, security, testing, and production deployment.

---

## 📚 Table of Contents

1. [What is Express.js?](#1-what-is-expressjs)
2. [Why Use Express.js?](#2-why-use-expressjs)
3. [Prerequisites](#3-prerequisites)
4. [Installation](#4-installation)
5. [Create Your First Express Server](#5-create-your-first-express-server)
6. [Project Structure](#6-project-structure)
7. [Routing](#7-routing)
8. [HTTP Methods](#8-http-methods)
9. [Request and Response](#9-request-and-response)
10. [Route Parameters](#10-route-parameters)
11. [Query Parameters](#11-query-parameters)
12. [Request Body](#12-request-body)
13. [Middleware](#13-middleware)
14. [Custom Middleware](#14-custom-middleware)
15. [Router](#15-router)
16. [Controllers](#16-controllers)
17. [Services](#17-services)
18. [Error Handling](#18-error-handling)
19. [Environment Variables](#19-environment-variables)
20. [CORS](#20-cors)
21. [REST API](#21-rest-api)
22. [Authentication](#22-authentication)
23. [Database Integration](#23-database-integration)
24. [Validation](#24-validation)
25. [Security](#25-security)
26. [Logging](#26-logging)
27. [File Uploads](#27-file-uploads)
28. [Testing](#28-testing)
29. [Production](#29-production)
30. [Common Commands](#30-common-commands)
31. [Best Practices](#31-best-practices)
32. [Common Problems](#32-common-problems)
33. [Useful Packages](#33-useful-packages)
34. [Learning Roadmap](#34-learning-roadmap)
35. [Official Resources](#35-official-resources)

---

# 1. What is Express.js?

**Express.js** is a web framework for **Node.js**.

It provides tools for building:

* Web servers
* REST APIs
* Backend applications
* Microservices
* Authentication systems
* CRUD applications
* Real-time applications
* API gateways

Express makes it easier to work with HTTP requests and responses.

### Basic architecture

```text
Client
   │
   │ HTTP Request
   ▼
Express Server
   │
   ├── Middleware
   │
   ├── Router
   │
   ├── Controller
   │
   ├── Service
   │
   └── Database
   │
   ▼
HTTP Response
   │
   ▼
Client
```

---

# 2. Why Use Express.js?

Express is popular because it is:

* Lightweight
* Flexible
* Easy to learn
* Fast
* Middleware-based
* JavaScript/TypeScript friendly
* Well integrated with Node.js
* Suitable for REST APIs

Example:

```text
Next.js Frontend
       │
       │ HTTP
       ▼
Express API
       │
       ▼
PostgreSQL
```

This architecture is useful for applications where the frontend and backend are separated.

---

# 3. Prerequisites

Before learning Express.js, you should understand:

### JavaScript

Learn:

* Variables
* Functions
* Objects
* Arrays
* Classes
* Modules
* Promises
* async/await
* Error handling

### Node.js

Learn:

* npm
* package.json
* Node modules
* Environment variables
* HTTP
* File system basics

### HTTP

Understand:

```text
GET
POST
PUT
PATCH
DELETE
```

Also learn:

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

---

# 4. Installation

Create a project:

```bash
mkdir express-learning
cd express-learning
```

Initialize npm:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

For development:

```bash
npm install --save-dev nodemon
```

---

# 5. Create Your First Express Server

Create:

```text
server.js
```

Add:

```js
const express = require("express");

const app = express();

const PORT = 3000;

app.get("/", (req, res) => {
  res.send("Hello Express!");
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

Run:

```bash
node server.js
```

Open:

```text
http://localhost:3000
```

---

# 6. Project Structure

A small Express application can start with:

```text
express-app/
├── src/
│   ├── controllers/
│   ├── routes/
│   ├── services/
│   ├── middleware/
│   ├── models/
│   ├── config/
│   └── app.js
│
├── server.js
├── package.json
├── .env
├── .gitignore
└── README.md
```

For a larger application:

```text
src/
├── config/
├── controllers/
├── middleware/
├── routes/
├── services/
├── repositories/
├── models/
├── validators/
├── utils/
└── app.js
```

---

# 7. Routing

Routing determines how the server responds to different URLs.

Basic route:

```js
app.get("/", (req, res) => {
  res.send("Home");
});
```

Another route:

```js
app.get("/users", (req, res) => {
  res.send("Users");
});
```

Result:

```text
GET /          → Home
GET /users     → Users
```

---

# 8. HTTP Methods

Express supports the main HTTP methods.

### GET

Used to retrieve data.

```js
app.get("/users", (req, res) => {
  res.json([]);
});
```

### POST

Used to create data.

```js
app.post("/users", (req, res) => {
  res.status(201).json({
    message: "User created"
  });
});
```

### PUT

Used to replace/update a resource.

```js
app.put("/users/:id", (req, res) => {
  res.json({
    message: "User updated"
  });
});
```

### PATCH

Used for partial updates.

```js
app.patch("/users/:id", (req, res) => {
  res.json({
    message: "User partially updated"
  });
});
```

### DELETE

Used to delete data.

```js
app.delete("/users/:id", (req, res) => {
  res.json({
    message: "User deleted"
  });
});
```

---

# 9. Request and Response

Express provides two important objects:

```js
req
res
```

### Request

The request contains information sent by the client.

Examples:

```js
req.params
req.query
req.body
req.headers
```

### Response

The response sends information back to the client.

Examples:

```js
res.send()
res.json()
res.status()
res.redirect()
```

Example:

```js
app.get("/hello", (req, res) => {
  res.status(200).json({
    message: "Hello!"
  });
});
```

---

# 10. Route Parameters

Route parameters are dynamic values inside URLs.

Example:

```js
app.get("/users/:id", (req, res) => {
  const id = req.params.id;

  res.json({
    userId: id
  });
});
```

Request:

```text
GET /users/25
```

Result:

```json
{
  "userId": "25"
}
```

Multiple parameters:

```js
app.get("/users/:userId/posts/:postId", (req, res) => {
  const { userId, postId } = req.params;

  res.json({
    userId,
    postId
  });
});
```

---

# 11. Query Parameters

Query parameters are usually used for filtering, searching, sorting, or pagination.

Example:

```text
GET /users?name=wassel&age=22
```

Access them:

```js
app.get("/users", (req, res) => {
  const { name, age } = req.query;

  res.json({
    name,
    age
  });
});
```

Example:

```text
/products?category=computer&sort=price
```

---

# 12. Request Body

To receive JSON data:

```js
app.use(express.json());
```

Then:

```js
app.post("/users", (req, res) => {
  const { name, email } = req.body;

  res.json({
    name,
    email
  });
});
```

Request:

```json
{
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

# 13. Middleware

Middleware is one of the most important concepts in Express.

Middleware functions execute between the request and the response.

```text
Request
   ↓
Middleware
   ↓
Middleware
   ↓
Route
   ↓
Response
```

Example:

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);

  next();
});
```

`next()` passes control to the next middleware.

---

# 14. Custom Middleware

Create:

```text
middleware/logger.js
```

Example:

```js
const logger = (req, res, next) => {
  console.log({
    method: req.method,
    url: req.url,
    time: new Date()
  });

  next();
};

module.exports = logger;
```

Use it:

```js
const logger = require("./middleware/logger");

app.use(logger);
```

---

# 15. Router

Routers help organize routes.

Create:

```text
routes/user.routes.js
```

```js
const express = require("express");

const router = express.Router();

router.get("/", (req, res) => {
  res.json([]);
});

router.get("/:id", (req, res) => {
  res.json({
    id: req.params.id
  });
});

module.exports = router;
```

Register:

```js
const userRoutes = require("./routes/user.routes");

app.use("/api/users", userRoutes);
```

Now:

```text
GET /api/users
GET /api/users/10
```

---

# 16. Controllers

Controllers contain request-handling logic.

Example:

```text
controllers/
└── user.controller.js
```

```js
const getUsers = async (req, res) => {
  res.json([]);
};

const getUser = async (req, res) => {
  res.json({
    id: req.params.id
  });
};

module.exports = {
  getUsers,
  getUser
};
```

Routes:

```js
const express = require("express");

const {
  getUsers,
  getUser
} = require("../controllers/user.controller");

const router = express.Router();

router.get("/", getUsers);
router.get("/:id", getUser);

module.exports = router;
```

---

# 17. Services

Services contain business logic.

Example:

```text
services/
└── user.service.js
```

```js
const getUsers = async () => {
  // Database logic
  return [];
};

module.exports = {
  getUsers
};
```

Controller:

```js
const userService = require("../services/user.service");

const getUsers = async (req, res) => {
  const users = await userService.getUsers();

  res.json(users);
};
```

Recommended architecture:

```text
Route
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database
```

---

# 18. Error Handling

Express provides error-handling middleware.

Example:

```js
app.use((err, req, res, next) => {
  console.error(err);

  res.status(500).json({
    message: "Internal Server Error"
  });
});
```

The error middleware has four parameters:

```js
(err, req, res, next)
```

Example:

```js
app.get("/error", async (req, res, next) => {
  try {
    throw new Error("Something went wrong");
  } catch (error) {
    next(error);
  }
});
```

---

# 19. Environment Variables

Install:

```bash
npm install dotenv
```

Create:

```text
.env
```

Example:

```env
PORT=3000
DATABASE_URL=postgresql://localhost:5432/mydb
JWT_SECRET=change-this-secret
```

Load variables:

```js
require("dotenv").config();

const port = process.env.PORT || 3000;
```

Never commit `.env`.

Add:

```text
.env
```

to:

```text
.gitignore
```

---

# 20. CORS

CORS controls which origins can communicate with your API.

Install:

```bash
npm install cors
```

Basic configuration:

```js
const cors = require("cors");

app.use(cors());
```

Specific frontend:

```js
app.use(
  cors({
    origin: "http://localhost:3000",
    credentials: true
  })
);
```

For production, configure allowed origins carefully instead of allowing every origin.

---

# 21. REST API

A REST API commonly follows resource-based URLs.

Example:

```text
GET     /api/users
GET     /api/users/:id
POST    /api/users
PUT     /api/users/:id
PATCH   /api/users/:id
DELETE  /api/users/:id
```

Example response:

```json
{
  "id": 1,
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

# 22. Authentication

Authentication determines **who the user is**.

Common methods:

* Session authentication
* JWT
* OAuth
* API keys

Example JWT flow:

```text
Login
  ↓
Validate credentials
  ↓
Generate JWT
  ↓
Client stores token
  ↓
Client sends token
  ↓
Authentication middleware
  ↓
Protected route
```

A common header is:

```text
Authorization: Bearer <token>
```

Example middleware:

```js
const authMiddleware = (req, res, next) => {
  const header = req.headers.authorization;

  if (!header) {
    return res.status(401).json({
      message: "Unauthorized"
    });
  }

  next();
};
```

---

# 23. Database Integration

Express does not force you to use a specific database.

You can use:

### SQL

* PostgreSQL
* MySQL
* MariaDB
* SQLite

### NoSQL

* MongoDB

### ORMs / ODMs

* Prisma
* Sequelize
* TypeORM
* Mongoose

Example architecture:

```text
Express
   ↓
Controller
   ↓
Service
   ↓
Prisma
   ↓
PostgreSQL
```

For your backend projects, a common stack is:

```text
Next.js
   ↓
Express.js
   ↓
Prisma
   ↓
PostgreSQL
```

---

# 24. Validation

Never trust client input.

Example validation:

```js
if (!email) {
  return res.status(400).json({
    message: "Email is required"
  });
}
```

For larger applications, use validation libraries such as:

```text
Zod
Joi
express-validator
```

Example with Zod:

```js
const userSchema = z.object({
  name: z.string().min(2),
  email: z.string().email()
});
```

---

# 25. Security

Important Express security practices:

### Validate input

Never trust:

```text
req.body
req.params
req.query
```

### Use Helmet

```bash
npm install helmet
```

```js
const helmet = require("helmet");

app.use(helmet());
```

### Rate limiting

```bash
npm install express-rate-limit
```

Example:

```js
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  limit: 100
});

app.use(limiter);
```

### Other security practices

* Hash passwords
* Use HTTPS
* Protect secrets
* Validate input
* Configure CORS
* Use secure cookies
* Implement authorization
* Avoid exposing stack traces in production
* Keep dependencies updated

---

# 26. Logging

For development:

```js
console.log("Request received");
```

For real applications, use logging libraries such as:

```text
Morgan
Pino
Winston
```

Example with Morgan:

```bash
npm install morgan
```

```js
const morgan = require("morgan");

app.use(morgan("dev"));
```

---

# 27. File Uploads

A popular package for handling multipart/form-data is:

```bash
npm install multer
```

Example:

```js
const multer = require("multer");

const upload = multer({
  dest: "uploads/"
});

app.post("/upload", upload.single("file"), (req, res) => {
  res.json({
    message: "File uploaded"
  });
});
```

For production applications, consider object storage such as:

```text
AWS S3
Cloudinary
MinIO
```

---

# 28. Testing

Backend applications should be tested.

Common tools:

```text
Jest
Vitest
Supertest
```

You can test:

* Routes
* Controllers
* Services
* Authentication
* Validation
* Database operations

Example test concept:

```text
POST /api/users
       ↓
Valid input
       ↓
201 Created
```

---

# 29. Production

Before deploying Express:

### Environment

Use production environment variables.

### Security

Enable:

```text
HTTPS
Helmet
CORS
Rate limiting
Input validation
```

### Process management

Possible tools:

```text
PM2
Docker
systemd
```

### Reverse proxy

A common architecture:

```text
Internet
   ↓
Nginx
   ↓
Express
   ↓
PostgreSQL
```

Nginx can handle:

* HTTPS
* Reverse proxy
* Static files
* Load balancing
* Request routing

---

# 30. Common Commands

Create project:

```bash
mkdir my-api
cd my-api
npm init -y
```

Install Express:

```bash
npm install express
```

Install development tools:

```bash
npm install --save-dev nodemon
```

Run:

```bash
node server.js
```

Run with nodemon:

```bash
npx nodemon server.js
```

Check installed packages:

```bash
npm list
```

Update packages:

```bash
npm update
```

Remove a package:

```bash
npm uninstall express
```

---

# 31. Best Practices

## Use a clear project structure

```text
routes
controllers
services
repositories
middleware
config
utils
```

## Use environment variables

Do not hard-code:

```text
Passwords
API keys
JWT secrets
Database URLs
```

## Validate all user input

Treat all external input as untrusted.

## Use proper HTTP status codes

Example:

```text
200 → Success
201 → Created
204 → No Content
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
409 → Conflict
422 → Validation Error
500 → Server Error
```

## Keep business logic outside routes

Avoid:

```js
app.post("/users", async (req, res) => {
  // 200 lines of code
});
```

Prefer:

```text
Route
 ↓
Controller
 ↓
Service
 ↓
Repository
```

## Use TypeScript for larger projects

TypeScript provides:

* Type safety
* Better IDE support
* Better maintainability
* Safer refactoring

---

# 32. Common Problems

## Port already in use

Error:

```text
EADDRINUSE
```

Find the process:

```bash
sudo lsof -i :3000
```

Stop it:

```bash
kill <PID>
```

---

## CORS error

Typical problem:

```text
Access-Control-Allow-Origin
```

Check:

```js
app.use(
  cors({
    origin: "http://localhost:3000",
    credentials: true
  })
);
```

Make sure the frontend origin exactly matches the configured origin.

---

## Cannot read req.body

Make sure JSON middleware is enabled:

```js
app.use(express.json());
```

---

## 404 error

Check:

```text
HTTP method
URL
Router
app.use()
Route order
```

Example:

```js
app.use("/api/users", userRoutes);
```

The route inside `userRoutes` should not incorrectly repeat `/api/users`.

---

## Environment variable undefined

Check that:

```js
require("dotenv").config();
```

is loaded before accessing:

```js
process.env.DATABASE_URL
```

---

# 33. Useful Packages

| Package              | Purpose                    |
| -------------------- | -------------------------- |
| `express`            | Web framework              |
| `cors`               | CORS handling              |
| `dotenv`             | Environment variables      |
| `helmet`             | Security headers           |
| `morgan`             | HTTP logging               |
| `pino`               | High-performance logging   |
| `express-rate-limit` | Rate limiting              |
| `multer`             | File uploads               |
| `zod`                | Validation                 |
| `joi`                | Validation                 |
| `jsonwebtoken`       | JWT                        |
| `bcrypt`             | Password hashing           |
| `cookie-parser`      | Cookie parsing             |
| `compression`        | Response compression       |
| `nodemon`            | Development server restart |
| `supertest`          | HTTP testing               |

---

# 34. Learning Roadmap

Follow this order:

```text
1. Node.js fundamentals
        ↓
2. Express installation
        ↓
3. First server
        ↓
4. Routing
        ↓
5. HTTP methods
        ↓
6. Request / Response
        ↓
7. Parameters
        ↓
8. Middleware
        ↓
9. Routers
        ↓
10. Controllers
        ↓
11. Services
        ↓
12. Error handling
        ↓
13. REST API
        ↓
14. Database
        ↓
15. Validation
        ↓
16. Authentication
        ↓
17. Authorization
        ↓
18. Security
        ↓
19. Testing
        ↓
20. Docker
        ↓
21. Nginx
        ↓
22. CI/CD
        ↓
23. Production deployment
```

---

# 35. Official Resources

### Express.js

* Official website: https://expressjs.com/
* Official documentation: https://expressjs.com/en/starter/installing.html
* Routing: https://expressjs.com/en/guide/routing.html
* Middleware: https://expressjs.com/en/guide/using-middleware.html
* Error handling: https://expressjs.com/en/guide/error-handling.html

### Node.js

* Official website: https://nodejs.org/
* Documentation: https://nodejs.org/docs/

### npm

* Official website: https://www.npmjs.com/
* Documentation: https://docs.npmjs.com/

---

# 🧠 Express.js Cheat Sheet

## Create server

```js
const express = require("express");

const app = express();

app.use(express.json());

app.get("/", (req, res) => {
  res.json({
    message: "Hello World"
  });
});

app.listen(3000);
```

## GET

```js
app.get("/users", handler);
```

## POST

```js
app.post("/users", handler);
```

## PUT

```js
app.put("/users/:id", handler);
```

## PATCH

```js
app.patch("/users/:id", handler);
```

## DELETE

```js
app.delete("/users/:id", handler);
```

## Route parameter

```js
req.params.id
```

## Query parameter

```js
req.query.search
```

## Request body

```js
req.body
```

## Header

```js
req.headers.authorization
```

## JSON response

```js
res.json(data);
```

## Status code

```js
res.status(200).json(data);
```

## Middleware

```js
app.use(middleware);
```

## Router

```js
const router = express.Router();
```

---

# 🎯 Practice Projects

After learning the fundamentals, build these projects.

### Level 1 — Hello API

Build:

```text
GET /
GET /about
GET /health
```

### Level 2 — Todo API

Features:

```text
Create todo
Get todos
Get todo
Update todo
Delete todo
```

### Level 3 — User API

Features:

```text
Register
Login
Get profile
Update profile
Delete account
```

### Level 4 — Authentication API

Implement:

```text
JWT
Password hashing
Protected routes
Roles
Authorization
```

### Level 5 — Full-stack application

Recommended architecture:

```text
Next.js
    ↓
Express.js
    ↓
Service Layer
    ↓
Prisma
    ↓
PostgreSQL
```

Add:

```text
Authentication
Authorization
Validation
Error handling
Logging
Docker
Nginx
CI/CD
```

---

# 🚀 Final Goal

The goal of learning Express.js is not only to memorize commands.

You should be able to build a production-ready backend with:

```text
                    ┌───────────────┐
                    │   Frontend    │
                    │ Next.js/React │
                    └───────┬───────┘
                            │
                           HTTP
                            │
                    ┌───────▼───────┐
                    │    Nginx      │
                    │ Reverse Proxy │
                    └───────┬───────┘
                            │
                    ┌───────▼───────┐
                    │   Express.js  │
                    │      API      │
                    └───────┬───────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
          ┌──────▼──────┐       ┌──────▼──────┐
          │ PostgreSQL  │       │    Redis    │
          └─────────────┘       └─────────────┘
```

This foundation is useful for **backend development, full-stack development, DevOps, and cloud engineering**.
