# 🗄️ SQL vs NoSQL

A practical guide to understanding the differences between **SQL and NoSQL databases**, their data models, strengths, limitations, use cases, and how to choose a database for an application.

---

# 📌 Overview

Databases can be broadly categorized into:

```text
Databases
│
├── SQL / Relational
│   ├── PostgreSQL
│   ├── MySQL
│   ├── MariaDB
│   ├── SQL Server
│   └── Oracle Database
│
└── NoSQL / Non-Relational
    ├── MongoDB
    ├── Redis
    ├── Cassandra
    ├── DynamoDB
    └── Neo4j
```

The main difference is the **data model** and the way applications interact with the data.

---

# 🟦 What is SQL?

**SQL** stands for:

> Structured Query Language

SQL is a language used to work with relational databases.

Relational databases organize data into:

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

+----+-------+-------------------+
| id | name  | email             |
+----+-------+-------------------+
| 1  | Ali   | ali@example.com   |
| 2  | Sara  | sara@example.com  |
+----+-------+-------------------+
```

SQL databases are also commonly called **relational databases**.

---

# 🟢 What is NoSQL?

**NoSQL** is commonly used to describe databases that don't primarily use the traditional relational table model.

The term is often interpreted as:

> Not Only SQL

NoSQL databases use different models depending on the database.

Examples:

```text
Document
Key-Value
Wide-Column
Graph
```

For example, MongoDB stores documents:

```json
{
  "_id": "123",
  "name": "Ali",
  "email": "ali@example.com"
}
```

---

# 🧩 Types of NoSQL Databases

NoSQL is not one single database model.

## 1. Document Databases

Store data as documents.

Examples:

```text
MongoDB
CouchDB
```

Example:

```json
{
  "name": "Ali",
  "age": 22,
  "skills": ["Node.js", "React"]
}
```

---

## 2. Key-Value Databases

Store data as:

```text
key → value
```

Example:

```text
user:123 → "Ali"
```

Examples:

```text
Redis
Amazon DynamoDB
```

---

## 3. Wide-Column Databases

Organize data using rows and column families.

Examples:

```text
Apache Cassandra
ScyllaDB
```

These are designed for certain large-scale distributed workloads.

---

## 4. Graph Databases

Store:

```text
Nodes
Relationships
Properties
```

Example:

```text
Ali
 │
 │ follows
 ▼
Sara
```

Examples:

```text
Neo4j
Amazon Neptune
```

---

# 🏗️ SQL Data Model

A relational database might contain:

```text
users
├── id
├── name
└── email

products
├── id
├── name
└── price

orders
├── id
├── user_id
└── total
```

Relationships connect the tables:

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

---

# 🍃 NoSQL Data Model

MongoDB might store a user as:

```json
{
  "_id": "user123",
  "name": "Ali",
  "email": "ali@example.com",
  "addresses": [
    {
      "city": "Sousse",
      "country": "Tunisia"
    }
  ]
}
```

Related information can sometimes be embedded directly into the document.

---

# 🆚 SQL vs NoSQL

| Feature        | SQL                                                     | NoSQL                                           |
| -------------- | ------------------------------------------------------- | ----------------------------------------------- |
| Common model   | Relational                                              | Document, key-value, graph, wide-column         |
| Main structure | Tables                                                  | Depends on database                             |
| Schema         | Usually structured                                      | Often more flexible                             |
| Relationships  | Strong relational support                               | Depends on database                             |
| Query language | SQL                                                     | Database-specific APIs/languages                |
| Transactions   | Strong support                                          | Depends on database                             |
| Joins          | Native feature                                          | Varies                                          |
| Scaling        | Often vertical + replication; distributed options exist | Often designed with distributed scaling in mind |
| Data format    | Rows/columns                                            | Varies                                          |
| Examples       | PostgreSQL, MySQL                                       | MongoDB, Redis, Cassandra                       |

---

# 📊 SQL Example

Suppose we have:

### Users

```text
id | name
---|------
1  | Ali
2  | Sara
```

### Orders

```text
id | user_id | total
---|---------|------
1  | 1       | 100
2  | 1       | 250
3  | 2       | 80
```

We can use a JOIN:

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
Ali  → 100
Ali  → 250
Sara → 80
```

---

# 🍃 MongoDB Example

The same type of information could be represented as documents.

```json
{
  "_id": 1,
  "name": "Ali",
  "orders": [
    {
      "total": 100
    },
    {
      "total": 250
    }
  ]
}
```

Another approach is to keep orders in a separate collection and reference the user.

There is no single correct MongoDB schema for every application.

---

# 🔐 Schema Differences

## SQL

Relational databases normally define a schema explicitly.

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

The table defines the expected structure.

---

## NoSQL

Document databases such as MongoDB allow documents in a collection to have different fields.

Example:

```json
{
  "name": "Ali",
  "email": "ali@example.com"
}
```

Another document might contain:

```json
{
  "name": "Sara",
  "email": "sara@example.com",
  "age": 22
}
```

This does not mean NoSQL databases have **no schema**.

Application-level validation and database-level schema validation can still be used.

---

# 🔗 Relationships

Relationships are a major difference in how data is commonly modeled.

## SQL

Relationships are represented using keys.

```text
users
  │
  │ user_id
  ▼
orders
```

Example:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id)
);
```

---

## NoSQL

Relationships can be modeled using:

### Embedded documents

```json
{
  "name": "Ali",
  "address": {
    "city": "Sousse"
  }
}
```

### References

```json
{
  "userId": "123",
  "total": 100
}
```

The correct approach depends on how the application reads and writes the data.

---

# 📦 Embedding vs Referencing

MongoDB commonly provides two broad modeling approaches.

## Embedding

```json
{
  "name": "Ali",
  "address": {
    "city": "Sousse",
    "country": "Tunisia"
  }
}
```

Useful when related data is commonly retrieved together and has an appropriate size/lifecycle.

---

## Referencing

```json
{
  "name": "Ali",
  "addressId": "address123"
}
```

Useful when data is shared, independently managed, or would otherwise become too large or duplicated.

---

# 🔄 Transactions

Transactions allow multiple operations to be treated as one logical unit.

Example:

```text
Transfer money
│
├── Debit Account A
│
└── Credit Account B
```

If one operation fails, the transaction can be rolled back according to the database's transaction semantics.

---

# 🟦 SQL Transactions

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

Rollback:

```sql
ROLLBACK;
```

---

# 🟢 NoSQL Transactions

Modern NoSQL databases can also support transactions.

For example, MongoDB supports multi-document transactions.

However, transaction capabilities and performance characteristics differ between database systems.

Always check the documentation of the specific database you use.

---

# 🧱 ACID

ACID describes important transaction properties:

```text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

These concepts are important when working with both relational and some NoSQL databases.

Do not assume:

```text
SQL = ACID
NoSQL = No ACID
```

That is an oversimplification.

Modern NoSQL databases can provide strong consistency and transaction support depending on the database and configuration.

---

# ⚡ Performance

It is incorrect to say:

> NoSQL is always faster than SQL.

Or:

> SQL is always faster than NoSQL.

Performance depends on:

```text
Data model
Query patterns
Indexes
Data volume
Hardware
Network
Concurrency
Database configuration
Application architecture
```

The database should be evaluated against the actual workload.

---

# 📈 Scaling

## Vertical Scaling

Increase the resources of a server:

```text
CPU ↑
RAM ↑
Storage ↑
```

Both SQL and NoSQL databases can use vertical scaling.

---

## Horizontal Scaling

Add more machines/nodes:

```text
        Database
        /       \
       /         \
    Node 1      Node 2
```

Many NoSQL systems were designed with distributed workloads and horizontal scaling as major use cases.

Relational databases can also scale horizontally using approaches such as:

* Read replicas
* Partitioning
* Sharding
* Distributed SQL systems

---

# 🔍 Indexes

Both SQL and NoSQL databases can use indexes.

SQL example:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

MongoDB example:

```javascript
db.users.createIndex({
  email: 1
});
```

Indexes can improve supported queries but also consume storage and can increase write/maintenance costs.

---

# 🧮 Normalization

Normalization is primarily associated with relational database design.

Instead of storing:

```text
order_id
user_name
user_email
product_name
```

repeatedly, separate entities can be stored in related tables.

```text
users
orders
products
```

Relationships connect them.

Common normal forms:

```text
1NF
2NF
3NF
BCNF
```

---

# 📦 Denormalization

Denormalization intentionally duplicates or restructures data to optimize certain access patterns.

Example:

Instead of joining several tables for every request, some frequently needed information might be stored together.

Denormalization can improve read performance in some workloads, but it introduces additional consistency and update considerations.

It can be used in both SQL and NoSQL systems.

---

# 🟦 SQL Advantages

Relational databases can be particularly useful when an application needs:

* Strong relational modeling
* Complex queries
* JOINs
* Referential integrity
* Structured schemas
* Transactions
* Mature SQL tooling
* Analytical queries

Examples:

```text
Banking
Accounting
ERP
Inventory
Orders
Payments
Business applications
```

---

# 🟦 SQL Considerations

SQL databases may require more deliberate schema design.

Changes to a production schema should be managed carefully through:

```text
Migrations
Testing
Backups
Deployment procedures
```

Highly relational schemas can also require joins and more involved query planning.

---

# 🟢 NoSQL Advantages

NoSQL databases can be useful when applications need:

* Flexible document structures
* Data models that naturally fit documents
* High-volume distributed workloads
* Simple access patterns
* Rapidly changing document structures
* Specialized data models

Examples:

```text
Content management
Catalogs
Event data
Real-time applications
Caching
Large distributed workloads
```

The exact characteristics depend heavily on the specific NoSQL database.

---

# 🟢 NoSQL Considerations

NoSQL does not automatically eliminate database design.

You still need to think about:

```text
Data modeling
Indexes
Consistency
Transactions
Data duplication
Query patterns
Data growth
Backups
Security
```

A flexible document model can make some changes easier, but poorly designed documents can also create performance and maintenance problems.

---

# 🧠 When to Use SQL

SQL can be a good fit when:

```text
Your data is strongly relational
        ↓
You need joins
        ↓
You need referential integrity
        ↓
You need transactions
        ↓
You need complex queries
        ↓
Use a relational database
```

Examples:

```text
PostgreSQL
MySQL
MariaDB
SQL Server
Oracle
```

---

# 🧠 When to Use NoSQL

NoSQL can be a good fit when:

```text
Your data naturally fits a NoSQL model
        ↓
You have specific distributed workloads
        ↓
You need flexible document structures
        ↓
Your access patterns fit the selected database
        ↓
Use an appropriate NoSQL database
```

Examples:

```text
MongoDB
Redis
Cassandra
DynamoDB
Neo4j
```

---

# ⚠️ Common Misconceptions

## ❌ "NoSQL means no SQL"

Not exactly.

NoSQL is commonly understood as:

> Not Only SQL

Some NoSQL databases provide their own query languages or support SQL-like interfaces.

---

## ❌ "NoSQL has no schema"

Not necessarily.

A NoSQL database may have flexible documents, but applications can still enforce schemas and validation.

---

## ❌ "NoSQL is always faster"

Not true.

Performance depends on the workload, data model, indexes, infrastructure, and queries.

---

## ❌ "SQL cannot scale"

Not true.

Relational databases can scale through:

```text
Vertical scaling
Read replicas
Partitioning
Sharding
Distributed SQL
Caching
```

---

## ❌ "SQL is only for small applications"

Not true.

Relational databases are used in applications ranging from small projects to very large production systems.

---

# 🆚 PostgreSQL vs MongoDB

PostgreSQL:

```text
Relational
     ↓
Tables
     ↓
Rows / Columns
     ↓
SQL
     ↓
Relationships
```

MongoDB:

```text
Document
     ↓
Collections
     ↓
Documents
     ↓
BSON
     ↓
Document-oriented queries
```

Both are capable production databases, but they model data differently.

---

# 📊 PostgreSQL vs MongoDB Example

## PostgreSQL

```sql
SELECT
    users.name,
    orders.total
FROM users
JOIN orders
    ON users.id = orders.user_id;
```

---

## MongoDB

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "user"
    }
  }
]);
```

MongoDB supports aggregation and joins through features such as `$lookup`, although the data-modeling approach is often different from a relational design.

---

# 🏗️ Choosing a Database

Don't choose a database only because it is popular.

Consider:

```text
1. What does the data look like?
        ↓
2. How is the data queried?
        ↓
3. How important are relationships?
        ↓
4. What consistency guarantees are required?
        ↓
5. What transaction capabilities are required?
        ↓
6. What is the expected workload?
        ↓
7. How will the system scale?
        ↓
8. What operational tools are available?
        ↓
9. What does the team know?
        ↓
10. What are the backup and security requirements?
```

---

# 📋 Decision Checklist

Before selecting a database, ask:

### Data

* Is the data strongly relational?
* Is the structure naturally document-oriented?
* Will the schema change frequently?

### Queries

* Do I need complex JOINs?
* What are the most common queries?
* Are queries predictable?
* What indexes will be needed?

### Consistency

* What consistency guarantees are required?
* Are multi-step transactions necessary?
* Can some data be eventually consistent?

### Scale

* How much data will be stored?
* How many requests are expected?
* What are the read/write ratios?
* Is horizontal scaling required?

### Operations

* How will backups work?
* How will monitoring work?
* How will migrations be handled?
* How will security be managed?

---

# 🛠️ SQL Example Workflow

Create a database:

```sql
CREATE DATABASE shop;
```

Create a table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

Insert data:

```sql
INSERT INTO users (name, email)
VALUES ('Ali', 'ali@example.com');
```

Read data:

```sql
SELECT *
FROM users;
```

Update:

```sql
UPDATE users
SET name = 'Ahmed'
WHERE id = 1;
```

Delete:

```sql
DELETE FROM users
WHERE id = 1;
```

---

# 🍃 MongoDB Example Workflow

Create/select a database:

```javascript
use shop
```

Insert:

```javascript
db.users.insertOne({
  name: "Ali",
  email: "ali@example.com"
});
```

Read:

```javascript
db.users.find();
```

Update:

```javascript
db.users.updateOne(
  { name: "Ali" },
  { $set: { name: "Ahmed" } }
);
```

Delete:

```javascript
db.users.deleteOne({
  name: "Ahmed"
});
```

---

# 🔄 SQL to MongoDB Concepts

| Relational  | MongoDB                              |
| ----------- | ------------------------------------ |
| Database    | Database                             |
| Table       | Collection                           |
| Row         | Document                             |
| Column      | Field                                |
| Primary Key | `_id`                                |
| JOIN        | `$lookup` / modeling                 |
| Index       | Index                                |
| SQL query   | MongoDB query                        |
| Schema      | Document structure / validation      |
| Foreign Key | Reference / application relationship |

These are useful conceptual mappings, but they are **not exact equivalents**.

---

# 🚀 Backend Development

A Node.js application can work with either SQL or NoSQL.

## SQL

```text
Next.js
   ↓
Express / Node.js
   ↓
Prisma / Drizzle / pg
   ↓
PostgreSQL
```

## NoSQL

```text
Next.js
   ↓
Express / Node.js
   ↓
MongoDB Driver / Mongoose
   ↓
MongoDB
```

---

# 🧩 ORM / ODM

For SQL databases:

```text
Prisma
Drizzle
TypeORM
Sequelize
```

For MongoDB:

```text
MongoDB Driver
Mongoose
```

Example Prisma:

```javascript
const users = await prisma.user.findMany();
```

Example Mongoose:

```javascript
const users = await User.find();
```

---

# 🐳 Docker

Both SQL and NoSQL databases can be run using Docker.

PostgreSQL:

```bash
docker run \
  --name postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres
```

MongoDB:

```bash
docker run \
  --name mongodb \
  -p 27017:27017 \
  -d mongo
```

---

# 📚 Related Documentation

* [Database Learning](./DATABASE_LEARNING.md)
* [PostgreSQL Learning](./POSTGRESQL_LEARNING.md)
* [MongoDB Learning](./MONGODB_LEARNING.md)

---

# 🔗 Official Documentation

### SQL / PostgreSQL

🌐 [PostgreSQL](https://www.postgresql.org/)

📖 [PostgreSQL Documentation](https://www.postgresql.org/docs/)

### MongoDB

🌐 [MongoDB](https://www.mongodb.com/)

📖 [MongoDB Documentation](https://www.mongodb.com/docs/)

### Redis

🌐 [Redis](https://redis.io/)

📖 [Redis Documentation](https://redis.io/docs/)

### MySQL

🌐 [MySQL](https://www.mysql.com/)

📖 [MySQL Documentation](https://dev.mysql.com/doc/)

---

# 📝 Quick Cheat Sheet

```text
SQL
│
├── Relational
├── Tables
├── Rows
├── Columns
├── Foreign Keys
├── JOINs
├── SQL
└── PostgreSQL / MySQL

NoSQL
│
├── Document
├── Key-Value
├── Graph
├── Wide-Column
└── MongoDB / Redis / Cassandra / Neo4j
```

### Remember

```text
SQL
→ Relational data model

NoSQL
→ Multiple non-relational data models

Neither is automatically better.
Choose based on:
→ Data model
→ Query patterns
→ Consistency requirements
→ Transactions
→ Scale
→ Operational requirements
```

---

# 🎯 Final Summary

SQL and NoSQL are different approaches to storing and querying data.

**SQL databases** use relational structures such as tables, rows, columns, keys, and relationships.

**NoSQL databases** cover several different models, including documents, key-value data, graphs, and wide-column storage.

The important skill is not memorizing which database is "better." Instead, understand:

```text
Data Model
     ↓
Query Patterns
     ↓
Consistency
     ↓
Transactions
     ↓
Indexes
     ↓
Scale
     ↓
Security
     ↓
Operations
     ↓
Database Choice
```

Understanding these concepts makes it easier to work with **PostgreSQL, MongoDB, Redis, MySQL, and other database technologies**.
