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


---

# 34. Linux Filesystem in More Detail

Linux has a single filesystem tree that starts at `/`.

### `/`

The root of the entire filesystem.

```bash
cd /
ls
```

### `/home`

Contains normal users' home directories.

```text
/home/wassel
/home/user
```

### `/root`

Home directory of the root user.

### `/etc`

System and application configuration.

Examples:

```text
/etc/ssh/
/etc/nginx/
/etc/systemd/
/etc/hosts
```

### `/var`

Frequently changing data.

Important examples:

```text
/var/log/
/var/lib/
/var/cache/
```

### `/tmp`

Temporary files.

### `/usr`

Programs, libraries, documentation, and other user-space files.

### `/opt`

Optional third-party software.

---

# 35. Absolute vs Relative Paths

An **absolute path** starts from `/`.

```bash
/home/wassel/projects/app
```

A **relative path** starts from the current directory.

```bash
projects/app
```

Check the current directory:

```bash
pwd
```

Go to an absolute path:

```bash
cd /home/wassel/projects
```

Go up one directory:

```bash
cd ..
```

Go to the current user's home:

```bash
cd ~
```

---

# 36. Hidden Files

Linux files beginning with `.` are hidden.

Show them:

```bash
ls -la
```

Common examples:

```text
~/.ssh/
~/.config/
~/.bashrc
~/.zshrc
.git/
.env
```

Do not delete hidden files unless you understand their purpose.

---

# 37. File Types

Check the type of a file:

```bash
file app.js
```

Common types:

```text
regular file
directory
symbolic link
executable
device
```

List symbolic links:

```bash
ls -l
```

---

# 38. Symbolic Links

Create a symbolic link:

```bash
ln -s /path/to/original shortcut
```

Example:

```bash
ln -s ~/projects/my-api ~/my-api
```

Remove the link:

```bash
rm ~/my-api
```

Removing a symbolic link does not remove the original directory.

---

# 39. Hard Links

Linux also supports hard links:

```bash
ln file.txt backup.txt
```

Hard links have different behavior from symbolic links and are generally less common in everyday development.

For development workflows, understand symbolic links first.

---

# 40. File Ownership

Check ownership:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 wassel wassel 1200 app.js
```

Change owner:

```bash
sudo chown wassel:wassel app.js
```

Change recursively:

```bash
sudo chown -R wassel:wassel project/
```

Be careful with recursive ownership changes.

---

# 41. chmod in Detail

Permissions are represented for:

```text
user
group
others
```

Example:

```text
-rwxr-xr--
```

Means:

```text
user   → rwx
group  → r-x
others → r--
```

Numeric permissions:

```text
r = 4
w = 2
x = 1
```

Examples:

```bash
chmod 755 script.sh
chmod 644 config.json
chmod 600 ~/.ssh/id_ed25519
```

A private SSH key should normally have restrictive permissions.

---

# 42. umask

`umask` controls default permissions for newly created files and directories.

Check:

```bash
umask
```

Example:

```bash
umask 022
```

This affects the default permissions assigned when new files are created.

---

# 43. Process Information

Every running process has a PID.

Find a process:

```bash
pgrep node
```

Detailed information:

```bash
ps -p PID -f
```

Find processes listening on a port:

```bash
sudo lsof -i :4000
```

Stop a process:

```bash
kill PID
```

Force stop:

```bash
kill -9 PID
```

---

# 44. Signals

Common Linux signals:

```text
SIGTERM → ask a process to terminate
SIGKILL → force termination
SIGINT  → interrupt, commonly Ctrl+C
SIGHUP  → hangup/reload depending on application
```

For example:

```bash
kill -TERM PID
```

Prefer graceful termination before `SIGKILL`.

---

# 45. Background Jobs

Run a command in the background:

```bash
npm run dev &
```

List jobs:

```bash
jobs
```

Bring a job to the foreground:

```bash
fg
```

Suspend a foreground process:

```text
Ctrl + Z
```

Continue it in the background:

```bash
bg
```

---

# 46. `nohup`

Run a command that should continue after the terminal closes:

```bash
nohup node server.js > app.log 2>&1 &
```

For production services, prefer systemd, Docker, or another proper process-management approach.

---

# 47. CPU and Memory Monitoring

Use:

```bash
top
```

Or:

```bash
htop
```

Memory:

```bash
free -h
```

CPU information:

```bash
lscpu
```

Load average:

```bash
uptime
```

---

# 48. Disk Monitoring

Filesystem usage:

```bash
df -h
```

Directory usage:

```bash
du -sh project/
```

Top-level directory sizes:

```bash
du -h --max-depth=1 | sort -h
```

Find large files:

```bash
find . -type f -size +100M
```

---

# 49. Inodes

Linux filesystems also have inode limits.

Check inode usage:

```bash
df -i
```

A filesystem can have free disk space but still run out of inodes if it contains a huge number of small files.

This can happen with:

```text
node_modules
cache directories
temporary files
logs
```

---

# 50. Environment Configuration

A common development pattern is:

```text
.env
.env.example
```

Example `.env.example`:

```env
PORT=4000
DATABASE_URL=
JWT_SECRET=
```

`.env.example` can be committed.

Actual secrets should normally not be committed:

```gitignore
.env
.env.local
.env.production
```

---

# 51. Shell Configuration

Bash configuration:

```text
~/.bashrc
```

Zsh configuration:

```text
~/.zshrc
```

After changing configuration:

```bash
source ~/.bashrc
```

Or:

```bash
source ~/.zshrc
```

Example alias:

```bash
alias ll="ls -lah"
```

---

# 52. Useful Command-Line Shortcuts

```text
Ctrl + C  → stop/interrupt
Ctrl + Z  → suspend
Ctrl + D  → exit/end input
Ctrl + L  → clear terminal
Ctrl + A  → beginning of line
Ctrl + E  → end of line
Ctrl + R  → search command history
```

History:

```bash
history
```

Search previous commands:

```text
Ctrl + R
```

---

# 53. `man` and Help

Read the manual:

```bash
man ls
```

Search for help:

```bash
ls --help
```

Examples:

```bash
man chmod
man ssh
man systemctl
man grep
```

Exit `man`:

```text
q
```

---

# 54. Package Installation Safety

Before installing a package:

```bash
apt search package-name
apt show package-name
```

Understand what the package does.

Avoid copying commands from unknown websites that execute scripts with:

```bash
curl ... | bash
```

unless you have inspected and trust the source.

---

# 55. Networking Concepts

Important concepts:

```text
IP address
MAC address
Port
TCP
UDP
DNS
HTTP
HTTPS
SSH
Routing
Firewall
```

Example:

```text
Browser
   ↓
DNS
   ↓
IP address
   ↓
TCP connection
   ↓
Port 443
   ↓
HTTPS server
```

---

# 56. DNS

DNS converts domain names into IP addresses.

Example:

```bash
nslookup google.com
```

Or:

```bash
dig google.com
```

Install `dig` if necessary:

```bash
sudo apt install dnsutils
```

---

# 57. Localhost

`localhost` normally refers to the local machine.

Common address:

```text
127.0.0.1
```

IPv6:

```text
::1
```

Example:

```bash
curl http://localhost:4000
```

---

# 58. Ports

A port identifies a network service.

Common development ports:

```text
22   → SSH
80   → HTTP
443  → HTTPS
3000 → Next.js / frontend
4000 → Node.js API
5000 → development API
5432 → PostgreSQL
6379 → Redis
```

Check:

```bash
ss -tulpn
```

---

# 59. HTTP Status Codes

Important API status codes:

```text
200 → OK
201 → Created
204 → No Content
301 → Permanent redirect
302 → Temporary redirect
400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found
409 → Conflict
422 → Validation error
429 → Too Many Requests
500 → Internal Server Error
502 → Bad Gateway
503 → Service Unavailable
```

---

# 60. Logs: What to Look For

When debugging a server, look for:

```text
ERROR
WARN
connection refused
permission denied
address already in use
out of memory
timeout
authentication failed
database connection failed
```

Useful commands:

```bash
tail -f app.log
journalctl -f
grep "ERROR" app.log
```

---

# 61. Port Already in Use

If Node.js says a port is already in use:

```text
EADDRINUSE
```

Find the process:

```bash
sudo lsof -i :4000
```

Then stop it:

```bash
kill PID
```

Or use another port:

```bash
PORT=4001 npm run dev
```

---

# 62. Permission Denied

If you see:

```text
Permission denied
```

Check:

```bash
ls -l file
```

Check ownership:

```bash
ls -ln file
```

Check directory permissions:

```bash
ls -ld directory
```

Do not immediately solve every permission problem with:

```bash
sudo chmod 777
```

Understand the actual ownership and permission issue first.

---

# 63. Disk Full

If an application stops writing files:

```bash
df -h
```

Check large directories:

```bash
sudo du -xh /var | sort -h | tail
```

Check logs:

```bash
journalctl --disk-usage
```

Clean package cache when appropriate:

```bash
sudo apt clean
```

---

# 64. Memory Problems

Check:

```bash
free -h
```

Then:

```bash
top
```

Look for applications consuming excessive memory.

For Node.js, monitor:

```text
RSS
heap usage
process count
container memory
```

---

# 65. Linux + PostgreSQL

A common development stack is:

```text
Linux
 ↓
Node.js
 ↓
PostgreSQL
```

PostgreSQL commonly listens on:

```text
5432
```

Check:

```bash
ss -tulpn | grep 5432
```

Useful PostgreSQL commands are documented separately in the PostgreSQL learning guide.

---

# 66. Linux + Redis

Redis commonly uses:

```text
6379
```

Check:

```bash
ss -tulpn | grep 6379
```

Typical uses:

```text
Caching
Sessions
Rate limiting
Queues
Pub/Sub
```

---

# 67. Linux + Nginx

Nginx is commonly used as:

```text
Reverse Proxy
Web Server
TLS termination
Load balancer
```

Typical architecture:

```text
Internet
   ↓
Nginx :443
   ↓
Node.js :4000
   ↓
PostgreSQL :5432
```

Official documentation:

https://nginx.org/en/docs/

---

# 68. Linux + Docker Architecture

A modern application might look like:

```text
Linux Server
│
├── Nginx
│
├── Node.js API container
│
├── PostgreSQL container
│
└── Redis container
```

Docker Compose can manage these services together.

---

# 69. Linux Server Deployment Checklist

Before deploying:

```text
[ ] Update the server
[ ] Create a non-root user
[ ] Configure SSH keys
[ ] Configure firewall
[ ] Install Git
[ ] Install Docker
[ ] Configure environment variables
[ ] Deploy application
[ ] Configure logs
[ ] Configure HTTPS
[ ] Configure backups
[ ] Add monitoring
[ ] Test restart/recovery
```

---

# 70. Production vs Development

Development:

```text
localhost
debug logs
hot reload
development database
local environment variables
```

Production:

```text
HTTPS
restricted firewall
secure secrets
logging
monitoring
backups
resource limits
automatic restart
database protection
```

Never assume a development configuration is safe for production.

---

# 71. Recommended Learning Exercises

### Exercise 1

Create:

```text
~/projects/linux-practice/
├── notes/
├── scripts/
└── logs/
```

### Exercise 2

Create a Bash script that:

```text
1. Prints the current directory
2. Prints the current user
3. Prints the date
4. Prints memory usage
5. Prints disk usage
```

### Exercise 3

Start a Node.js server on port `4000`.

Find it using:

```bash
ss -tulpn | grep 4000
```

### Exercise 4

Use `curl` to call the API.

### Exercise 5

Run the API inside Docker.

### Exercise 6

Connect to a remote Linux server using SSH.

### Exercise 7

Configure Nginx as a reverse proxy.

---

# 72. Linux Learning Progress Checklist

## Fundamentals

```text
[ ] Terminal
[ ] Files
[ ] Directories
[ ] Paths
[ ] Hidden files
[ ] Permissions
[ ] Ownership
[ ] Users
[ ] sudo
```

## System

```text
[ ] Processes
[ ] Signals
[ ] Services
[ ] systemd
[ ] Logs
[ ] CPU
[ ] Memory
[ ] Disk
[ ] Inodes
```

## Networking

```text
[ ] IP addresses
[ ] Ports
[ ] TCP
[ ] DNS
[ ] HTTP
[ ] HTTPS
[ ] SSH
[ ] Firewall
```

## Development

```text
[ ] Git
[ ] Node.js
[ ] npm
[ ] Environment variables
[ ] PostgreSQL
[ ] Redis
[ ] Docker
[ ] Nginx
```

## Cloud / DevOps

```text
[ ] Remote servers
[ ] SSH deployment
[ ] Docker Compose
[ ] CI/CD
[ ] Cloud infrastructure
[ ] Monitoring
[ ] Backups
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
