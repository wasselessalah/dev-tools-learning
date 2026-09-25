# 🔄 Reverse Proxy Learning Guide

A practical guide to understanding and using **Reverse Proxy**, especially with **Nginx**, Node.js, Next.js, Docker, HTTPS, APIs, WebSockets, and production deployments.

---

## 📚 Table of Contents

* [1. What Is a Reverse Proxy?](#1-what-is-a-reverse-proxy)
* [2. Reverse Proxy vs Forward Proxy](#2-reverse-proxy-vs-forward-proxy)
* [3. How a Reverse Proxy Works](#3-how-a-reverse-proxy-works)
* [4. Why Use a Reverse Proxy?](#4-why-use-a-reverse-proxy)
* [5. Nginx as a Reverse Proxy](#5-nginx-as-a-reverse-proxy)
* [6. Basic Configuration](#6-basic-configuration)
* [7. Reverse Proxy with Node.js](#7-reverse-proxy-with-nodejs)
* [8. Reverse Proxy with Next.js](#8-reverse-proxy-with-nextjs)
* [9. Frontend + Backend Architecture](#9-frontend--backend-architecture)
* [10. Important Headers](#10-important-headers)
* [11. Understanding `proxy_pass`](#11-understanding-proxy_pass)
* [12. Path Routing](#12-path-routing)
* [13. WebSockets](#13-websockets)
* [14. HTTPS and SSL](#14-https-and-ssl)
* [15. Load Balancing](#15-load-balancing)
* [16. Timeouts](#16-timeouts)
* [17. Reverse Proxy with Docker](#17-reverse-proxy-with-docker)
* [18. Security](#18-security)
* [19. Troubleshooting](#19-troubleshooting)
* [20. Production Architecture](#20-production-architecture)
* [21. Practice Projects](#21-practice-projects)
* [22. Cheat Sheet](#22-cheat-sheet)
* [23. Learning Checklist](#23-learning-checklist)
* [24. Official Resources](#24-official-resources)

---

# 1. What Is a Reverse Proxy?

A **reverse proxy** is a server that receives requests from clients and forwards those requests to one or more backend servers.

The client communicates with the reverse proxy instead of directly communicating with the application server.

### Without a reverse proxy

```text
Browser
   |
   | HTTP
   v
Node.js :4000
```

The user accesses:

```text
http://example.com:4000
```

### With a reverse proxy

```text
Browser
   |
   | HTTPS
   v
Nginx :443
   |
   | HTTP
   v
Node.js :4000
```

The user accesses:

```text
https://example.com
```

Nginx receives the request and forwards it to Node.js.

Nginx's `proxy_pass` directive is specifically designed to pass requests to another server.

---

# 2. Reverse Proxy vs Forward Proxy

## Forward Proxy

A forward proxy works on behalf of the **client**.

```text
Client
   |
   v
Forward Proxy
   |
   v
Internet
```

Example:

```text
Company PC
    |
    v
Corporate Proxy
    |
    v
Internet
```

The destination server may see the proxy instead of the original client.

---

## Reverse Proxy

A reverse proxy works on behalf of the **server/application**.

```text
Client
   |
   v
Reverse Proxy
   |
   +----> Backend 1
   |
   +----> Backend 2
   |
   +----> Backend 3
```

Examples:

* Nginx
* HAProxy
* Traefik
* Caddy

---

# 3. How a Reverse Proxy Works

Suppose you have:

```text
Frontend
https://example.com

Backend
http://127.0.0.1:4000
```

The browser sends:

```http
GET /api/users
Host: example.com
```

Nginx receives the request.

It forwards:

```text
GET /api/users
        |
        v
http://127.0.0.1:4000/api/users
```

The backend responds:

```json
{
  "users": []
}
```

Nginx then sends the response back to the browser.

```text
Browser
   |
   | HTTPS
   v
Nginx
   |
   | HTTP
   v
Express
   |
   v
PostgreSQL
```

---

# 4. Why Use a Reverse Proxy?

Reverse proxies are commonly used for:

* HTTPS termination
* Domain routing
* API routing
* Load balancing
* Security
* Hiding internal ports
* Static files
* WebSockets
* Compression
* Request limits
* Access control
* Logging
* Multiple applications on one server

For example:

```text
https://example.com
        |
        v
      Nginx
       / \
      /   \
     v     v
 Next.js  Express
 :3000    :4000
```

The public user does not need to know the internal ports.

---

# 5. Nginx as a Reverse Proxy

Install Nginx on Ubuntu:

```bash
sudo apt update
sudo apt install nginx
```

Check the version:

```bash
nginx -v
```

Check the service:

```bash
sudo systemctl status nginx
```

Start Nginx:

```bash
sudo systemctl start nginx
```

Enable it at boot:

```bash
sudo systemctl enable nginx
```

---

# 6. Basic Configuration

Suppose your application runs on:

```text
127.0.0.1:4000
```

Create an Nginx server block:

```nginx
server {
    listen 80;

    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:4000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Nginx officially documents `proxy_pass` and `proxy_set_header` for this type of configuration.

---

## Test the configuration

```bash
sudo nginx -t
```

Expected:

```text
syntax is ok
test is successful
```

Reload:

```bash
sudo systemctl reload nginx
```

---

# 7. Reverse Proxy with Node.js

Suppose Express runs on:

```text
127.0.0.1:4000
```

Example Express server:

```js
import express from "express";

const app = express();

app.get("/api/health", (req, res) => {
    res.json({
        status: "ok"
    });
});

app.listen(4000, () => {
    console.log("API running on port 4000");
});
```

Without Nginx:

```text
http://server-ip:4000/api/health
```

With Nginx:

```text
https://example.com/api/health
```

Configuration:

```nginx
server {
    listen 80;
    server_name example.com;

    location /api/ {
        proxy_pass http://127.0.0.1:4000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

# 8. Reverse Proxy with Next.js

Suppose Next.js runs on:

```text
127.0.0.1:3000
```

Start the application:

```bash
npm run build
npm start
```

Nginx:

```nginx
server {
    listen 80;

    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Architecture:

```text
Internet
   |
   v
Nginx :80/:443
   |
   v
Next.js :3000
```

---

# 9. Frontend + Backend Architecture

A common production architecture is:

```text
                    Internet
                       |
                       v
                 ┌───────────┐
                 │   Nginx   │
                 │ :80 / :443│
                 └─────┬─────┘
                       |
              ┌────────┴────────┐
              |                 |
              v                 v
        Next.js :3000     Express :4000
                                |
                                v
                         PostgreSQL :5432
```

For example:

```text
example.com
    |
    v
Next.js

api.example.com
    |
    v
Express
```

Nginx:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Second server:

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:4000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

# 10. Important Headers

When Nginx forwards a request, headers can be explicitly configured with `proxy_set_header`.

## Host

```nginx
proxy_set_header Host $host;
```

Preserves the requested hostname.

Example:

```text
example.com
```

---

## X-Real-IP

```nginx
proxy_set_header X-Real-IP $remote_addr;
```

Passes the client's IP address to the backend.

---

## X-Forwarded-For

```nginx
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

Keeps the forwarding chain of client IP addresses. Nginx documents `$proxy_add_x_forwarded_for` as the incoming `X-Forwarded-For` value with the current client address appended.

---

## X-Forwarded-Proto

```nginx
proxy_set_header X-Forwarded-Proto $scheme;
```

Tells the backend whether the original request used:

```text
http
```

or:

```text
https
```

---

## Recommended basic headers

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

---

# 11. Understanding `proxy_pass`

The most important reverse proxy directive is:

```nginx
proxy_pass http://127.0.0.1:4000;
```

It defines where Nginx sends the request.

---

## Without URI

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:4000;
}
```

A request such as:

```text
/api/users
```

is passed with the request URI preserved.

---

## With URI

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:4000/;
}
```

The trailing `/` changes URI mapping behavior.

Nginx's official documentation explains that when `proxy_pass` contains a URI, the location-matching part of the normalized request URI is replaced by the URI specified in `proxy_pass`.

This is one of the most common Nginx configuration mistakes.

---

# 12. Path Routing

You can route different paths to different applications.

Example:

```text
example.com/
    -> Next.js :3000

example.com/api/
    -> Express :4000

example.com/admin/
    -> Admin application :5000
```

Nginx:

```nginx
server {
    listen 80;
    server_name example.com;

    location /api/ {
        proxy_pass http://127.0.0.1:4000;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:5000;
    }

    location / {
        proxy_pass http://127.0.0.1:3000;
    }
}
```

This allows one public domain to expose multiple internal applications.

---

# 13. WebSockets

WebSockets create a persistent connection between the client and server.

Examples:

* Chat applications
* Real-time notifications
* Live dashboards
* Multiplayer applications

Nginx supports WebSocket proxying, but it requires specific configuration.

Example:

```nginx
location /socket.io/ {
    proxy_pass http://127.0.0.1:4000;

    proxy_http_version 1.1;

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

Architecture:

```text
Browser
   |
   | WebSocket
   v
Nginx
   |
   | WebSocket
   v
Node.js / Socket.IO
```

---

# 14. HTTPS and SSL

A production application should normally use HTTPS.

Architecture:

```text
Browser
   |
   | HTTPS :443
   v
Nginx
   |
   | HTTP
   v
Node.js :4000
```

Nginx handles TLS/HTTPS while the internal application can remain on a private HTTP port.

Example:

```nginx
server {
    listen 443 ssl;
    server_name api.example.com;

    ssl_certificate /etc/letsencrypt/live/api.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.example.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:4000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

HTTP can redirect to HTTPS:

```nginx
server {
    listen 80;
    server_name api.example.com;

    return 301 https://$host$request_uri;
}
```

---

# 15. Load Balancing

A reverse proxy can distribute traffic across multiple application instances.

Example:

```text
                    Nginx
                      |
          ┌───────────┼───────────┐
          |           |           |
          v           v           v
       Node.js     Node.js     Node.js
        :4001       :4002       :4003
```

Nginx uses an `upstream` group:

```nginx
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
```

Nginx supports upstream server groups and HTTP load balancing; round-robin is the default load-balancing method when another method is not configured.

---

## Load balancing methods

Depending on the configuration, you can use methods such as:

```text
Round Robin
Least Connections
IP Hash
Hash
Random
```

Example:

```nginx
upstream backend {
    least_conn;

    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
}
```

---

# 16. Timeouts

Reverse proxies communicate with upstream applications using timeouts.

Important directives include:

```nginx
proxy_connect_timeout 60s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
```

Example:

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:4000;

    proxy_connect_timeout 60s;
    proxy_send_timeout 60s;
    proxy_read_timeout 60s;
}
```

`proxy_read_timeout` controls the time between successive reads from the proxied server; it is not a total response-duration limit.

For long-running requests:

```nginx
proxy_read_timeout 300s;
```

Use long timeouts carefully.

---

# 17. Reverse Proxy with Docker

A common architecture is:

```text
Internet
    |
    v
Nginx
    |
    +------> frontend container
    |
    +------> backend container
    |
    +------> database container
```

Example Docker Compose:

```yaml
services:

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    depends_on:
      - frontend
      - backend

  frontend:
    build: ./frontend
    expose:
      - "3000"

  backend:
    build: ./backend
    expose:
      - "4000"

  postgres:
    image: postgres:16
```

Nginx configuration:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://frontend:3000;
    }

    location /api/ {
        proxy_pass http://backend:4000;
    }
}
```

Inside Docker, services communicate using their **service names**:

```text
frontend:3000
backend:4000
```

not:

```text
localhost:3000
localhost:4000
```

---

# 18. Security

A reverse proxy can help reduce the exposure of internal services.

Instead of exposing:

```text
:3000
:4000
:5000
:5432
```

to the public internet, expose only:

```text
:80
:443
```

Example:

```text
Internet
   |
   +--> 80
   |
   +--> 443
        |
        v
      Nginx
        |
        +--> :3000
        +--> :4000
        +--> :5000
        +--> PostgreSQL
```

---

## Firewall

For Ubuntu:

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

Check:

```bash
sudo ufw status
```

---

## Do not expose PostgreSQL unnecessarily

Avoid:

```text
Internet
   |
   v
PostgreSQL :5432
```

Prefer:

```text
Internet
   |
   v
Nginx
   |
   v
Backend
   |
   v
PostgreSQL
```

---

# 19. Troubleshooting

## Problem: 502 Bad Gateway

Usually means Nginx cannot successfully communicate with the upstream application.

Check:

```bash
curl http://127.0.0.1:4000
```

Check the application:

```bash
sudo systemctl status my-app
```

Check listening ports:

```bash
sudo ss -lntp
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

## Problem: 504 Gateway Timeout

Possible causes:

* Backend is slow
* Database query is slow
* Backend is blocked
* Timeout is too short
* Network problem

Check:

```bash
curl -v http://127.0.0.1:4000/api/test
```

Possible configuration:

```nginx
proxy_read_timeout 120s;
```

Do not increase the timeout blindly. First investigate why the upstream request is slow.

---

## Problem: CORS error

Example:

```text
Frontend:
https://example.com

Backend:
https://api.example.com
```

CORS must be configured correctly in the backend.

For Express:

```js
import cors from "cors";

app.use(
    cors({
        origin: "https://example.com",
        credentials: true
    })
);
```

Nginx reverse proxy configuration does not automatically solve application-level CORS policy.

---

## Problem: Wrong API path

Example:

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:4000/;
}
```

versus:

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:4000;
}
```

The trailing `/` changes URI mapping behavior.

Always test the resulting backend URL carefully.

---

## Problem: Nginx configuration changed but nothing happened

Run:

```bash
sudo nginx -t
```

Then:

```bash
sudo systemctl reload nginx
```

If necessary:

```bash
sudo systemctl restart nginx
```

---

# 20. Production Architecture

A typical production architecture:

```text
                         INTERNET
                            |
                            v
                    ┌──────────────┐
                    │    HTTPS     │
                    │   Port 443   │
                    └──────┬───────┘
                           |
                           v
                    ┌──────────────┐
                    │     Nginx    │
                    │ Reverse Proxy│
                    └──────┬───────┘
                           |
              ┌────────────┼────────────┐
              |            |            |
              v            v            v
          Next.js       Express      WebSocket
           :3000         :4000        :4000
                           |
                           v
                     PostgreSQL
                        :5432
```

Example domain structure:

```text
example.com
```

Frontend:

```text
https://example.com
```

API:

```text
https://api.example.com
```

WebSocket:

```text
wss://api.example.com/socket.io/
```

---

# 21. Practice Projects

## 🟢 Project 1 — Basic Reverse Proxy

Create:

```text
Browser
   |
   v
Nginx
   |
   v
Node.js
```

Requirements:

* Node.js on port `4000`
* Nginx on port `80`
* Domain or local hostname
* `/api/health` endpoint

---

## 🟡 Project 2 — Next.js + Express

Create:

```text
Nginx
   |
   +--> Next.js :3000
   |
   +--> Express :4000
```

Routes:

```text
/       -> Next.js
/api/*  -> Express
```

---

## 🟠 Project 3 — HTTPS

Add:

```text
HTTP :80
   |
   v
HTTPS :443
   |
   v
Nginx
```

Learn:

* SSL certificates
* HTTP → HTTPS redirect
* TLS termination

---

## 🔴 Project 4 — Docker Reverse Proxy

Create:

```text
Nginx container
Frontend container
Backend container
PostgreSQL container
```

Use:

```text
docker compose
```

---

## 🟣 Project 5 — Load Balancing

Run:

```text
Node.js :4001
Node.js :4002
Node.js :4003
```

Configure Nginx:

```text
             Nginx
          /    |    \
         /     |     \
     :4001   :4002   :4003
```

Test multiple requests and observe the upstream servers receiving traffic.

---

# 22. Cheat Sheet

## Test Nginx

```bash
sudo nginx -t
```

## Reload Nginx

```bash
sudo systemctl reload nginx
```

## Restart Nginx

```bash
sudo systemctl restart nginx
```

## Status

```bash
sudo systemctl status nginx
```

## Access logs

```bash
sudo tail -f /var/log/nginx/access.log
```

## Error logs

```bash
sudo tail -f /var/log/nginx/error.log
```

## Check ports

```bash
sudo ss -lntp
```

## Test backend

```bash
curl http://127.0.0.1:4000
```

## Test through Nginx

```bash
curl http://example.com
```

---

## Basic reverse proxy

```nginx
location / {
    proxy_pass http://127.0.0.1:4000;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

## WebSocket

```nginx
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

## Timeout

```nginx
proxy_connect_timeout 60s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
```

---

# 23. Learning Checklist

## Fundamentals

* [ ] Understand what a reverse proxy is
* [ ] Understand reverse proxy vs forward proxy
* [ ] Understand client → proxy → server
* [ ] Understand Nginx server blocks
* [ ] Understand `location`

## Nginx

* [ ] Install Nginx
* [ ] Create a server block
* [ ] Use `proxy_pass`
* [ ] Use `proxy_set_header`
* [ ] Read Nginx logs
* [ ] Test configuration
* [ ] Reload configuration

## Backend

* [ ] Reverse proxy Node.js
* [ ] Reverse proxy Express
* [ ] Reverse proxy Next.js
* [ ] Configure API routes
* [ ] Understand CORS
* [ ] Configure WebSockets

## Networking

* [ ] Understand ports
* [ ] Understand HTTP/HTTPS
* [ ] Understand DNS
* [ ] Understand localhost
* [ ] Understand public vs private ports
* [ ] Understand forwarded headers

## Production

* [ ] Configure HTTPS
* [ ] Configure firewall
* [ ] Protect internal ports
* [ ] Configure timeouts
* [ ] Configure logging
* [ ] Configure load balancing
* [ ] Deploy with Docker

---

# 24. Official Resources

* [Nginx Official Website](https://nginx.org/)
* [Nginx Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)
* [Nginx Proxy Module](https://nginx.org/en/docs/http/ngx_http_proxy_module.html)
* [Nginx HTTP Load Balancing](https://nginx.org/en/docs/http/load_balancing.html)
* [Nginx Upstream Module](https://nginx.org/en/docs/http/ngx_http_upstream_module.html)

---

# 🚀 Recommended Learning Path

Follow this order:

```text
Nginx
   ↓
Reverse Proxy
   ↓
Node.js + Nginx
   ↓
Next.js + Nginx
   ↓
API Routing
   ↓
HTTPS / SSL
   ↓
WebSockets
   ↓
Docker + Nginx
   ↓
Load Balancing
   ↓
Security
   ↓
CI/CD
   ↓
Cloud Deployment
```

---

# 🎯 Final Goal

You should eventually be comfortable deploying an application like:

```text
                         INTERNET
                            │
                            ▼
                    ┌──────────────┐
                    │   DNS / SSL  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    NGINX     │
                    │Reverse Proxy │
                    └──────┬───────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
          Next.js       Express      WebSocket
           :3000         :4000        :4000
                           │
                           ▼
                      PostgreSQL
                           │
                           ▼
                          Redis
```

This architecture gives you a strong foundation for learning **Linux, Nginx, backend development, Docker, DevOps, cloud deployment, and production networking**.
