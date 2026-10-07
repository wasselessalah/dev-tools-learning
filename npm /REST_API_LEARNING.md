# 🌐 REST API Learning

A practical guide to learning **REST APIs (Representational State Transfer)**, from HTTP fundamentals to designing, building, securing, testing, documenting, and deploying production-ready APIs.

This documentation is especially useful when working with **Node.js, Express.js, Next.js, PostgreSQL, MongoDB, Docker, and cloud environments**.

---

## 📚 Table of Contents

1. [What is an API?](#1-what-is-an-api)
2. [What is REST?](#2-what-is-rest)
3. [What is a REST API?](#3-what-is-a-rest-api)
4. [Client-Server Architecture](#4-client-server-architecture)
5. [HTTP Fundamentals](#5-http-fundamentals)
6. [HTTP Methods](#6-http-methods)
7. [HTTP Status Codes](#7-http-status-codes)
8. [HTTP Headers](#8-http-headers)
9. [Request Structure](#9-request-structure)
10. [Response Structure](#10-response-structure)
11. [URL Structure](#11-url-structure)
12. [Path Parameters](#12-path-parameters)
13. [Query Parameters](#13-query-parameters)
14. [Request Body](#14-request-body)
15. [JSON](#15-json)
16. [CRUD](#16-crud)
17. [RESTful URL Design](#17-restful-url-design)
18. [Resource Naming](#18-resource-naming)
19. [API Versioning](#19-api-versioning)
20. [Authentication](#20-authentication)
21. [Authorization](#21-authorization)
22. [JWT](#22-jwt)
23. [Cookies and Sessions](#23-cookies-and-sessions)
24. [CORS](#24-cors)
25. [Validation](#25-validation)
26. [Error Handling](#26-error-handling)
27. [Pagination](#27-pagination)
28. [Filtering](#28-filtering)
29. [Sorting](#29-sorting)
30. [Searching](#30-searching)
31. [Rate Limiting](#31-rate-limiting)
32. [Caching](#32-caching)
33. [Idempotency](#33-idempotency)
34. [API Security](#34-api-security)
35. [REST API with Express.js](#35-rest-api-with-expressjs)
36. [Database Integration](#36-database-integration)
37. [Testing APIs](#37-testing-apis)
38. [API Documentation](#38-api-documentation)
39. [Production Architecture](#39-production-architecture)
40. [Best Practices](#40-best-practices)
41. [Common Problems](#41-common-problems)
42. [Practice Projects](#42-practice-projects)
43. [Learning Roadmap](#43-learning-roadmap)
44. [Cheat Sheet](#44-cheat-sheet)
45. [Official Resources](#45-official-resources)

---

# 1. What is an API?

**API** means:

> Application Programming Interface

An API allows different software applications to communicate with each other.

Example:

```text
Frontend
   │
   │ HTTP Request
   ▼
Backend API
   │
   ▼
Database
```

For example, a frontend might request:

```http
GET /api/users
```

The backend returns:

```json
[
  {
    "id": 1,
    "name": "Wassel"
  }
]
```

---

# 2. What is REST?

**REST** means:

> Representational State Transfer

REST is an architectural style for designing networked applications.

REST uses standard HTTP concepts such as:

```text
GET
POST
PUT
PATCH
DELETE
```

REST focuses on **resources**.

Examples of resources:

```text
/users
/products
/orders
/posts
/comments
```

---

# 3. What is a REST API?

A REST API is an API designed around REST principles and HTTP.

Example:

```text
GET    /api/users
GET    /api/users/10
POST   /api/users
PUT    /api/users/10
PATCH  /api/users/10
DELETE /api/users/10
```

The resource is:

```text
users
```

The HTTP method describes the operation.

---

# 4. Client-Server Architecture

A REST API usually follows a client-server architecture.

```text
┌──────────────┐
│    Client    │
│ React/Next.js│
└──────┬───────┘
       │
       │ HTTP
       ▼
┌──────────────┐
│   REST API   │
│   Express    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│   Database   │
│ PostgreSQL   │
└──────────────┘
```

The client and server are separated.

This means the frontend does not need to know how the database works.

---

# 5. HTTP Fundamentals

REST APIs commonly use **HTTP** or **HTTPS**.

Example:

```http
GET /api/users HTTP/1.1
Host: example.com
Authorization: Bearer TOKEN
```

The server responds:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "message": "Success"
}
```

---

# 6. HTTP Methods

## GET

Retrieve data.

```http
GET /api/users
```

Example:

```http
GET /api/users/10
```

---

## POST

Create a resource.

```http
POST /api/users
```

Request:

```json
{
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

## PUT

Replace an existing resource.

```http
PUT /api/users/10
```

Example:

```json
{
  "name": "Wassel Essalah",
  "email": "wassel@example.com"
}
```

---

## PATCH

Partially update a resource.

```http
PATCH /api/users/10
```

Example:

```json
{
  "name": "Wassel"
}
```

---

## DELETE

Delete a resource.

```http
DELETE /api/users/10
```

---

# 7. HTTP Status Codes

Status codes tell the client what happened.

## 2xx — Success

| Code | Meaning    |
| ---- | ---------- |
| 200  | OK         |
| 201  | Created    |
| 202  | Accepted   |
| 204  | No Content |

Example:

```http
HTTP/1.1 201 Created
```

---

## 4xx — Client Error

| Code | Meaning               |
| ---- | --------------------- |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 405  | Method Not Allowed    |
| 409  | Conflict              |
| 422  | Unprocessable Content |
| 429  | Too Many Requests     |

---

## 5xx — Server Error

| Code | Meaning               |
| ---- | --------------------- |
| 500  | Internal Server Error |
| 501  | Not Implemented       |
| 502  | Bad Gateway           |
| 503  | Service Unavailable   |
| 504  | Gateway Timeout       |

---

# 8. HTTP Headers

Headers contain additional information about requests and responses.

Example:

```http
Content-Type: application/json
Authorization: Bearer TOKEN
Accept: application/json
```

Common headers:

```text
Content-Type
Accept
Authorization
User-Agent
Cookie
Cache-Control
Origin
```

---

# 9. Request Structure

A request can contain:

```text
Method
URL
Headers
Parameters
Query
Body
```

Example:

```http
POST /api/users?source=web HTTP/1.1
Content-Type: application/json
Authorization: Bearer TOKEN
```

Body:

```json
{
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

# 10. Response Structure

A response contains:

```text
Status Code
Headers
Body
```

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "id": 1,
  "name": "Wassel"
}
```

---

# 11. URL Structure

Example:

```text
https://api.example.com/api/v1/users/10?active=true
```

Breakdown:

```text
https
  ↓
Protocol

api.example.com
  ↓
Host

/api/v1/users/10
  ↓
Path

?active=true
  ↓
Query
```

---

# 12. Path Parameters

Path parameters identify a specific resource.

Example:

```text
GET /api/users/25
```

Here:

```text
25
```

is the user ID.

Express:

```js
app.get("/api/users/:id", (req, res) => {
  const id = req.params.id;

  res.json({
    id
  });
});
```

---

# 13. Query Parameters

Query parameters are useful for:

* Filtering
* Searching
* Sorting
* Pagination

Example:

```text
GET /api/users?role=admin
```

Multiple:

```text
GET /api/users?role=admin&active=true
```

Pagination:

```text
GET /api/users?page=2&limit=20
```

---

# 14. Request Body

The request body contains data sent to the server.

Example:

```http
POST /api/users
Content-Type: application/json
```

```json
{
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

Express:

```js
app.use(express.json());

app.post("/api/users", (req, res) => {
  const { name, email } = req.body;

  res.status(201).json({
    name,
    email
  });
});
```

---

# 15. JSON

REST APIs commonly exchange data using **JSON**.

Example:

```json
{
  "id": 1,
  "name": "Wassel",
  "skills": [
    "React",
    "Node.js",
    "Express.js"
  ]
}
```

JSON supports:

```text
String
Number
Boolean
Array
Object
null
```

---

# 16. CRUD

CRUD represents the four main database operations:

```text
Create
Read
Update
Delete
```

Mapping to HTTP:

| CRUD   | HTTP      | Endpoint     |
| ------ | --------- | ------------ |
| Create | POST      | `/users`     |
| Read   | GET       | `/users`     |
| Read   | GET       | `/users/:id` |
| Update | PUT/PATCH | `/users/:id` |
| Delete | DELETE    | `/users/:id` |

---

# 17. RESTful URL Design

Prefer resource-based URLs.

### Good

```text
GET /users
GET /users/10
POST /users
PATCH /users/10
DELETE /users/10
```

### Avoid

```text
GET /getUsers
POST /createUser
GET /deleteUser/10
POST /updateUser
```

The HTTP method already describes the operation.

---

# 18. Resource Naming

Use nouns rather than verbs.

Good:

```text
/users
/products
/orders
```

Avoid:

```text
/getUsers
/createProduct
/deleteOrder
```

Use plural resource names consistently.

```text
/users
/products
/orders
```

---

# 19. API Versioning

API versions help you evolve APIs without breaking existing clients.

Example:

```text
/api/v1/users
/api/v2/users
```

A common approach:

```text
https://api.example.com/api/v1/users
```

Example:

```text
v1
├── users
├── products
└── orders

v2
├── users
├── products
└── orders
```

Do not create a new version for every small change. Version when there are meaningful breaking changes.

---

# 20. Authentication

Authentication answers:

> Who are you?

Common approaches:

```text
JWT
Sessions
OAuth 2.0
API Keys
```

Example:

```text
POST /api/auth/login
```

Request:

```json
{
  "email": "user@example.com",
  "password": "password"
}
```

Response might contain:

```json
{
  "accessToken": "TOKEN"
}
```

---

# 21. Authorization

Authorization answers:

> What are you allowed to do?

Example:

```text
User
 ├── Read profile
 └── Update profile

Admin
 ├── Read users
 ├── Update users
 └── Delete users
```

Example:

```http
DELETE /api/users/10
Authorization: Bearer TOKEN
```

The backend verifies whether the authenticated user has permission.

---

# 22. JWT

JWT means:

> JSON Web Token

Typical flow:

```text
              Login
                │
                ▼
       Validate credentials
                │
                ▼
          Generate JWT
                │
                ▼
             Client
                │
                │ Authorization header
                ▼
             API
                │
                ▼
       Verify JWT signature
                │
                ▼
          Protected route
```

Header:

```http
Authorization: Bearer <token>
```

A JWT commonly contains:

```text
Header
Payload
Signature
```

Never put sensitive secrets directly inside the JWT payload.

---

# 23. Cookies and Sessions

Another authentication approach uses sessions.

Flow:

```text
Login
  ↓
Server creates session
  ↓
Session ID stored in cookie
  ↓
Client sends cookie
  ↓
Server validates session
```

Secure cookies should generally use appropriate flags such as:

```text
HttpOnly
Secure
SameSite
```

---

# 24. CORS

CORS means:

> Cross-Origin Resource Sharing

Example:

```text
Frontend:
https://app.example.com

API:
https://api.example.com
```

The browser treats these as different origins.

The API must explicitly allow the frontend origin when cross-origin browser requests are needed.

Express:

```js
const cors = require("cors");

app.use(
  cors({
    origin: "https://app.example.com",
    credentials: true
  })
);
```

Avoid blindly using:

```js
origin: "*"
```

when credentials are required.

---

# 25. Validation

Always validate incoming data.

Bad:

```js
const user = await createUser(req.body);
```

Better:

```text
Request
   ↓
Validation
   ↓
Business logic
   ↓
Database
```

Example:

```js
const userSchema = z.object({
  name: z.string().min(2),
  email: z.string().email()
});
```

Invalid input should produce an appropriate client error.

---

# 26. Error Handling

Use consistent error responses.

Example:

```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found"
  }
}
```

Another example:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email address"
  }
}
```

Avoid exposing:

```text
Database passwords
Internal stack traces
Secret keys
Internal infrastructure details
```

in production responses.

---

# 27. Pagination

Pagination prevents APIs from returning huge amounts of data.

Example:

```text
GET /api/users?page=2&limit=20
```

Response:

```json
{
  "data": [
    {
      "id": 21,
      "name": "User 21"
    }
  ],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

---

# 28. Filtering

Example:

```text
GET /api/products?category=laptop
```

Multiple filters:

```text
GET /api/products?category=laptop&brand=lenovo
```

The API should validate filter values.

---

# 29. Sorting

Example:

```text
GET /api/products?sort=price
```

Descending:

```text
GET /api/products?sort=-price
```

Multiple sorting fields:

```text
GET /api/products?sort=category,price
```

Define and document your sorting format clearly.

---

# 30. Searching

Example:

```text
GET /api/users?search=wassel
```

Another example:

```text
GET /api/products?q=laptop
```

For large datasets, implement search efficiently using database indexes or a dedicated search system.

---

# 31. Rate Limiting

Rate limiting protects APIs from excessive requests.

Example:

```text
100 requests / 15 minutes
```

Particularly important for:

```text
Login
Register
Password reset
OTP
Public APIs
Expensive operations
```

Architecture:

```text
Client
  ↓
Rate Limiter
  ↓
REST API
```

---

# 32. Caching

Caching can improve API performance.

Example:

```text
Client
  ↓
API
  ↓
Redis
  ↓
Database
```

Instead of querying the database every time:

```text
Request
  ↓
Redis cache
  ↓
Cache hit → Response
```

If cache miss:

```text
Redis
  ↓
Database
  ↓
Redis
  ↓
Response
```

Common caching technology:

```text
Redis
```

---

# 33. Idempotency

An operation is **idempotent** when repeating the same request has the same intended effect as making it once.

Generally:

```text
GET     → Idempotent
PUT     → Idempotent
DELETE  → Idempotent
POST    → Not necessarily idempotent
```

For important operations such as payments, an **idempotency key** can help prevent duplicate processing.

Example:

```http
POST /api/payments
Idempotency-Key: 7f8c9d...
```

The server can use the key to recognize retries of the same operation.

---

# 34. API Security

Important security practices:

### HTTPS

Use HTTPS in production.

```text
HTTP ❌
HTTPS ✅
```

### Authentication

Verify user identity.

### Authorization

Verify permissions.

### Validation

Validate all input.

### Rate limiting

Protect sensitive endpoints.

### Password hashing

Never store plain-text passwords.

Use strong password hashing algorithms and appropriate libraries.

### Security headers

Use appropriate security headers.

### Secrets

Never commit:

```text
.env
API keys
Passwords
JWT secrets
Database credentials
```

### SQL Injection

Use parameterized queries or a safe ORM/database API.

### XSS

Properly handle and encode untrusted data.

### CSRF

Consider CSRF protections when using cookie-based authentication.

---

# 35. REST API with Express.js

Example project:

```text
rest-api/
├── src/
│   ├── controllers/
│   │   └── user.controller.js
│   ├── routes/
│   │   └── user.routes.js
│   ├── services/
│   │   └── user.service.js
│   ├── middleware/
│   │   └── auth.js
│   └── app.js
│
├── server.js
├── package.json
└── .env
```

Application:

```js
const express = require("express");

const app = express();

app.use(express.json());

app.get("/api/users", (req, res) => {
  res.json([
    {
      id: 1,
      name: "Wassel"
    }
  ]);
});

app.post("/api/users", (req, res) => {
  const { name } = req.body;

  res.status(201).json({
    id: 2,
    name
  });
});

app.listen(3000, () => {
  console.log("API running on port 3000");
});
```

---

# 36. Database Integration

A typical production stack:

```text
Next.js
   │
   │ HTTP
   ▼
Express REST API
   │
   ▼
Service Layer
   │
   ▼
Prisma
   │
   ▼
PostgreSQL
```

Example resource:

```text
/api/users
```

Database:

```text
users
├── id
├── name
├── email
└── created_at
```

REST API:

```text
GET     /api/users
GET     /api/users/:id
POST    /api/users
PATCH   /api/users/:id
DELETE  /api/users/:id
```

---

# 37. Testing APIs

You can test REST APIs using:

* Postman
* Insomnia
* cURL
* HTTPie
* Automated tests

### cURL

GET:

```bash
curl http://localhost:3000/api/users
```

POST:

```bash
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Wassel"}'
```

---

# 38. API Documentation

Good APIs should be documented.

Document:

```text
Endpoint
HTTP method
Description
Parameters
Headers
Request body
Response
Status codes
Authentication
Errors
```

Example:

```text
POST /api/users

Description:
Create a new user.

Authentication:
Required

Request:
{
  "name": "Wassel",
  "email": "wassel@example.com"
}

Response:
201 Created
```

A widely used API description format is:

```text
OpenAPI
```

Tools such as Swagger UI can present OpenAPI documentation interactively.

---

# 39. Production Architecture

A practical production architecture:

```text
                    Internet
                       │
                       ▼
                ┌──────────────┐
                │     DNS      │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │    Nginx     │
                │ Reverse Proxy│
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Express API  │
                └──────┬───────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
       ┌─────────────┐   ┌─────────────┐
       │ PostgreSQL  │   │    Redis    │
       └─────────────┘   └─────────────┘
```

For larger systems:

```text
                    Load Balancer
                         │
             ┌───────────┼───────────┐
             ▼           ▼           ▼
         Express 1   Express 2   Express 3
             │           │           │
             └───────────┼───────────┘
                         ▼
                    PostgreSQL
                         │
                       Redis
```

---

# 40. Best Practices

## Use nouns

```text
/users
/products
/orders
```

Instead of:

```text
/getUsers
/createUser
```

## Use HTTP methods correctly

```text
GET    → Read
POST   → Create
PUT    → Replace
PATCH  → Partial update
DELETE → Delete
```

## Return appropriate status codes

Do not return:

```http
200 OK
```

for every situation.

## Validate input

Never trust client input.

## Use HTTPS

Protect data in transit.

## Keep APIs consistent

Use consistent:

```text
URL structure
Response structure
Error structure
Naming
Pagination
Authentication
```

## Version breaking changes

Example:

```text
/api/v1
/api/v2
```

## Document the API

Use OpenAPI/Swagger or another clear documentation system.

## Log important events

Monitor:

```text
Requests
Errors
Authentication failures
Slow requests
Database errors
```

---

# 41. Common Problems

## 404 Not Found

Check:

```text
URL
HTTP method
Router
API prefix
Version
```

Example:

```text
/api/v1/users
```

is different from:

```text
/api/users
```

---

## 401 Unauthorized

Usually means the client has not provided valid authentication credentials.

Check:

```text
Authorization header
Token
Session
Cookie
```

---

## 403 Forbidden

The client may be authenticated but does not have permission.

Example:

```text
USER → cannot delete users
ADMIN → can delete users
```

---

## 400 Bad Request

Usually indicates invalid request data or malformed input.

Check:

```text
JSON
Parameters
Query
Headers
Validation
```

---

## 409 Conflict

Useful when a request conflicts with existing data.

Example:

```json
{
  "error": "EMAIL_ALREADY_EXISTS"
}
```

---

## 429 Too Many Requests

The client has exceeded a rate limit.

Check:

```text
Rate limiter
Request frequency
Retry behavior
```

---

## 500 Internal Server Error

The server encountered an unexpected problem.

Check:

```text
Server logs
Database
Environment variables
Application code
External services
```

---

# 42. Practice Projects

## 🟢 Level 1 — Todo REST API

Create:

```text
GET    /api/todos
GET    /api/todos/:id
POST   /api/todos
PATCH  /api/todos/:id
DELETE /api/todos/:id
```

Features:

```text
Create todo
Read todo
Update todo
Delete todo
```

---

## 🟡 Level 2 — User Management API

Features:

```text
Register
Login
Profile
Update profile
Delete account
```

Endpoints:

```text
POST   /api/auth/register
POST   /api/auth/login
GET    /api/users/me
PATCH  /api/users/me
DELETE /api/users/me
```

---

## 🟠 Level 3 — Blog API

Resources:

```text
Users
Posts
Comments
Categories
```

Endpoints:

```text
GET    /api/posts
GET    /api/posts/:id
POST   /api/posts
PATCH  /api/posts/:id
DELETE /api/posts/:id
```

---

## 🔴 Level 4 — E-commerce API

Resources:

```text
Users
Products
Categories
Cart
Orders
Payments
```

Add:

```text
Authentication
Authorization
Pagination
Filtering
Searching
Validation
Rate limiting
Caching
```

---

## ☁️ Level 5 — Production Cloud API

Build:

```text
Next.js
   ↓
Nginx
   ↓
Express
   ↓
PostgreSQL
   ↓
Redis
```

Then add:

```text
Docker
CI/CD
GitHub Actions
HTTPS
Monitoring
Logging
Cloud deployment
```

---

# 43. Learning Roadmap

Follow this order:

```text
1. HTTP fundamentals
        ↓
2. HTTP methods
        ↓
3. Status codes
        ↓
4. Headers
        ↓
5. JSON
        ↓
6. URLs
        ↓
7. Path parameters
        ↓
8. Query parameters
        ↓
9. Request body
        ↓
10. CRUD
        ↓
11. RESTful design
        ↓
12. Express.js
        ↓
13. Database
        ↓
14. Validation
        ↓
15. Error handling
        ↓
16. Authentication
        ↓
17. Authorization
        ↓
18. Security
        ↓
19. Pagination / Filtering
        ↓
20. Caching
        ↓
21. Testing
        ↓
22. OpenAPI
        ↓
23. Docker
        ↓
24. Nginx
        ↓
25. CI/CD
        ↓
26. Cloud deployment
```

---

# 44. Cheat Sheet

## HTTP Methods

```text
GET     → Read
POST    → Create
PUT     → Replace
PATCH   → Update partially
DELETE  → Delete
```

## Status Codes

```text
200 → OK
201 → Created
204 → No Content

400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
409 → Conflict
422 → Validation Error
429 → Too Many Requests

500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
504 → Gateway Timeout
```

## URL Examples

```text
GET /api/users

GET /api/users/10

POST /api/users

PATCH /api/users/10

DELETE /api/users/10
```

## Query Examples

```text
?page=2
&limit=20
&sort=name
&search=wassel
&active=true
```

## Authentication

```http
Authorization: Bearer <TOKEN>
```

---

# 45. Official Resources

### REST / HTTP

* MDN HTTP documentation: https://developer.mozilla.org/en-US/docs/Web/HTTP
* HTTP Semantics: https://httpwg.org/specs/
* RFC Editor: https://www.rfc-editor.org/

### Express.js

* Express official website: https://expressjs.com/
* Express routing: https://expressjs.com/en/guide/routing.html
* Express middleware: https://expressjs.com/en/guide/using-middleware.html

### OpenAPI

* OpenAPI Specification: https://spec.openapis.org/oas/latest.html
* Swagger: https://swagger.io/

### Node.js

* Node.js official documentation: https://nodejs.org/docs/

---

# 🎯 Final Goal

After completing this documentation, you should be able to design and build a REST API such as:

```text
                     ┌──────────────┐
                     │  Next.js     │
                     │   Frontend   │
                     └──────┬───────┘
                            │
                           HTTPS
                            │
                     ┌──────▼───────┐
                     │    Nginx     │
                     │ Reverse Proxy│
                     └──────┬───────┘
                            │
                     ┌──────▼───────┐
                     │   Express    │
                     │   REST API   │
                     └──────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
        ┌──────────┐   ┌─────────┐   ┌──────────┐
        │PostgreSQL│   │  Redis  │   │ External │
        │          │   │         │   │ Services │
        └──────────┘   └─────────┘   └──────────┘
```

The main objective is to understand not only **how to create endpoints**, but how to design a **secure, consistent, scalable, documented, and production-ready API**.
