# 🏗️ API Design Learning

A practical guide to designing **clean, consistent, secure, scalable, and maintainable APIs**.

This documentation focuses on **API architecture and design decisions**, including resource modeling, URL conventions, HTTP methods, status codes, request/response formats, validation, errors, pagination, filtering, authentication, authorization, versioning, idempotency, caching, rate limiting, documentation, and production architecture.

This guide is designed to complement:

* `REST_API_LEARNING.md`
* `EXPRESS_LEARNING.md`
* `DATABASE_LEARNING.md`
* `POSTGRESQL_LEARNING.md`
* `REDIS_LEARNING.md`
* `NGINX_LEARNING.md`
* `DOCKER_LEARNING.md`
* `CI_CD_LEARNING.md`

---

## 📚 Table of Contents

1. [What is API Design?](#1-what-is-api-design)
2. [Why API Design Matters](#2-why-api-design-matters)
3. [API Design Principles](#3-api-design-principles)
4. [API Architecture](#4-api-architecture)
5. [Resource Modeling](#5-resource-modeling)
6. [Naming Conventions](#6-naming-conventions)
7. [URL Design](#7-url-design)
8. [HTTP Methods](#8-http-methods)
9. [HTTP Status Codes](#9-http-status-codes)
10. [Request Design](#10-request-design)
11. [Response Design](#11-response-design)
12. [API Response Envelope](#12-api-response-envelope)
13. [Error Design](#13-error-design)
14. [Validation Design](#14-validation-design)
15. [Pagination](#15-pagination)
16. [Filtering](#16-filtering)
17. [Sorting](#17-sorting)
18. [Searching](#18-searching)
19. [Relationships](#19-relationships)
20. [Nested Resources](#20-nested-resources)
21. [Authentication](#21-authentication)
22. [Authorization](#22-authorization)
23. [API Versioning](#23-api-versioning)
24. [Idempotency](#24-idempotency)
25. [Caching](#25-caching)
26. [Rate Limiting](#26-rate-limiting)
27. [Security](#27-security)
28. [Consistency](#28-consistency)
29. [Backward Compatibility](#29-backward-compatibility)
30. [API Documentation](#30-api-documentation)
31. [API Testing](#31-api-testing)
32. [Observability](#32-observability)
33. [Performance](#33-performance)
34. [API Architecture Patterns](#34-api-architecture-patterns)
35. [Example API Design](#35-example-api-design)
36. [Bad vs Good API Design](#36-bad-vs-good-api-design)
37. [Production Architecture](#37-production-architecture)
38. [Best Practices](#38-best-practices)
39. [Common Mistakes](#39-common-mistakes)
40. [Practice Projects](#40-practice-projects)
41. [Learning Roadmap](#41-learning-roadmap)
42. [API Design Checklist](#42-api-design-checklist)
43. [Cheat Sheet](#43-cheat-sheet)
44. [Official Resources](#44-official-resources)

---

# 1. What is API Design?

**API design** is the process of deciding how applications communicate with your backend.

It includes decisions about:

```text
URLs
HTTP methods
Request formats
Response formats
Status codes
Authentication
Authorization
Validation
Errors
Pagination
Filtering
Versioning
Security
Performance
Documentation
```

Example:

```text
Frontend
   │
   │ GET /api/v1/users
   ▼
API
   │
   ▼
Database
```

A good API should be:

* Easy to understand
* Predictable
* Consistent
* Secure
* Maintainable
* Scalable
* Well documented

---

# 2. Why API Design Matters

Poor API design creates problems later.

For example:

```text
Frontend
    ↓
API
    ↓
Mobile App
    ↓
Third-party integrations
```

If your API changes unpredictably, all clients can break.

Good API design provides:

```text
Consistency
     ↓
Easier development
     ↓
Easier testing
     ↓
Easier maintenance
     ↓
Easier scaling
```

---

# 3. API Design Principles

Important principles:

## 1. Simplicity

Keep endpoints easy to understand.

Good:

```text
GET /api/v1/users
```

Bad:

```text
GET /api/getAllUsersFromDatabase
```

---

## 2. Consistency

Use the same conventions everywhere.

For example:

```text
/api/v1/users
/api/v1/products
/api/v1/orders
```

Avoid mixing:

```text
/api/users
/api/products-list
/api/getOrders
```

---

## 3. Predictability

A developer should be able to guess how your API works.

If:

```text
GET /users/10
```

returns user 10, then similar resources should behave consistently.

---

## 4. Security

Never trust client input.

```text
Client
  ↓
Validation
  ↓
Authentication
  ↓
Authorization
  ↓
Business Logic
  ↓
Database
```

---

## 5. Backward Compatibility

Existing clients should not break unnecessarily when the API evolves.

---

# 4. API Architecture

A typical backend:

```text
                    Client
                      │
                      ▼
                 API Gateway
                      │
                      ▼
                  REST API
                      │
              ┌───────┴───────┐
              ▼               ▼
          Services         Cache
              │
              ▼
           Database
```

For a simple Express application:

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

Example:

```text
GET /api/v1/users/10
        ↓
User Router
        ↓
User Controller
        ↓
User Service
        ↓
User Repository
        ↓
PostgreSQL
```

---

# 5. Resource Modeling

REST APIs should be designed around **resources**.

Examples:

```text
users
products
orders
payments
courses
students
projects
```

A resource usually represents a business entity.

Example:

```text
User
 ├── id
 ├── name
 ├── email
 └── createdAt
```

API:

```text
/users
/users/:id
```

---

# 6. Naming Conventions

Use clear nouns.

### Recommended

```text
/users
/products
/orders
/payments
```

### Avoid

```text
/getUsers
/createProduct
/deleteOrder
/updatePayment
```

HTTP methods already describe the operation.

---

## Use plural resource names

Recommended:

```text
/users
/products
/orders
```

This makes the API predictable.

---

## Use lowercase

Recommended:

```text
/api/v1/user-profiles
```

Avoid:

```text
/api/v1/UserProfiles
```

---

## Use hyphens

Recommended:

```text
/user-profiles
/order-items
/payment-methods
```

Avoid:

```text
/user_profiles
/userProfiles
```

---

# 7. URL Design

A well-designed API might look like:

```text
/api/v1/users
/api/v1/users/10
/api/v1/users/10/orders
/api/v1/products
/api/v1/products/25
```

Basic pattern:

```text
/api/{version}/{resource}
```

Example:

```text
/api/v1/users
```

---

# 8. HTTP Methods

Use HTTP methods according to their semantics.

| Method | Purpose          |
| ------ | ---------------- |
| GET    | Retrieve         |
| POST   | Create / submit  |
| PUT    | Replace          |
| PATCH  | Partially modify |
| DELETE | Remove           |

Example:

```text
GET    /users
POST   /users
GET    /users/10
PUT    /users/10
PATCH  /users/10
DELETE /users/10
```

---

## PUT vs PATCH

### PUT

Use when replacing the representation of a resource.

```http
PUT /users/10
```

```json
{
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

### PATCH

Use for partial modifications.

```http
PATCH /users/10
```

```json
{
  "name": "Wassel"
}
```

---

# 9. HTTP Status Codes

Use status codes consistently.

### Success

```text
200 OK
201 Created
202 Accepted
204 No Content
```

### Client errors

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Content
429 Too Many Requests
```

### Server errors

```text
500 Internal Server Error
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout
```

Example:

```http
POST /api/v1/users

HTTP/1.1 201 Created
```

---

# 10. Request Design

A request can contain:

```text
Path parameters
Query parameters
Headers
Body
```

Example:

```http
POST /api/v1/users?source=web
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

# 11. Response Design

Responses should be predictable.

Example:

```json
{
  "id": 10,
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

For collections:

```json
[
  {
    "id": 1,
    "name": "User 1"
  },
  {
    "id": 2,
    "name": "User 2"
  }
]
```

For larger APIs, a consistent response structure can be useful.

---

# 12. API Response Envelope

One possible convention:

```json
{
  "success": true,
  "data": {
    "id": 10,
    "name": "Wassel"
  }
}
```

Collection:

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Wassel"
    }
  ]
}
```

Pagination:

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

The important rule is **consistency**. Choose a response convention and use it throughout the API.

---

# 13. Error Design

Errors should be predictable.

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

Validation:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request",
    "details": [
      {
        "field": "email",
        "message": "Invalid email address"
      }
    ]
  }
}
```

---

## Error codes

Prefer stable machine-readable codes:

```text
USER_NOT_FOUND
EMAIL_ALREADY_EXISTS
INVALID_CREDENTIALS
VALIDATION_ERROR
UNAUTHORIZED
FORBIDDEN
```

The frontend can use these codes instead of depending on English error messages.

---

# 14. Validation Design

Validation should happen before business logic.

```text
Request
   ↓
Parse
   ↓
Validate
   ↓
Authenticate
   ↓
Authorize
   ↓
Business Logic
   ↓
Database
```

Example:

```json
{
  "name": "",
  "email": "invalid"
}
```

Response:

```http
422 Unprocessable Content
```

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "details": [
      {
        "field": "name",
        "message": "Name is required"
      },
      {
        "field": "email",
        "message": "Invalid email"
      }
    ]
  }
}
```

---

# 15. Pagination

Never return millions of records in one response.

Bad:

```text
GET /users
```

returning:

```text
10,000,000 users
```

Better:

```text
GET /users?page=1&limit=20
```

Response:

```json
{
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 10000,
    "totalPages": 500
  }
}
```

---

## Offset pagination

```text
?page=2&limit=20
```

Easy to implement.

Suitable for many applications.

---

## Cursor pagination

Example:

```text
/users?limit=20&cursor=eyJpZCI6MTAwfQ
```

Useful for:

* Large datasets
* Infinite scrolling
* High-volume systems

---

# 16. Filtering

Filtering allows clients to request specific data.

Example:

```text
GET /products?category=laptop
```

Multiple:

```text
GET /products?category=laptop&brand=lenovo
```

Numeric filters:

```text
GET /products?minPrice=500&maxPrice=1500
```

Keep filtering syntax documented and consistent.

---

# 17. Sorting

Example:

```text
GET /products?sort=price
```

Descending:

```text
GET /products?sort=-price
```

Multiple:

```text
GET /products?sort=category,price
```

Only allow supported fields.

Do not blindly pass arbitrary query fields into database queries.

---

# 18. Searching

Example:

```text
GET /products?search=laptop
```

For simple systems, database search may be enough.

For larger systems:

```text
API
 ↓
Search Service
 ↓
Elasticsearch / OpenSearch
```

or use database-native full-text search where appropriate.

---

# 19. Relationships

Resources often have relationships.

Example:

```text
User
 ├── Orders
 └── Projects

Order
 ├── User
 └── Products
```

Possible APIs:

```text
GET /users/10/orders
GET /orders/25
GET /orders/25/products
```

---

# 20. Nested Resources

Nested URLs can represent relationships.

Example:

```text
/users/10/orders
```

Meaning:

> Orders belonging to user 10.

Another:

```text
/orders/25/items
```

Meaning:

> Items belonging to order 25.

Avoid excessive nesting.

Bad:

```text
/users/10/orders/25/items/3/products/7
```

Prefer:

```text
/orders/25/items/3
```

when the deeper resource can be uniquely identified independently.

---

# 21. Authentication

Authentication verifies identity.

Common approaches:

```text
Sessions
JWT
OAuth 2.0
API Keys
```

Example:

```text
POST /api/v1/auth/login
```

Request:

```json
{
  "email": "user@example.com",
  "password": "password"
}
```

Response:

```json
{
  "accessToken": "TOKEN"
}
```

---

# 22. Authorization

Authorization determines what an authenticated user can do.

Example roles:

```text
USER
ADMIN
MANAGER
MODERATOR
```

Example:

```text
GET /api/v1/users
```

Maybe:

```text
ADMIN → allowed
USER  → forbidden
```

Return:

```http
403 Forbidden
```

Authorization can also be based on permissions rather than roles:

```text
users:read
users:create
users:update
users:delete
```

---

# 23. API Versioning

Versioning protects clients from breaking changes.

Example:

```text
/api/v1/users
/api/v2/users
```

A breaking change might include:

```text
Changing field meaning
Removing fields
Changing required fields
Changing response structure
Changing authentication requirements
```

Do not version for every tiny improvement.

---

## Versioning strategies

### URL versioning

```text
/api/v1/users
```

Simple and visible.

### Header versioning

```http
Accept: application/vnd.example.v1+json
```

More complex.

### Query versioning

```text
/api/users?version=1
```

Less common.

For learning and many practical projects, URL versioning is straightforward.

---

# 24. Idempotency

Idempotency matters when clients retry requests.

Example:

```text
POST /payments
```

Suppose the network fails after the payment is processed.

The client retries.

Without protection:

```text
Payment 1
Payment 2
```

Potential duplicate payment.

Use:

```http
Idempotency-Key: 8f4c7d2a
```

The server stores the result associated with the key.

A retry using the same key can return the original result instead of creating a duplicate operation.

---

# 25. Caching

Caching reduces expensive work.

Example:

```text
Client
  ↓
API
  ↓
Redis
  ↓
PostgreSQL
```

Cacheable data might include:

```text
Product catalog
Public configuration
Popular content
Frequently requested resources
```

HTTP can also use cache headers:

```http
Cache-Control: max-age=300
ETag: "abc123"
```

Be careful caching:

```text
Private data
Authentication responses
Sensitive information
```

---

# 26. Rate Limiting

Rate limiting protects APIs from abuse.

Example:

```text
100 requests / 15 minutes
```

Different endpoints can have different limits.

Example:

```text
GET /products
→ 1000 requests/hour

POST /auth/login
→ 10 requests/minute
```

Especially protect:

```text
Login
Register
Password reset
OTP
Payment
Public API endpoints
```

---

# 27. Security

A well-designed API should consider:

### HTTPS

Encrypt traffic.

```text
HTTPS
```

instead of plain HTTP in production.

### Authentication

Verify identity.

### Authorization

Verify permissions.

### Input validation

Never trust:

```text
Body
Query
Parameters
Headers
Cookies
```

### Rate limiting

Prevent abuse.

### Password security

Never store plain-text passwords.

Use a suitable password hashing algorithm and secure credential handling.

### SQL injection protection

Use:

```text
Parameterized queries
ORMs
Safe database APIs
```

### Secrets

Never commit:

```text
Database passwords
API keys
JWT secrets
Cloud credentials
```

---

# 28. Consistency

Consistency is one of the most important API design principles.

If one endpoint returns:

```json
{
  "data": {},
  "error": null
}
```

while another returns:

```json
{
  "result": {},
  "message": "success"
}
```

developers will have difficulty using the API.

Choose a convention.

For example:

```json
{
  "success": true,
  "data": {}
}
```

and:

```json
{
  "success": false,
  "error": {}
}
```

---

# 29. Backward Compatibility

Suppose version 1 returns:

```json
{
  "id": 1,
  "name": "Wassel"
}
```

Adding a new optional field is usually less disruptive:

```json
{
  "id": 1,
  "name": "Wassel",
  "avatar": "..."
}
```

Removing `name` could break clients.

Avoid breaking changes whenever possible.

---

## Safer evolution

Instead of:

```text
Remove old field
```

consider:

```text
Add new field
 ↓
Mark old field deprecated
 ↓
Notify clients
 ↓
Provide migration period
 ↓
Remove in a planned breaking version
```

---

# 30. API Documentation

Every production API should have documentation.

Document:

```text
Endpoint
Method
Description
Authentication
Parameters
Query parameters
Request body
Response
Status codes
Errors
Examples
```

Example:

```text
POST /api/v1/users

Authentication:
Required

Request:

{
  "name": "Wassel",
  "email": "wassel@example.com"
}

Response:

201 Created

{
  "id": 10,
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

## OpenAPI

OpenAPI can describe APIs in a machine-readable format.

Example:

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0

paths:
  /users:
    get:
      summary: Get users
```

Tools can generate:

```text
Interactive documentation
Client SDKs
Validation
API testing support
```

---

# 31. API Testing

Test APIs at multiple levels.

### Unit tests

Test individual functions.

```text
Service
Validation
Utility
```

### Integration tests

Test components together.

```text
API + Database
```

### End-to-end tests

Test the complete flow.

```text
Client
 ↓
API
 ↓
Database
```

Example:

```text
POST /users
   ↓
User created
   ↓
GET /users/:id
   ↓
User returned
```

---

# 32. Observability

Production APIs need visibility.

Three important areas:

```text
Logs
Metrics
Traces
```

---

## Logs

Record:

```text
Request
Response status
Errors
Authentication failures
Important business events
```

---

## Metrics

Monitor:

```text
Request count
Error rate
Latency
CPU
Memory
Database performance
```

---

## Tracing

Distributed tracing helps follow a request across services.

```text
Frontend
   ↓
API Gateway
   ↓
Service A
   ↓
Service B
   ↓
Database
```

A trace can connect these operations.

---

# 33. Performance

API performance depends on multiple components.

```text
Client
 ↓
Network
 ↓
Reverse Proxy
 ↓
API
 ↓
Cache
 ↓
Database
```

Improve performance with:

### Database indexes

Index frequently queried fields.

### Pagination

Avoid huge responses.

### Caching

Use Redis or HTTP caching where appropriate.

### Compression

Reduce response size when appropriate.

### Connection pooling

Reuse database connections.

### Avoid N+1 queries

Instead of:

```text
1 query for users
+
100 queries for orders
```

design database access to avoid unnecessary repeated queries.

---

# 34. API Architecture Patterns

## Layered architecture

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

Good for many business applications.

---

## Modular architecture

Organize by business domain:

```text
src/
├── users/
│   ├── user.routes.ts
│   ├── user.controller.ts
│   ├── user.service.ts
│   └── user.repository.ts
│
├── orders/
│   ├── order.routes.ts
│   ├── order.controller.ts
│   ├── order.service.ts
│   └── order.repository.ts
│
└── products/
    ├── product.routes.ts
    ├── product.controller.ts
    ├── product.service.ts
    └── product.repository.ts
```

This can scale better than organizing everything only by technical layer.

---

## Microservices

Large systems may separate domains:

```text
API Gateway
     │
 ┌───┼──────────┐
 ▼   ▼          ▼
User Order    Payment
Service Service Service
```

Do not choose microservices simply because they are popular.

A modular monolith can be a better starting point for many applications.

---

# 35. Example API Design

Let's design a simple e-commerce API.

## Resources

```text
users
products
categories
orders
order-items
```

---

## Users

```text
GET    /api/v1/users
GET    /api/v1/users/:id
POST   /api/v1/users
PATCH  /api/v1/users/:id
DELETE /api/v1/users/:id
```

---

## Products

```text
GET    /api/v1/products
GET    /api/v1/products/:id
POST   /api/v1/products
PATCH  /api/v1/products/:id
DELETE /api/v1/products/:id
```

Filtering:

```text
GET /api/v1/products?category=laptops
```

Pagination:

```text
GET /api/v1/products?page=1&limit=20
```

Sorting:

```text
GET /api/v1/products?sort=-price
```

---

## Orders

```text
GET    /api/v1/orders
GET    /api/v1/orders/:id
POST   /api/v1/orders
PATCH  /api/v1/orders/:id
```

User orders:

```text
GET /api/v1/users/:id/orders
```

---

# 36. Bad vs Good API Design

## ❌ Bad

```text
GET /getAllUsers
POST /createUser
POST /deleteUser
GET /getProductById?id=10
```

## ✅ Better

```text
GET    /users
POST   /users
DELETE /users/10
GET    /products/10
```

---

## ❌ Inconsistent responses

```json
{
  "users": []
}
```

Another endpoint:

```json
{
  "results": []
}
```

Another:

```json
{
  "data": []
}
```

## ✅ Consistent

```json
{
  "data": []
}
```

---

## ❌ Poor error

```json
{
  "message": "Something went wrong"
}
```

## ✅ Better

```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found"
  }
}
```

---

# 37. Production Architecture

A production API might look like:

```text
                         Internet
                            │
                            ▼
                    ┌───────────────┐
                    │      DNS      │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │     Nginx     │
                    │ Reverse Proxy │
                    └───────┬───────┘
                            │
                     ┌──────▼──────┐
                     │ API / Load  │
                     │  Balancer   │
                     └──────┬──────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ API #1   │  │ API #2   │  │ API #3   │
        │ Express  │  │ Express  │  │ Express  │
        └────┬─────┘  └────┬─────┘  └────┬─────┘
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                     ┌───────────┐
                     │   Redis   │
                     └─────┬─────┘
                           │
                     ┌─────▼─────┐
                     │PostgreSQL │
                     └───────────┘
```

Supporting infrastructure:

```text
Docker
GitHub Actions
Monitoring
Logging
HTTPS
Secrets Management
Backups
```

---

# 38. Best Practices

## URL

Use:

```text
/api/v1/users
```

Not:

```text
/api/getUsers
```

## HTTP methods

Use them according to their semantics.

## Status codes

Return meaningful status codes.

## Validation

Validate every external input.

## Authentication

Protect private resources.

## Authorization

Check permissions.

## Pagination

Paginate large collections.

## Filtering

Allow controlled filtering.

## Rate limiting

Protect expensive endpoints.

## Caching

Cache appropriate data.

## Documentation

Document every public endpoint.

## Logging

Log important events without exposing secrets.

## Monitoring

Monitor latency, errors, traffic, and infrastructure.

## Versioning

Plan for API evolution.

## Consistency

Use the same conventions everywhere.

---

# 39. Common Mistakes

### Mistake 1 — Using verbs in URLs

```text
/createUser
/getUsers
```

Prefer:

```text
POST /users
GET /users
```

---

### Mistake 2 — Returning 200 for everything

Bad:

```http
HTTP/1.1 200 OK
```

for validation errors.

Use an appropriate client error status.

---

### Mistake 3 — No validation

Never directly trust:

```text
req.body
req.query
req.params
```

---

### Mistake 4 — Exposing internal errors

Do not return:

```text
Database connection failed at /home/server/app...
```

to clients.

Log internal details on the server and return a safe error response.

---

### Mistake 5 — No pagination

Avoid:

```text
GET /users
```

returning an unlimited collection.

---

### Mistake 6 — Inconsistent naming

Avoid:

```text
/user
/products
/getOrders
/order-list
```

Use consistent resource naming.

---

### Mistake 7 — No API version strategy

Large APIs need a plan for breaking changes.

---

### Mistake 8 — Overengineering

Do not automatically use:

```text
Microservices
Event-driven architecture
Multiple databases
Complex API gateways
```

for a small application.

Start simple and evolve when requirements justify it.

---

# 40. Practice Projects

## 🟢 Project 1 — Todo API

Design:

```text
GET    /api/v1/todos
GET    /api/v1/todos/:id
POST   /api/v1/todos
PATCH  /api/v1/todos/:id
DELETE /api/v1/todos/:id
```

Add:

```text
Validation
Pagination
Filtering
Errors
```

---

## 🟡 Project 2 — User Management API

Resources:

```text
users
roles
permissions
```

Add:

```text
Authentication
Authorization
Validation
Rate limiting
```

---

## 🟠 Project 3 — Blog API

Resources:

```text
users
posts
comments
categories
```

Design relationships:

```text
User → Posts
Post → Comments
Post → Categories
```

---

## 🔴 Project 4 — E-commerce API

Resources:

```text
users
products
categories
cart
orders
payments
```

Implement:

```text
Authentication
Authorization
Pagination
Filtering
Sorting
Searching
Caching
Rate limiting
Idempotency
```

---

## ☁️ Project 5 — Production Cloud API

Architecture:

```text
Next.js
   ↓
Nginx
   ↓
Express
   ↓
Redis
   ↓
PostgreSQL
```

Add:

```text
Docker
GitHub Actions
HTTPS
Monitoring
Logging
Backups
Cloud deployment
```

---

# 41. Learning Roadmap

Follow this order:

```text
1. HTTP
   ↓
2. REST
   ↓
3. Resources
   ↓
4. URL design
   ↓
5. HTTP methods
   ↓
6. Status codes
   ↓
7. Request design
   ↓
8. Response design
   ↓
9. Error design
   ↓
10. Validation
   ↓
11. Pagination
   ↓
12. Filtering
   ↓
13. Sorting
   ↓
14. Relationships
   ↓
15. Authentication
   ↓
16. Authorization
   ↓
17. Versioning
   ↓
18. Idempotency
   ↓
19. Caching
   ↓
20. Rate limiting
   ↓
21. Security
   ↓
22. Testing
   ↓
23. Documentation
   ↓
24. Observability
   ↓
25. Performance
   ↓
26. Deployment
```

---

# 42. API Design Checklist

Before releasing an API, check:

## Design

* [ ] Resources are clearly defined
* [ ] URLs use consistent naming
* [ ] HTTP methods are used correctly
* [ ] Status codes are meaningful
* [ ] API versioning strategy exists

## Requests

* [ ] Request schemas are documented
* [ ] Input is validated
* [ ] Query parameters are controlled
* [ ] Authentication is defined

## Responses

* [ ] Response format is consistent
* [ ] Error format is consistent
* [ ] Sensitive information is not exposed
* [ ] Pagination is implemented where needed

## Security

* [ ] HTTPS is enabled
* [ ] Authentication is secure
* [ ] Authorization is enforced
* [ ] Rate limiting is configured
* [ ] Secrets are protected
* [ ] Input is validated
* [ ] Database queries are safe

## Performance

* [ ] Database indexes are considered
* [ ] Pagination is used
* [ ] Caching is considered
* [ ] N+1 queries are avoided
* [ ] Response sizes are reasonable

## Operations

* [ ] Logs exist
* [ ] Metrics exist
* [ ] Errors are monitored
* [ ] API documentation exists
* [ ] Backups exist
* [ ] Deployment process is documented

---

# 43. Cheat Sheet

## Resource design

```text
/users
/products
/orders
/payments
```

## CRUD

```text
POST   /users       → Create
GET    /users       → List
GET    /users/10    → Get one
PUT    /users/10    → Replace
PATCH  /users/10    → Modify
DELETE /users/10    → Delete
```

## Query

```text
?page=1
&limit=20
&sort=-createdAt
&search=wassel
&status=active
```

## Versioning

```text
/api/v1/users
/api/v2/users
```

## Authentication

```http
Authorization: Bearer <TOKEN>
```

## Common statuses

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
429 → Rate Limited

500 → Server Error
```

## Architecture

```text
Client
  ↓
Nginx
  ↓
API
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

# 44. Official Resources

### HTTP

* MDN HTTP: https://developer.mozilla.org/en-US/docs/Web/HTTP
* HTTP Semantics: https://httpwg.org/specs/
* RFC Editor: https://www.rfc-editor.org/

### REST

* Fielding's dissertation: https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm

### OpenAPI

* OpenAPI Specification: https://spec.openapis.org/oas/latest.html
* Swagger: https://swagger.io/

### Express.js

* Express: https://expressjs.com/
* Express documentation: https://expressjs.com/en/guide/routing.html

### Node.js

* Node.js: https://nodejs.org/
* Node.js documentation: https://nodejs.org/docs/

---

# 🎯 Final Goal

The goal of API design is not simply to create endpoints.

A good API should make this interaction predictable:

```text
                    ┌───────────────┐
                    │    Client     │
                    │ React / Next  │
                    └───────┬───────┘
                            │
                         HTTPS
                            │
                    ┌───────▼───────┐
                    │     Nginx     │
                    │ Reverse Proxy │
                    └───────┬───────┘
                            │
                    ┌───────▼───────┐
                    │   REST API    │
                    │    Express    │
                    └───────┬───────┘
                            │
                 ┌──────────┼──────────┐
                 ▼          ▼          ▼
             Service     Redis     PostgreSQL
                 │
                 ▼
             Business
               Logic
```

A well-designed API should be:

```text
Simple
Consistent
Predictable
Secure
Documented
Testable
Maintainable
Scalable
Observable
```

The objective is to design APIs that other developers can understand and use **without needing to know how the backend is implemented**.
