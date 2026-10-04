# 📁 Linux Files and Directories

A practical guide to understanding and managing **files and directories in Linux**.

---

## 📚 Table of Contents

1. [Linux Filesystem](#1-linux-filesystem)
2. [Important Directories](#2-important-directories)
3. [Paths](#3-paths)
4. [List Files](#4-list-files)
5. [Create Files](#5-create-files)
6. [Create Directories](#6-create-directories)
7. [Navigate Directories](#7-navigate-directories)
8. [Copy Files and Directories](#8-copy-files-and-directories)
9. [Move and Rename](#9-move-and-rename)
10. [Delete Files and Directories](#10-delete-files-and-directories)
11. [View File Content](#11-view-file-content)
12. [Hidden Files](#12-hidden-files)
13. [Wildcards](#13-wildcards)
14. [Search Files](#14-search-files)
15. [File Information](#15-file-information)
16. [Links](#16-links)
17. [Useful Commands](#17-useful-commands)
18. [Practice Exercises](#18-practice-exercises)

---

# 1. Linux Filesystem

Linux uses a hierarchical filesystem.

The top-level directory is:

```text
/
```

This is called the **root directory**.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── tmp
├── usr
└── var
```

Everything in Linux starts from `/`.

---

# 2. Important Directories

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/home` | Users' home directories |
| `/root` | Home directory of the root user |
| `/etc` | System configuration |
| `/var` | Variable data and logs |
| `/tmp` | Temporary files |
| `/usr` | Applications and system resources |
| `/bin` | Essential commands |
| `/sbin` | System administration commands |
| `/dev` | Device files |
| `/proc` | Process and kernel information |
| `/opt` | Optional software |
| `/mnt` | Temporary mount points |
| `/media` | Removable media |

Example:

```bash
cd /home
```

---

# 3. Paths

A path tells Linux where a file or directory is located.

## Absolute Path

An absolute path starts from `/`.

Example:

```text
/home/wassel/projects/app
```

You can use:

```bash
cd /home/wassel/projects
```

---

## Relative Path

A relative path starts from your current directory.

Example:

```bash
cd projects
```

If you are currently in:

```text
/home/wassel
```

Linux will go to:

```text
/home/wassel/projects
```

---

## Special Paths

### Current Directory

```text
.
```

Example:

```bash
ls .
```

### Parent Directory

```text
..
```

Example:

```bash
cd ..
```

### Home Directory

```text
~
```

Example:

```bash
cd ~
```

### Root Directory

```text
/
```

Example:

```bash
cd /
```

---

# 4. List Files

## Basic `ls`

```bash
ls
```

List files in the current directory.

---

## Long Format

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 wassel wassel 1200 Oct 4 notes.txt
```

---

## Show Hidden Files

```bash
ls -a
```

---

## Long Format + Hidden Files

```bash
ls -la
```

---

## Human-Readable File Sizes

```bash
ls -lh
```

---

## Sort by Modification Time

```bash
ls -lt
```

---

## Common Combination

```bash
ls -lah
```

---

# 5. Create Files

## Using `touch`

```bash
touch file.txt
```

Create an empty file.

---

## Create Multiple Files

```bash
touch file1.txt file2.txt file3.txt
```

---

## Create a File with Content

Using `echo`:

```bash
echo "Hello Linux" > hello.txt
```

Check it:

```bash
cat hello.txt
```

---

## Append Content

```bash
echo "Another line" >> hello.txt
```

The `>>` operator adds content without deleting the existing content.

---

# 6. Create Directories

## Create One Directory

```bash
mkdir projects
```

---

## Create Multiple Directories

```bash
mkdir frontend backend database
```

---

## Create Nested Directories

```bash
mkdir -p projects/frontend/src
```

The `-p` option creates the parent directories if they don't exist.

---

## Example

```bash
mkdir -p my-project/src/components
```

Result:

```text
my-project/
└── src/
    └── components/
```

---

# 7. Navigate Directories

## Show Current Directory

```bash
pwd
```

Example:

```text
/home/wassel/projects
```

---

## Enter a Directory

```bash
cd projects
```

---

## Go Back

```bash
cd ..
```

---

## Go to Home

```bash
cd ~
```

or:

```bash
cd
```

---

## Go to Root

```bash
cd /
```

---

## Return to Previous Directory

```bash
cd -
```

---

# 8. Copy Files and Directories

The main command is:

```bash
cp
```

## Copy a File

```bash
cp file.txt backup.txt
```

---

## Copy File to Directory

```bash
cp file.txt documents/
```

---

## Copy Multiple Files

```bash
cp file1.txt file2.txt documents/
```

---

## Copy a Directory

Use `-r`:

```bash
cp -r projects projects_backup
```

The `-r` means **recursive**.

---

## Preserve File Attributes

```bash
cp -p file.txt backup.txt
```

---

# 9. Move and Rename

The main command is:

```bash
mv
```

## Rename a File

```bash
mv old.txt new.txt
```

---

## Move a File

```bash
mv file.txt documents/
```

---

## Move and Rename

```bash
mv file.txt documents/new-name.txt
```

---

## Rename a Directory

```bash
mv old-project new-project
```

---

# 10. Delete Files and Directories

⚠️ Be careful with `rm`. Deleted files may not be recoverable through the normal command line.

## Delete a File

```bash
rm file.txt
```

---

## Delete Multiple Files

```bash
rm file1.txt file2.txt
```

---

## Delete an Empty Directory

```bash
rmdir empty-directory
```

---

## Delete a Directory and Its Contents

```bash
rm -r project
```

---

## Force Delete

```bash
rm -f file.txt
```

---

## Recursive + Force

```bash
rm -rf project
```

⚠️ **Be extremely careful with `rm -rf`.**

Never run commands such as:

```bash
rm -rf /
```

---

# 11. View File Content

## `cat`

Display the entire file:

```bash
cat file.txt
```

---

## `less`

Useful for large files:

```bash
less file.txt
```

Navigate with:

```text
Space    Next page
b        Previous page
↑        Up
↓        Down
q        Quit
```

---

## `head`

Show the first lines:

```bash
head file.txt
```

Show the first 20 lines:

```bash
head -n 20 file.txt
```

---

## `tail`

Show the last lines:

```bash
tail file.txt
```

Show the last 20 lines:

```bash
tail -n 20 file.txt
```

---

## Follow a File in Real Time

Very useful for logs:

```bash
tail -f application.log
```

Stop with:

```text
Ctrl + C
```

---

# 12. Hidden Files

Linux considers files beginning with `.` to be hidden.

Example:

```text
.env
.gitignore
.bashrc
.config
```

List hidden files:

```bash
ls -a
```

Example:

```text
.
..
.bashrc
.config
projects
```

### Important

`.` means:

```text
Current directory
```

`..` means:

```text
Parent directory
```

---

# 13. Wildcards

Wildcards allow you to work with multiple files.

## `*`

Matches multiple characters.

Example:

```bash
ls *.txt
```

Shows all `.txt` files.

---

## `?`

Matches exactly one character.

Example:

```bash
ls file?.txt
```

Matches:

```text
file1.txt
file2.txt
fileA.txt
```

But not:

```text
file10.txt
```

---

## Character Sets

```bash
ls file[123].txt
```

Matches:

```text
file1.txt
file2.txt
file3.txt
```

---

# 14. Search Files

## `find`

Search for a file by name:

```bash
find . -name "file.txt"
```

---

## Find All `.txt` Files

```bash
find . -name "*.txt"
```

---

## Case-Insensitive Search

```bash
find . -iname "*.TXT"
```

---

## Find Directories

```bash
find . -type d
```

---

## Find Files

```bash
find . -type f
```

---

## Find Files by Size

Example:

```bash
find . -type f -size +100M
```

Find files larger than 100 MB.

---

# 15. File Information

## `file`

Determine the type of a file:

```bash
file document.pdf
```

Example:

```text
document.pdf: PDF document
```

---

## `stat`

Display detailed information:

```bash
stat file.txt
```

You can see:

- File size
- Permissions
- Owner
- Modification time
- Access time
- Inode
- File type

---

## Disk Usage

### File Size

```bash
du -h file.txt
```

### Directory Size

```bash
du -sh project/
```

Example:

```text
250M    project/
```

---

## Filesystem Disk Usage

```bash
df -h
```

This shows available disk space.

---

# 16. Links

Linux supports two main types of links:

- Hard links
- Symbolic links

---

## Symbolic Link

Create a symbolic link:

```bash
ln -s original.txt shortcut.txt
```

Example:

```text
original.txt
shortcut.txt -> original.txt
```

Check it:

```bash
ls -l
```

---

## Why Use Symbolic Links?

They are useful for:

- Configuration files
- Shortcuts
- Application versions
- Development environments
- Server configurations

Example:

```bash
ln -s /var/www/my-app/current /var/www/my-app/public
```

---

## Hard Link

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Hard links point to the same underlying inode.

---

# 17. Useful Commands

| Command | Purpose |
|---|---|
| `pwd` | Show current directory |
| `ls` | List files |
| `cd` | Change directory |
| `mkdir` | Create directory |
| `rmdir` | Remove empty directory |
| `touch` | Create file |
| `cp` | Copy files/directories |
| `mv` | Move/rename |
| `rm` | Delete files/directories |
| `cat` | Display file |
| `less` | Read large files |
| `head` | Show beginning |
| `tail` | Show end |
| `find` | Search files |
| `file` | Identify file type |
| `stat` | File information |
| `du` | Disk usage |
| `df` | Filesystem usage |
| `ln` | Create links |

---

# 18. Practice Exercises

## Exercise 1 — Create a Project

Create this structure:

```text
linux-practice/
├── documents/
├── images/
├── projects/
└── notes.txt
```

Commands:

```bash
mkdir linux-practice
cd linux-practice

mkdir documents images projects
touch notes.txt
```

Check:

```bash
ls -la
```

---

## Exercise 2 — Create Nested Directories

Create:

```text
projects/
└── web/
    ├── frontend/
    └── backend/
```

Command:

```bash
mkdir -p projects/web/frontend projects/web/backend
```

Check:

```bash
find projects -type d
```

---

## Exercise 3 — Copy Files

Create a file:

```bash
echo "Linux practice" > notes.txt
```

Copy it:

```bash
cp notes.txt documents/
```

Check:

```bash
ls documents/
```

---

## Exercise 4 — Move and Rename

Rename:

```bash
mv notes.txt linux-notes.txt
```

Move it:

```bash
mv linux-notes.txt documents/
```

---

## Exercise 5 — Search

Find all `.txt` files:

```bash
find . -type f -name "*.txt"
```

---

## Exercise 6 — Create a Symbolic Link

```bash
ln -s documents/linux-notes.txt notes-link.txt
```

Check:

```bash
ls -l
```

---

# 🎯 Important Commands to Remember

For daily Linux development, these commands are especially important:

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
less
head
tail
find
file
stat
du
df
ln
```

These commands form the foundation for working with files and directories in **Linux, DevOps, Cloud Engineering, Docker, and server administration**.