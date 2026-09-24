# 🔐 SSH Learning Guide

A practical guide to **SSH (Secure Shell)** for Linux administration, remote servers, development, deployment, and DevOps.

## 📚 Table of Contents

- [What is SSH?](#-what-is-ssh)
- [How SSH Works](#-how-ssh-works)
- [Installing SSH](#-installing-ssh)
- [Basic SSH Connection](#-basic-ssh-connection)
- [SSH Keys](#-ssh-keys)
- [SSH Config](#-ssh-config)
- [Copying Files](#-copying-files)
- [SSH Agent](#-ssh-agent)
- [SSH Server Configuration](#-ssh-server-configuration)
- [Security Best Practices](#-security-best-practices)
- [SSH with GitHub](#-ssh-with-github)
- [SSH for Deployment](#-ssh-for-deployment)
- [SSH Tunneling](#-ssh-tunneling)
- [Troubleshooting](#-troubleshooting)
- [Practice Projects](#-practice-projects)
- [Cheat Sheet](#-cheat-sheet)
- [Learning Checklist](#-learning-checklist)

---

## 🔐 What is SSH?

**SSH (Secure Shell)** is a network protocol used to securely connect to another computer over a network.

Common uses:

- Connect to Linux servers
- Manage VPS servers
- Execute commands remotely
- Copy files
- Deploy applications
- Manage cloud servers
- Connect GitHub using SSH keys
- Create secure tunnels
- Remote administration

Example:

```bash
ssh user@server-ip
```

## ⚙️ How SSH Works

```text
Your Computer
     │
     │ SSH
     ▼
Internet / Network
     │
     ▼
Linux Server
     │
     ├── SSH Server
     ├── Application
     ├── Database
     └── Files
```

The SSH client runs on your computer and the SSH server (`sshd`) runs on the remote machine.

---

## 🐧 Installing SSH

### Ubuntu / Debian

Install the client:

```bash
sudo apt update
sudo apt install openssh-client
```

Check:

```bash
ssh -V
```

Install the SSH server:

```bash
sudo apt install openssh-server
```

Check the service:

```bash
sudo systemctl status ssh
```

Enable at startup:

```bash
sudo systemctl enable ssh
```

Start:

```bash
sudo systemctl start ssh
```

---

## 🔌 Basic SSH Connection

```bash
ssh username@server-ip
```

Example:

```bash
ssh ubuntu@192.168.1.100
```

Using a domain:

```bash
ssh user@example.com
```

Using another port:

```bash
ssh -p 2222 user@example.com
```

---

## 🔑 SSH Keys

An SSH key pair contains:

```text
Private Key
    +
Public Key
```

Typical files:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

The **private key must remain secret**. The public key can be installed on servers.

### Generate a key

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Check:

```bash
ls -la ~/.ssh
```

---

## 📤 Copy Public Key to a Server

```bash
ssh-copy-id user@server-ip
```

Then:

```bash
ssh user@server-ip
```

### Manual installation

Display the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

On the server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Then:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## 🗂️ Important SSH Files

```text
~/.ssh/
├── id_ed25519
├── id_ed25519.pub
├── known_hosts
├── authorized_keys
└── config
```

- `id_ed25519` → private key
- `id_ed25519.pub` → public key
- `known_hosts` → known server identities
- `authorized_keys` → allowed public keys
- `config` → SSH client configuration

---

## ⚙️ SSH Config

Create:

```bash
nano ~/.ssh/config
```

Example:

```sshconfig
Host my-server
    HostName 203.0.113.10
    User ubuntu
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Now connect with:

```bash
ssh my-server
```

---

## 📁 Copying Files

### SCP

Local → server:

```bash
scp file.txt user@server:/home/user/
```

Directory:

```bash
scp -r my-project user@server:/home/user/
```

Server → local:

```bash
scp user@server:/home/user/file.txt .
```

### rsync

```bash
rsync -avz ./project/ user@server:/home/user/project/
```

With SSH:

```bash
rsync -avz -e ssh ./project/ user@server:/home/user/project/
```

---

## 🤖 SSH Agent

Start:

```bash
eval "$(ssh-agent -s)"
```

Add your key:

```bash
ssh-add ~/.ssh/id_ed25519
```

List keys:

```bash
ssh-add -l
```

Remove all:

```bash
ssh-add -D
```

---

## 🧰 Useful SSH Commands

Connect:

```bash
ssh user@server
```

Specific key:

```bash
ssh -i ~/.ssh/id_ed25519 user@server
```

Execute a remote command:

```bash
ssh user@server "docker ps"
```

Debug:

```bash
ssh -v user@server
```

More detailed debugging:

```bash
ssh -vvv user@server
```

---

## 🔌 SSH Port

The default SSH port is:

```text
22
```

Check listening ports:

```bash
sudo ss -tlnp
```

Check SSH:

```bash
sudo ss -tlnp | grep ssh
```

Test a remote port:

```bash
nc -zv server-ip 22
```

---

## ⚙️ SSH Server Configuration

Main configuration:

```bash
/etc/ssh/sshd_config
```

Backup before editing:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Edit:

```bash
sudo nano /etc/ssh/sshd_config
```

Common settings:

```text
Port 22
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication yes
```

Validate:

```bash
sudo sshd -t
```

Restart:

```bash
sudo systemctl restart ssh
```

**Important:** Keep an existing SSH session open while testing configuration changes so you do not accidentally lock yourself out.

---

## 🛡️ Security Best Practices

### Use SSH keys

Prefer public-key authentication over passwords.

### Protect the private key

```bash
chmod 600 ~/.ssh/id_ed25519
```

### Protect `.ssh`

```bash
chmod 700 ~/.ssh
```

### Disable direct root login

```text
PermitRootLogin no
```

### Use a normal user with sudo

```bash
sudo adduser deploy
sudo usermod -aG sudo deploy
```

### Use a firewall

```bash
sudo ufw allow ssh
sudo ufw enable
```

### Disable password authentication

Only after confirming key authentication works:

```text
PasswordAuthentication no
```

Then validate:

```bash
sudo sshd -t
```

---

## 🐙 SSH with GitHub

Generate a key:

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Display it:

```bash
cat ~/.ssh/id_ed25519.pub
```

Add the public key to:

**GitHub → Settings → SSH and GPG keys → New SSH key**

Test:

```bash
ssh -T git@github.com
```

Clone using SSH:

```bash
git clone git@github.com:username/repository.git
```

Change an existing remote:

```bash
git remote set-url origin git@github.com:username/repository.git
```

Check:

```bash
git remote -v
```

---

## 🚀 SSH for Deployment

Typical architecture:

```text
Developer
   │
   │ git push
   ▼
GitHub
   │
   │ CI/CD
   ▼
Linux Server
   │
   ├── SSH
   ├── Docker
   ├── Nginx
   ├── Node.js
   └── PostgreSQL
```

Example:

```bash
ssh deploy@server
```

For a Node.js application:

```bash
cd /var/www/my-app
git pull
npm ci
npm run build
```

For Docker:

```bash
cd /var/www/my-app
docker compose pull
docker compose up -d
```

---

## 🔀 SSH Tunneling

### Local Port Forwarding

```bash
ssh -L 5433:localhost:5432 user@server
```

Architecture:

```text
Your PC :5433
     ↓
SSH Tunnel
     ↓
Server localhost:5432
```

Useful for accessing a private PostgreSQL database.

### Remote Port Forwarding

```bash
ssh -R 8080:localhost:3000 user@server
```

### SOCKS Proxy

```bash
ssh -D 8080 user@server
```

---

## 🐛 Troubleshooting

### Permission denied (publickey)

Check:

```bash
ls -la ~/.ssh
ssh-add -l
```

Try:

```bash
ssh -i ~/.ssh/id_ed25519 user@server
```

Debug:

```bash
ssh -vvv user@server
```

### Connection refused

Check:

```bash
sudo systemctl status ssh
sudo ss -tlnp | grep :22
sudo ufw status
```

### Connection timed out

Possible causes:

- Server is offline
- Wrong IP
- Firewall blocks SSH
- Cloud security group blocks SSH
- Wrong port
- Network problem

### Host key warning

If you see:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

First verify that the server was legitimately rebuilt or its SSH host key changed.

After verification:

```bash
ssh-keygen -R server-ip
```

Then reconnect.

---

## 🧪 Practice Projects

### Project 1 — Local SSH

Install OpenSSH Server on Linux and connect from another machine.

### Project 2 — SSH Keys

Create an Ed25519 key and configure key authentication.

### Project 3 — VPS

Create a Linux VPS and connect using SSH.

### Project 4 — Secure Server

Configure:

- Non-root user
- SSH keys
- UFW
- SSH configuration

### Project 5 — Application Deployment

Deploy a Node.js application to a Linux server using SSH.

### Project 6 — Docker Deployment

Deploy a Docker Compose application through SSH.

### Project 7 — Database Tunnel

Keep PostgreSQL private and access it through an SSH tunnel.

---

## 📋 SSH Cheat Sheet

| Task | Command |
|---|---|
| Check SSH version | `ssh -V` |
| Connect | `ssh user@server` |
| Custom port | `ssh -p 2222 user@server` |
| Custom key | `ssh -i ~/.ssh/id_ed25519 user@server` |
| Generate key | `ssh-keygen -t ed25519` |
| Copy key | `ssh-copy-id user@server` |
| Copy file | `scp file user@server:/path` |
| Copy directory | `scp -r dir user@server:/path` |
| Sync directory | `rsync -avz dir/ user@server:/path/` |
| Debug connection | `ssh -vvv user@server` |
| Start agent | `eval "$(ssh-agent -s)"` |
| Add key | `ssh-add ~/.ssh/id_ed25519` |
| List keys | `ssh-add -l` |
| Test GitHub | `ssh -T git@github.com` |
| Local tunnel | `ssh -L local:host:remote user@server` |

---

## ✅ Learning Checklist

- [ ] Understand SSH
- [ ] Install OpenSSH
- [ ] Connect to a Linux server
- [ ] Generate an Ed25519 key
- [ ] Configure key authentication
- [ ] Understand `authorized_keys`
- [ ] Learn `~/.ssh/config`
- [ ] Copy files with SCP
- [ ] Synchronize files with rsync
- [ ] Use SSH Agent
- [ ] Configure SSH server
- [ ] Configure UFW
- [ ] Secure a VPS
- [ ] Use SSH with GitHub
- [ ] Deploy an application through SSH
- [ ] Create an SSH tunnel
- [ ] Troubleshoot SSH connections

---

## 🔗 Official Resources

- [OpenSSH](https://www.openssh.com/)
- [OpenSSH Manual](https://www.openssh.com/manual.html)
- [Ubuntu OpenSSH Documentation](https://documentation.ubuntu.com/server/how-to/security/openssh-server/)
- [GitHub — Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

---

## 🎯 Next Step

Combine SSH with:

```text
Linux
  ↓
SSH
  ↓
Git & GitHub
  ↓
Docker
  ↓
Nginx
  ↓
CI/CD
  ↓
AWS / Cloud
```

This provides a strong foundation for **Linux administration, DevOps, cloud engineering, and application deployment**.
