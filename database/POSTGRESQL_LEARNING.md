# 🐘 PostgreSQL Learning Guide

A practical guide to learning **PostgreSQL**, from the basics to advanced concepts used in real-world backend applications.

---

## 📚 Table of Contents

1. [What is PostgreSQL?](#-what-is-postgresql)
2. [Why PostgreSQL?](#-why-postgresql)
3. [PostgreSQL Architecture](#-postgresql-architecture)
4. [Installation](#-installation)
5. [PostgreSQL Commands](#-postgresql-commands)
6. [Databases, Schemas, and Tables](#-databases-schemas-and-tables)
7. [Data Types](#-data-types)
8. [Create a Table](#-create-a-table)
9. [CRUD Operations](#-crud-operations)
10. [Filtering Data](#-filtering-data)
11. [Sorting and Pagination](#-sorting-and-pagination)
12. [Aggregate Functions](#-aggregate-functions)
13. [GROUP BY and HAVING](#-group-by-and-having)
14. [JOINs](#-joins)
15. [Subqueries](#-subqueries)
16. [CTEs](#-common-table-expressions-cte)
17. [Window Functions](#-window-functions)
18. [Constraints](#-constraints)
19. [Indexes](#-indexes)
20. [EXPLAIN and Query Performance](#-explain-and-query-performance)
21. [Transactions](#-transactions)
22. [Isolation Levels](#-isolation-levels)
23. [Views](#-views)
24. [JSON and JSONB](#-json-and-jsonb)
25. [PostgreSQL Arrays](#-postgresql-arrays)
26. [Roles and Permissions](#-roles-and-permissions)
27. [Backup and Restore](#-backup-and-restore)
28. [Migrations](#-migrations)
29. [PostgreSQL with Node.js](#-postgresql-with-nodejs)
30. [PostgreSQL with Prisma](#-postgresql-with-prisma)
31. [Docker](#-postgresql-with-docker)
32. [Security](#-security)
33. [Performance Best Practices](#-performance-best-practices)
34. [Common Errors](#-common-errors)
35. [Useful Cheat Sheet](#-cheat-sheet)
36. [Learning Path](#-learning-path)
37. [Official Documentation](#-official-documentation)

---

# 🐘 What is PostgreSQL?

**PostgreSQL** is an open-source **object-relational database management system (ORDBMS)**.

It is commonly used for:

* Web applications
* Backend APIs
* SaaS applications
* Financial systems
* Analytics
* Enterprise applications
* Cloud applications
* Data-intensive applications

PostgreSQL uses **SQL** as its primary query language.

Example:

```sql
SELECT *
FROM users;
```

---

# ⭐ Why PostgreSQL?

PostgreSQL provides:

* SQL support
* ACID transactions
* Foreign keys
* Constraints
* Indexes
* JOINs
* JSON/JSONB
* Arrays
* Views
* CTEs
* Window functions
* Full-text search
* Extensions
* Replication
* Strong concurrency support

It works very well with backend technologies such as:

```text
Next.js
Node.js
Express
NestJS
FastAPI
Django
Spring Boot
Laravel
Ruby on Rails
```

---

# 🏗️ PostgreSQL Architecture

A simplified PostgreSQL architecture:

```text
Application
     │
     ▼
Database Driver
     │
     ▼
PostgreSQL Server
     │
     ├── Database
     │     ├── Schema
     │     │     ├── Tables
     │     │     ├── Views
     │     │     ├── Indexes
     │     │     └── Functions
     │
     └── Roles / Permissions
```

Example:

```text
PostgreSQL Server
│
├── postgres
│
├── myapp
│   ├── public
│   │   ├── users
│   │   ├── products
│   │   └── orders
│   │
│   └── analytics
│       └── reports
│
└── another_database
```

---

# 💻 Installation

## Ubuntu / Debian

Install PostgreSQL:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

Check the service:

```bash
sudo systemctl status postgresql
```

Start PostgreSQL:

```bash
sudo systemctl start postgresql
```

Enable PostgreSQL at startup:

```bash
sudo systemctl enable postgresql
```

Check the installed client:

```bash
psql --version
```

---

# 🔌 Connecting to PostgreSQL

Switch to the PostgreSQL system user:

```bash
sudo -u postgres psql
```

You should see:

```text
postgres=#
```

Exit:

```sql
\q
```

---

# 🧰 PostgreSQL Commands

Inside `psql`:

### List databases

```sql
\l
```

### Connect to a database

```sql
\c mydatabase
```

### List tables

```sql
\dt
```

### Describe a table

```sql
\d users
```

### List schemas

```sql
\dn
```

### List users/roles

```sql
\du
```

### Show current database

```sql
SELECT current_database();
```

### Show current user

```sql
SELECT current_user;
```

### Exit

```sql
\q
```

---

# 🗄️ Databases, Schemas, and Tables

PostgreSQL has several organizational levels:

```text
PostgreSQL Server
       │
       ▼
    Database
       │
       ▼
     Schema
       │
       ▼
     Tables
       │
       ├── Columns
       └── Rows
```

---

## Create a Database

```sql
CREATE DATABASE myapp;
```

Connect:

```sql
\c myapp
```

---

## Delete a Database

```sql
DROP DATABASE myapp;
```

⚠️ This permanently removes the database and its data.

---

# 📂 Schemas

Create a schema:

```sql
CREATE SCHEMA analytics;
```

Create a table inside it:

```sql
CREATE TABLE analytics.events (
    id SERIAL PRIMARY KEY,
    event_name TEXT NOT NULL
);
```

List schemas:

```sql
\dn
```

---

# 📋 Tables

Create a table:

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

View tables:

```sql
\dt
```

Describe:

```sql
\d users
```

---

# 🔤 Data Types

Common PostgreSQL data types:

| Type          | Example                                |
| ------------- | -------------------------------------- |
| `INTEGER`     | `25`                                   |
| `BIGINT`      | `1000000`                              |
| `NUMERIC`     | `1250.50`                              |
| `REAL`        | `10.5`                                 |
| `VARCHAR`     | `"Wassel"`                             |
| `TEXT`        | `"Hello"`                              |
| `BOOLEAN`     | `true`                                 |
| `DATE`        | `2026-09-23`                           |
| `TIMESTAMP`   | `2026-09-23 15:30:00`                  |
| `TIMESTAMPTZ` | Timestamp with timezone                |
| `UUID`        | `550e8400-e29b-41d4-a716-446655440000` |
| `JSON`        | JSON data                              |
| `JSONB`       | Binary JSON                            |
| `TEXT[]`      | Array of text                          |

---

# 🔑 Primary Keys

A primary key uniquely identifies a row.

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL
);
```

Example:

```text
id | name
---|--------
1  | Ahmed
2  | Ali
3  | Sara
```

Each `id` must be unique.

---

# 🔗 Foreign Keys

Foreign keys create relationships between tables.

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL
);
```

```sql
CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT REFERENCES users(id),
    total NUMERIC(10,2)
);
```

Relationship:

```text
users
  │
  │ 1
  │
  └───────────< orders
                  many
```

---

# ✏️ CRUD Operations

CRUD means:

```text
C → Create
R → Read
U → Update
D → Delete
```

---

## CREATE

Insert data:

```sql
INSERT INTO users (name, email, age)
VALUES ('Wassel', 'wassel@example.com', 22);
```

Multiple rows:

```sql
INSERT INTO users (name, email, age)
VALUES
    ('Ahmed', 'ahmed@example.com', 25),
    ('Sara', 'sara@example.com', 23),
    ('Ali', 'ali@example.com', 27);
```

---

## RETURNING

PostgreSQL supports `RETURNING`.

```sql
INSERT INTO users (name, email)
VALUES ('Wassel', 'wassel@example.com')
RETURNING *;
```

Useful in backend applications.

---

# 🔎 READ

Select everything:

```sql
SELECT *
FROM users;
```

Select specific columns:

```sql
SELECT id, name, email
FROM users;
```

---

# ✏️ UPDATE

Update one row:

```sql
UPDATE users
SET age = 23
WHERE id = 1;
```

Update multiple columns:

```sql
UPDATE users
SET
    name = 'Wassel Essalah',
    age = 23
WHERE id = 1;
```

Always be careful with:

```sql
UPDATE users
SET age = 23;
```

Without `WHERE`, every row is updated.

---

# 🗑️ DELETE

Delete one row:

```sql
DELETE FROM users
WHERE id = 1;
```

Delete all rows:

```sql
DELETE FROM users;
```

⚠️ Be careful with production databases.

---

# 🔍 Filtering Data

Use `WHERE`:

```sql
SELECT *
FROM users
WHERE age > 20;
```

Multiple conditions:

```sql
SELECT *
FROM users
WHERE age >= 20
AND age <= 30;
```

---

## AND

```sql
SELECT *
FROM users
WHERE age > 20
AND name = 'Wassel';
```

---

## OR

```sql
SELECT *
FROM users
WHERE age < 20
OR age > 30;
```

---

## NOT

```sql
SELECT *
FROM users
WHERE NOT age = 20;
```

---

## IN

```sql
SELECT *
FROM users
WHERE id IN (1, 2, 3);
```

---

## BETWEEN

```sql
SELECT *
FROM users
WHERE age BETWEEN 20 AND 30;
```

---

## LIKE

```sql
SELECT *
FROM users
WHERE name LIKE 'Wa%';
```

`%` means any number of characters.

Example:

```text
Wassel
Walid
Wassim
```

---

## ILIKE

PostgreSQL provides case-insensitive matching with `ILIKE`.

```sql
SELECT *
FROM users
WHERE name ILIKE 'wa%';
```

---

## NULL

Check for `NULL`:

```sql
SELECT *
FROM users
WHERE age IS NULL;
```

Not null:

```sql
SELECT *
FROM users
WHERE age IS NOT NULL;
```

Do not use:

```sql
WHERE age = NULL
```

Use:

```sql
WHERE age IS NULL
```

---

# ↕️ Sorting and Pagination

## ORDER BY

Ascending:

```sql
SELECT *
FROM users
ORDER BY age ASC;
```

Descending:

```sql
SELECT *
FROM users
ORDER BY age DESC;
```

---

## LIMIT

```sql
SELECT *
FROM users
LIMIT 10;
```

---

## OFFSET

```sql
SELECT *
FROM users
LIMIT 10
OFFSET 20;
```

This can be used for pagination.

Example:

```text
Page 1 → LIMIT 10 OFFSET 0
Page 2 → LIMIT 10 OFFSET 10
Page 3 → LIMIT 10 OFFSET 20
```

For large datasets, **cursor/keyset pagination** is often preferable to large offsets.

Example:

```sql
SELECT *
FROM users
WHERE id > 100
ORDER BY id
LIMIT 20;
```

---

# 📊 Aggregate Functions

Common aggregate functions:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## COUNT

```sql
SELECT COUNT(*)
FROM users;
```

---

## AVG

```sql
SELECT AVG(age)
FROM users;
```

---

## SUM

```sql
SELECT SUM(total)
FROM orders;
```

---

## MIN

```sql
SELECT MIN(age)
FROM users;
```

---

## MAX

```sql
SELECT MAX(age)
FROM users;
```

---

# 📦 GROUP BY and HAVING

Example:

```sql
SELECT age, COUNT(*)
FROM users
GROUP BY age;
```

Result:

```text
age | count
----|------
20  | 5
21  | 3
22  | 8
```

---

## HAVING

`HAVING` filters groups.

```sql
SELECT age, COUNT(*)
FROM users
GROUP BY age
HAVING COUNT(*) > 5;
```

Difference:

```text
WHERE  → filters rows
HAVING → filters groups
```

---

# 🔗 JOINs

JOINs combine data from multiple tables.

Example:

```text
users
----------------
id | name

orders
----------------
id | user_id | total
```

---

## INNER JOIN

```sql
SELECT
    users.name,
    orders.total
FROM users
INNER JOIN orders
    ON users.id = orders.user_id;
```

Returns matching records.

---

## LEFT JOIN

```sql
SELECT
    users.name,
    orders.total
FROM users
LEFT JOIN orders
    ON users.id = orders.user_id;
```

Returns all users, even users without orders.

---

## RIGHT JOIN

```sql
SELECT
    users.name,
    orders.total
FROM users
RIGHT JOIN orders
    ON users.id = orders.user_id;
```

---

## FULL OUTER JOIN

```sql
SELECT
    users.name,
    orders.total
FROM users
FULL OUTER JOIN orders
    ON users.id = orders.user_id;
```

---

# 🔍 Subqueries

A subquery is a query inside another query.

Example:

```sql
SELECT *
FROM users
WHERE age > (
    SELECT AVG(age)
    FROM users
);
```

This returns users whose age is above the average.

---

# 🧩 Common Table Expressions (CTE)

CTEs use `WITH`.

```sql
WITH adult_users AS (
    SELECT *
    FROM users
    WHERE age >= 18
)
SELECT *
FROM adult_users;
```

CTEs can make complex queries easier to read.

---

## Multiple CTEs

```sql
WITH user_orders AS (
    SELECT
        user_id,
        SUM(total) AS total_spent
    FROM orders
    GROUP BY user_id
)
SELECT *
FROM user_orders
WHERE total_spent > 1000;
```

---

# 🪟 Window Functions

Window functions calculate values across related rows without grouping them into one row.

Example:

```sql
SELECT
    name,
    age,
    AVG(age) OVER () AS average_age
FROM users;
```

---

## ROW_NUMBER

```sql
SELECT
    name,
    ROW_NUMBER() OVER (ORDER BY age DESC) AS position
FROM users;
```

---

## RANK

```sql
SELECT
    name,
    age,
    RANK() OVER (ORDER BY age DESC) AS rank
FROM users;
```

Window functions are useful for:

* Rankings
* Reports
* Analytics
* Running totals
* Comparisons between rows

---

# 🔒 Constraints

Constraints protect data integrity.

Common constraints:

```text
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
DEFAULT
```

---

## NOT NULL

```sql
name TEXT NOT NULL
```

---

## UNIQUE

```sql
email TEXT UNIQUE
```

---

## CHECK

```sql
age INTEGER CHECK (age >= 18)
```

---

## DEFAULT

```sql
created_at TIMESTAMPTZ DEFAULT NOW()
```

---

## Combined Example

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    age INTEGER CHECK (age >= 18),
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

# 🚀 Indexes

Indexes improve the performance of many queries.

Example:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Then:

```sql
SELECT *
FROM users
WHERE email = 'wassel@example.com';
```

can use the index.

---

## Composite Index

```sql
CREATE INDEX idx_users_name_age
ON users(name, age);
```

The order of columns matters.

---

## Unique Index

```sql
CREATE UNIQUE INDEX idx_users_email_unique
ON users(email);
```

---

## Partial Index

```sql
CREATE INDEX idx_active_users
ON users(email)
WHERE active = TRUE;
```

---

## Delete an Index

```sql
DROP INDEX idx_users_email;
```

---

## ⚠️ Index Trade-off

Indexes can improve reads but they also:

* Consume storage
* Increase write overhead
* Need maintenance
* Are not useful for every query

Do not automatically index every column.

---

# 🔬 EXPLAIN and Query Performance

Use:

```sql
EXPLAIN
SELECT *
FROM users
WHERE email = 'wassel@example.com';
```

For actual execution information:

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'wassel@example.com';
```

You may see operations such as:

```text
Seq Scan
Index Scan
Bitmap Index Scan
Nested Loop
Hash Join
Sort
Aggregate
```

---

## Example

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE age > 20;
```

Use `EXPLAIN ANALYZE` carefully on write queries because it actually executes the statement.

---

# 🔄 Transactions

Transactions allow multiple operations to behave as one logical unit.

Example:

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

If something goes wrong:

```sql
ROLLBACK;
```

---

# 💳 Transaction Example

Imagine transferring money:

```text
Account A
    ↓
-100

Account B
    ↓
+100
```

Both operations should succeed together.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

---

# 🔐 Isolation Levels

PostgreSQL supports transaction isolation levels including:

```text
Read Committed
Repeatable Read
Serializable
```

PostgreSQL also supports:

```text
Read Uncommitted
```

which behaves like `Read Committed`.

Example:

```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- queries

COMMIT;
```

Higher isolation can provide stronger consistency but may increase contention or transaction retries.

---

# 👁️ Views

A view is a saved query.

Create:

```sql
CREATE VIEW active_users AS
SELECT id, name, email
FROM users
WHERE active = TRUE;
```

Use:

```sql
SELECT *
FROM active_users;
```

Delete:

```sql
DROP VIEW active_users;
```

---

# ⚡ Materialized Views

A materialized view stores the query result.

```sql
CREATE MATERIALIZED VIEW user_statistics AS
SELECT
    COUNT(*) AS total_users,
    AVG(age) AS average_age
FROM users;
```

Refresh:

```sql
REFRESH MATERIALIZED VIEW user_statistics;
```

Useful for expensive reports that do not need real-time results.

---

# 🧾 JSON and JSONB

PostgreSQL supports JSON data.

Example:

```sql
CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    metadata JSONB
);
```

Insert:

```sql
INSERT INTO products (name, metadata)
VALUES (
    'Laptop',
    '{"brand": "MSI", "ram": 32, "gpu": "RTX 3060"}'
);
```

Query:

```sql
SELECT *
FROM products
WHERE metadata->>'brand' = 'MSI';
```

---

## JSONB

`JSONB` is usually preferred when you need to query and index JSON data.

Example:

```sql
CREATE INDEX idx_products_metadata
ON products
USING GIN (metadata);
```

---

## JSON Operators

Get JSON value:

```sql
SELECT metadata->'brand'
FROM products;
```

Get text value:

```sql
SELECT metadata->>'brand'
FROM products;
```

Nested value:

```sql
SELECT metadata->'specs'->>'ram'
FROM products;
```

---

# 📚 PostgreSQL Arrays

PostgreSQL supports arrays.

Example:

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT,
    skills TEXT[]
);
```

Insert:

```sql
INSERT INTO users (name, skills)
VALUES (
    'Wassel',
    ARRAY['React', 'Next.js', 'Node.js']
);
```

Query:

```sql
SELECT *
FROM users
WHERE 'React' = ANY(skills);
```

---

# 👤 Roles and Permissions

PostgreSQL uses roles for authentication and authorization.

List roles:

```sql
\du
```

Create a role:

```sql
CREATE ROLE app_user WITH LOGIN PASSWORD 'strong-password';
```

Create a database:

```sql
CREATE DATABASE myapp;
```

Grant privileges:

```sql
GRANT CONNECT ON DATABASE myapp TO app_user;
```

Connect to the database:

```sql
\c myapp
```

Grant schema usage:

```sql
GRANT USAGE ON SCHEMA public TO app_user;
```

Grant table privileges:

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON ALL TABLES IN SCHEMA public
TO app_user;
```

---

# 🔐 Environment Variables

For applications, credentials should not be hardcoded.

Example `.env`:

```env
DATABASE_URL="postgresql://app_user:password@localhost:5432/myapp"
```

Never commit secrets:

```text
.env
```

Add it to `.gitignore`:

```gitignore
.env
.env.local
```

---

# 💾 Backup and Restore

## pg_dump

Backup a database:

```bash
pg_dump myapp > backup.sql
```

---

## Restore SQL Backup

```bash
psql myapp < backup.sql
```

---

## Custom Format

```bash
pg_dump -Fc myapp -f backup.dump
```

Restore:

```bash
pg_restore -d myapp backup.dump
```

---

## Backup Specific Tables

```bash
pg_dump \
  -t users \
  myapp > users.sql
```

---

# 🔄 Migrations

A migration records database schema changes.

Example:

```text
Migration 001
    ↓
Create users

Migration 002
    ↓
Add email

Migration 003
    ↓
Add orders
```

Migration tools include:

* Prisma Migrate
* Drizzle Kit
* TypeORM migrations
* Knex migrations
* Flyway
* Liquibase

Example Prisma:

```bash
npx prisma migrate dev --name add_users
```

---

# 🟢 PostgreSQL with Node.js

Install the PostgreSQL driver:

```bash
npm install pg
```

Example:

```javascript
import pg from "pg";

const { Pool } = pg;

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

const result = await pool.query(
  "SELECT * FROM users WHERE id = $1",
  [1]
);

console.log(result.rows);
```

---

# 🔒 Parameterized Queries

Avoid building SQL using string concatenation.

❌ Bad:

```javascript
const email = req.body.email;

const query = `
  SELECT *
  FROM users
  WHERE email = '${email}'
`;
```

This can create SQL injection vulnerabilities.

✅ Better:

```javascript
const result = await pool.query(
  `
  SELECT *
  FROM users
  WHERE email = $1
  `,
  [email]
);
```

Use parameters instead of directly inserting user input into SQL.

---

# ⚡ Connection Pooling

For backend applications, use a connection pool.

```javascript
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 10,
});
```

Conceptually:

```text
Node.js API
     │
     ▼
Connection Pool
 ┌──────────────┐
 │ Connection 1 │
 │ Connection 2 │
 │ Connection 3 │
 │ Connection 4 │
 └──────────────┘
     │
     ▼
 PostgreSQL
```

Pooling avoids creating a new database connection for every request.

---

# 🧩 PostgreSQL with Prisma

Install Prisma:

```bash
npm install prisma @prisma/client
```

Initialize:

```bash
npx prisma init
```

Example `.env`:

```env
DATABASE_URL="postgresql://postgres:password@localhost:5432/myapp"
```

Example Prisma schema:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id        Int      @id @default(autoincrement())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
}
```

Create a migration:

```bash
npx prisma migrate dev --name init
```

Generate Prisma Client:

```bash
npx prisma generate
```

Open Prisma Studio:

```bash
npx prisma studio
```

---

# 🐳 PostgreSQL with Docker

Run PostgreSQL:

```bash
docker run \
  --name postgres-db \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=myapp \
  -p 5432:5432 \
  -d postgres
```

Check container:

```bash
docker ps
```

Stop:

```bash
docker stop postgres-db
```

Start:

```bash
docker start postgres-db
```

Remove:

```bash
docker rm postgres-db
```

---

# 🐳 PostgreSQL with Docker Compose

Create:

```text
compose.yaml
```

```yaml
services:
  postgres:
    image: postgres
    container_name: postgres-db
    restart: unless-stopped

    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: myapp

    ports:
      - "5432:5432"

    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Start:

```bash
docker compose up -d
```

Stop:

```bash
docker compose down
```

View logs:

```bash
docker compose logs postgres
```

---

# 🏗️ PostgreSQL in a Backend Architecture

Example:

```text
                    ┌──────────────┐
                    │   Frontend   │
                    │ Next.js      │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │   Backend    │
                    │ Express      │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │ PostgreSQL   │
                    └──────────────┘
```

Example project:

```text
my-app/
├── frontend/
│   └── Next.js
│
├── backend/
│   ├── Express
│   ├── routes/
│   ├── controllers/
│   ├── services/
│   └── database/
│
└── database/
    └── migrations/
```

---

# 🔐 PostgreSQL Security

Important security practices:

### 1. Strong passwords

Use strong credentials.

### 2. Environment variables

Do not hardcode database passwords.

### 3. Least privilege

Give applications only the permissions they need.

### 4. Parameterized queries

Protect against SQL injection.

### 5. Restrict network access

Do not expose PostgreSQL publicly unless required and properly secured.

### 6. Use TLS when appropriate

Encrypt database connections when communicating over untrusted networks.

### 7. Backups

Maintain tested backups.

---

# ⚡ Performance Best Practices

## 1. Use indexes carefully

```sql
CREATE INDEX idx_users_email
ON users(email);
```

---

## 2. Select only required columns

Instead of:

```sql
SELECT *
FROM users;
```

Prefer:

```sql
SELECT id, name, email
FROM users;
```

---

## 3. Analyze slow queries

```sql
EXPLAIN ANALYZE
SELECT *
FROM users
WHERE email = 'test@example.com';
```

---

## 4. Use pagination

```sql
SELECT id, name
FROM users
ORDER BY id
LIMIT 20;
```

For large datasets, consider keyset/cursor pagination.

---

## 5. Avoid unnecessary queries

Instead of repeatedly querying the database:

```text
Request
  ↓
Query
  ↓
Query
  ↓
Query
  ↓
Query
```

design efficient data access patterns.

---

## 6. Use connection pooling

```text
Application
     ↓
Connection Pool
     ↓
PostgreSQL
```

---

## 7. Keep transactions short

Long-running transactions can hold locks and interfere with other operations.

---

# 🧠 PostgreSQL vs MySQL

| Feature          | PostgreSQL | MySQL              |
| ---------------- | ---------- | ------------------ |
| SQL              | ✅          | ✅                  |
| Open Source      | ✅          | ✅                  |
| Transactions     | ✅          | ✅                  |
| Foreign Keys     | ✅          | ✅                  |
| JSON/JSONB       | ✅          | ✅                  |
| Advanced SQL     | Strong     | Strong             |
| Arrays           | Native     | Different approach |
| Extensions       | Extensive  | Available          |
| Window Functions | ✅          | ✅                  |
| CTEs             | ✅          | ✅                  |

Both are widely used relational databases.

The choice depends on application requirements, team experience, ecosystem, and operational constraints.

---

# 🧠 PostgreSQL vs MongoDB

| PostgreSQL                 | MongoDB                         |
| -------------------------- | ------------------------------- |
| Relational                 | Document-oriented               |
| Tables                     | Collections                     |
| Rows                       | Documents                       |
| Columns                    | Fields                          |
| SQL                        | MongoDB Query API               |
| Foreign keys               | References/application logic    |
| JOINs                      | `$lookup` / application queries |
| Strong relational modeling | Flexible document modeling      |
| JSONB available            | BSON native                     |

---

# 🧪 Practical Example

Imagine a small e-commerce application.

## Users

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```

## Products

```sql
CREATE TABLE products (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10,2) NOT NULL
);
```

## Orders

```sql
CREATE TABLE orders (
    id BIGSERIAL PRIMARY KEY,
    user_id BIGINT NOT NULL REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## Order Items

```sql
CREATE TABLE order_items (
    id BIGSERIAL PRIMARY KEY,
    order_id BIGINT NOT NULL REFERENCES orders(id),
    product_id BIGINT NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL CHECK (quantity > 0)
);
```

Relationship:

```text
users
  │
  │ 1
  ▼
orders
  │
  │ 1
  ▼
order_items
  │
  │ many
  ▼
products
```

---

# 🔎 Example Query

Find orders with customer names:

```sql
SELECT
    orders.id AS order_id,
    users.name AS customer,
    orders.created_at
FROM orders
JOIN users
    ON orders.user_id = users.id
ORDER BY orders.created_at DESC;
```

---

# 📊 Example Reporting Query

Calculate total order quantity by product:

```sql
SELECT
    products.name,
    SUM(order_items.quantity) AS total_quantity
FROM order_items
JOIN products
    ON order_items.product_id = products.id
GROUP BY products.id, products.name
ORDER BY total_quantity DESC;
```

---

# 🛠️ Useful PostgreSQL Tools

## psql

Official PostgreSQL command-line client.

```bash
psql
```

---

## pgAdmin

Graphical PostgreSQL administration tool.

Useful for:

* Database management
* SQL queries
* Users
* Tables
* Monitoring

---

## DBeaver

A database management application supporting PostgreSQL and many other databases.

---

## Prisma Studio

Useful when using Prisma:

```bash
npx prisma studio
```

---

# ❌ Common Errors

## Database does not exist

```text
database "myapp" does not exist
```

Create it:

```sql
CREATE DATABASE myapp;
```

---

## Authentication failed

```text
password authentication failed
```

Check:

* Username
* Password
* Host
* Port
* PostgreSQL configuration

---

## Connection refused

```text
connection refused
```

Check PostgreSQL:

```bash
sudo systemctl status postgresql
```

---

## Port already in use

Check:

```bash
sudo ss -ltnp | grep 5432
```

Docker users can check:

```bash
docker ps
```

---

## Permission denied

Check role permissions:

```sql
\du
```

And grants:

```sql
\dp
```

---

# 🧰 PostgreSQL Cheat Sheet

## Database

```sql
CREATE DATABASE myapp;
DROP DATABASE myapp;
```

---

## Table

```sql
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    name TEXT NOT NULL
);
```

---

## Insert

```sql
INSERT INTO users (name)
VALUES ('Wassel');
```

---

## Select

```sql
SELECT *
FROM users;
```

---

## Update

```sql
UPDATE users
SET name = 'Ahmed'
WHERE id = 1;
```

---

## Delete

```sql
DELETE FROM users
WHERE id = 1;
```

---

## Count

```sql
SELECT COUNT(*)
FROM users;
```

---

## Join

```sql
SELECT *
FROM users
JOIN orders
ON users.id = orders.user_id;
```

---

## Index

```sql
CREATE INDEX idx_users_name
ON users(name);
```

---

## Transaction

```sql
BEGIN;

-- queries

COMMIT;
```

Rollback:

```sql
ROLLBACK;
```

---

## Backup

```bash
pg_dump myapp > backup.sql
```

---

## Restore

```bash
psql myapp < backup.sql
```

---

## PostgreSQL CLI

```text
\l       List databases
\c db    Connect to database
\dt      List tables
\d table Describe table
\dn      List schemas
\du      List roles
\q       Exit
```

---

# 🗺️ PostgreSQL Learning Path

A recommended learning order:

```text
1. SQL Basics
      ↓
2. PostgreSQL Installation
      ↓
3. Databases & Schemas
      ↓
4. Tables & Data Types
      ↓
5. CRUD
      ↓
6. WHERE / ORDER BY / LIMIT
      ↓
7. GROUP BY / Aggregation
      ↓
8. JOINs
      ↓
9. Constraints
      ↓
10. Indexes
      ↓
11. Transactions
      ↓
12. CTEs
      ↓
13. Window Functions
      ↓
14. JSONB
      ↓
15. Roles & Permissions
      ↓
16. Backup & Restore
      ↓
17. Performance
      ↓
18. Node.js Integration
      ↓
19. ORM / Prisma
      ↓
20. Docker
      ↓
21. Production Deployment
```

---

# 🚀 Practice Projects

After learning the fundamentals, build small projects.

## 🟢 Beginner

### 1. User Management

Features:

```text
Create user
List users
Update user
Delete user
Search users
```

---

## 🟡 Intermediate

### 2. E-commerce Database

Tables:

```text
users
products
categories
orders
order_items
payments
```

Practice:

* Foreign keys
* JOINs
* Transactions
* Indexes
* Aggregations

---

## 🟠 Intermediate

### 3. Blog API

```text
users
posts
comments
categories
tags
```

Practice:

* One-to-many
* Many-to-many
* JOINs
* Pagination
* Full-text search

---

## 🔴 Advanced

### 4. SaaS Application

Example:

```text
organizations
users
memberships
projects
tasks
subscriptions
payments
audit_logs
```

Practice:

* PostgreSQL
* Transactions
* Indexes
* Roles
* JSONB
* Migrations
* Query optimization
* Backend API

---

# 🌐 PostgreSQL in a Modern Stack

A common stack:

```text
Frontend
   │
   ▼
Next.js
   │
   ▼
Express / Node.js
   │
   ▼
Prisma / pg
   │
   ▼
PostgreSQL
   │
   ▼
Docker / Cloud Server
```

For your backend learning path, PostgreSQL is especially useful with:

```text
Next.js
Node.js
Express
Prisma
Docker
REST APIs
Authentication
SaaS
Cloud
DevOps
```

---

# 📖 Official Documentation

### PostgreSQL

🌐 https://www.postgresql.org/

### PostgreSQL Documentation

🌐 https://www.postgresql.org/docs/

### SQL Commands

🌐 https://www.postgresql.org/docs/current/sql-commands.html

### PostgreSQL Tutorial

🌐 https://www.postgresql.org/docs/current/tutorial.html

### PostgreSQL Data Types

🌐 https://www.postgresql.org/docs/current/datatype.html

### PostgreSQL Indexes

🌐 https://www.postgresql.org/docs/current/indexes.html

### PostgreSQL Transactions

🌐 https://www.postgresql.org/docs/current/tutorial-transactions.html

### PostgreSQL JSON

🌐 https://www.postgresql.org/docs/current/datatype-json.html

### PostgreSQL Client Applications

🌐 https://www.postgresql.org/docs/current/reference-client.html

---

# 🔗 Related Documentation

Inside this repository:

* [Database Learning](./DATABASE_LEARNING.md)
* [SQL vs NoSQL](./SQL_VS_NOSQL.md)
* [MongoDB Learning](./MONGODB_LEARNING.md)

---

# 📝 Summary

PostgreSQL is a powerful relational database system built around SQL.

The most important concepts to learn are:

```text
SQL
│
├── Databases
├── Schemas
├── Tables
├── Rows
├── Columns
├── Primary Keys
├── Foreign Keys
├── Constraints
├── CRUD
├── JOINs
├── Aggregations
├── Indexes
├── Transactions
├── CTEs
├── Window Functions
├── JSONB
├── Roles
├── Backups
└── Performance
```

For backend development, a strong PostgreSQL foundation combined with:

```text
Node.js
+
Express
+
Prisma
+
Docker
+
REST API
```

provides a solid foundation for building production-style applications.
