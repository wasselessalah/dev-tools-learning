# 🌐 Nginx Learning Guide

A practical guide to **Nginx** for web servers, reverse proxies, static files, HTTPS, load balancing, and application deployment.

---

## 📚 Table of Contents

- [What is Nginx?](#-what-is-nginx)
- [Nginx Architecture](#-nginx-architecture)
- [Installation](#-installation)
- [Basic Commands](#-basic-commands)
- [Configuration](#-configuration)
- [Server Blocks](#-server-blocks)
- [Static Websites](#-static-websites)
- [Reverse Proxy](#-reverse-proxy)
- [Node.js with Nginx](#-nodejs-with-nginx)
- [Next.js with Nginx](#-nextjs-with-nginx)
- [HTTPS with Certbot](#-https-with-certbot)
- [Load Balancing](#-load-balancing)
- [Security](#-security)
- [Logs](#-logs)
- [Troubleshooting](#-troubleshooting)
- [Practice Projects](#-practice-projects)
- [Cheat Sheet](#-cheat-sheet)
- [Learning Checklist](#-learning-checklist)
- [Official Resources](#-official-resources)

---

## 🌐 What is Nginx?

**Nginx** is a high-performance web server and reverse proxy.

It can be used for:

- Serving HTML, CSS, JavaScript, and images
- Reverse proxying backend applications
- HTTPS termination
- Load balancing
- Caching
- Compression
- Routing traffic
- Serving multiple domains
- Protecting backend services

Typical production architecture:

```text
Internet
   │
   ▼
Nginx :80 / :443
   │
   ├── Next.js
   ├── Node.js / Express
   ├── FastAPI
   └── Other services
```

---

## 🏗️ Nginx Architecture

```text
                    ┌──────────────┐
                    │   Browser    │
                    └──────┬───────┘
                           │
                     HTTP / HTTPS
                           │
                           ▼
                    ┌──────────────┐
                    │    Nginx     │
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
          Next.js       Express      FastAPI
          :3000          :4000         :8000
```

Nginx receives the public request and forwards it to the appropriate application.

---

## 📦 Installation

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install nginx
```

Check version:

```bash
nginx -v
```

Check service:

```bash
sudo systemctl status nginx
```

Start:

```bash
sudo systemctl start nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

---

## 🧰 Basic Commands

Start:

```bash
sudo systemctl start nginx
```

Stop:

```bash
sudo systemctl stop nginx
```

Restart:

```bash
sudo systemctl restart nginx
```

Reload configuration:

```bash
sudo systemctl reload nginx
```

Check status:

```bash
sudo systemctl status nginx
```

Test configuration:

```bash
sudo nginx -t
```

Show version:

```bash
nginx -v
```

---

## 📁 Important Nginx Locations

On Ubuntu/Debian:

```text
/etc/nginx/
├── nginx.conf
├── sites-available/
├── sites-enabled/
├── conf.d/
├── snippets/
└── mime.types
```

Important files:

```text
/etc/nginx/nginx.conf
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/
```

Logs:

```text
/var/log/nginx/access.log
/var/log/nginx/error.log
```

---

## ⚙️ Configuration

Main configuration:

```bash
sudo nano /etc/nginx/nginx.conf
```

Test after changing configuration:

```bash
sudo nginx -t
```

Then reload:

```bash
sudo systemctl reload nginx
```

Basic configuration:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        root /var/www/html;
        index index.html;
    }
}
```

---

## 🌍 Server Blocks

A server block allows Nginx to handle a website or domain.

Create:

```bash
sudo nano /etc/nginx/sites-available/example.com
```

Example:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Enable:

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

Test:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

---

## 📄 Static Websites

Example directory:

```bash
sudo mkdir -p /var/www/example.com
```

Create:

```bash
sudo nano /var/www/example.com/index.html
```

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Website</title>
</head>
<body>
    <h1>Hello from Nginx!</h1>
</body>
</html>
```

Set ownership:

```bash
sudo chown -R $USER:$USER /var/www/example.com
```

Nginx can now serve the static website.

---

## 🔄 Reverse Proxy

A reverse proxy forwards requests from Nginx to another application.

Example backend:

```text
localhost:4000
```

Nginx:

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:4000;

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
Client
  │
  ▼
api.example.com
  │
  ▼
Nginx :80/:443
  │
  ▼
Express :4000
```

---

## 🟢 Node.js with Nginx

Suppose your Express application runs on:

```text
127.0.0.1:4000
```

Nginx configuration:

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:4000;

        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

The Node.js server does not need to be publicly exposed on port `4000`.

Users access:

```text
https://api.example.com
```

instead of:

```text
http://server-ip:4000
```

---

## ▲ Next.js with Nginx

Suppose Next.js runs on:

```text
localhost:3000
```

Configuration:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

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
Browser
   ↓
Nginx
   ↓
Next.js :3000
```

---

## 🔐 HTTPS with Certbot

Certbot can automate TLS certificates from Let's Encrypt.

Install:

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

Request a certificate:

```bash
sudo certbot --nginx -d example.com -d www.example.com
```

Test renewal:

```bash
sudo certbot renew --dry-run
```

Check certificates:

```bash
sudo certbot certificates
```

After HTTPS:

```text
https://example.com
```

---

## ⚖️ Load Balancing

Nginx can distribute requests across multiple backend servers.

Example:

```nginx
upstream backend {
    server 127.0.0.1:4000;
    server 127.0.0.1:4001;
    server 127.0.0.1:4002;
}

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://backend;
    }
}
```

Architecture:

```text
              ┌── Express :4000
              │
Nginx ────────┼── Express :4001
              │
              └── Express :4002
```

---

## 🔒 Security

### Hide Nginx version

In:

```text
/etc/nginx/nginx.conf
```

Use:

```nginx
server_tokens off;
```

### Firewall

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

SSH:

```bash
sudo ufw allow ssh
```

Check:

```bash
sudo ufw status
```

### Security headers

Example:

```nginx
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
```

Test changes carefully and verify that they do not break your application.

---

## 📊 Logs

Access log:

```bash
sudo tail -f /var/log/nginx/access.log
```

Error log:

```bash
sudo tail -f /var/log/nginx/error.log
```

Last 100 errors:

```bash
sudo tail -n 100 /var/log/nginx/error.log
```

Search logs:

```bash
sudo grep "404" /var/log/nginx/access.log
```

---

## 🐛 Troubleshooting

### Nginx won't start

Check:

```bash
sudo nginx -t
```

Then:

```bash
sudo systemctl status nginx
```

And:

```bash
sudo journalctl -u nginx
```

### Port already in use

Check:

```bash
sudo ss -tulpn | grep ':80'
```

For HTTPS:

```bash
sudo ss -tulpn | grep ':443'
```

### 502 Bad Gateway

Usually means Nginx cannot reach the upstream application.

Check:

```bash
curl http://127.0.0.1:4000
```

Check your application:

```bash
sudo systemctl status your-app
```

If using Docker:

```bash
docker ps
docker logs CONTAINER
```

### 404 Not Found

Check:

- `root`
- `location`
- `try_files`
- file permissions
- `server_name`
- enabled site configuration

### Configuration changes do not work

Run:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Check enabled sites:

```bash
ls -la /etc/nginx/sites-enabled/
```

---

## 🐳 Nginx with Docker

Example:

```yaml
services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
```

Architecture:

```text
Internet
   ↓
Nginx Container
   ↓
Application Container
```

For Docker Compose, services can communicate using their service names:

```nginx
proxy_pass http://backend:4000;
```

---

## 🚀 Production Architecture

A common deployment:

```text
                    Internet
                       │
                       ▼
                  ┌─────────┐
                  │  Nginx  │
                  │  80/443 │
                  └────┬────┘
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
         Frontend             Backend
        Next.js :3000       Express :4000
                                 │
                         ┌───────┴───────┐
                         ▼               ▼
                    PostgreSQL        Redis
```

Nginx handles public HTTP/HTTPS traffic while internal applications can remain private.

---

## 🧪 Practice Projects

### Project 1 — Static Website

Serve an HTML website with Nginx.

### Project 2 — Reverse Proxy

Run an Express API on port `4000` and expose it through Nginx.

### Project 3 — Next.js Deployment

Run Next.js on port `3000` and use Nginx as a reverse proxy.

### Project 4 — HTTPS

Configure a domain and enable HTTPS using Certbot.

### Project 5 — Full Stack Deployment

Deploy:

```text
Nginx
  ↓
Next.js
  ↓
Express
  ↓
PostgreSQL
```

### Project 6 — Docker

Run Nginx + backend + PostgreSQL using Docker Compose.

---

## 📋 Nginx Cheat Sheet

| Task | Command |
|---|---|
| Install | `sudo apt install nginx` |
| Version | `nginx -v` |
| Status | `sudo systemctl status nginx` |
| Start | `sudo systemctl start nginx` |
| Stop | `sudo systemctl stop nginx` |
| Restart | `sudo systemctl restart nginx` |
| Reload | `sudo systemctl reload nginx` |
| Test config | `sudo nginx -t` |
| Access logs | `sudo tail -f /var/log/nginx/access.log` |
| Error logs | `sudo tail -f /var/log/nginx/error.log` |
| Check port 80 | `sudo ss -tulpn \| grep ':80'` |
| Check port 443 | `sudo ss -tulpn \| grep ':443'` |

---

## ✅ Learning Checklist

- [ ] Understand Nginx
- [ ] Install Nginx
- [ ] Understand configuration files
- [ ] Create a server block
- [ ] Serve a static website
- [ ] Configure a reverse proxy
- [ ] Proxy a Node.js API
- [ ] Proxy a Next.js application
- [ ] Configure domains
- [ ] Configure HTTPS
- [ ] Learn Certbot
- [ ] Read Nginx logs
- [ ] Configure firewall rules
- [ ] Learn load balancing
- [ ] Use Nginx with Docker
- [ ] Deploy a full-stack application

---

## 🔗 Official Resources

- [Nginx Official Website](https://nginx.org/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Nginx Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)
- [Let's Encrypt](https://letsencrypt.org/)
- [Certbot](https://certbot.eff.org/)

---

## 🎯 Next Step

Combine Nginx with:

```text
Linux
  ↓
SSH
  ↓
Nginx
  ↓
Node.js
  ↓
Docker
  ↓
PostgreSQL
  ↓
CI/CD
  ↓
AWS / Cloud
```

This gives you a strong foundation for **web server administration, backend deployment, DevOps, and cloud engineering**.
