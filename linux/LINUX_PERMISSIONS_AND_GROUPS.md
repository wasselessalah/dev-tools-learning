# 🔐 Linux Permissions and Groups

A practical guide to understanding **Linux users, groups, file ownership, permissions, `chmod`, `chown`, `chgrp`, and `sudo`**.

---

## 📚 Table of Contents

1. [Linux Users](#1-linux-users)
2. [Linux Groups](#2-linux-groups)
3. [File Ownership](#3-file-ownership)
4. [Understanding Permissions](#4-understanding-permissions)
5. [Reading `ls -l`](#5-reading-ls--l)
6. [Permission Types](#6-permission-types)
7. [Changing Permissions with chmod](#7-changing-permissions-with-chmod)
8. [Numeric Permissions](#8-numeric-permissions)
9. [Symbolic Permissions](#9-symbolic-permissions)
10. [Changing Ownership with chown](#10-changing-ownership-with-chown)
11. [Changing Groups with chgrp](#11-changing-groups-with-chgrp)
12. [Managing Users](#12-managing-users)
13. [Managing Groups](#13-managing-groups)
14. [sudo and Root](#14-sudo-and-root)
15. [umask](#15-umask)
16. [Special Permissions](#16-special-permissions)
17. [Practical Examples](#17-practical-examples)
18. [Practice Exercises](#18-practice-exercises)
19. [Useful Commands](#19-useful-commands)

---

# 1. Linux Users

Linux is a multi-user operating system.

Each user has:

- A username
- A user ID (UID)
- A home directory
- A primary group
- Permissions

Check the current user:

```bash
whoami
```

Example:

```text
wassel
```

---

## List Logged-in Users

```bash
who
```

or:

```bash
w
```

---

## User ID Information

```bash
id
```

Example:

```text
uid=1000(wassel) gid=1000(wassel) groups=1000(wassel),27(sudo)
```

---

# 2. Linux Groups

A **group** is a collection of users.

Groups make it easier to manage permissions for multiple users.

For example:

```text
developers
├── wassel
├── ali
└── ahmed
```

You can give the `developers` group access to a project instead of giving permissions to each user individually.

---

## Show Current User's Groups

```bash
groups
```

or:

```bash
id
```

---

## List System Groups

```bash
getent group
```

---

# 3. File Ownership

Every file and directory has:

- An owner
- A group
- Permissions

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 wassel developers 1200 Oct 4 notes.txt
```

Here:

```text
Owner: wassel
Group: developers
```

---

# 4. Understanding Permissions

Linux has three main permission categories:

```text
User       Group       Others
 ↓           ↓           ↓
rwx         rwx         rwx
```

They are:

### User

The owner of the file.

### Group

Users belonging to the file's group.

### Others

Everyone else.

---

# 5. Reading `ls -l`

Example:

```bash
ls -l file.txt
```

Output:

```text
-rw-r--r-- 1 wassel developers 1200 Oct 4 18:30 file.txt
```

The first section:

```text
-rw-r--r--
```

Can be divided into:

```text
- | rw- | r-- | r--
  |     |     |
  |     |     └── Others
  |     └──────── Group
  └────────────── Owner
```

The first character indicates the file type.

```text
-    Regular file
d    Directory
l    Symbolic link
```

---

# 6. Permission Types

Linux has three basic permissions.

| Permission | Symbol | Meaning |
|---|---|---|
| Read | `r` | Read content |
| Write | `w` | Modify content |
| Execute | `x` | Execute file / enter directory |
| No permission | `-` | Permission not granted |

---

## Read Permission

```text
r
```

For a file:

- Read its content.

For a directory:

- List its contents.

---

## Write Permission

```text
w
```

For a file:

- Modify the file.

For a directory:

- Create/delete/rename entries inside it, subject to the directory's other permissions.

---

## Execute Permission

```text
x
```

For a file:

- Execute the file as a program/script.

For a directory:

- Enter/access the directory.

Example:

```bash
cd project
```

requires appropriate execute permission on the directory.

---

# 7. Changing Permissions with `chmod`

The `chmod` command changes file permissions.

Syntax:

```bash
chmod permissions file
```

Example:

```bash
chmod 755 script.sh
```

---

# 8. Numeric Permissions

Each permission has a numeric value:

| Permission | Value |
|---|---:|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |
| `-` | 0 |

Add the values together.

---

## Examples

### Read Only

```text
r--
```

Calculation:

```text
4 + 0 + 0 = 4
```

---

### Read + Write

```text
rw-
```

Calculation:

```text
4 + 2 + 0 = 6
```

---

### Read + Execute

```text
r-x
```

Calculation:

```text
4 + 0 + 1 = 5
```

---

### Read + Write + Execute

```text
rwx
```

Calculation:

```text
4 + 2 + 1 = 7
```

---

## Common Permission Values

| Number | Permission |
|---:|---|
| `0` | `---` |
| `1` | `--x` |
| `2` | `-w-` |
| `3` | `-wx` |
| `4` | `r--` |
| `5` | `r-x` |
| `6` | `rw-` |
| `7` | `rwx` |

---

## `755`

```bash
chmod 755 script.sh
```

Means:

```text
Owner:  rwx = 7
Group:  r-x = 5
Others: r-x = 5
```

Result:

```text
-rwxr-xr-x
```

---

## `644`

```bash
chmod 644 file.txt
```

Means:

```text
Owner:  rw- = 6
Group:  r-- = 4
Others: r-- = 4
```

Result:

```text
-rw-r--r--
```

This is a common permission for regular files.

---

# 9. Symbolic Permissions

You can also use letters with `chmod`.

The categories are:

```text
u = user/owner
g = group
o = others
a = all
```

Operators:

```text
+ = add permission
- = remove permission
= = set permission
```

---

## Add Execute Permission

```bash
chmod u+x script.sh
```

---

## Remove Write Permission

```bash
chmod u-w file.txt
```

---

## Add Group Write Permission

```bash
chmod g+w project.txt
```

---

## Remove Others' Read Permission

```bash
chmod o-r file.txt
```

---

## Give Everyone Execute Permission

```bash
chmod a+x script.sh
```

---

# 10. Changing Ownership with `chown`

`chown` changes the owner of a file.

Syntax:

```bash
sudo chown user file
```

Example:

```bash
sudo chown wassel file.txt
```

---

## Change Owner and Group

```bash
sudo chown wassel:developers file.txt
```

Now:

```text
Owner: wassel
Group: developers
```

---

## Change Ownership Recursively

For a directory and everything inside it:

```bash
sudo chown -R wassel:developers project/
```

⚠️ Use `-R` carefully, especially on system directories.

---

# 11. Changing Groups with `chgrp`

`chgrp` changes the group ownership.

Example:

```bash
sudo chgrp developers project.txt
```

Check:

```bash
ls -l project.txt
```

---

## Change Group Recursively

```bash
sudo chgrp -R developers project/
```

---

# 12. Managing Users

## Create a User

On Ubuntu:

```bash
sudo adduser john
```

---

## Delete a User

```bash
sudo deluser john
```

---

## Add User to a Group

```bash
sudo usermod -aG developers john
```

The important options are:

```text
-a = append
-G = supplementary groups
```

---

## Check User Groups

```bash
groups john
```

or:

```bash
id john
```

---

## Remove User from a Group

On Ubuntu:

```bash
sudo deluser john developers
```

---

# 13. Managing Groups

## Create a Group

```bash
sudo groupadd developers
```

---

## Delete a Group

```bash
sudo groupdel developers
```

---

## Add a User to a Group

```bash
sudo usermod -aG developers john
```

---

## Check Group Information

```bash
getent group developers
```

Example:

```text
developers:x:1001:john,wassel
```

---

# 14. `sudo` and Root

`root` is the Linux superuser.

Root has very high privileges and can:

- Change system files
- Install software
- Modify users
- Change permissions
- Stop services
- Access protected files

---

## Run a Command as Administrator

```bash
sudo command
```

Example:

```bash
sudo apt update
```

---

## Open a Root Shell

```bash
sudo -i
```

Check:

```bash
whoami
```

Output:

```text
root
```

Exit:

```bash
exit
```

---

⚠️ Avoid using root unnecessarily.

Prefer:

```bash
sudo command
```

instead of working permanently as root.

---

# 15. `umask`

`umask` controls the default permissions assigned when new files and directories are created.

Check your current value:

```bash
umask
```

Example:

```text
0022
```

A common default is:

```text
0022
```

The resulting default permissions depend on the program creating the file, but commonly:

```text
Files:       644
Directories: 755
```

---

# 16. Special Permissions

Linux also has special permissions.

The main ones are:

- SUID
- SGID
- Sticky Bit

---

## SUID

SUID allows an executable to run with the permissions of its owner.

Example:

```text
-rwsr-xr-x
```

Set SUID:

```bash
chmod u+s program
```

Numeric form:

```bash
chmod 4755 program
```

---

## SGID

SGID on a directory causes new files/directories created inside it to inherit the directory's group.

Example:

```bash
chmod g+s shared/
```

Numeric form:

```bash
chmod 2775 shared/
```

This is useful for shared project directories.

---

## Sticky Bit

The sticky bit is commonly used on shared directories such as `/tmp`.

It prevents users from deleting or renaming files owned by other users when the directory is configured with the sticky bit.

Example:

```bash
chmod +t shared/
```

Numeric form:

```bash
chmod 1777 shared/
```

---

# 17. Practical Examples

## Example 1 — Make a Script Executable

Create:

```bash
touch script.sh
```

Add content:

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "Hello Linux"' >> script.sh
```

Check permissions:

```bash
ls -l script.sh
```

Make it executable:

```bash
chmod +x script.sh
```

Run it:

```bash
./script.sh
```

---

## Example 2 — Create a Shared Project

Create a group:

```bash
sudo groupadd developers
```

Create a project:

```bash
sudo mkdir /opt/my-project
```

Change group:

```bash
sudo chgrp developers /opt/my-project
```

Set permissions:

```bash
sudo chmod 2775 /opt/my-project
```

Add a user:

```bash
sudo usermod -aG developers wassel
```

Check:

```bash
groups wassel
```

---

## Example 3 — Private File

Create:

```bash
touch secret.txt
```

Set permissions:

```bash
chmod 600 secret.txt
```

Result:

```text
-rw-------
```

Only the owner can read and write the file.

---

## Example 4 — Public Readable File

```bash
chmod 644 document.txt
```

Result:

```text
-rw-r--r--
```

Owner:

```text
read + write
```

Group:

```text
read
```

Others:

```text
read
```

---

## Example 5 — Shared Directory

```bash
mkdir shared
chmod 2775 shared
```

The `2` enables SGID.

New files inside the directory inherit the directory's group.

---

# 18. Practice Exercises

## Exercise 1 — Check Your Permissions

Run:

```bash
ls -l
```

Choose a file and identify:

```text
Owner:
Group:
Owner permissions:
Group permissions:
Other permissions:
```

---

## Exercise 2 — Create a Private File

Create:

```bash
touch private.txt
```

Set:

```bash
chmod 600 private.txt
```

Check:

```bash
ls -l private.txt
```

Expected:

```text
-rw-------
```

---

## Exercise 3 — Create an Executable Script

Create:

```bash
touch hello.sh
```

Add:

```bash
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello Linux"' >> hello.sh
```

Make it executable:

```bash
chmod 755 hello.sh
```

Run:

```bash
./hello.sh
```

---

## Exercise 4 — Create a Group

Create:

```bash
sudo groupadd developers
```

Check:

```bash
getent group developers
```

---

## Exercise 5 — Add Your User to the Group

```bash
sudo usermod -aG developers $USER
```

Then check:

```bash
groups
```

You may need to log out and log back in for the new group membership to appear in a new session.

---

## Exercise 6 — Create a Shared Directory

```bash
sudo mkdir /opt/shared-project
sudo chgrp developers /opt/shared-project
sudo chmod 2775 /opt/shared-project
```

Check:

```bash
ls -ld /opt/shared-project
```

---

# 19. Useful Commands

| Command | Purpose |
|---|---|
| `whoami` | Show current user |
| `id` | Show user and group IDs |
| `groups` | Show user's groups |
| `ls -l` | Show permissions and ownership |
| `chmod` | Change permissions |
| `chown` | Change owner |
| `chgrp` | Change group |
| `adduser` | Create user |
| `deluser` | Delete user |
| `usermod` | Modify user |
| `groupadd` | Create group |
| `groupdel` | Delete group |
| `getent group` | Show group information |
| `sudo` | Run command with elevated privileges |
| `umask` | Show/change default permission mask |

---

# 🎯 Permission Cheat Sheet

```text
r = 4
w = 2
x = 1
```

Common permissions:

```text
600 = rw-------
644 = rw-r--r--
700 = rwx------
755 = rwxr-xr-x
775 = rwxrwxr-x
777 = rwxrwxrwx
```

Common special permissions:

```text
4755 = SUID
2775 = SGID
1777 = Sticky Bit
```

---

# 🎯 Important Commands to Remember

```bash
ls -l
whoami
id
groups
chmod
chown
chgrp
sudo
usermod
adduser
groupadd
groupdel
umask
```

Understanding **users, groups, ownership, and permissions** is essential for:

- Linux administration
- Backend development
- DevOps
- Docker
- Cloud Engineering
- Server administration
- SSH
- Web servers
- CI/CD