# 🐧 Linux Beginner Guide

A practical Linux guide for software development, Cloud, DevOps, Docker,
and server administration.

## 1. What is Linux?

Linux is an open-source operating system kernel used by distributions
such as Ubuntu, Debian, Fedora, Arch Linux, Rocky Linux, and AlmaLinux.

Official resources:

-   https://ubuntu.com/
-   https://www.kernel.org/
-   https://documentation.ubuntu.com/

------------------------------------------------------------------------

## 2. Essential Terminal Commands

### Navigation

``` bash
pwd
ls
ls -la
cd folder
cd ..
cd ~
```

### Files and directories

``` bash
mkdir project
mkdir -p project/src
touch app.js
cp app.js backup.js
mv app.js src/app.js
rm file.txt
rm -r folder
```

Be careful with `rm -r`.

### Read files

``` bash
cat file.txt
less file.txt
head file.txt
tail file.txt
tail -f app.log
```

------------------------------------------------------------------------

## 3. Search

Search text:

``` bash
grep "error" app.log
grep -r "TODO" src/
```

Find files:

``` bash
find . -name "*.js"
```

Modern alternative:

``` bash
rg "TODO"
```

------------------------------------------------------------------------

## 4. Permissions

Check permissions:

``` bash
ls -l
```

Make a script executable:

``` bash
chmod +x script.sh
./script.sh
```

Common permissions:

``` text
4 = read
2 = write
1 = execute

755 = rwx/r-x/r-x
644 = rw-/r--/r--
```

Avoid `chmod 777` unless you understand why it is required.

------------------------------------------------------------------------

## 5. Users and sudo

``` bash
whoami
id
groups
```

Run an administrative command:

``` bash
sudo apt update
```

Use `sudo` only when necessary.

------------------------------------------------------------------------

## 6. Package Management

Ubuntu/Debian use APT.

``` bash
sudo apt update
sudo apt upgrade
sudo apt install curl
sudo apt remove curl
apt search package-name
apt show package-name
```

------------------------------------------------------------------------

## 7. Processes

``` bash
ps aux
top
pgrep node
kill PID
```

Use `kill -9 PID` only when normal termination does not work.

Optional:

``` bash
sudo apt install htop
htop
```

------------------------------------------------------------------------

## 8. Ports and Networking

Show IP addresses:

``` bash
ip addr
```

Show routes:

``` bash
ip route
```

Show listening ports:

``` bash
ss -tulpn
```

Test an API:

``` bash
curl http://localhost:4000
```

DNS:

``` bash
nslookup example.com
```

------------------------------------------------------------------------

## 9. curl

GET request:

``` bash
curl http://localhost:4000/api/users
```

POST request:

``` bash
curl -X POST http://localhost:4000/api/users   -H "Content-Type: application/json"   -d '{"name":"Wassel"}'
```

Show response headers:

``` bash
curl -I https://example.com
```

------------------------------------------------------------------------

## 10. Environment Variables

Temporary variable:

``` bash
export NODE_ENV=development
```

Read it:

``` bash
echo $NODE_ENV
```

Common variables:

``` text
PORT
DATABASE_URL
NODE_ENV
API_URL
JWT_SECRET
```

Never commit secrets to Git.

------------------------------------------------------------------------

## 11. PATH

``` bash
echo $PATH
which node
command -v node
```

`PATH` tells the shell where to find executable programs.

------------------------------------------------------------------------

## 12. Shells

Common shells:

``` text
bash
zsh
fish
```

Check your shell:

``` bash
echo $SHELL
```

------------------------------------------------------------------------

## 13. Bash Scripts

Create:

``` bash
touch script.sh
chmod +x script.sh
```

Example:

``` bash
#!/bin/bash

name="Wassel"

echo "Hello $name"
```

Run:

``` bash
./script.sh
```

------------------------------------------------------------------------

## 14. SSH

SSH is essential for remote Linux servers.

Connect:

``` bash
ssh user@server-ip
```

Generate an SSH key:

``` bash
ssh-keygen -t ed25519
```

Typical files:

``` text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Never share your private key.

Official OpenSSH:

https://www.openssh.com/

------------------------------------------------------------------------

## 15. SSH Config

Create:

``` text
~/.ssh/config
```

Example:

``` sshconfig
Host my-server
    HostName 203.0.113.10
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519
```

Connect:

``` bash
ssh my-server
```

------------------------------------------------------------------------

## 16. Disk and Memory

Disk:

``` bash
df -h
du -sh .
```

Memory:

``` bash
free -h
```

CPU:

``` bash
lscpu
```

System:

``` bash
uname -a
```

------------------------------------------------------------------------

## 17. systemd Services

Check a service:

``` bash
systemctl status nginx
```

Start:

``` bash
sudo systemctl start nginx
```

Stop:

``` bash
sudo systemctl stop nginx
```

Enable at startup:

``` bash
sudo systemctl enable nginx
```

------------------------------------------------------------------------

## 18. Logs

View system logs:

``` bash
journalctl
```

Follow logs:

``` bash
journalctl -f
```

Service logs:

``` bash
journalctl -u nginx
```

------------------------------------------------------------------------

## 19. Linux + Node.js

Check Node.js:

``` bash
node -v
npm -v
which node
```

Run an application:

``` bash
node server.js
```

Run a development script:

``` bash
npm run dev
```

------------------------------------------------------------------------

## 20. Linux + Git

Configure Git:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Clone:

``` bash
git clone https://github.com/user/repository.git
```

Check:

``` bash
git status
```

------------------------------------------------------------------------

## 21. Linux + Docker

Check Docker:

``` bash
docker --version
docker compose version
```

Test Docker:

``` bash
docker run hello-world
```

Containers:

``` bash
docker ps
docker ps -a
```

------------------------------------------------------------------------

## 22. Firewall

Ubuntu commonly uses UFW.

``` bash
sudo ufw status
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Be careful when changing firewall rules on a remote server.

------------------------------------------------------------------------

## 23. Useful Development Tools

Recommended:

``` text
Git
curl
wget
jq
ripgrep
tree
htop
tmux
Docker
SSH
```

Install examples:

``` bash
sudo apt install curl wget jq ripgrep tree htop tmux
```

------------------------------------------------------------------------

## 24. tmux

Create a session:

``` bash
tmux new -s dev
```

List sessions:

``` bash
tmux ls
```

Reconnect:

``` bash
tmux attach -t dev
```

tmux is useful for long-running work on remote servers.

------------------------------------------------------------------------

## 25. Compression

Create a tar archive:

``` bash
tar -czf project.tar.gz project/
```

Extract:

``` bash
tar -xzf project.tar.gz
```

ZIP:

``` bash
zip -r project.zip project/
unzip project.zip
```

------------------------------------------------------------------------

## 26. Pipes and Redirection

Pipe output:

``` bash
ps aux | grep node
```

Write output:

``` bash
echo "Hello" > file.txt
```

Append:

``` bash
echo "New line" >> file.txt
```

Redirect errors:

``` bash
command 2> error.log
```

------------------------------------------------------------------------

## 27. Linux Directory Structure

Important directories:

``` text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── opt
├── proc
├── root
├── tmp
├── usr
└── var
```

Useful locations:

``` text
/home → user home directories
/etc  → configuration
/var  → logs and changing data
/tmp  → temporary files
/opt  → optional software
/usr  → programs and libraries
```

------------------------------------------------------------------------

## 28. Security Basics

``` text
✓ Keep the system updated
✓ Use SSH keys
✓ Protect private keys
✓ Use strong passwords
✓ Avoid unnecessary sudo
✓ Use a firewall
✓ Disable unnecessary services
✓ Keep backups
✓ Never commit secrets
✓ Monitor logs
```

------------------------------------------------------------------------

## 29. Troubleshooting Workflow

When an API is not working:

### 1. Check the process

``` bash
ps aux | grep node
```

### 2. Check the port

``` bash
ss -tulpn | grep 4000
```

### 3. Check logs

``` bash
journalctl -u my-service
```

### 4. Test locally

``` bash
curl http://localhost:4000
```

### 5. Check resources

``` bash
free -h
df -h
top
```

------------------------------------------------------------------------

# 30. Learning Roadmap

## Beginner

``` text
Terminal
↓
Files and directories
↓
Permissions
↓
Users
↓
sudo
↓
Package management
```

## Developer

``` text
Git
↓
SSH
↓
curl
↓
Environment variables
↓
Processes
↓
Ports
↓
Node.js
↓
npm
```

## DevOps

``` text
Linux services
↓
Logs
↓
Networking
↓
Firewall
↓
Docker
↓
Docker Compose
↓
CI/CD
↓
Cloud servers
↓
Monitoring
```

------------------------------------------------------------------------

# 31. Practice Projects

### Project 1 --- CLI Practice

Practice creating, moving, copying, searching, and deleting files.

### Project 2 --- Node.js Server

Build:

``` text
Linux + Node.js + Express
```

Run the API on port `4000`.

### Project 3 --- Remote Server

Practice:

``` text
SSH
Users
Firewall
Node.js
Git
Environment variables
Logs
```

### Project 4 --- Docker Server

Deploy:

``` text
Linux
↓
Docker
↓
Node.js API
↓
PostgreSQL
```

------------------------------------------------------------------------

# 32. Official Resources

-   Ubuntu: https://ubuntu.com/
-   Ubuntu Documentation: https://documentation.ubuntu.com/
-   Linux Kernel: https://www.kernel.org/
-   GNU Coreutils: https://www.gnu.org/software/coreutils/
-   OpenSSH: https://www.openssh.com/
-   Bash: https://www.gnu.org/software/bash/
-   curl: https://curl.se/
-   Docker: https://www.docker.com/

------------------------------------------------------------------------

# 33. Command Cheat Sheet

``` bash
# Navigation
pwd
ls -la
cd ..
cd ~

# Files
touch file.txt
mkdir folder
cp file.txt backup.txt
mv file.txt folder/
rm file.txt

# Reading
cat file.txt
less file.txt
head file.txt
tail -f app.log

# Search
grep "error" app.log
find . -name "*.js"
rg "TODO"

# Permissions
chmod +x script.sh
chown user:user file.txt

# Processes
ps aux
top
kill PID

# Network
ip addr
ip route
ss -tulpn
curl http://localhost:4000

# SSH
ssh user@server
ssh-keygen -t ed25519

# Packages
sudo apt update
sudo apt upgrade
sudo apt install package

# Disk / memory
df -h
du -sh .
free -h

# Services
systemctl status service
sudo systemctl start service
sudo systemctl stop service

# Logs
journalctl
journalctl -f
journalctl -u service
```

## Final Goal

Be comfortable using Linux as a development and Cloud environment:

``` text
Linux
 ↓
Terminal
 ↓
Git / GitHub
 ↓
Node.js
 ↓
Docker
 ↓
PostgreSQL
 ↓
CI/CD
 ↓
Cloud Server
 ↓
Monitoring
```
