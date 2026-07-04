# Git Basic Operations Tutorial

## Table of Contents

- [What is Git?](#what-is-git)
- [Installation](#installation)
- [Configuration](#configuration)
- [Creating a Repository](#creating-a-repository)
- [Basic Workflow](#basic-workflow)
- [Branching](#branching)
- [Remote Repositories](#remote-repositories)
- [Undoing Changes](#undoing-changes)
- [Viewing History](#viewing-history)
- [Common Scenarios](#common-scenarios)
- [Quick Reference](#quick-reference)

---

## What is Git?

Git is a **distributed version control system** that tracks changes in your code. It allows multiple developers to work together, maintain history of changes, and revert to previous states when needed.

---

## Installation

| OS | Command |
|----|---------|
| **Windows** | Download from [git-scm.com](https://git-scm.com) |
| **macOS** | `brew install git` |
| **Ubuntu/Debian** | `sudo apt-get install git` |
| **CentOS/RHEL** | `sudo yum install git` |

Verify installation:

```bash
git --version
```

---

## Configuration

Set up your identity (required before making commits):

```bash
# Set your name
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Set default editor
git config --global core.editor "code --wait"

# View all configurations
git config --list
```

| Scope | Flag | Description |
|-------|------|-------------|
| System | `--system` | Applies to all users on the machine |
| Global | `--global` | Applies to all repositories for current user |
| Local | `--local` | Applies only to current repository |

---

## Creating a Repository

### Option 1: Initialize a new repository

```bash
# Create a new project folder
mkdir my-project
cd my-project

# Initialize git repository
git init
```

> This creates a hidden `.git` folder that tracks all changes.

### Option 2: Clone an existing repository

```bash
# Clone via HTTPS
git clone https://github.com/username/repository.git

# Clone via SSH
git clone git@github.com:username/repository.git

# Clone into a specific folder
git clone https://github.com/username/repository.git my-folder
```

---

## Basic Workflow

The basic Git workflow follows this pattern:

```
Working Directory → Staging Area → Local Repository → Remote Repository
     (edit)          (git add)       (git commit)        (git push)
```

### 1. Check Status

Shows which files are modified, staged, or untracked.

```bash
git status

# Short format
git status -s
```

**Status symbols:**

| Symbol | Meaning |
|--------|---------|
| `??` | Untracked (new file) |
| `M` | Modified |
| `A` | Added to staging |
| `D` | Deleted |
| `R` | Renamed |

### 2. Add Files to Staging Area

Moves changes from working directory to staging area.

```bash
# Add a specific file
git add filename.txt

# Add multiple files
git add file1.txt file2.txt

# Add all files in a directory
git add src/

# Add all changes (new, modified, deleted)
git add .

# Add all modified and deleted files (not new files)
git add -u

# Interactively choose parts of files to add
git add -p
```

### 3. Commit Changes

Saves staged changes to the local repository with a message.

```bash
# Commit with a message
git commit -m "Add login feature"

# Commit with multi-line message
git commit -m "Title of commit" -m "Detailed description here"

# Add and commit tracked files in one step (skips git add for modified files)
git commit -am "Update existing files"

# Amend the last commit (edit message or add files)
git commit --amend -m "Corrected commit message"
```

**Good commit message format:**

```
type: short description (max 50 chars)

Optional longer description explaining what and why (max 72 chars per line)
```

Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### 4. Push Changes

Uploads local commits to the remote repository.

```bash
# Push to remote (first time - set upstream)
git push -u origin main

# Push to remote (after upstream is set)
git push

# Push a specific branch
git push origin feature-branch

# Force push (use with caution!)
git push --force
```

### 5. Pull Changes

Downloads and integrates changes from the remote repository.

```bash
# Pull changes (fetch + merge)
git pull

# Pull from specific remote and branch
git pull origin main

# Pull with rebase instead of merge
git pull --rebase
```

---

## Branching

Branches let you work on features separately without affecting the main code.

```
main:     A---B---C---F
               \     /
feature:        D---E
```

### Create and Switch Branches

```bash
# List all branches (* marks current branch)
git branch

# List all branches including remote
git branch -a

# Create a new branch
git branch feature-login

# Switch to a branch
git checkout feature-login

# Create and switch in one command
git checkout -b feature-login

# Modern alternative (Git 2.23+)
git switch feature-login
git switch -c new-feature   # create and switch
```

### Merge Branches

Combines changes from one branch into another.

```bash
# Switch to the branch you want to merge INTO
git checkout main

# Merge feature branch into current branch
git merge feature-login

# Merge with a commit message
git merge feature-login -m "Merge feature-login into main"

# Cancel a merge with conflicts
git merge --abort
```

### Delete Branches

```bash
# Delete a local branch (only if merged)
git branch -d feature-login

# Force delete a local branch
git branch -D feature-login

# Delete a remote branch
git push origin --delete feature-login
```

### Rename a Branch

```bash
# Rename current branch
git branch -m new-name

# Rename a specific branch
git branch -m old-name new-name
```

---

## Remote Repositories

Remotes are versions of your repository hosted on the internet (e.g., GitHub, GitLab).

### Manage Remotes

```bash
# View remote repositories
git remote -v

# Add a remote
git remote add origin https://github.com/username/repo.git

# Change remote URL
git remote set-url origin https://github.com/username/new-repo.git

# Remove a remote
git remote remove origin

# Rename a remote
git remote rename origin upstream
```

### Fetch Changes

Downloads changes from remote but doesn't merge them.

```bash
# Fetch from default remote
git fetch

# Fetch from specific remote
git fetch origin

# Fetch all remotes
git fetch --all
```

### Difference Between Fetch and Pull

| Command | Action |
|---------|--------|
| `git fetch` | Downloads changes only (safe, no auto-merge) |
| `git pull` | Downloads AND merges changes (fetch + merge) |

---

## Undoing Changes

### Discard Working Directory Changes

```bash
# Discard changes in a specific file
git checkout -- filename.txt

# Modern alternative (Git 2.23+)
git restore filename.txt

# Discard ALL uncommitted changes
git checkout -- .
git restore .
```

### Unstage Files

```bash
# Remove file from staging area (keep changes in working directory)
git reset HEAD filename.txt

# Modern alternative
git restore --staged filename.txt

# Unstage everything
git reset HEAD .
```

### Undo Commits

```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes in working directory (unstaged)
git reset --mixed HEAD~1    # (default behavior)
git reset HEAD~1

# Undo last commit, DISCARD all changes (dangerous!)
git reset --hard HEAD~1

# Undo last 3 commits
git reset --hard HEAD~3
```

### Revert a Commit (Safe)

Creates a new commit that undoes a previous commit. Safe for shared branches.

```bash
# Revert the most recent commit
git revert HEAD

# Revert a specific commit by hash
git revert abc1234

# Revert without auto-committing
git revert --no-commit abc1234
```

### Reset vs Revert

| Command | Use Case | History |
|---------|----------|---------|
| `git reset` | Private/local branches | Rewrites history |
| `git revert` | Shared/public branches | Preserves history |

---

## Viewing History

### View Commit Log

```bash
# Full log
git log

# One-line format
git log --oneline

# Show last 5 commits
git log -5

# Show log with graph
git log --oneline --graph --all

# Show log with file changes
git log --stat

# Show log for a specific file
git log -- filename.txt

# Search commits by message
git log --grep="login"

# Show commits by author
git log --author="John"

# Show commits in date range
git log --since="2024-01-01" --until="2024-06-30"
```

### View Differences

```bash
# Show unstaged changes (working directory vs staging)
git diff

# Show staged changes (staging vs last commit)
git diff --staged

# Compare two branches
git diff main..feature-branch

# Compare specific file
git diff filename.txt

# Show word-level diff
git diff --word-diff

# Compare two commits
git diff abc1234 def5678
```

### View a Specific Commit

```bash
# Show details of a commit
git show abc1234

# Show what changed in last commit
git show HEAD

# Show file content at a specific commit
git show abc1234:filename.txt
```

### Blame (Who Changed What)

```bash
# Show who last modified each line
git blame filename.txt

# Blame specific lines
git blame -L 10,20 filename.txt
```

---

## Common Scenarios

### Scenario 1: Start a New Feature

```bash
git checkout main
git pull
git checkout -b feature/user-authentication
# ... make changes ...
git add .
git commit -m "feat: add user authentication"
git push -u origin feature/user-authentication
# Create Pull Request on GitHub/GitLab
```

### Scenario 2: Fix a Merge Conflict

```bash
git pull origin main
# CONFLICT occurs

# 1. Open conflicted files - look for conflict markers:
# <<<<<<< HEAD
# your changes
# =======
# incoming changes
# >>>>>>> branch-name

# 2. Edit the file to resolve conflicts (remove markers, keep desired code)

# 3. Mark as resolved and commit
git add .
git commit -m "fix: resolve merge conflicts"
```

### Scenario 3: Stash Changes Temporarily

```bash
# Save current work without committing
git stash

# Save with a description
git stash save "work in progress: login page"

# List stashed changes
git stash list

# Apply most recent stash (keeps stash)
git stash apply

# Apply and remove most recent stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Delete a stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

### Scenario 4: Ignore Files

Create a `.gitignore` file in your project root:

```plaintext
# Dependencies
node_modules/
vendor/

# Environment files
.env
.env.local

# Build output
dist/
build/
*.exe

# IDE files
.vscode/
.idea/
*.swp

# OS files
.DS_Store
Thumbs.db

# Logs
*.log
logs/
```

```bash
# Stop tracking a file that's already tracked
git rm --cached filename.txt

# Stop tracking a folder
git rm -r --cached folder_name/
```

### Scenario 5: Tagging Releases

```bash
# Create a lightweight tag
git tag v1.0.0

# Create an annotated tag (recommended)
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tag a specific commit
git tag -a v1.0.0 abc1234 -m "Release version 1.0.0"

# List all tags
git tag

# Push a specific tag
git push origin v1.0.0

# Push all tags
git push origin --tags

# Delete a local tag
git tag -d v1.0.0

# Delete a remote tag
git push origin --delete v1.0.0
```

### Scenario 6: Cherry-Pick a Commit

Apply a specific commit from one branch to another.

```bash
# Switch to target branch
git checkout main

# Apply specific commit
git cherry-pick abc1234

# Cherry-pick without committing
git cherry-pick --no-commit abc1234
```

---

## Quick Reference

### Most Used Commands

```bash
git init                    # Initialize repository
git clone <url>             # Clone repository
git status                  # Check status
git add .                   # Stage all changes
git commit -m "message"     # Commit changes
git push                    # Push to remote
git pull                    # Pull from remote
git branch                  # List branches
git checkout -b <branch>    # Create & switch branch
git merge <branch>          # Merge branch
git log --oneline           # View history
git diff                    # View changes
git stash                   # Stash changes
git remote -v               # View remotes
```

### Workflow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                        REMOTE REPOSITORY                     │
│                    (GitHub / GitLab / Azure)                  │
└────────────────────────┬───────────────┬────────────────────┘
                         │               ▲
                    git fetch/        git push
                    git pull             │
                         │               │
                         ▼               │
┌─────────────────────────────────────────────────────────────┐
│                      LOCAL REPOSITORY                         │
│                        (.git folder)                          │
└────────────────────────┬───────────────┬────────────────────┘
                         │               ▲
                    git checkout/     git commit
                    git reset            │
                         │               │
                         ▼               │
┌─────────────────────────────────────────────────────────────┐
│                       STAGING AREA                            │
│                         (Index)                               │
└────────────────────────┬───────────────┬────────────────────┘
                         │               ▲
                   git checkout/      git add
                   git restore           │
                         │               │
                         ▼               │
┌─────────────────────────────────────────────────────────────┐
│                    WORKING DIRECTORY                          │
│                    (Your actual files)                        │
└─────────────────────────────────────────────────────────────┘
```

### Git Command Categories

| Category | Commands |
|----------|----------|
| **Setup** | `init`, `clone`, `config` |
| **Snapshot** | `add`, `commit`, `status`, `diff`, `stash`, `reset` |
| **Branch** | `branch`, `checkout`, `switch`, `merge`, `rebase` |
| **Share** | `remote`, `fetch`, `pull`, `push` |
| **Inspect** | `log`, `show`, `blame`, `diff` |
| **Undo** | `reset`, `revert`, `restore`, `checkout` |

---

## Tips & Best Practices

1. **Commit often** — Small, focused commits are easier to understand and revert
2. **Write clear messages** — Future you will thank present you
3. **Pull before push** — Always get latest changes before pushing
4. **Use branches** — Never work directly on `main`
5. **Don't commit secrets** — Use `.gitignore` and environment variables
6. **Review before commit** — Use `git diff --staged` to review what you're committing
7. **Use `.gitignore` early** — Set it up at the start of your project
8. **Don't force push shared branches** — It rewrites history for everyone

---

*Happy coding! 🚀*
