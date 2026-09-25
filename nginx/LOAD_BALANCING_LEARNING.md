# ⚖️ Load Balancing Learning Guide

A practical guide to understanding and implementing **Load Balancing** with **Nginx**, Node.js, Docker, APIs, and production systems.

---

## 📚 Table of Contents

* [1. What Is Load Balancing?](#1-what-is-load-balancing)
* [2. Why Use Load Balancing?](#2-why-use-load-balancing)
* [3. How Load Balancing Works](#3-how-load-balancing-works)
* [4. Load Balancer vs Reverse Proxy](#4-load-balancer-vs-reverse-proxy)
* [5. Basic Nginx Load Balancing](#5-basic-nginx-load-balancing)
* [6. Upstream Servers](#6-upstream-servers)
* [7. Round Robin](#7-round-robin)
* [8. Weighted Load Balancing](#8-weighted-load-balancing)
* [9. Least Connections](#9-least-connections)
* [10. IP Hash](#10-ip-hash)
* [11. Least Time](#11-least-time)
* [12. Server Failure Handling](#12-server-failure-handling)
* [13. Backup Servers](#13-backup-servers)
* [14. Connection Limits](#14-connection-limits)
* [15. Session Persistence](#15-session-persistence)
* [16. Stateless Applications](#16-stateless-applications)
* [17. Load Balancing with Node.js](#17-load-balancing-with-nodejs)
* [18. Load Balancing with Docker](#18-load-balancing-with-docker)
* [19. Load Balancing Architecture](#19-load-balancing-architecture)
* [20. Database Considerations](#20-database-considerations)
* [21. Monitoring](#21-monitoring)
* [22. Troubleshooting](#22-troubleshooting)
* [23. Practice Projects](#23-practice-projects)
* [24. Cheat Sheet](#24-cheat-sheet)
* [25. Learning Checklist](#25-learning-checklist)
* [26. Official Resources](#26-official-resources)

---

# 1. What Is Load Balancing?

**Load balancing** is the process of distributing incoming traffic across multiple servers or application instances.

Instead of:

```text
Client
   |
   v
Server
```

you have:

```text
                    Client
                       |
                       v
                 Load Balancer
                       |
          ┌────────────┼────────────┐
          |            |            |
          v            v            v
       Server 1     Server 2     Server 3
```

The goal is to prevent one server from handling all the traffic.

Nginx can distribute HTTP traffic across multiple application servers and supports several balancing methods.

---

# 2. Why Use Load Balancing?

Load balancing can help with:

* Scalability
* Performance
* Availability
* Fault tolerance
* Traffic distribution
* Horizontal scaling
* Maintenance
* Application reliability

For example:

```text
1000 requests
      |
      v
Load Balancer
   /   |   \
  /    |    \
333   333   334
 |     |     |
Server Server Server
  1      2      3
```

Instead of one server processing all 1000 requests:

```text
1000 requests
      |
      v
 Server 1
```

---

# 3. How Load Balancing Works

Imagine three Node.js applications:

```text
Node.js #1 → 127.0.0.1:4001
Node.js #2 → 127.0.0.1:4002
Node.js #3 → 127.0.0.1:4003
```

Nginx sits in front:

```text
                    Internet
                       |
                       v
                    Nginx
                       |
          ┌────────────┼────────────┐
          |            |            |
          v            v            v
       :4001         :4002         :4003
          |            |            |
       Node.js      Node.js      Node.js
```

The client only needs to know:

```text
https://api.example.com
```

It does not need to know:

```text
:4001
:4002
:4003
```

---

# 4. Load Balancer vs Reverse Proxy

A reverse proxy forwards requests to backend servers.

```text
Client
  |
  v
Reverse Proxy
  |
  v
Backend
```

A load balancer distributes requests between multiple backend servers.

```text
Client
  |
  v
Load Balancer
  |
  +----> Backend 1
  |
  +----> Backend 2
  |
  +----> Backend 3
```

Nginx can perform both roles.

```text
                 Nginx
          Reverse Proxy
                 +
           Load Balancer
                 |
       ┌─────────┼─────────┐
       v         v         v
    Server 1  Server 2  Server 3
```

---

# 5. Basic Nginx Load Balancing

The core configuration uses an `upstream` group.

```nginx
http {

    upstream backend {
        server 127.0.0.1:4001;
        server 127.0.0.1:4002;
        server 127.0.0.1:4003;
    }

    server {
        listen 80;

        server_name api.example.com;

        location / {
            proxy_pass http://backend;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

Nginx's `upstream` directive defines a group of backend servers that can then be referenced by `proxy_pass`.

---

# 6. Upstream Servers

An upstream is a group of backend servers.

```nginx
upstream backend {

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}
```

Then:

```nginx
location / {
    proxy_pass http://backend;
}
```

Architecture:

```text
                 Nginx
                   |
                   v
             upstream backend
              /      |      \
             /       |       \
            v        v        v
         :4001     :4002     :4003
```

---

# 7. Round Robin

Round robin distributes requests between servers in sequence.

Example:

```text
Request 1 → Server 1
Request 2 → Server 2
Request 3 → Server 3
Request 4 → Server 1
Request 5 → Server 2
Request 6 → Server 3
```

Configuration:

```nginx
upstream backend {
    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:4003;
}
```

Round robin is the default balancing method for an Nginx upstream group when another method is not configured.

---

# 8. Weighted Load Balancing

Sometimes servers have different capacities.

Example:

```text
Server 1 → powerful
Server 2 → medium
Server 3 → small
```

You can give servers different weights.

```nginx
upstream backend {

    server 127.0.0.1:4001 weight=5;

    server 127.0.0.1:4002 weight=3;

    server 127.0.0.1:4003 weight=1;
}
```

Conceptually:

```text
Server 1 → 5
Server 2 → 3
Server 3 → 1
```

A higher weight means the server receives a larger share of traffic under the balancing method.

---

# 9. Least Connections

With least connections, Nginx sends the next request to a server with fewer active connections.

Configuration:

```nginx
upstream backend {

    least_conn;

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}
```

Example:

```text
Server 1 → 20 connections
Server 2 → 5 connections
Server 3 → 12 connections

New request
     |
     v
Server 2
```

This can be useful when requests have significantly different processing times. Nginx documents `least_conn` as selecting the server with the least number of active connections, taking weights into account.

---

# 10. IP Hash

IP hash uses the client's IP address to determine the backend server.

```nginx
upstream backend {

    ip_hash;

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}
```

Conceptually:

```text
Client A
   |
   +----> Server 1

Client B
   |
   +----> Server 2

Client C
   |
   +----> Server 3
```

The same client will generally be sent to the same server while that server is available.

### Important

IP hash is not a replacement for proper shared session storage.

If you use:

```text
Redis
```

or a database for sessions, your application can remain stateless and does not need to depend on a particular application instance.

---

# 11. Least Time

Nginx also supports least-time balancing in applicable versions/configurations.

Example:

```nginx
upstream backend {

    least_time header;

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}
```

The balancing decision considers response time and active connections.

Nginx documents:

```text
least_time header
least_time last_byte
least_time last_byte inflight
```

with availability depending on the Nginx edition/version.

---

# 12. Server Failure Handling

Suppose:

```text
Server 1 → ❌
Server 2 → ✅
Server 3 → ✅
```

The load balancer should avoid repeatedly sending traffic to a failed server.

Nginx provides passive failure handling using parameters such as:

```nginx
max_fails
fail_timeout
```

Example:

```nginx
upstream backend {

    server 127.0.0.1:4001
        max_fails=3
        fail_timeout=30s;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}
```

Nginx can temporarily avoid an upstream server after communication failures.

---

# 13. Backup Servers

You can define a backup server.

```nginx
upstream backend {

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:5000 backup;
}
```

Architecture:

```text
Normal traffic
     |
     +----> Server 1
     |
     +----> Server 2

             Server 3
             Backup
```

The backup server is used when the primary servers are unavailable according to the upstream selection behavior.

---

# 14. Connection Limits

You can limit the number of simultaneous active connections to an upstream server.

Example:

```nginx
upstream backend {

    server 127.0.0.1:4001 max_conns=100;

    server 127.0.0.1:4002 max_conns=100;
}
```

This can help prevent an individual backend from being overloaded.

Nginx documents `max_conns` as limiting the maximum number of simultaneous active connections to a proxied server.

---

# 15. Session Persistence

Load balancing creates an important question:

> What happens if a user's requests go to different servers?

Example:

```text
Request 1 → Server 1
Request 2 → Server 2
Request 3 → Server 3
```

If the session exists only in Server 1's memory:

```text
Server 1
  |
  +-- session = user123
```

then Server 2 does not know about it.

---

## Bad architecture

```text
             Load Balancer
              /    |    \
             v     v     v
          Server Server Server
             |
          Memory Session
```

---

## Better architecture

Use shared session storage:

```text
             Load Balancer
              /    |    \
             v     v     v
          Server Server Server
              \     |     /
               \    |    /
                  Redis
```

Or:

```text
Servers
   |
   v
PostgreSQL
```

This allows any backend instance to retrieve the required session/state.

---

# 16. Stateless Applications

A scalable backend should ideally be **stateless**.

Instead of:

```text
Server 1
  |
  +-- user session
```

use:

```text
Server 1 ──┐
Server 2 ──┼──> Redis / Database
Server 3 ──┘
```

For authentication, a common architecture is:

```text
Browser
   |
   v
Nginx
   |
   +------> API Server 1
   |
   +------> API Server 2
   |
   +------> API Server 3
                 |
                 v
               Redis
                 |
                 v
             PostgreSQL
```

---

# 17. Load Balancing with Node.js

Create three Node.js servers.

## Server 1

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.json({
        server: "server-1"
    });
});

app.listen(4001, () => {
    console.log("Server 1 running on port 4001");
});
```

## Server 2

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.json({
        server: "server-2"
    });
});

app.listen(4002, () => {
    console.log("Server 2 running on port 4002");
});
```

## Server 3

```js
import express from "express";

const app = express();

app.get("/", (req, res) => {
    res.json({
        server: "server-3"
    });
});

app.listen(4003, () => {
    console.log("Server 3 running on port 4003");
});
```

Nginx:

```nginx
upstream node_backend {

    server 127.0.0.1:4001;

    server 127.0.0.1:4002;

    server 127.0.0.1:4003;
}

server {

    listen 80;

    server_name api.example.com;

    location / {

        proxy_pass http://node_backend;

        proxy_set_header Host $host;

        proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Test:

```bash
curl http://api.example.com
```

You should see responses from different application instances over multiple requests.

---

# 18. Load Balancing with Docker

Docker makes it easy to run multiple application instances.

Architecture:

```text
                    Nginx
                      |
          ┌───────────┼───────────┐
          |           |           |
          v           v           v
       Backend 1   Backend 2   Backend 3
       :4001       :4002       :4003
```

Example:

```yaml
services:

  backend1:
    build: ./backend
    environment:
      SERVER_NAME: backend-1
    expose:
      - "4000"

  backend2:
    build: ./backend
    environment:
      SERVER_NAME: backend-2
    expose:
      - "4000"

  backend3:
    build: ./backend
    environment:
      SERVER_NAME: backend-3
    expose:
      - "4000"
```

Nginx can proxy to the service names:

```nginx
upstream backend {

    server backend1:4000;

    server backend2:4000;

    server backend3:4000;
}
```

Then:

```nginx
location / {
    proxy_pass http://backend;
}
```

---

# 19. Load Balancing Architecture

A small production architecture might look like:

```text
                         Internet
                            |
                            v
                     ┌─────────────┐
                     │    Nginx    │
                     │Load Balancer│
                     └──────┬──────┘
                            |
              ┌─────────────┼─────────────┐
              |             |             |
              v             v             v
          API Server    API Server    API Server
              1             2             3
              |             |             |
              └─────────────┼─────────────┘
                            |
                       ┌────┴────┐
                       |         |
                       v         v
                    Redis    PostgreSQL
```

---

# 20. Database Considerations

Adding more backend servers does **not** automatically mean you should add more databases.

Example:

```text
Nginx
  |
  ├── API 1 ──┐
  ├── API 2 ──┼──> PostgreSQL
  └── API 3 ──┘
```

The database can become the bottleneck.

You should monitor:

```text
CPU
Memory
Connections
Queries
Locks
Disk I/O
Query latency
Connection pool
```

For PostgreSQL applications, use a proper connection pool and avoid creating unnecessary database connections for every request.

---

# 21. Monitoring

A load-balanced system should be monitored.

Important metrics:

### Application

```text
Requests/sec
Response time
Error rate
CPU
Memory
Active connections
```

### Nginx

```text
Requests
5xx errors
4xx errors
Upstream failures
Latency
Connections
```

### Database

```text
CPU
Memory
Connections
Slow queries
Locks
Disk usage
```

### Infrastructure

```text
CPU
RAM
Disk
Network
Load average
```

Useful tools:

```text
Prometheus
Grafana
Nginx logs
Node.js metrics
Docker stats
```

---

# 22. Troubleshooting

## 502 Bad Gateway

Possible causes:

```text
Nginx
  |
  X
Backend unavailable
```

Check:

```bash
sudo ss -lntp
```

Test each backend:

```bash
curl http://127.0.0.1:4001
curl http://127.0.0.1:4002
curl http://127.0.0.1:4003
```

Check Nginx:

```bash
sudo nginx -t
```

Check logs:

```bash
sudo tail -f /var/log/nginx/error.log
```

---

## One server receives all traffic

Check the upstream:

```nginx
upstream backend {

    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:4003;
}
```

Make sure the `proxy_pass` references the upstream:

```nginx
proxy_pass http://backend;
```

---

## Sessions randomly disappear

Possible architecture:

```text
Request 1 → Server 1
Request 2 → Server 2
```

but sessions exist only in server memory.

Use shared state:

```text
Redis
```

or:

```text
PostgreSQL
```

or another shared session mechanism.

---

## Backend is overloaded

Check:

```text
CPU
RAM
Database
Connections
Slow requests
```

Then consider:

```text
More backend instances
```

instead of simply increasing server resources.

---

# 23. Practice Projects

## 🟢 Project 1 — Two Node.js Servers

Create:

```text
Node.js :4001
Node.js :4002
```

Put Nginx in front:

```text
Client
  |
  v
Nginx
  |
  +----> :4001
  |
  +----> :4002
```

Return the server name from each application.

---

## 🟡 Project 2 — Three Servers

Run:

```text
:4001
:4002
:4003
```

Configure:

```nginx
upstream backend {
    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:4003;
}
```

Test with:

```bash
for i in {1..10}; do
    curl http://localhost
    echo
done
```

---

## 🟠 Project 3 — Weighted Load Balancing

Configure:

```nginx
upstream backend {
    server 127.0.0.1:4001 weight=5;
    server 127.0.0.1:4002 weight=2;
    server 127.0.0.1:4003 weight=1;
}
```

Send many requests and compare the distribution.

---

## 🔴 Project 4 — Least Connections

Use:

```nginx
upstream backend {

    least_conn;

    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:4003;
}
```

Create endpoints with different response times.

Observe how the request distribution changes.

---

## 🟣 Project 5 — Docker Load Balancing

Create:

```text
Nginx
Backend 1
Backend 2
Backend 3
Redis
PostgreSQL
```

Architecture:

```text
                  Nginx
                    |
          ┌─────────┼─────────┐
          v         v         v
       Backend1  Backend2  Backend3
          \         |         /
           \        |        /
                 Redis
                   |
               PostgreSQL
```

---

# 24. Cheat Sheet

## Basic upstream

```nginx
upstream backend {
    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:4003;
}
```

## Proxy to upstream

```nginx
location / {
    proxy_pass http://backend;
}
```

## Weighted

```nginx
upstream backend {
    server 127.0.0.1:4001 weight=3;
    server 127.0.0.1:4002 weight=1;
}
```

## Least connections

```nginx
upstream backend {

    least_conn;

    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
}
```

## IP hash

```nginx
upstream backend {

    ip_hash;

    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
}
```

## Backup

```nginx
upstream backend {
    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
    server 127.0.0.1:5000 backup;
}
```

## Failure configuration

```nginx
server 127.0.0.1:4001
    max_fails=3
    fail_timeout=30s;
```

## Connection limit

```nginx
server 127.0.0.1:4001 max_conns=100;
```

## Test configuration

```bash
sudo nginx -t
```

## Reload

```bash
sudo systemctl reload nginx
```

## View logs

```bash
sudo tail -f /var/log/nginx/access.log
```

```bash
sudo tail -f /var/log/nginx/error.log
```

---

# 25. Learning Checklist

## Fundamentals

* [ ] Understand load balancing
* [ ] Understand horizontal scaling
* [ ] Understand vertical scaling
* [ ] Understand reverse proxy
* [ ] Understand upstream servers
* [ ] Understand scalability
* [ ] Understand fault tolerance

## Nginx

* [ ] Configure `upstream`
* [ ] Configure `proxy_pass`
* [ ] Understand round robin
* [ ] Understand weights
* [ ] Understand `least_conn`
* [ ] Understand `ip_hash`
* [ ] Understand failure handling
* [ ] Understand backup servers
* [ ] Understand `max_conns`

## Backend

* [ ] Run multiple Node.js instances
* [ ] Make applications stateless
* [ ] Understand shared sessions
* [ ] Use Redis when appropriate
* [ ] Use PostgreSQL correctly
* [ ] Understand connection pooling

## Docker

* [ ] Run multiple containers
* [ ] Create Docker networks
* [ ] Use Docker Compose
* [ ] Put Nginx in front of containers
* [ ] Scale backend services
* [ ] Monitor containers

## Production

* [ ] HTTPS
* [ ] Monitoring
* [ ] Logging
* [ ] Health endpoints
* [ ] Error handling
* [ ] Database monitoring
* [ ] Redis
* [ ] CI/CD
* [ ] Cloud deployment

---

# 26. Official Resources

* [Nginx Load Balancing](https://nginx.org/en/docs/http/load_balancing.html)
* [Nginx Upstream Module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)
* [Nginx Proxy Module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)
* [Nginx Documentation](https://nginx.org/en/docs/)

---

# 🚀 Recommended Learning Path

```text
Reverse Proxy
      ↓
Load Balancing
      ↓
Round Robin
      ↓
Weighted Load Balancing
      ↓
Least Connections
      ↓
IP Hash
      ↓
Health / Failure Handling
      ↓
Stateless Backend
      ↓
Redis
      ↓
Docker
      ↓
Monitoring
      ↓
CI/CD
      ↓
Cloud Load Balancers
      ↓
Kubernetes
```

---

# 🎯 Final Architecture

A strong learning target is:

```text
                         INTERNET
                            |
                            v
                    ┌──────────────┐
                    │    NGINX     │
                    │Load Balancer │
                    └──────┬───────┘
                           |
              ┌────────────┼────────────┐
              |            |            |
              v            v            v
          Backend 1    Backend 2    Backend 3
           Node.js      Node.js      Node.js
              |            |            |
              └────────────┼────────────┘
                           |
                    ┌──────┴──────┐
                    |             |
                    v             v
                  Redis       PostgreSQL
```

The key concept is:

```text
                 MANY CLIENTS
                      |
                      v
               LOAD BALANCER
                      |
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
     SERVER 1      SERVER 2      SERVER 3
```

This is the foundation for understanding **horizontal scaling, high availability, reverse proxies, distributed applications, Docker, Kubernetes, and cloud infrastructure**.
