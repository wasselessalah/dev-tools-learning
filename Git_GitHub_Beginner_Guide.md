# Git & GitHub — Beginner Guide

A practical guide to learning **Git** and **GitHub** from zero.

---

## 1. What You Will Learn

By the end of this guide, you should understand:

- What Git is
- What GitHub is
- The difference between Git and GitHub
- How Git works
- How to install Git
- How to configure Git
- How to create a Git repository
- How to make commits
- How to use branches
- How to connect a local project to GitHub
- How to push and pull code
- How to clone a repository
- How to work with remote repositories
- How to create and use Pull Requests
- How to solve basic merge conflicts
- Basic Git best practices

---

# 2. What Is Git?

**Git** is a distributed version control system.

It helps developers track changes in their code.

For example, imagine you have:

```text
my-project/
├── index.html
├── style.css
└── script.js
```

You make changes every day.

Without Git, it can become difficult to know:

- What changed?
- Who changed it?
- When was it changed?
- Why was it changed?
- How can I return to an older version?

Git solves these problems.

### Simple definition

> Git is a tool that tracks the history of changes in your project.

Git works **locally on your computer**.

---

# 3. What Is GitHub?

**GitHub** is an online platform that hosts Git repositories.

It provides tools for collaboration and software development.

With GitHub, you can:

- Store your repositories online
- Share your code
- Collaborate with other developers
- Review code
- Create Pull Requests
- Report issues
- Manage projects
- Create documentation
- Build open-source projects

### Simple definition

> GitHub is an online platform where Git repositories can be stored, shared, and collaboratively developed.

---

# 4. Git vs GitHub

| Git | GitHub |
|---|---|
| Software/tool | Online platform |
| Runs locally | Runs mainly online |
| Tracks code changes | Hosts Git repositories |
| Works without internet | Usually requires internet for remote operations |
| Created for version control | Adds collaboration features |
| Uses commands like `git commit` | Provides web UI, Pull Requests, Issues, etc. |

### Easy way to remember

```text
Git = Version Control
GitHub = Online Collaboration Platform
```

Git and GitHub are **not the same thing**.

You can use Git without GitHub.

You can also use GitHub because Git is the version-control system behind your repository.

---

# 5. Git Installation

## Official Git Website

Download Git from the official website:

https://git-scm.com/

## Git Downloads

https://git-scm.com/install/

Choose the version for your operating system.

### Windows

Download the Windows installer and follow the installation wizard.

### macOS

You can install Git using Xcode Command Line Tools or another supported installation method.

### Linux

For Ubuntu/Debian:

```bash
sudo apt update
sudo apt install git
```

For Fedora:

```bash
sudo dnf install git
```

For Arch Linux:

```bash
sudo pacman -S git
```

---

# 6. Verify Git Installation

After installing Git, open your terminal and run:

```bash
git --version
```

Example:

```text
git version 2.x.x
```

If you see a Git version, Git is installed correctly.

---

# 7. Configure Git

Before using Git, configure your identity.

Set your name:

```bash
git config --global user.name "Your Name"
```

Set your email:

```bash
git config --global user.email "you@example.com"
```

Example:

```bash
git config --global user.name "Wassel Essalah"
git config --global user.email "you@example.com"
```

Check your configuration:

```bash
git config --global --list
```

You can also check individual values:

```bash
git config --global user.name
git config --global user.email
```

---

# 8. Git Basic Concepts

Before learning commands, understand these concepts.

## Working Directory

Your actual project files.

Example:

```text
my-project/
├── src/
├── package.json
└── README.md
```

## Repository

A Git repository contains the history of your project.

A repository is created with:

```bash
git init
```

Git creates a hidden `.git` directory.

```text
my-project/
├── .git/
├── src/
├── package.json
└── README.md
```

Do not manually edit the `.git` directory.

---

# 9. The Three Main Git Areas

Git can be understood using three main areas:

```text
Working Directory
       ↓
   Staging Area
       ↓
     Commit
```

## Working Directory

You modify your files here.

## Staging Area

You choose which changes should be included in the next commit.

```bash
git add file.txt
```

## Repository / Commit History

You permanently record the staged changes in Git history.

```bash
git commit -m "Add new feature"
```

---

# 10. Create Your First Git Repository

Create a project:

```bash
mkdir my-project
cd my-project
```

Initialize Git:

```bash
git init
```

You should see something similar to:

```text
Initialized empty Git repository
```

---

# 11. Check Repository Status

Use:

```bash
git status
```

This is one of the most important Git commands.

It tells you:

- Which files are modified
- Which files are untracked
- Which files are staged
- Which branch you are using

Example:

```text
On branch main

Untracked files:
  README.md
```

---

# 12. Create a File

For example:

```bash
touch README.md
```

Add some content:

```text
# My Project

This is my first Git project.
```

Check the status:

```bash
git status
```

Git will detect the new file.

---

# 13. Git Add

Add a specific file:

```bash
git add README.md
```

Or add all modified/new files:

```bash
git add .
```

Check the status:

```bash
git status
```

The file should now appear under **Changes to be committed**.

---

# 14. Git Commit

A commit saves a snapshot of your staged changes.

```bash
git commit -m "Add README"
```

Good commit messages explain what changed.

Examples:

```bash
git commit -m "Add login page"
git commit -m "Fix authentication bug"
git commit -m "Update user profile UI"
git commit -m "Add database connection"
```

Avoid vague messages such as:

```bash
git commit -m "update"
git commit -m "stuff"
git commit -m "changes"
```

---

# 15. View Commit History

Use:

```bash
git log
```

A shorter version:

```bash
git log --oneline
```

Example:

```text
a82f1c2 Add authentication
4f7b921 Add README
```

---

# 16. The Basic Git Workflow

A very common Git workflow is:

```text
1. Modify files
       ↓
2. git status
       ↓
3. git add .
       ↓
4. git commit -m "message"
       ↓
5. git push
```

Commands:

```bash
git status
git add .
git commit -m "Describe your changes"
git push
```

---

# 17. What Is a Branch?

A **branch** is an independent line of development.

The main branch is commonly called:

```text
main
```

You can create a feature branch:

```bash
git switch -c feature/login
```

Now you can work on the login feature without directly changing `main`.

---

# 18. List Branches

```bash
git branch
```

Example:

```text
* feature/login
  main
```

The `*` indicates the current branch.

---

# 19. Switch Branches

Switch to `main`:

```bash
git switch main
```

Switch to another branch:

```bash
git switch feature/login
```

Older Git syntax also exists:

```bash
git checkout main
```

For new projects, `git switch` is generally easier to understand.

---

# 20. Create a Branch

Modern syntax:

```bash
git switch -c feature/profile
```

This:

1. Creates the branch
2. Switches to it

Equivalent older syntax:

```bash
git checkout -b feature/profile
```

---

# 21. Merge a Branch

Suppose you have:

```text
main
feature/login
```

First switch to `main`:

```bash
git switch main
```

Then merge:

```bash
git merge feature/login
```

Git will integrate the feature branch into `main`.

---

# 22. Delete a Branch

After merging:

```bash
git branch -d feature/login
```

Force deletion:

```bash
git branch -D feature/login
```

Be careful with `-D` because it can delete a branch containing unmerged work.

---

# 23. What Is a Remote Repository?

A **remote repository** is another copy of your Git repository, usually hosted online.

For example:

```text
Your Computer
     |
     | Git
     ↓
GitHub Repository
```

A remote is commonly called:

```text
origin
```

---

# 24. Create a GitHub Account

Go to:

https://github.com/

Create an account if you do not already have one.

Your GitHub username will be used in your repository URLs.

Example:

```text
https://github.com/username/my-project
```

---

# 25. Create a GitHub Repository

On GitHub:

1. Click **New repository**
2. Enter a repository name
3. Choose Public or Private
4. Create the repository

Example repository name:

```text
my-first-git-project
```

---

# 26. Connect Local Git to GitHub

After creating the GitHub repository, connect your local repository.

Using HTTPS:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
```

Example:

```bash
git remote add origin https://github.com/wasselessalah/my-first-git-project.git
```

Check the remote:

```bash
git remote -v
```

---

# 27. Rename Your Main Branch

Many projects use `main`.

Set your current branch to `main`:

```bash
git branch -M main
```

---

# 28. Push Your Project to GitHub

Push the `main` branch:

```bash
git push -u origin main
```

The `-u` option sets the upstream relationship.

After that, you can usually use:

```bash
git push
```

---

# 29. Push Workflow

After making changes:

```bash
git status
git add .
git commit -m "Update project"
git push
```

Your changes will be uploaded to GitHub.

---

# 30. Clone a GitHub Repository

If a project already exists on GitHub, you can download its Git repository with:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Example:

```bash
git clone https://github.com/wasselessalah/my-project.git
```

Then:

```bash
cd my-project
```

---

# 31. Pull Changes

If another developer pushed changes to GitHub, download and integrate them:

```bash
git pull
```

A common workflow is:

```text
GitHub
   ↓
git pull
   ↓
Your local project
```

---

# 32. Fetch vs Pull

These commands are related but different.

## git fetch

Downloads information from the remote repository without integrating it into your current branch.

```bash
git fetch
```

## git pull

Fetches changes and then integrates them into your current branch.

```bash
git pull
```

Simple idea:

```text
fetch = download remote information

pull = fetch + integrate
```

---

# 33. Git Remote Commands

List remotes:

```bash
git remote -v
```

Add a remote:

```bash
git remote add origin URL
```

Change a remote URL:

```bash
git remote set-url origin URL
```

Remove a remote:

```bash
git remote remove origin
```

---

# 34. .gitignore

Some files should **not** be committed.

For example, in a Node.js project:

```text
node_modules/
.env
.next/
dist/
```

Create a file:

```text
.gitignore
```

Example:

```gitignore
node_modules/
.env
.next/
dist/
coverage/
```

This tells Git to ignore these files/directories.

---

# 35. Never Commit Secrets

Do not commit:

```text
.env
API keys
passwords
private tokens
database credentials
secret keys
```

Bad:

```env
DATABASE_URL="secret-database-url"
API_KEY="my-secret-key"
```

Instead, keep secrets in environment variables and ignore `.env`.

If a secret has already been pushed to GitHub, simply deleting it from the latest file is **not enough** because it may remain in Git history. Rotate/revoke the exposed secret immediately.

---

# 36. Git Diff

See changes that have not been staged:

```bash
git diff
```

See staged changes:

```bash
git diff --staged
```

This is useful before committing.

---

# 37. Git Restore

Discard changes in a file that have not been committed:

```bash
git restore file.txt
```

Restore all unstaged changes:

```bash
git restore .
```

**Warning:** this can permanently remove your local uncommitted changes.

---

# 38. Unstage a File

If you accidentally run:

```bash
git add file.txt
```

You can remove it from the staging area:

```bash
git restore --staged file.txt
```

Your file changes remain in your working directory.

---

# 39. Amend the Last Commit

If you forgot something in your last commit:

```bash
git add .
git commit --amend
```

Or:

```bash
git commit --amend -m "Better commit message"
```

Be careful when amending commits that have already been pushed and shared.

---

# 40. GitHub Pull Requests

A **Pull Request (PR)** is a request to merge changes from one branch into another.

Typical workflow:

```text
main
  ↓
create feature branch
  ↓
make changes
  ↓
commit
  ↓
push branch
  ↓
open Pull Request
  ↓
code review
  ↓
merge
```

Example:

```bash
git switch -c feature/login

# Make changes

git add .
git commit -m "Add login feature"
git push -u origin feature/login
```

Then create a Pull Request on GitHub.

---

# 41. What Is a Merge Conflict?

A merge conflict happens when Git cannot automatically combine changes.

Example:

Developer A changes:

```text
Hello World
```

Developer B changes the same line to:

```text
Hello GitHub
```

Git may not know which version should be kept.

You may see:

```text
<<<<<<< HEAD
Hello World
=======
Hello GitHub
>>>>>>> feature/example
```

You must manually choose the correct version.

Then:

```bash
git add .
git commit -m "Resolve merge conflict"
```

---

# 42. Useful Git Commands

| Command | Purpose |
|---|---|
| `git init` | Create a repository |
| `git status` | Check repository status |
| `git add .` | Stage changes |
| `git commit` | Save a snapshot |
| `git log` | View history |
| `git branch` | List branches |
| `git switch` | Change branches |
| `git merge` | Merge branches |
| `git clone` | Copy a repository |
| `git remote -v` | Show remotes |
| `git push` | Upload commits |
| `git pull` | Download and integrate changes |
| `git fetch` | Download remote information |
| `git diff` | View changes |
| `git restore` | Restore files |
| `git stash` | Temporarily save changes |

---

# 43. Git Stash

Sometimes you are working on a feature but need to switch branches without committing incomplete work.

You can temporarily save your changes:

```bash
git stash
```

Switch branches:

```bash
git switch main
```

Later, return to your branch and restore the changes:

```bash
git stash pop
```

---

# 44. Recommended Branch Naming

Use descriptive names.

Examples:

```text
feature/login
feature/user-profile
feature/payment
fix/login-error
fix/navbar-mobile
refactor/auth-service
docs/github-guide
```

Avoid:

```text
test
new
branch1
aaa
mybranch
```

---

# 45. Good Commit Messages

Good:

```bash
git commit -m "Add Google authentication"
git commit -m "Fix profile loading state"
git commit -m "Update invoice PDF layout"
git commit -m "Add user search endpoint"
```

Bad:

```bash
git commit -m "update"
git commit -m "fix"
git commit -m "hello"
git commit -m "final"
```

A commit should describe the change.

---

# 46. A Complete Beginner Example

Let's create a project from zero.

## Step 1 — Create project

```bash
mkdir github-learning
cd github-learning
```

## Step 2 — Initialize Git

```bash
git init
```

## Step 3 — Create README

```bash
echo "# GitHub Learning" > README.md
```

## Step 4 — Check status

```bash
git status
```

## Step 5 — Stage

```bash
git add README.md
```

## Step 6 — Commit

```bash
git commit -m "Add README"
```

## Step 7 — Create GitHub repository

Create:

```text
github-learning
```

on GitHub.

## Step 8 — Connect GitHub

```bash
git remote add origin https://github.com/USERNAME/github-learning.git
```

## Step 9 — Use main

```bash
git branch -M main
```

## Step 10 — Push

```bash
git push -u origin main
```

Your project is now on GitHub.

---

# 47. Daily Developer Workflow

A simple workflow for personal projects:

```bash
git status
```

Work on your project.

Then:

```bash
git diff
```

Review your changes.

Stage them:

```bash
git add .
```

Commit:

```bash
git commit -m "Describe what you changed"
```

Push:

```bash
git push
```

Before starting work again:

```bash
git pull
```

---

# 48. Team Workflow

For a team project, a common workflow is:

```text
main
 │
 ├── feature/authentication
 │
 ├── feature/profile
 │
 └── fix/payment-error
```

Each developer works on a separate branch.

Example:

```bash
git switch main
git pull

git switch -c feature/profile
```

Make changes:

```bash
git add .
git commit -m "Add profile page"
```

Push:

```bash
git push -u origin feature/profile
```

Then create a Pull Request on GitHub.

---

# 49. GitHub Repository Structure

A professional project might contain:

```text
my-project/
├── .git/
├── .github/
│   └── workflows/
├── src/
├── public/
├── .gitignore
├── README.md
├── package.json
└── LICENSE
```

The `.github` directory can contain GitHub-specific configuration such as GitHub Actions workflows.

---

# 50. GitHub Actions

**GitHub Actions** allows you to automate tasks.

For example:

```text
Push code
   ↓
GitHub Actions
   ↓
Install dependencies
   ↓
Run tests
   ↓
Run lint
   ↓
Build application
```

It is commonly used for:

- CI/CD
- Automated tests
- Linting
- Builds
- Deployment
- Scheduled tasks

Example workflow location:

```text
.github/workflows/ci.yml
```

---

# 51. Git Tags

Tags can mark important versions.

Example:

```bash
git tag v1.0.0
```

Push the tag:

```bash
git push origin v1.0.0
```

Tags are useful for releases:

```text
v1.0.0
v1.1.0
v2.0.0
```

---

# 52. GitHub Issues

GitHub Issues can be used to track:

- Bugs
- Features
- Tasks
- Improvements
- Questions

Example:

```text
Issue #12
Title: Fix mobile navigation

Description:
The navigation menu does not close on mobile.
```

---

# 53. GitHub README

A good `README.md` explains your project.

Example structure:

```markdown
# My Project

Short project description.

## Features

- Authentication
- User profiles
- Real-time chat

## Tech Stack

- Next.js
- Node.js
- PostgreSQL

## Installation

```bash
npm install
npm run dev
```

## Environment Variables

Create a `.env` file and configure the required variables.

## Author

Your Name
```

---

# 54. HTTPS vs SSH

There are two common ways to authenticate with GitHub.

## HTTPS

Example:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

Easy to start with.

## SSH

Example:

```bash
git clone git@github.com:USERNAME/REPOSITORY.git
```

SSH is popular for developers who work with GitHub frequently.

You normally configure an SSH key once and then use it for authentication.

---

# 55. GitHub CLI

GitHub also provides a command-line tool called **GitHub CLI (`gh`)**.

It allows you to interact with GitHub from the terminal.

Examples:

```bash
gh auth login
```

Create a repository:

```bash
gh repo create
```

View repositories:

```bash
gh repo list
```

Create a Pull Request:

```bash
gh pr create
```

Git and GitHub CLI are different:

```text
git = version control
gh  = GitHub command-line interface
```

---

# 56. Important Git Mental Model

Remember this:

```text
             Git
              |
      ┌───────┴────────┐
      ↓                ↓
 Local Repository   Remote Repository
      |                |
      |                |
   commits          GitHub
      |                |
      └──── push ──────┤
      ┌──── pull ──────┘
```

The most important operations are:

```text
Local → GitHub
git push

GitHub → Local
git pull
```

---

# 57. The Most Important Commands to Memorize

Start with these:

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

git init
git status
git add .
git commit -m "message"

git log --oneline

git branch
git switch -c feature/name
git switch main
git merge feature/name

git remote -v
git remote add origin URL

git push
git pull
git clone URL
```

You do **not** need to memorize every Git command immediately.

Learn the basic workflow first.

---

# 58. Recommended Learning Order

## Level 1 — Basics

Learn:

```text
Git
GitHub
Repository
Working directory
Staging
Commit
Remote
```

Commands:

```bash
git init
git status
git add
git commit
git log
```

## Level 2 — GitHub

Learn:

```text
GitHub repository
remote
push
pull
clone
```

Commands:

```bash
git remote
git push
git pull
git clone
```

## Level 3 — Branches

Learn:

```text
branch
switch
merge
Pull Request
```

Commands:

```bash
git branch
git switch
git merge
```

## Level 4 — Collaboration

Learn:

```text
Pull Requests
code review
merge conflicts
Issues
GitHub Projects
GitHub Actions
```

## Level 5 — Advanced Git

Later learn:

```text
git rebase
git cherry-pick
git reset
git revert
git reflog
interactive rebase
```

Do not start with advanced commands.

---

# 59. Common Beginner Mistakes

## Mistake 1 — Committing secrets

Never commit:

```text
.env
passwords
API keys
private keys
```

## Mistake 2 — Using unclear commits

Avoid:

```text
update
fix
test
final
```

## Mistake 3 — Working directly on main

For team projects, prefer feature branches.

## Mistake 4 — Running commands without understanding them

Especially:

```bash
git reset --hard
git clean
git push --force
git branch -D
```

These can cause data loss or affect shared history.

## Mistake 5 — Ignoring conflicts

Do not blindly delete conflict markers. Understand which changes should remain.

---

# 60. Git Cheat Sheet

### Check Git

```bash
git --version
```

### Configure Git

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### Create repository

```bash
git init
```

### Check status

```bash
git status
```

### Stage

```bash
git add .
```

### Commit

```bash
git commit -m "message"
```

### History

```bash
git log --oneline
```

### Create branch

```bash
git switch -c feature/name
```

### Switch branch

```bash
git switch main
```

### Merge

```bash
git merge feature/name
```

### Add remote

```bash
git remote add origin URL
```

### Push

```bash
git push
```

### Pull

```bash
git pull
```

### Clone

```bash
git clone URL
```

### See changes

```bash
git diff
```

### Temporarily save changes

```bash
git stash
git stash pop
```

---

# 61. Mini Practice Exercises

## Exercise 1 — First Repository

Create:

```text
git-practice/
└── README.md
```

Then:

```bash
git init
git add .
git commit -m "Initial commit"
```

---

## Exercise 2 — Branch

Create:

```bash
git switch -c feature/about
```

Add an `about.html` file.

Commit it:

```bash
git add .
git commit -m "Add about page"
```

Switch to main:

```bash
git switch main
```

Merge:

```bash
git merge feature/about
```

---

## Exercise 3 — GitHub

Create a GitHub repository and connect it:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git branch -M main
git push -u origin main
```

---

## Exercise 4 — Clone

Clone your repository into another directory:

```bash
git clone https://github.com/USERNAME/REPOSITORY.git
```

---

# 62. Final Summary

Remember these four ideas:

### Git

```text
Git tracks changes in your project.
```

### GitHub

```text
GitHub hosts Git repositories and provides collaboration tools.
```

### Commit

```text
A commit records a snapshot of your changes.
```

### Push / Pull

```text
push = local → remote

pull = remote → local
```

The basic workflow is:

```text
Edit
  ↓
git status
  ↓
git add .
  ↓
git commit
  ↓
git push
```

And when you start working again:

```text
git pull
  ↓
Edit
  ↓
git add .
  ↓
git commit
  ↓
git push
```

---

# 63. Official Resources

- Git: https://git-scm.com/
- Git Documentation: https://git-scm.com/doc
- GitHub: https://github.com/
- GitHub Docs: https://docs.github.com/
- GitHub Skills: https://skills.github.com/

---

## Next Step

After learning this guide, build a small project and use Git for every change.

A good progression is:

```text
Project
  ↓
git init
  ↓
first commit
  ↓
GitHub repository
  ↓
push
  ↓
feature branch
  ↓
Pull Request
  ↓
merge
  ↓
GitHub Actions
```

The goal is not to memorize Git commands.

The goal is to understand the workflow and use Git naturally while developing.
