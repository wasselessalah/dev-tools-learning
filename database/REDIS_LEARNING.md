# 🔴 Redis Learning Guide

A practical guide to learning **Redis** for backend development, caching, authentication, real-time applications, queues, and DevOps.

---

## 📚 Table of Contents

* [What is Redis?](#-what-is-redis)
* [Why Redis?](#-why-redis)
* [Redis vs PostgreSQL vs MongoDB](#-redis-vs-postgresql-vs-mongodb)
* [Installing Redis](#-installing-redis)
* [Redis CLI](#-redis-cli)
* [Redis Keys and Values](#-redis-keys-and-values)
* [Redis Data Types](#-redis-data-types)
* [Strings](#-1-strings)
* [Hashes](#-2-hashes)
* [Lists](#-3-lists)
* [Sets](#-4-sets)
* [Sorted Sets](#-5-sorted-sets)
* [Streams](#-6-streams)
* [Expiration and TTL](#-expiration-and-ttl)
* [Caching](#-caching)
* [Counters](#-counters)
* [Pub/Sub](#-pubsub)
* [Transactions](#-transactions)
* [Pipelines](#-pipelines)
* [Redis Persistence](#-redis-persistence)
* [Redis Security](#-redis-security)
* [Redis with Node.js](#-redis-with-nodejs)
* [Redis with Express](#-redis-with-express)
* [Redis with Next.js](#-redis-with-nextjs)
* [Redis with Docker](#-redis-with-docker)
* [Redis and PostgreSQL](#-redis-and-postgresql)
* [Sessions](#-sessions)
* [Rate Limiting](#-rate-limiting)
* [Queues](#-queues)
* [Real-Time Applications](#-real-time-applications)
* [Production Architecture](#-production-architecture)
* [Common Commands](#-common-commands)
* [Troubleshooting](#-troubleshooting)
* [Practice Projects](#-practice-projects)
* [Learning Roadmap](#-learning-roadmap)
* [Checklist](#-learning-checklist)
* [Useful Resources](#-useful-resources)

---

# 🔴 What is Redis?

**Redis** is an in-memory data store that provides fast access to data using keys and values.

It can be used as:

* ⚡ Cache
* 🗄️ Database
* 📬 Message broker
* 🔄 Queue
* 📡 Pub/Sub system
* 📊 Real-time data store
* 🔎 Search/vector data infrastructure

Redis provides several native data structures including:

* Strings
* Hashes
* Lists
* Sets
* Sorted Sets
* Streams
* JSON
* Geospatial data
* Probabilistic structures
* Vector sets

Official Redis documentation describes Redis as an in-memory data store with use cases including caching, databases, messaging, and streaming.

---

# 🎯 Why Redis?

Redis is especially useful when an application needs very fast access to frequently used data.

Typical examples:

```text
User request
     │
     ▼
   API
     │
     ▼
  Redis
     │
     ├── Cache hit → return data
     │
     └── Cache miss
             │
             ▼
        PostgreSQL
```

Common use cases:

### 1. Caching

```text
Database → Redis → API
```

Instead of querying PostgreSQL for every request, frequently requested data can be stored temporarily in Redis.

### 2. Sessions

```text
User
 ↓
Login
 ↓
Session
 ↓
Redis
```

### 3. Rate limiting

```text
IP / User
    ↓
Redis counter
    ↓
Request allowed?
```

### 4. Queues

```text
API
 ↓
Redis Queue
 ↓
Worker
 ↓
Email / PDF / Notification
```

### 5. Real-time systems

Redis can be used with Pub/Sub and Streams for messaging and event processing.

---

# 🆚 Redis vs PostgreSQL vs MongoDB

| Feature            | Redis                       | PostgreSQL       | MongoDB           |
| ------------------ | --------------------------- | ---------------- | ----------------- |
| Type               | In-memory data store        | Relational DB    | Document DB       |
| Primary model      | Key/value + data structures | Tables/relations | Documents         |
| SQL                | ❌                           | ✅                | ❌                 |
| Very fast cache    | ✅                           | ⚠️               | ⚠️                |
| Complex relations  | ❌                           | ✅                | Limited           |
| Flexible documents | ✅                           | JSONB            | ✅                 |
| Transactions       | ✅                           | ✅                | ✅                 |
| Persistent storage | ✅                           | ✅                | ✅                 |
| Typical role       | Cache / realtime / queues   | Main database    | Document database |

A common production architecture is:

```text
             ┌─────────────┐
             │   Next.js   │
             └──────┬──────┘
                    │
                    ▼
             ┌─────────────┐
             │   Express   │
             └──────┬──────┘
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
     ┌─────────┐        ┌────────────┐
     │  Redis  │        │ PostgreSQL │
     │  Cache  │        │ Main DB    │
     └─────────┘        └────────────┘
```

---

# 💻 Installing Redis

## Ubuntu

```bash
sudo apt update
sudo apt install redis-server
```

Check:

```bash
redis-server --version
```

Start Redis:

```bash
sudo systemctl start redis-server
```

Enable at startup:

```bash
sudo systemctl enable redis-server
```

Check status:

```bash
sudo systemctl status redis-server
```

---

# 🐳 Redis with Docker

Run Redis:

```bash
docker run -d \
  --name redis \
  -p 6379:6379 \
  redis
```

Check:

```bash
docker ps
```

Open Redis CLI:

```bash
docker exec -it redis redis-cli
```

Test:

```redis
PING
```

Expected:

```text
PONG
```

---

# 🖥️ Redis CLI

Connect:

```bash
redis-cli
```

Test the connection:

```redis
PING
```

Response:

```text
PONG
```

Get server information:

```redis
INFO
```

Check Redis server version:

```redis
INFO server
```

---

# 🔑 Redis Keys and Values

Basic structure:

```text
key → value
```

Example:

```redis
SET username "wassel"
GET username
```

Result:

```text
"wassel"
```

Check whether a key exists:

```redis
EXISTS username
```

Delete:

```redis
DEL username
```

Rename:

```redis
RENAME username user_name
```

Find keys:

```redis
SCAN 0
```

> Prefer `SCAN` for production key-space inspection instead of using `KEYS *` on large datasets.

---

# 🧱 Redis Data Types

Redis provides several native data structures.

| Type       | Example use           |
| ---------- | --------------------- |
| String     | Cache, token, counter |
| Hash       | User/session object   |
| List       | Queue                 |
| Set        | Unique values         |
| Sorted Set | Leaderboard           |
| Stream     | Event processing      |
| JSON       | JSON documents        |
| Geospatial | Locations             |

---

# 1️⃣ Strings

Strings are the simplest Redis data type.

```redis
SET username "wassel"
GET username
```

Numbers:

```redis
SET counter 10
INCR counter
```

Result:

```text
11
```

Increment:

```redis
INCR counter
```

Increment by 10:

```redis
INCRBY counter 10
```

Decrease:

```redis
DECR counter
```

Set multiple values:

```redis
MSET name "Wassel" country "Tunisia"
```

Get multiple values:

```redis
MGET name country
```

---

# 2️⃣ Hashes

Hashes are useful for objects.

Example:

```redis
HSET user:1 name "Wassel" age 22 country "Tunisia"
```

Get one field:

```redis
HGET user:1 name
```

Get all fields:

```redis
HGETALL user:1
```

Update:

```redis
HSET user:1 age 23
```

Delete a field:

```redis
HDEL user:1 country
```

Example structure:

```text
user:1
 ├── name
 ├── age
 └── country
```

Hashes are commonly useful for sessions and compact objects.

---

# 3️⃣ Lists

Lists contain ordered values.

Add to the beginning:

```redis
LPUSH tasks "task-1"
```

Add to the end:

```redis
RPUSH tasks "task-2"
```

Read:

```redis
LRANGE tasks 0 -1
```

Remove from beginning:

```redis
LPOP tasks
```

Remove from end:

```redis
RPOP tasks
```

Example queue:

```text
tasks
 ↓
task-1
task-2
task-3
```

---

# 4️⃣ Sets

Sets store unique values.

Add:

```redis
SADD skills "Node.js"
SADD skills "Docker"
SADD skills "Redis"
```

Get members:

```redis
SMEMBERS skills
```

Check membership:

```redis
SISMEMBER skills "Docker"
```

Remove:

```redis
SREM skills "Docker"
```

Count:

```redis
SCARD skills
```

Useful for:

* Unique tags
* User interests
* Permissions
* Unique visitors

---

# 5️⃣ Sorted Sets

Sorted sets contain values associated with scores.

Example leaderboard:

```redis
ZADD leaderboard 100 "Wassel"
ZADD leaderboard 200 "Ahmed"
ZADD leaderboard 150 "Ali"
```

Get ranking:

```redis
ZRANGE leaderboard 0 -1 WITHSCORES
```

Reverse order:

```redis
ZREVRANGE leaderboard 0 -1 WITHSCORES
```

Sorted sets are useful for:

* Leaderboards
* Rankings
* Scores
* Priority systems
* Time-based ordering

---

# 6️⃣ Streams

Redis Streams are designed for event and message-stream processing.

Example:

```redis
XADD events * user "wassel" action "login"
```

Read:

```redis
XRANGE events - +
```

Streams can be used for:

```text
Application
     ↓
Redis Stream
     ↓
Workers
     ├── Email
     ├── Notifications
     └── Analytics
```

Streams are different from simple Pub/Sub because messages can be stored and consumed later.

---

# ⏱️ Expiration and TTL

One of Redis's most useful features for caching is expiration.

Set:

```redis
SET session:user:1 "abc123"
```

Expire after 3600 seconds:

```redis
EXPIRE session:user:1 3600
```

Check TTL:

```redis
TTL session:user:1
```

Set with expiration directly:

```redis
SET cache:user:1 "Wassel" EX 300
```

This means:

```text
cache:user:1
     │
     └── expires after 300 seconds
```

Remove expiration:

```redis
PERSIST cache:user:1
```

---

# ⚡ Caching

A common cache pattern is:

```text
Request
   │
   ▼
Redis
   │
   ├── HIT ──────► Return data
   │
   └── MISS
          │
          ▼
      PostgreSQL
          │
          ▼
        Redis
          │
          ▼
      Return data
```

Example:

```javascript
const cachedUser = await redis.get(`user:${id}`);

if (cachedUser) {
  return JSON.parse(cachedUser);
}

const user = await database.user.findUnique({
  where: { id },
});

await redis.set(
  `user:${id}`,
  JSON.stringify(user),
  { EX: 300 }
);

return user;
```

The cache expires after 5 minutes.

---

# 📊 Cache-Aside Pattern

The most common application caching pattern is:

```text
1. Check Redis
       ↓
2. Found?
   ├── YES → Return
   │
   └── NO
        ↓
3. Query database
        ↓
4. Store result in Redis
        ↓
5. Return result
```

Example:

```javascript
async function getUser(id) {
  const key = `user:${id}`;

  const cached = await redis.get(key);

  if (cached) {
    return JSON.parse(cached);
  }

  const user = await getUserFromDatabase(id);

  await redis.set(key, JSON.stringify(user), {
    EX: 300,
  });

  return user;
}
```

---

# 📈 Counters

Redis is useful for atomic counters.

```redis
SET page:views 0
```

Increment:

```redis
INCR page:views
```

Result:

```text
1
```

Again:

```redis
INCR page:views
```

Result:

```text
2
```

Useful for:

* Page views
* API requests
* Likes
* Downloads
* Login attempts
* Rate limiting

---

# 📡 Pub/Sub

Redis Pub/Sub allows clients to publish messages to channels.

## Subscribe

Terminal 1:

```redis
SUBSCRIBE notifications
```

## Publish

Terminal 2:

```redis
PUBLISH notifications "Hello Wassel"
```

Architecture:

```text
Publisher
    │
    ▼
Redis Channel
    │
    ├── Subscriber 1
    ├── Subscriber 2
    └── Subscriber 3
```

Useful for:

* Notifications
* Chat events
* Real-time updates
* Application events

Redis documents Pub/Sub as a command-based message-passing system.

---

# 🔄 Transactions

Redis transactions use:

```text
MULTI
EXEC
DISCARD
WATCH
```

Example:

```redis
MULTI
SET balance 100
INCR transactions
EXEC
```

Redis executes transaction commands sequentially without another client being served between them during the transaction execution.

---

# 🚀 Pipelines

Pipelining allows multiple commands to be sent together instead of waiting for each response individually.

Without pipeline:

```text
Client → Redis
       ← response

Client → Redis
       ← response

Client → Redis
       ← response
```

With pipeline:

```text
Client ───────────────► Redis
       command 1
       command 2
       command 3
              ◄──────── responses
```

This can reduce network round trips.

---

# 💾 Redis Persistence

Redis is primarily an in-memory data store, but it supports persistence to disk.

Main persistence mechanisms include:

### RDB

Periodic snapshots.

```text
Redis
  ↓
Snapshot
  ↓
dump.rdb
```

### AOF

Logs write operations.

```text
SET user:1 Wassel
SET user:2 Ahmed
INCR counter
```

These operations are recorded so the dataset can be reconstructed.

### RDB + AOF

Both can be used together.

Redis documentation describes RDB as point-in-time snapshots and AOF as a log of write operations.

---

# 🔐 Redis Security

Never expose Redis directly to the public internet without proper protection.

Important practices:

* Use authentication
* Use ACLs where appropriate
* Restrict network access
* Use private networks
* Use TLS where required
* Keep Redis updated
* Avoid exposing port `6379`
* Use strong credentials
* Monitor connections
* Apply least privilege

Example firewall concept:

```text
Internet
   │
   X
Redis 6379
```

Instead:

```text
Internet
   │
   ▼
Application
   │
   ▼
Private Redis
```

---

# 🟢 Redis with Node.js

Install the official Node.js client:

```bash
npm install redis
```

Redis currently documents `node-redis` as the recommended JavaScript/Node.js client for most use cases.

---

## Connect

```javascript
import { createClient } from "redis";

const redis = createClient();

redis.on("error", (error) => {
  console.error("Redis Client Error", error);
});

await redis.connect();

console.log("Redis connected");
```

The default local Redis connection uses port `6379`.

---

## SET / GET

```javascript
await redis.set("name", "Wassel");

const name = await redis.get("name");

console.log(name);
```

---

## Hash

```javascript
await redis.hSet("user:1", {
  name: "Wassel",
  age: "22",
  country: "Tunisia",
});

const user = await redis.hGetAll("user:1");

console.log(user);
```

---

## Expiration

```javascript
await redis.set(
  "verification-code",
  "123456",
  {
    EX: 300,
  }
);
```

The key expires after 5 minutes.

---

## Delete

```javascript
await redis.del("user:1");
```

---

## Close connection

```javascript
await redis.quit();
```

---

# 🌐 Redis with Express

Example:

```javascript
import express from "express";
import { createClient } from "redis";

const app = express();

const redis = createClient();

redis.on("error", (error) => {
  console.error("Redis error:", error);
});

await redis.connect();

app.get("/users/:id", async (req, res) => {
  const { id } = req.params;

  const key = `user:${id}`;

  const cached = await redis.get(key);

  if (cached) {
    return res.json({
      source: "redis",
      data: JSON.parse(cached),
    });
  }

  // Replace this with your database query
  const user = {
    id,
    name: "Wassel",
  };

  await redis.set(
    key,
    JSON.stringify(user),
    {
      EX: 300,
    }
  );

  return res.json({
    source: "database",
    data: user,
  });
});

app.listen(4000, () => {
  console.log("Server running on port 4000");
});
```

---

# ▲ Redis with Next.js

Redis can be used from Next.js server-side code.

Install:

```bash
npm install redis
```

Create:

```text
src/
└── lib/
    └── redis.ts
```

Example:

```typescript
import { createClient } from "redis";

const redis = createClient({
  url: process.env.REDIS_URL,
});

redis.on("error", (error) => {
  console.error("Redis Error:", error);
});

export async function getRedis() {
  if (!redis.isOpen) {
    await redis.connect();
  }

  return redis;
}
```

Environment variable:

```env
REDIS_URL=redis://localhost:6379
```

For production, use the Redis URL provided by your Redis provider.

---

# 🐳 Redis with Docker Compose

Example:

```yaml
services:

  redis:
    image: redis:latest
    container_name: redis
    restart: unless-stopped
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

Start:

```bash
docker compose up -d
```

Check:

```bash
docker compose ps
```

Open CLI:

```bash
docker exec -it redis redis-cli
```

Test:

```redis
PING
```

---

# 🗄️ Redis and PostgreSQL

Redis should not automatically replace PostgreSQL.

A common architecture is:

```text
                ┌─────────────┐
                │   Backend   │
                └──────┬──────┘
                       │
              ┌────────┴────────┐
              │                 │
              ▼                 ▼
        ┌───────────┐     ┌────────────┐
        │   Redis   │     │ PostgreSQL │
        │   Cache   │     │ Main DB    │
        └───────────┘     └────────────┘
```

Use PostgreSQL for:

* Users
* Payments
* Orders
* Products
* Relationships
* Permanent application data

Use Redis for:

* Cache
* Sessions
* Counters
* Rate limiting
* Temporary data
* Queues
* Real-time messaging

---

# 🔑 Sessions with Redis

Example:

```text
User login
    ↓
Backend
    ↓
Create session
    ↓
Redis
```

Example key:

```text
session:abc123
```

Value:

```json
{
  "userId": "123",
  "role": "USER"
}
```

With expiration:

```redis
SET session:abc123 '{"userId":"123","role":"USER"}' EX 3600
```

---

# 🚦 Rate Limiting

Redis can implement counters with expiration.

Example:

```text
IP: 192.168.1.10
Requests: 20
Window: 60 seconds
```

Concept:

```redis
INCR rate:192.168.1.10
EXPIRE rate:192.168.1.10 60
```

Application logic:

```text
Request
   ↓
Redis counter
   ↓
Count < limit?
 ├── YES → Allow
 └── NO  → HTTP 429
```

For production applications, use an established rate-limiting library or carefully implement atomic operations to avoid race conditions.

---

# 📬 Queues

Redis can be used as part of queue architectures.

Example:

```text
API
 │
 ▼
Redis
 │
 ▼
Queue
 │
 ├── Worker 1
 ├── Worker 2
 └── Worker 3
```

Typical jobs:

* Send email
* Generate PDF
* Process image
* Send notification
* Run background task
* Process webhook

Popular Node.js tools include:

```text
BullMQ
```

---

# ⚡ Real-Time Applications

Redis can be used with WebSockets and other real-time architectures.

Example:

```text
              Redis
             /     \
            /       \
       Server 1    Server 2
          │           │
       Users        Users
```

This can help multiple application instances share events.

Example:

```text
User A
  ↓
Server 1
  ↓
Redis Pub/Sub
  ↓
Server 2
  ↓
User B
```

---

# 🏗️ Production Architecture

A larger application might look like:

```text
                    Internet
                       │
                       ▼
                 ┌───────────┐
                 │   Nginx   │
                 └─────┬─────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
        ┌──────────┐      ┌──────────┐
        │ Node.js  │      │ Node.js  │
        │ Server 1 │      │ Server 2 │
        └────┬─────┘      └────┬─────┘
             │                 │
             └────────┬────────┘
                      │
              ┌───────┴────────┐
              ▼                ▼
         ┌─────────┐      ┌────────────┐
         │  Redis  │      │ PostgreSQL │
         │  Cache  │      │ Main DB    │
         └─────────┘      └────────────┘
```

---

# 🧪 Common Redis Commands

## Connection

```redis
PING
INFO
```

## Strings

```redis
SET key value
GET key
MSET key1 value1 key2 value2
MGET key1 key2
INCR counter
DECR counter
```

## Keys

```redis
EXISTS key
DEL key
EXPIRE key 60
TTL key
PERSIST key
SCAN 0
```

## Hashes

```redis
HSET user:1 name Wassel
HGET user:1 name
HGETALL user:1
HDEL user:1 name
```

## Lists

```redis
LPUSH tasks task1
RPUSH tasks task2
LRANGE tasks 0 -1
LPOP tasks
RPOP tasks
```

## Sets

```redis
SADD skills Node.js
SMEMBERS skills
SISMEMBER skills Node.js
SREM skills Node.js
```

## Sorted Sets

```redis
ZADD leaderboard 100 Wassel
ZRANGE leaderboard 0 -1 WITHSCORES
ZREVRANGE leaderboard 0 -1 WITHSCORES
```

## Pub/Sub

```redis
SUBSCRIBE notifications
PUBLISH notifications "Hello"
```

## Streams

```redis
XADD events * type login
XRANGE events - +
```

---

# 🔍 Monitoring Redis

Useful commands:

```redis
INFO
```

Check memory:

```redis
INFO memory
```

Check clients:

```redis
INFO clients
```

Check statistics:

```redis
INFO stats
```

See slow commands:

```redis
SLOWLOG GET
```

Check configuration:

```redis
CONFIG GET *
```

---

# 🐛 Troubleshooting

## Redis not running

Check:

```bash
sudo systemctl status redis-server
```

Start:

```bash
sudo systemctl start redis-server
```

---

## Cannot connect

Test:

```bash
redis-cli ping
```

Expected:

```text
PONG
```

Check port:

```bash
ss -lntp | grep 6379
```

---

## Docker Redis not working

Check:

```bash
docker ps
```

Logs:

```bash
docker logs redis
```

Open shell:

```bash
docker exec -it redis redis-cli
```

---

## Check Redis memory

```redis
INFO memory
```

---

# 🧪 Practice Projects

## 🟢 Beginner

### Project 1 — Redis CLI Practice

Practice:

```text
SET
GET
DEL
EXPIRE
TTL
INCR
```

---

### Project 2 — User Cache

Build:

```text
Node.js
   ↓
PostgreSQL
   ↓
Redis cache
```

Features:

* Get user
* Cache user
* TTL
* Delete cache after update

---

## 🟡 Intermediate

### Project 3 — API Rate Limiter

Build:

```text
Express API
     ↓
Redis
     ↓
Rate limiter
```

Example:

```text
100 requests / minute / IP
```

---

### Project 4 — Session Manager

Build:

```text
Login
  ↓
Session
  ↓
Redis
```

Features:

* Login
* Logout
* Session expiration
* Session lookup

---

### Project 5 — Leaderboard

Use:

```text
Sorted Sets
```

Features:

* Add player
* Add score
* Get ranking
* Get top 10
* Update score

---

## 🔴 Advanced

### Project 6 — Background Job System

Architecture:

```text
Express
  ↓
Redis Queue
  ↓
Workers
  ├── Email
  ├── PDF
  └── Notifications
```

---

### Project 7 — Real-Time Chat

Architecture:

```text
Next.js
   │
WebSocket
   │
Node.js
   │
Redis
   │
PostgreSQL
```

---

### Project 8 — Distributed API

Build:

```text
             Nginx
             /   \
            /     \
       Node 1    Node 2
          \        /
           \      /
            Redis
              │
          PostgreSQL
```

Learn:

* Load balancing
* Shared cache
* Sessions
* Rate limiting
* Pub/Sub
* Docker

---

# 🛣️ Learning Roadmap

```text
Redis Basics
     ↓
Redis CLI
     ↓
Keys / Values
     ↓
Strings
     ↓
Hashes
     ↓
Lists
     ↓
Sets
     ↓
Sorted Sets
     ↓
TTL / Expiration
     ↓
Caching
     ↓
Node.js + Redis
     ↓
Express + Redis
     ↓
Sessions
     ↓
Rate Limiting
     ↓
Pub/Sub
     ↓
Streams
     ↓
Transactions
     ↓
Persistence
     ↓
Docker
     ↓
Production Redis
```

---

# ✅ Learning Checklist

## Fundamentals

* [ ] Understand Redis
* [ ] Install Redis
* [ ] Use Redis CLI
* [ ] Understand keys
* [ ] Understand TTL
* [ ] Understand expiration

## Data Structures

* [ ] Strings
* [ ] Hashes
* [ ] Lists
* [ ] Sets
* [ ] Sorted Sets
* [ ] Streams

## Backend

* [ ] Node.js + Redis
* [ ] Express + Redis
* [ ] Caching
* [ ] Sessions
* [ ] Rate limiting
* [ ] Queues
* [ ] Pub/Sub

## Production

* [ ] Persistence
* [ ] Security
* [ ] Monitoring
* [ ] Docker
* [ ] Redis + PostgreSQL
* [ ] Multiple Node.js instances
* [ ] High availability concepts

---

# 🧠 Important Concepts to Learn

Before using Redis in production, understand:

```text
Cache invalidation
TTL
Eviction
Persistence
Memory management
Atomic operations
Transactions
Pipelines
Pub/Sub
Streams
Replication
High availability
Redis Cluster
Security
Monitoring
```

---

# 📌 Redis Commands Cheat Sheet

| Command     | Purpose               |
| ----------- | --------------------- |
| `SET`       | Store value           |
| `GET`       | Read value            |
| `DEL`       | Delete key            |
| `EXISTS`    | Check key             |
| `EXPIRE`    | Set expiration        |
| `TTL`       | Check expiration      |
| `INCR`      | Increment             |
| `HSET`      | Set hash field        |
| `HGET`      | Get hash field        |
| `HGETALL`   | Get hash              |
| `LPUSH`     | Add to list           |
| `LPOP`      | Remove from list      |
| `SADD`      | Add to set            |
| `SMEMBERS`  | Get set members       |
| `ZADD`      | Add sorted-set member |
| `ZRANGE`    | Get sorted-set range  |
| `XADD`      | Add stream event      |
| `XREAD`     | Read stream           |
| `PUBLISH`   | Publish message       |
| `SUBSCRIBE` | Subscribe to channel  |
| `MULTI`     | Start transaction     |
| `EXEC`      | Execute transaction   |
| `SCAN`      | Iterate through keys  |

Redis's current command documentation provides the complete command reference and groups commands by data type and functionality.

---

# 🔗 Useful Resources

### Redis

* [Redis Official Website](https://redis.io/)
* [Redis Documentation](https://redis.io/docs/latest/)
* [Redis Open Source](https://redis.io/docs/latest/get-started/)
* [Redis Data Types](https://redis.io/docs/latest/develop/data-types/)
* [Redis Commands](https://redis.io/docs/latest/commands/)
* [Redis Command Cheat Sheet](https://redis.io/tutorials/howtos/quick-start/cheat-sheet/)

### Node.js

* [Node.js + Redis](https://redis.io/docs/latest/develop/clients/nodejs/)
* [Node.js Redis Client](https://redis.io/docs/latest/integrate/node-redis/)
* [Redis Node.js Quick Start](https://redis.io/tutorials/develop/node/gettingstarted/)

### Advanced

* [Redis Transactions](https://redis.io/docs/latest/develop/using-commands/transactions/)
* [Redis Persistence](https://redis.io/docs/latest/operate/oss_and_stack/management/persistence/)
* [Redis Client Libraries](https://redis.io/docs/latest/develop/clients/)

---

# 🎯 Final Goal

After completing this guide, you should be able to build applications using:

```text
Next.js
   ↓
Node.js / Express
   ↓
┌───────────────┐
│ Redis         │
│               │
│ • Cache       │
│ • Sessions    │
│ • Rate Limit  │
│ • Queues      │
│ • Pub/Sub     │
│ • Streams     │
└───────────────┘
   ↓
PostgreSQL
```

The most important practical skills to focus on first are:

```text
Redis CLI
   ↓
Strings + Hashes
   ↓
TTL
   ↓
Caching
   ↓
Node.js + Redis
   ↓
Sessions
   ↓
Rate Limiting
   ↓
Queues
   ↓
Pub/Sub
   ↓
Streams
```
