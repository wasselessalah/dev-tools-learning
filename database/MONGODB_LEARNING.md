# 🍃 MongoDB Learning Guide

A practical guide to learning **MongoDB**, from the fundamentals to advanced concepts used in modern backend applications.

---

## 📚 Table of Contents

1. [What is MongoDB?](#-what-is-mongodb)
2. [Why MongoDB?](#-why-mongodb)
3. [MongoDB Architecture](#-mongodb-architecture)
4. [MongoDB vs SQL](#-mongodb-vs-sql)
5. [Installation](#-installation)
6. [MongoDB Shell](#-mongodb-shell)
7. [Databases and Collections](#-databases-and-collections)
8. [Documents and BSON](#-documents-and-bson)
9. [ObjectId](#-objectid)
10. [CRUD Operations](#-crud-operations)
11. [Insert Documents](#-insert-documents)
12. [Find Documents](#-find-documents)
13. [Query Operators](#-query-operators)
14. [Update Documents](#-update-documents)
15. [Delete Documents](#-delete-documents)
16. [Sorting and Pagination](#-sorting-and-pagination)
17. [Projection](#-projection)
18. [Indexes](#-indexes)
19. [Aggregation](#-aggregation)
20. [Aggregation Pipeline](#-aggregation-pipeline)
21. [Embedded Documents](#-embedded-documents)
22. [References](#-references)
23. [Schema Design](#-schema-design)
24. [Validation](#-schema-validation)
25. [Transactions](#-transactions)
26. [MongoDB Users and Authentication](#-mongodb-users-and-authentication)
27. [Backup and Restore](#-backup-and-restore)
28. [MongoDB Atlas](#-mongodb-atlas)
29. [MongoDB with Node.js](#-mongodb-with-nodejs)
30. [MongoDB with Mongoose](#-mongodb-with-mongoose)
31. [MongoDB with Docker](#-mongodb-with-docker)
32. [Performance](#-performance-best-practices)
33. [Security](#-security)
34. [Common Errors](#-common-errors)
35. [Cheat Sheet](#-cheat-sheet)
36. [Learning Path](#-learning-path)
37. [Official Documentation](#-official-documentation)

---

# 🍃 What is MongoDB?

**MongoDB** is a NoSQL database that stores data in **documents** instead of traditional relational tables.

MongoDB uses a document-oriented data model based on **BSON**.

A simplified structure:

```text
MongoDB
│
├── Database
│
├── Collection
│
└── Document
    ├── Field
    ├── Field
    └── Field
```

Example document:

```json
{
  "_id": "ObjectId(...)",
  "name": "Wassel",
  "email": "wassel@example.com",
  "age": 23
}
```

---

# ⭐ Why MongoDB?

MongoDB is commonly used for:

* Web applications
* REST APIs
* Real-time applications
* Content management systems
* SaaS applications
* Prototypes
* Catalogs
* Event-based applications
* Applications with flexible document structures

MongoDB provides:

* Flexible document models
* BSON data types
* Indexes
* Aggregation pipelines
* Replication
* Transactions
* Horizontal scaling
* Schema validation
* MongoDB Atlas
* Drivers for many programming languages

---

# 🏗️ MongoDB Architecture

A simplified architecture:

```text
Application
     │
     ▼
MongoDB Driver
     │
     ▼
MongoDB Server
     │
     ├── Database
     │     │
     │     ├── Collection
     │     │      ├── Document
     │     │      ├── Document
     │     │      └── Document
     │     │
     │     └── Collection
     │
     └── Users / Roles
```

---

# 🆚 MongoDB vs SQL

A relational database usually has:

```text
Database
   │
   └── Table
        ├── Row
        └── Column
```

MongoDB uses:

```text
Database
   │
   └── Collection
        ├── Document
        └── Document
```

Conceptual mapping:

| SQL         | MongoDB                         |
| ----------- | ------------------------------- |
| Database    | Database                        |
| Table       | Collection                      |
| Row         | Document                        |
| Column      | Field                           |
| Primary Key | `_id`                           |
| JOIN        | `$lookup` / application queries |
| Schema      | Flexible document structure     |
| SQL         | MongoDB Query API               |

---

# 📦 Installation

There are several ways to use MongoDB:

```text
MongoDB Community Server
MongoDB Atlas
Docker
```

For development, Docker is also a convenient option.

---

# 🐧 MongoDB on Ubuntu

After installing MongoDB according to the official instructions, check the service:

```bash
sudo systemctl status mongod
```

Start:

```bash
sudo systemctl start mongod
```

Enable at startup:

```bash
sudo systemctl enable mongod
```

Check MongoDB:

```bash
mongosh
```

---

# 🐚 MongoDB Shell

The MongoDB command-line shell is:

```bash
mongosh
```

Show databases:

```javascript
show dbs
```

Show current database:

```javascript
db
```

Switch database:

```javascript
use myapp
```

Show collections:

```javascript
show collections
```

Exit:

```javascript
exit
```

---

# 🗄️ Databases and Collections

Switch to a database:

```javascript
use myapp
```

MongoDB creates the database when data is actually stored in it.

Create a collection:

```javascript
db.createCollection("users")
```

List collections:

```javascript
show collections
```

Drop a collection:

```javascript
db.users.drop()
```

---

# 📄 Documents and BSON

MongoDB stores documents using **BSON**.

BSON means:

```text
Binary JSON
```

Example:

```json
{
  "name": "Wassel",
  "age": 23,
  "active": true,
  "skills": [
    "React",
    "Next.js",
    "Node.js"
  ]
}
```

BSON supports additional data types beyond standard JSON.

Common types include:

```text
String
Boolean
Integer
Double
Decimal128
Date
ObjectId
Array
Embedded Document
Binary Data
Null
```

---

# 🆔 ObjectId

MongoDB automatically creates an `_id` field for documents unless you provide one.

Example:

```json
{
  "_id": ObjectId("68d123456789abcdef123456"),
  "name": "Wassel"
}
```

`ObjectId` is commonly used as the document identifier.

You can query it:

```javascript
db.users.findOne({
  _id: ObjectId("68d123456789abcdef123456")
})
```

---

# 🔄 CRUD Operations

CRUD means:

```text
C → Create
R → Read
U → Update
D → Delete
```

MongoDB provides methods such as:

```text
insertOne()
insertMany()

find()
findOne()

updateOne()
updateMany()
replaceOne()

deleteOne()
deleteMany()
```

---

# ➕ Insert Documents

## Insert One

```javascript
db.users.insertOne({
  name: "Wassel",
  email: "wassel@example.com",
  age: 23
})
```

---

## Insert Many

```javascript
db.users.insertMany([
  {
    name: "Ahmed",
    email: "ahmed@example.com",
    age: 25
  },
  {
    name: "Sara",
    email: "sara@example.com",
    age: 22
  }
])
```

---

# 🔎 Find Documents

Find all:

```javascript
db.users.find()
```

Find one:

```javascript
db.users.findOne()
```

Find by field:

```javascript
db.users.find({
  name: "Wassel"
})
```

---

## Find with Multiple Conditions

```javascript
db.users.find({
  age: 23,
  active: true
})
```

---

# 🔍 Query Operators

MongoDB provides many query operators.

Common operators:

```text
$eq
$ne
$gt
$gte
$lt
$lte
$in
$nin
$and
$or
$not
$exists
$regex
```

---

## `$gt`

Greater than:

```javascript
db.users.find({
  age: {
    $gt: 20
  }
})
```

---

## `$gte`

Greater than or equal:

```javascript
db.users.find({
  age: {
    $gte: 18
  }
})
```

---

## `$lt`

Less than:

```javascript
db.users.find({
  age: {
    $lt: 30
  }
})
```

---

## `$lte`

Less than or equal:

```javascript
db.users.find({
  age: {
    $lte: 30
  }
})
```

---

## `$in`

```javascript
db.users.find({
  age: {
    $in: [20, 23, 25]
  }
})
```

---

## `$nin`

```javascript
db.users.find({
  age: {
    $nin: [18, 19]
  }
})
```

---

## `$ne`

Not equal:

```javascript
db.users.find({
  age: {
    $ne: 18
  }
})
```

---

# 🔀 AND and OR

MongoDB normally treats multiple fields as an AND condition:

```javascript
db.users.find({
  age: {
    $gte: 18
  },
  active: true
})
```

Explicit `$and`:

```javascript
db.users.find({
  $and: [
    { age: { $gte: 18 } },
    { active: true }
  ]
})
```

---

## `$or`

```javascript
db.users.find({
  $or: [
    { age: 18 },
    { age: 23 }
  ]
})
```

---

# 🔤 Regular Expressions

Search names beginning with `Wa`:

```javascript
db.users.find({
  name: /^Wa/
})
```

Example matches:

```text
Wassel
Walid
Wassim
```

---

# ✏️ Update Documents

## updateOne

```javascript
db.users.updateOne(
  {
    email: "wassel@example.com"
  },
  {
    $set: {
      age: 24
    }
  }
)
```

---

## updateMany

```javascript
db.users.updateMany(
  {
    active: false
  },
  {
    $set: {
      archived: true
    }
  }
)
```

---

# 🛠️ Update Operators

Common operators:

```text
$set
$unset
$inc
$push
$pull
$addToSet
$rename
```

---

## `$set`

```javascript
db.users.updateOne(
  { name: "Wassel" },
  {
    $set: {
      age: 24
    }
  }
)
```

---

## `$inc`

Increment a value:

```javascript
db.products.updateOne(
  { name: "Laptop" },
  {
    $inc: {
      stock: 5
    }
  }
)
```

---

## `$unset`

Remove a field:

```javascript
db.users.updateOne(
  { name: "Wassel" },
  {
    $unset: {
      temporaryField: ""
    }
  }
)
```

---

## `$push`

Add an item to an array:

```javascript
db.users.updateOne(
  { name: "Wassel" },
  {
    $push: {
      skills: "Docker"
    }
  }
)
```

---

## `$addToSet`

Add only if it does not already exist:

```javascript
db.users.updateOne(
  { name: "Wassel" },
  {
    $addToSet: {
      skills: "Docker"
    }
  }
)
```

---

## `$pull`

Remove an array value:

```javascript
db.users.updateOne(
  { name: "Wassel" },
  {
    $pull: {
      skills: "Docker"
    }
  }
)
```

---

# 🔄 Upsert

An upsert updates a matching document or creates one if no document matches.

```javascript
db.users.updateOne(
  {
    email: "new@example.com"
  },
  {
    $set: {
      name: "New User"
    }
  },
  {
    upsert: true
  }
)
```

---

# 🗑️ Delete Documents

Delete one:

```javascript
db.users.deleteOne({
  email: "wassel@example.com"
})
```

Delete many:

```javascript
db.users.deleteMany({
  active: false
})
```

Delete all documents:

```javascript
db.users.deleteMany({})
```

⚠️ Be careful with destructive queries.

---

# ↕️ Sorting and Pagination

Sort ascending:

```javascript
db.users.find().sort({
  age: 1
})
```

Sort descending:

```javascript
db.users.find().sort({
  age: -1
})
```

---

## Limit

```javascript
db.users.find().limit(10)
```

---

## Skip

```javascript
db.users.find()
  .skip(20)
  .limit(10)
```

Example:

```text
Page 1 → skip(0)
Page 2 → skip(10)
Page 3 → skip(20)
```

For very large datasets, cursor/range-based pagination is often preferable to large `skip()` values.

Example:

```javascript
db.users.find({
  _id: {
    $gt: lastId
  }
})
.sort({
  _id: 1
})
.limit(20)
```

---

# 👁️ Projection

Projection controls which fields are returned.

Example:

```javascript
db.users.find(
  {},
  {
    name: 1,
    email: 1
  }
)
```

Exclude a field:

```javascript
db.users.find(
  {},
  {
    password: 0
  }
)
```

A common API pattern is to avoid returning sensitive fields such as password hashes.

---

# 🚀 Indexes

Indexes improve query performance.

Create an index:

```javascript
db.users.createIndex({
  email: 1
})
```

List indexes:

```javascript
db.users.getIndexes()
```

Drop an index:

```javascript
db.users.dropIndex("email_1")
```

---

# 🔐 Unique Index

Useful for unique values such as email addresses:

```javascript
db.users.createIndex(
  {
    email: 1
  },
  {
    unique: true
  }
)
```

Now duplicate emails are rejected.

---

# 🧩 Compound Index

Create an index using multiple fields:

```javascript
db.users.createIndex({
  active: 1,
  createdAt: -1
})
```

Compound indexes are useful when queries frequently filter or sort using the indexed fields.

---

# 🔎 Explain Query

Analyze a query:

```javascript
db.users.find({
  email: "wassel@example.com"
}).explain("executionStats")
```

You can inspect:

```text
executionTimeMillis
totalDocsExamined
totalKeysExamined
nReturned
```

A useful goal is to avoid examining large numbers of unnecessary documents.

---

# 📊 Aggregation

MongoDB provides the **aggregation framework** for processing data.

Example:

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$userId",
      total: {
        $sum: "$total"
      }
    }
  }
])
```

Aggregation is useful for:

* Reports
* Analytics
* Statistics
* Data transformation
* Grouping
* Filtering
* Joining collections

---

# 🔄 Aggregation Pipeline

An aggregation pipeline processes documents through stages.

```text
Documents
    │
    ▼
$match
    │
    ▼
$group
    │
    ▼
$sort
    │
    ▼
$project
    │
    ▼
Result
```

---

# 🔎 `$match`

Filter documents:

```javascript
{
  $match: {
    status: "paid"
  }
}
```

---

# 📦 `$group`

Group documents:

```javascript
{
  $group: {
    _id: "$category",
    total: {
      $sum: 1
    }
  }
}
```

---

# ↕️ `$sort`

```javascript
{
  $sort: {
    total: -1
  }
}
```

---

# 🎯 `$project`

Select or transform fields:

```javascript
{
  $project: {
    name: 1,
    total: 1
  }
}
```

---

# 🔢 `$count`

```javascript
{
  $count: "totalUsers"
}
```

---

# 🔗 `$lookup`

`$lookup` can combine documents from another collection.

Example:

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
])
```

Conceptually similar to a relational JOIN, although the document model is different.

---

# 🧩 Embedded Documents

MongoDB allows documents inside documents.

Example:

```json
{
  "name": "Wassel",
  "address": {
    "city": "Sousse",
    "country": "Tunisia"
  }
}
```

Query:

```javascript
db.users.find({
  "address.city": "Sousse"
})
```

---

# 📚 Embedded Arrays

Example:

```json
{
  "name": "Wassel",
  "skills": [
    "React",
    "Next.js",
    "Node.js"
  ]
}
```

Query:

```javascript
db.users.find({
  skills: "React"
})
```

---

# 🔗 References

Instead of embedding everything, documents can reference other documents.

Example:

```json
{
  "_id": ObjectId("..."),
  "userId": ObjectId("...")
}
```

For example:

```text
users
  │
  │ ObjectId
  ▼
orders
```

The application can fetch the related user separately, or `$lookup` can be used when appropriate.

---

# 🧠 Embedded Documents vs References

## Embed when:

* Data belongs closely to the parent
* Data is usually read together
* The embedded data has a suitable bounded size
* You want fewer queries

Example:

```json
{
  "name": "Wassel",
  "address": {
    "city": "Sousse",
    "country": "Tunisia"
  }
}
```

## Reference when:

* Data is shared
* Data changes independently
* The relationship is large
* Embedding would cause excessive document growth

Example:

```text
users
orders
products
```

---

# 🧱 Schema Design

MongoDB has a flexible schema, but this does **not** mean schema design is unnecessary.

A good MongoDB design considers:

```text
How data is queried
How data is updated
Relationship between entities
Document size
Read/write patterns
Indexes
Data ownership
Consistency requirements
```

---

# 🧪 Example Schema

User:

```json
{
  "_id": ObjectId("..."),
  "name": "Wassel",
  "email": "wassel@example.com",
  "skills": [
    "React",
    "Next.js",
    "Node.js"
  ],
  "createdAt": ISODate("2026-09-23T10:00:00Z")
}
```

Order:

```json
{
  "_id": ObjectId("..."),
  "userId": ObjectId("..."),
  "items": [
    {
      "productId": ObjectId("..."),
      "quantity": 2
    }
  ],
  "total": 1200,
  "status": "paid"
}
```

---

# ✅ Schema Validation

MongoDB can enforce validation rules using JSON Schema validation.

Example:

```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: [
        "name",
        "email"
      ],
      properties: {
        name: {
          bsonType: "string"
        },
        email: {
          bsonType: "string"
        }
      }
    }
  }
})
```

MongoDB can therefore combine flexible document modeling with explicit validation where needed.

---

# 🔄 Transactions

MongoDB supports transactions.

A transaction can contain multiple database operations.

Conceptually:

```text
BEGIN
   │
   ├── Operation 1
   │
   ├── Operation 2
   │
   └── Operation 3
         │
         ▼
      COMMIT
```

If an operation fails:

```text
ROLLBACK
```

MongoDB transactions require the appropriate deployment topology, such as a replica set or sharded cluster.

---

# 👤 MongoDB Users and Authentication

Create a user from `mongosh`:

```javascript
use admin

db.createUser({
  user: "app_user",
  pwd: "strong-password",
  roles: [
    {
      role: "readWrite",
      db: "myapp"
    }
  ]
})
```

Connect using credentials:

```bash
mongosh \
  --username app_user \
  --authenticationDatabase myapp
```

Use strong passwords and appropriate roles.

---

# 💾 Backup and Restore

MongoDB provides:

```text
mongodump
mongorestore
```

Backup:

```bash
mongodump \
  --uri="mongodb://localhost:27017/myapp" \
  --out=./backup
```

Restore:

```bash
mongorestore \
  --uri="mongodb://localhost:27017/myapp" \
  ./backup/myapp
```

Always test backups by performing a restore.

---

# ☁️ MongoDB Atlas

**MongoDB Atlas** is MongoDB's managed cloud database platform.

It provides:

* Managed MongoDB deployments
* Cloud hosting
* Monitoring
* Backups
* Security features
* Scaling options
* Connection management

Typical architecture:

```text
Next.js
   │
   ▼
Node.js / Express
   │
   ▼
MongoDB Atlas
```

---

# 🟢 MongoDB with Node.js

Install the official MongoDB driver:

```bash
npm install mongodb
```

Example:

```javascript
import { MongoClient } from "mongodb";

const client = new MongoClient(
  process.env.MONGODB_URI
);

await client.connect();

const db = client.db("myapp");

const users = db.collection("users");

const user = await users.findOne({
  email: "wassel@example.com"
});

console.log(user);
```

---

# 🔐 Environment Variables

Example:

```env
MONGODB_URI="mongodb://localhost:27017/myapp"
```

For MongoDB Atlas:

```env
MONGODB_URI="mongodb+srv://username:password@cluster.example.mongodb.net/myapp"
```

Never commit credentials.

`.gitignore`:

```gitignore
.env
.env.local
```

---

# 🧩 MongoDB with Mongoose

**Mongoose** is an ODM commonly used with Node.js and MongoDB.

Install:

```bash
npm install mongoose
```

Connect:

```javascript
import mongoose from "mongoose";

await mongoose.connect(
  process.env.MONGODB_URI
);
```

---

# 📋 Mongoose Schema

Example:

```javascript
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: true
    },

    email: {
      type: String,
      required: true,
      unique: true
    },

    age: {
      type: Number
    }
  },
  {
    timestamps: true
  }
);

export const User =
  mongoose.model("User", userSchema);
```

---

# 🔎 Mongoose Queries

Create:

```javascript
const user = await User.create({
  name: "Wassel",
  email: "wassel@example.com",
  age: 23
});
```

Find:

```javascript
const users = await User.find();
```

Find one:

```javascript
const user = await User.findOne({
  email: "wassel@example.com"
});
```

Update:

```javascript
await User.updateOne(
  {
    email: "wassel@example.com"
  },
  {
    $set: {
      age: 24
    }
  }
);
```

Delete:

```javascript
await User.deleteOne({
  email: "wassel@example.com"
});
```

---

# 🐳 MongoDB with Docker

Run MongoDB:

```bash
docker run \
  --name mongodb \
  -p 27017:27017 \
  -d mongo
```

Check:

```bash
docker ps
```

Stop:

```bash
docker stop mongodb
```

Start:

```bash
docker start mongodb
```

Remove:

```bash
docker rm mongodb
```

---

# 🐳 MongoDB with Docker Compose

Create:

```text
compose.yaml
```

```yaml
services:
  mongodb:
    image: mongo
    container_name: mongodb
    restart: unless-stopped

    ports:
      - "27017:27017"

    volumes:
      - mongodb_data:/data/db

volumes:
  mongodb_data:
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
docker compose logs mongodb
```

---

# ⚡ Performance Best Practices

## 1. Create useful indexes

Example:

```javascript
db.users.createIndex({
  email: 1
})
```

---

## 2. Analyze queries

```javascript
db.users.find({
  email: "wassel@example.com"
}).explain("executionStats")
```

---

## 3. Avoid unnecessary fields

Use projection:

```javascript
db.users.find(
  {},
  {
    name: 1,
    email: 1
  }
)
```

---

## 4. Design around access patterns

Think about how your application actually reads and writes data.

Example:

```text
What queries happen most often?
What fields are filtered?
What fields are sorted?
Which data is always read together?
```

---

## 5. Avoid unbounded document growth

Documents should not grow indefinitely because of arrays or embedded data.

For example, an ever-growing:

```json
{
  "messages": [
    "...",
    "...",
    "..."
  ]
}
```

may eventually become a poor document design.

Consider separate documents/collections for very large or unbounded datasets.

---

## 6. Use pagination

Instead of returning thousands of documents:

```javascript
db.users.find().limit(20)
```

Use a suitable pagination strategy.

---

# 🔐 Security Best Practices

### 1. Enable authentication

Do not expose an unauthenticated production database.

### 2. Use strong credentials

Avoid:

```text
password123
```

### 3. Use environment variables

Do not hardcode credentials.

### 4. Restrict network access

Only trusted applications should access the database.

### 5. Use TLS

Encrypt connections when appropriate.

### 6. Apply least privilege

Give applications only the permissions they require.

### 7. Protect sensitive fields

Do not unnecessarily return:

```text
password hashes
authentication tokens
private information
internal security fields
```

---

# 🏗️ MongoDB Backend Architecture

A common Node.js architecture:

```text
                ┌──────────────┐
                │   Frontend   │
                │   Next.js    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │   Backend    │
                │ Node/Express │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ MongoDB      │
                └──────────────┘
```

Example project:

```text
my-app/
│
├── frontend/
│   └── Next.js
│
├── backend/
│   ├── controllers/
│   ├── routes/
│   ├── services/
│   ├── models/
│   └── database/
│
└── .env
```

---

# 🧪 Practical Example: E-commerce

Collections:

```text
users
products
categories
orders
reviews
```

---

## User

```json
{
  "_id": ObjectId("..."),
  "name": "Wassel",
  "email": "wassel@example.com"
}
```

---

## Product

```json
{
  "_id": ObjectId("..."),
  "name": "Laptop",
  "price": 2500,
  "category": "Computers",
  "stock": 10
}
```

---

## Order

```json
{
  "_id": ObjectId("..."),
  "userId": ObjectId("..."),
  "items": [
    {
      "productId": ObjectId("..."),
      "quantity": 2,
      "price": 2500
    }
  ],
  "total": 5000,
  "status": "paid"
}
```

---

# 📊 E-commerce Aggregation Example

Calculate total sales:

```javascript
db.orders.aggregate([
  {
    $match: {
      status: "paid"
    }
  },
  {
    $group: {
      _id: null,
      totalSales: {
        $sum: "$total"
      }
    }
  }
])
```

---

# 🔎 Find Products by Price

```javascript
db.products.find({
  price: {
    $gte: 1000,
    $lte: 3000
  }
})
```

---

# 📦 Find Products with Stock

```javascript
db.products.find({
  stock: {
    $gt: 0
  }
})
```

---

# 🧮 Calculate Average Product Price

```javascript
db.products.aggregate([
  {
    $group: {
      _id: null,
      averagePrice: {
        $avg: "$price"
      }
    }
  }
])
```

---

# ❌ Common Errors

## MongoDB is not running

Check:

```bash
sudo systemctl status mongod
```

Or Docker:

```bash
docker ps
```

---

## Connection refused

Example:

```text
MongoServerSelectionError
```

Check:

```text
MongoDB service
Host
Port
Connection string
Firewall
Docker container
```

Default MongoDB port:

```text
27017
```

---

## Authentication failed

Check:

```text
Username
Password
Authentication database
User roles
Connection string
```

---

## Duplicate key error

Example:

```text
E11000 duplicate key error
```

This commonly happens with a unique index.

Example:

```javascript
db.users.createIndex(
  {
    email: 1
  },
  {
    unique: true
  }
)
```

Trying to insert the same email twice can cause a duplicate-key error.

---

# 🧰 MongoDB Cheat Sheet

## Start Shell

```bash
mongosh
```

---

## Databases

```javascript
show dbs
use myapp
db
```

---

## Collections

```javascript
show collections

db.createCollection("users")

db.users.drop()
```

---

## Insert

```javascript
db.users.insertOne({
  name: "Wassel"
})
```

---

## Insert Many

```javascript
db.users.insertMany([
  {
    name: "Ahmed"
  },
  {
    name: "Sara"
  }
])
```

---

## Find

```javascript
db.users.find()
```

---

## Find One

```javascript
db.users.findOne({
  name: "Wassel"
})
```

---

## Update

```javascript
db.users.updateOne(
  {
    name: "Wassel"
  },
  {
    $set: {
      age: 23
    }
  }
)
```

---

## Delete

```javascript
db.users.deleteOne({
  name: "Wassel"
})
```

---

## Sort

```javascript
db.users.find().sort({
  age: -1
})
```

---

## Limit

```javascript
db.users.find().limit(10)
```

---

## Index

```javascript
db.users.createIndex({
  email: 1
})
```

---

## Aggregation

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$userId",
      total: {
        $sum: "$total"
      }
    }
  }
])
```

---

## Explain

```javascript
db.users.find({
  email: "test@example.com"
}).explain("executionStats")
```

---

# 🗺️ MongoDB Learning Path

Recommended order:

```text
1. NoSQL Concepts
      ↓
2. MongoDB Installation
      ↓
3. mongosh
      ↓
4. Databases & Collections
      ↓
5. Documents & BSON
      ↓
6. ObjectId
      ↓
7. CRUD
      ↓
8. Query Operators
      ↓
9. Update Operators
      ↓
10. Sorting & Pagination
      ↓
11. Projection
      ↓
12. Indexes
      ↓
13. Aggregation
      ↓
14. Schema Design
      ↓
15. Embedding & References
      ↓
16. Validation
      ↓
17. Transactions
      ↓
18. Security
      ↓
19. Backup & Restore
      ↓
20. Node.js Driver
      ↓
21. Mongoose
      ↓
22. Docker
      ↓
23. MongoDB Atlas
      ↓
24. Performance
      ↓
25. Production Deployment
```

---

# 🚀 Practice Projects

## 🟢 Beginner

### 1. User Management API

Features:

```text
Create user
Get users
Get user
Update user
Delete user
Search users
```

Stack:

```text
Node.js
Express
MongoDB
```

---

## 🟡 Intermediate

### 2. Blog API

Collections:

```text
users
posts
comments
categories
```

Practice:

* CRUD
* References
* Embedded documents
* Indexes
* Pagination
* Aggregation

---

## 🟠 Intermediate

### 3. E-commerce API

Collections:

```text
users
products
categories
orders
reviews
```

Practice:

* Product search
* Pagination
* Indexes
* Aggregation
* Transactions

---

## 🔴 Advanced

### 4. SaaS Application

Collections:

```text
users
organizations
projects
tasks
subscriptions
payments
notifications
auditLogs
```

Practice:

* Authentication
* Authorization
* Schema design
* Indexes
* Aggregation
* Transactions
* Security
* MongoDB Atlas
* Docker
* Production deployment

---

# 🧠 MongoDB Concepts to Master

Before considering yourself comfortable with MongoDB, understand:

```text
BSON
Documents
Collections
ObjectId
CRUD
Query Operators
Update Operators
Indexes
Aggregation
$lookup
Embedding
References
Schema Design
Validation
Transactions
Replication
Sharding
Security
Backups
MongoDB Atlas
Node.js Driver
Mongoose
Docker
Performance
```

---

# 🌐 MongoDB in a Modern Stack

A common full-stack architecture:

```text
Next.js
   │
   ▼
Node.js / Express
   │
   ▼
MongoDB Driver / Mongoose
   │
   ▼
MongoDB
   │
   ├── Local
   ├── Docker
   └── MongoDB Atlas
```

MongoDB can be used with:

```text
Next.js
Node.js
Express
NestJS
FastAPI
Python
Java
Go
C#
```

---

# 📖 Official Documentation

### MongoDB

🌐 https://www.mongodb.com/

### MongoDB Documentation

🌐 https://www.mongodb.com/docs/

### MongoDB Manual

🌐 https://www.mongodb.com/docs/manual/

### MongoDB CRUD Operations

🌐 https://www.mongodb.com/docs/manual/crud/

### MongoDB Query Operators

🌐 https://www.mongodb.com/docs/manual/reference/operator/query/

### MongoDB Update Operators

🌐 https://www.mongodb.com/docs/manual/reference/operator/update/

### MongoDB Aggregation

🌐 https://www.mongodb.com/docs/manual/aggregation/

### MongoDB Indexes

🌐 https://www.mongodb.com/docs/manual/indexes/

### MongoDB Data Modeling

🌐 https://www.mongodb.com/docs/manual/data-modeling/

### MongoDB Transactions

🌐 https://www.mongodb.com/docs/manual/core/transactions/

### MongoDB Security

🌐 https://www.mongodb.com/docs/manual/security/

### MongoDB Atlas

🌐 https://www.mongodb.com/atlas/

### MongoDB Node.js Driver

🌐 https://www.mongodb.com/docs/drivers/node/current/

### Mongoose

🌐 https://mongoosejs.com/

---

# 🔗 Related Documentation

Inside this repository:

* [Database Learning](./DATABASE_LEARNING.md)
* [SQL vs NoSQL](./SQL_VS_NOSQL.md)
* [PostgreSQL Learning](./POSTGRESQL_LEARNING.md)

---

# 📝 Summary

MongoDB is a document-oriented NoSQL database based on BSON.

The most important concepts are:

```text
MongoDB
│
├── Databases
├── Collections
├── Documents
├── BSON
├── ObjectId
├── CRUD
├── Query Operators
├── Update Operators
├── Indexes
├── Aggregation
├── Embedding
├── References
├── Schema Design
├── Validation
├── Transactions
├── Security
├── Backups
├── MongoDB Atlas
└── Performance
```

For backend development, a useful learning stack is:

```text
Node.js
+
Express
+
MongoDB
+
Mongoose / MongoDB Driver
+
Docker
+
REST API
```

The key to learning MongoDB is not only memorizing commands. Focus on **document modeling, access patterns, indexes, aggregation, consistency, and application architecture**.
