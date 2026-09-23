# 🗄️ Database Learning

A practical guide to understanding **databases**, **DBMS**, **SQL**, **NoSQL**, database design, security, performance, backups, and how databases are used in modern applications.

---

## 📌 What is a Database?

A **database** is an organized collection of data that can be stored, accessed, managed, and updated efficiently.

For example, an application might store:

```text
Users
Products
Orders
Payments
Messages
Invoices
```

Instead of storing this information in separate files manually, a database provides tools to manage the data efficiently.

---

# 🧠 Why Do We Need Databases?

Applications need databases to:

* Store information
* Retrieve information
* Update information
* Delete information
* Search large amounts of data
* Maintain relationships between data
* Protect data
* Handle multiple users
* Support transactions
* Provide reliable backups

Example:

```text
User
  ↓
Application
  ↓
Database
  ↓
PostgreSQL
```

---

# 🏗️ Database Architecture

A typical web application can look like:

```text
┌──────────────┐
│   Frontend   │
│ React/Next.js│
└──────┬───────┘
       │
       │ HTTP
       ▼
┌──────────────┐
│   Backend    │
│ Node/Express │
└──────┬───────┘
       │
       │ Database connection
       ▼
┌──────────────┐
│   Database   │
│ PostgreSQL   │
└──────────────┘
```

The frontend normally should **not connect directly to the database**.

The backend handles database operations and application logic.

---

# 🗃️ Database vs DBMS

These terms are related but different.

## Database

The actual collection of stored data.

Example:

```text
users
products
orders
```

## DBMS

**Database Management System** is the software used to manage databases.

Examples:

```text
PostgreSQL
MySQL
MongoDB
Microsoft SQL Server
Oracle Database
SQLite
```

Think:

```text
Database
    ↓
Data

DBMS
    ↓
Software that manages the data
```

---

# 🔢 Types of Databases

There are several major database models.

```text
Databases
│
├── Relational / SQL
│   ├── PostgreSQL
│   ├── MySQL
│   ├── MariaDB
│   └── SQL Server
│
├── Document
│   ├── MongoDB
│   └── CouchDB
│
├── Key-Value
│   ├── Redis
│   └── Amazon DynamoDB
│
├── Graph
│   └── Neo4j
│
└── Wide-Column
    ├── Cassandra
    └── ScyllaDB
```

---

# 🟦 SQL Databases

SQL databases are generally **relational databases**.

They organize data into:

```text
Database
   ↓
Tables
   ↓
Rows
   ↓
Columns
```

Example:

```text
users

+----+--------+-------------------+
| id | name   | email             |
+----+--------+-------------------+
| 1  | Ali    | ali@example.com   |
| 2  | Sara   | sara@example.com  |
+----+--------+-------------------+
```

Popular SQL databases:

* PostgreSQL
* MySQL
* MariaDB
* Microsoft SQL Server
* Oracle Database
* SQLite

---

# 🟢 NoSQL Databases

NoSQL databases use different data models instead of the traditional relational table model.

MongoDB, for example, stores documents.

Example:

```json
{
  "_id": "123",
  "name": "Ali",
  "email": "ali@example.com"
}
```

Popular NoSQL databases:

* MongoDB
* Redis
* Cassandra
* DynamoDB
* Neo4j

---

# 🆚 SQL vs NoSQL

| Feature        | SQL                | NoSQL                              |
| -------------- | ------------------ | ---------------------------------- |
| Data model     | Relational         | Various                            |
| Schema         | Usually structured | Often more flexible                |
| Main structure | Tables             | Documents / key-value / graph etc. |
| Relationships  | Strong support     | Depends on database                |
| SQL queries    | Yes                | Usually different query APIs       |
| Transactions   | Strong support     | Depends on database                |
| Examples       | PostgreSQL, MySQL  | MongoDB, Redis                     |

See the dedicated guide:

👉 [SQL vs NoSQL](./SQL_VS_NOSQL.md)

---

# 📊 Relational Database Concepts

## Table

A table stores related data.

Example:

```text
users
```

---

## Row

A row represents one record.

```text
1 | Ali | ali@example.com
```

---

## Column

A column represents an attribute.

```text
id
name
email
```

---

# 🔑 Primary Key

A **primary key** uniquely identifies a row.

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(255)
);
```

Here:

```text
id
```

is the primary key.

Example:

```text
1 → Ali
2 → Sara
3 → John
```

Each ID should uniquely identify a user.

---

# 🔗 Foreign Key

A foreign key creates a relationship between tables.

Example:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    total DECIMAL(10, 2)
);
```

Relationship:

```text
users
  │
  │ id
  ▼
orders
  │
  └── user_id
```

This allows an order to belong to a user.

---

# 🔄 Database Relationships

Common relationships include:

### One-to-One

```text
User ───── Profile
```

One user has one profile.

---

### One-to-Many

```text
User
 │
 ├── Order
 ├── Order
 └── Order
```

One user can have multiple orders.

---

### Many-to-Many

```text
Users
 │
 ├──── UserRoles ──── Roles
 │
 └──── UserRoles ──── Roles
```

A user can have multiple roles, and a role can belong to multiple users.

Usually a junction table is used.

---

# 🧱 Database Schema

A schema defines how data is organized.

For a relational database:

```text
Database
│
├── users
├── products
├── orders
└── payments
```

A table schema defines:

```text
Column
Data type
Constraints
Relationships
Indexes
```

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 🧩 Data Types

Different databases provide different data types.

Common SQL types:

```text
INTEGER
BIGINT
DECIMAL
VARCHAR
TEXT
BOOLEAN
DATE
TIMESTAMP
JSON
UUID
```

Example:

```sql
CREATE TABLE products (
    id UUID PRIMARY KEY,
    name VARCHAR(150),
    price DECIMAL(10, 2),
    available BOOLEAN,
    created_at TIMESTAMP
);
```

---

# ✏️ CRUD

CRUD represents the four basic database operations.

```text
C → Create
R → Read
U → Update
D → Delete
```

---

## Create

```sql
INSERT INTO users (name, email)
VALUES ('Ali', 'ali@example.com');
```

---

## Read

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

# 🔍 Queries

A query requests information from a database.

Example:

```sql
SELECT name, email
FROM users
WHERE id = 1;
```

You can combine conditions:

```sql
SELECT *
FROM users
WHERE age >= 18
AND active = true;
```

---

# 📑 Sorting

```sql
SELECT *
FROM users
ORDER BY name ASC;
```

Descending:

```sql
SELECT *
FROM users
ORDER BY created_at DESC;
```

---

# 📊 Aggregation

Databases can calculate values.

Examples:

```sql
SELECT COUNT(*)
FROM users;
```

```sql
SELECT AVG(price)
FROM products;
```

```sql
SELECT SUM(total)
FROM orders;
```

Common aggregate functions:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

# 🔗 JOIN

JOIN allows you to retrieve related data from multiple tables.

Example:

```sql
SELECT
    users.name,
    orders.total
FROM users
JOIN orders
    ON users.id = orders.user_id;
```

Result:

```text
Ali   → 150.00
Sara  → 80.00
John  → 220.00
```

Common JOIN types:

```text
INNER JOIN
LEFT JOIN
RIGHT JOIN
FULL JOIN
CROSS JOIN
```

---

# 🧹 Database Normalization

Normalization is a database design technique used to organize relational data and reduce unnecessary duplication.

Instead of:

```text
orders

id | user_name | user_email | product
```

You can separate data:

```text
users
orders
products
```

Then connect them with IDs.

Example:

```text
users
  │
  │ user_id
  ▼
orders
  │
  │ product_id
  ▼
products
```

Common normal forms include:

```text
1NF
2NF
3NF
BCNF
```

For most application development, understanding **1NF, 2NF, and 3NF** is a useful starting point.

---

# ⚡ Database Indexes

An index helps a database find data more efficiently for supported queries.

Example:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Without an appropriate index, a database may need to inspect many rows.

With an index, the database can often locate matching values more efficiently.

However, indexes also have costs:

* Additional storage
* Slower writes
* More maintenance

Don't create indexes blindly.

---

# 🔐 Constraints

Constraints help protect data integrity.

Common constraints:

```text
PRIMARY KEY
FOREIGN KEY
NOT NULL
UNIQUE
CHECK
DEFAULT
```

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER CHECK (age >= 18),
    active BOOLEAN DEFAULT true
);
```

---

# 🔄 Transactions

A transaction groups multiple database operations into one logical unit.

Example:

```text
Transfer $100
│
├── Remove $100 from Account A
│
└── Add $100 to Account B
```

Both operations should succeed together.

If something fails, the transaction can be rolled back.

Example SQL:

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

If there is an error:

```sql
ROLLBACK;
```

---

# 🧪 ACID

Relational databases commonly provide strong transaction guarantees described by **ACID**:

```text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

## Atomicity

A transaction succeeds completely or is rolled back.

## Consistency

Transactions should preserve defined data rules.

## Isolation

Concurrent transactions should behave according to the database's isolation model.

## Durability

Committed data should survive appropriate failures according to the database's durability guarantees.

---

# 🔒 Database Security

Database security is critical.

Important practices:

### Use strong authentication

Don't use:

```text
username: admin
password: admin
```

---

### Never store plaintext passwords

Bad:

```text
password = "mypassword123"
```

Use a password hashing algorithm designed for passwords, such as:

```text
Argon2
bcrypt
scrypt
```

---

### Use environment variables

Don't hard-code credentials:

```javascript
const password = "my-secret-password";
```

Prefer environment variables:

```javascript
const password = process.env.DATABASE_PASSWORD;
```

Example:

```text
DATABASE_URL=postgresql://user:password@localhost:5432/app
```

And don't commit `.env` files containing secrets.

---

# 🌐 Database Connection

A backend application needs a connection to the database.

Example architecture:

```text
Next.js / React
       ↓
Express / Node.js
       ↓
Database Driver / ORM
       ↓
PostgreSQL
```

---

# 🟢 Database Drivers

A database driver allows application code to communicate with a database.

For PostgreSQL:

```bash
npm install pg
```

For MongoDB:

```bash
npm install mongodb
```

---

# 🧩 ORM

ORM means **Object-Relational Mapping**.

An ORM allows developers to work with database data using application-level models and APIs.

Popular ORMs/tools include:

```text
Prisma
Drizzle
TypeORM
Sequelize
```

Example Prisma query:

```javascript
const users = await prisma.user.findMany();
```

Instead of writing:

```sql
SELECT *
FROM users;
```

The ORM generates the appropriate database query.

---

# 🗃️ ODM

For document databases such as MongoDB, you may also encounter **ODM**.

ODM means:

> Object-Document Mapping

A popular MongoDB ODM is:

```text
Mongoose
```

Example:

```javascript
const users = await User.find();
```

---

# 🔄 Database Migrations

A migration describes a change to a database schema.

Example:

```text
Version 1
users
│
├── id
└── name

        ↓ migration

Version 2
users
│
├── id
├── name
└── email
```

Migration tools include:

```text
Prisma Migrate
Drizzle Kit
Knex
Flyway
Liquibase
```

Migrations are especially important when working with teams and deployment pipelines.

---

# 💾 Database Backups

Backups protect against:

* Hardware failure
* Accidental deletion
* Application bugs
* Data corruption
* Security incidents
* Operational mistakes

Common backup strategies:

```text
Full Backup
Incremental Backup
Differential Backup
Point-in-Time Recovery
```

A backup is only useful if it can actually be restored.

Always test your restore process.

---

# 📈 Database Performance

Database performance depends on many factors.

Important areas include:

```text
Indexes
Queries
Schema design
Connections
Transactions
Caching
Hardware
Database configuration
Data volume
```

Before optimizing, measure the actual bottleneck.

---

# ⚡ Database Connection Pooling

Creating a new database connection for every request can be inefficient.

Connection pooling maintains reusable connections.

Example:

```text
Application
│
├── Request 1 ──┐
├── Request 2 ──┤
├── Request 3 ──┤
└── Request 4 ──┘
                ↓
        Connection Pool
        ├── Connection 1
        ├── Connection 2
        ├── Connection 3
        └── Connection 4
                ↓
            Database
```

Connection pooling is common in production applications.

---

# 🚀 Database Scaling

There are several approaches to scaling databases.

## Vertical Scaling

Increase resources on one database server:

```text
More CPU
More RAM
Faster storage
```

---

## Horizontal Scaling

Use multiple database servers or nodes.

Examples include:

```text
Read Replicas
Replication
Sharding
Distributed databases
```

The appropriate strategy depends on the database and workload.

---

# 📖 Database Replication

Replication copies data between database instances.

Example:

```text
             Primary
                │
        ┌───────┴───────┐
        ▼               ▼
   Read Replica    Read Replica
```

A primary database may handle writes while replicas can serve some reads, depending on the architecture.

---

# 🧱 Database Sharding

Sharding divides data across multiple database nodes.

Example:

```text
Users 1–1,000,000
        ↓
     Server A

Users 1,000,001–2,000,000
        ↓
     Server B
```

Sharding can support very large workloads but adds significant architectural complexity.

---

# 🐳 Databases with Docker

Docker makes it easy to run databases locally.

Example PostgreSQL:

```bash
docker run \
  --name postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres
```

Check the container:

```bash
docker ps
```

Stop it:

```bash
docker stop postgres
```

---

# 🧩 Database with Docker Compose

Example:

```yaml
services:
  postgres:
    image: postgres:18
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: password
      POSTGRES_DB: myapp
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql

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

---

# 🟢 PostgreSQL

PostgreSQL is an open-source relational database.

It supports:

* SQL
* Transactions
* Foreign keys
* Indexes
* JSON/JSONB
* Full-text search
* Extensions
* Advanced data types

Learn more:

👉 [PostgreSQL Learning](./POSTGRESQL_LEARNING.md)

---

# 🍃 MongoDB

MongoDB is a document-oriented NoSQL database.

Example document:

```json
{
  "_id": "123",
  "name": "Ali",
  "email": "ali@example.com",
  "skills": [
    "JavaScript",
    "Node.js"
  ]
}
```

Learn more:

👉 [MongoDB Learning](./MONGODB_LEARNING.md)

---

# 🆚 PostgreSQL vs MongoDB

| Feature            | PostgreSQL                      | MongoDB                       |
| ------------------ | ------------------------------- | ----------------------------- |
| Type               | Relational                      | Document                      |
| Primary structure  | Tables                          | Collections/Documents         |
| Query language/API | SQL                             | MongoDB query API             |
| Schema             | Structured                      | Flexible/document-oriented    |
| Relationships      | Strong relational support       | Embedded documents/references |
| Transactions       | Supported                       | Supported                     |
| JSON data          | JSON/JSONB                      | Native document model         |
| Best fit           | Relational data and complex SQL | Document-oriented data models |

Neither database is universally appropriate for every application. Database choice should depend on the application's data model, consistency requirements, query patterns, scale, operational needs, and team expertise.

---

# 🧪 Database Development Workflow

A typical development workflow:

```text
1. Design data model
        ↓
2. Choose database
        ↓
3. Create schema
        ↓
4. Create migrations
        ↓
5. Connect backend
        ↓
6. Implement CRUD
        ↓
7. Add constraints
        ↓
8. Add indexes
        ↓
9. Test queries
        ↓
10. Add backups
        ↓
11. Deploy
        ↓
12. Monitor
```

---

# 🛠️ Useful Database Tools

### GUI Clients

Examples:

* pgAdmin
* DBeaver
* MongoDB Compass
* DataGrip

### CLI Tools

PostgreSQL:

```bash
psql
```

MongoDB:

```bash
mongosh
```

Docker:

```bash
docker
docker compose
```

---

# 🧠 Important Concepts to Learn

If you are learning backend development, focus on these concepts:

```text
Database
DBMS
SQL
NoSQL
Tables
Rows
Columns
Documents
Primary Keys
Foreign Keys
Relationships
CRUD
JOIN
Indexes
Constraints
Transactions
ACID
Normalization
Migrations
Connection Pooling
Backups
Replication
Caching
Scaling
Database Security
```

---

# 🎯 Recommended Learning Order

A good learning path is:

```text
Database Fundamentals
        ↓
SQL Fundamentals
        ↓
Tables & Relationships
        ↓
CRUD
        ↓
JOIN
        ↓
Constraints
        ↓
Indexes
        ↓
Transactions & ACID
        ↓
Normalization
        ↓
PostgreSQL
        ↓
MongoDB
        ↓
ORM / ODM
        ↓
Migrations
        ↓
Docker Databases
        ↓
Backups
        ↓
Performance
        ↓
Replication & Scaling
```

---

# 📚 Related Documentation

* [SQL vs NoSQL](./SQL_VS_NOSQL.md)
* [PostgreSQL Learning](./POSTGRESQL_LEARNING.md)
* [MongoDB Learning](./MONGODB_LEARNING.md)

---

# 🔗 Official Documentation

### PostgreSQL

🌐 [PostgreSQL](https://www.postgresql.org/)

📖 [PostgreSQL Documentation](https://www.postgresql.org/docs/)

### MongoDB

🌐 [MongoDB](https://www.mongodb.com/)

📖 [MongoDB Documentation](https://www.mongodb.com/docs/)

### Prisma

🌐 [Prisma](https://www.prisma.io/)

📖 [Prisma Documentation](https://www.prisma.io/docs/)

### Docker

🌐 [Docker](https://www.docker.com/)

📖 [Docker Documentation](https://docs.docker.com/)

---

# 📝 Cheat Sheet

| Concept     | Meaning                                            |
| ----------- | -------------------------------------------------- |
| Database    | Organized collection of data                       |
| DBMS        | Software that manages databases                    |
| SQL         | Language commonly used with relational databases   |
| NoSQL       | Family of non-relational database models           |
| Table       | Relational data structure                          |
| Row         | One record                                         |
| Column      | Data attribute                                     |
| Primary Key | Unique identifier                                  |
| Foreign Key | Reference to another table                         |
| CRUD        | Create, Read, Update, Delete                       |
| JOIN        | Combines related data                              |
| Index       | Data structure that can speed up supported queries |
| Transaction | Group of database operations                       |
| ACID        | Transaction reliability properties                 |
| ORM         | Object-Relational Mapping                          |
| ODM         | Object-Document Mapping                            |
| Migration   | Controlled schema change                           |
| Replication | Maintaining copies of data                         |
| Sharding    | Splitting data across nodes                        |

---

# ✅ Final Summary

A database is a fundamental part of most modern applications.

The key concepts to understand are:

```text
Data
 ↓
Database
 ↓
Schema
 ↓
Queries
 ↓
Relationships
 ↓
Transactions
 ↓
Indexes
 ↓
Security
 ↓
Backups
 ↓
Performance
 ↓
Scaling
```

For backend development, a strong foundation in **SQL, PostgreSQL, database design, transactions, indexes, and database security** is especially valuable. MongoDB and other NoSQL databases then add knowledge of alternative data models and storage patterns.
