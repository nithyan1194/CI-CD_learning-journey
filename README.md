# DevOps & CI/CD Fundamentals: Study Guide

A beginner-friendly reference covering core Version Control (Git) fundamentals, branching workflows, and Continuous Integration & Continuous Delivery (CI/CD) pipeline architecture.

---

## 1. High-Level CI/CD Concepts

* **Continuous Integration (CI):** Regularly merging code changes into a central repository where automated builds and test suites run to detect errors early.
* **Continuous Delivery (CD):** Maintaining code in a deployable state so that any passing build can be safely released to production at any time via a manual trigger.
* **Continuous Deployment (CD):** Automatically deploying every passing, verified build directly to production environments without human intervention.

---

## 2. Git Architecture & The Three Stages

Git tracks and manages code across three main states:

* **Working Directory:** The local folder where you create and edit your files.
* **Staging Area (Index):** The checkpoint where modified files are staged before being permanently committed.
* **Repository (`.git` directory):** The permanent local history containing your committed snapshots.

---

## 3. Git Command Quick Reference

### Status & History
* `git init` — Initialize a new Git repository locally.
* `git status` — Inspect modified, untracked, and staged files.
* `git log --oneline` — View a clean, condensed commit history.

### Tracking & Committing
* `git add <file>` — Stage a specific file.
* `git add .` — Stage all modifications in the current directory.
* `git commit -m "<message>"` — Record staged changes permanently with a descriptive message.

### Undoing & Stashing
* `git restore <file>` — Discard unstaged local changes in a file.
* `git restore --staged <file>` — Unstage a file while preserving local edits in your working directory.
* `git stash` — Temporarily save uncommitted local changes.
* `git stash pop` — Restore the most recently stashed changes.
* `git revert <commit-hash>` — Safely invert an existing commit by creating a new inverse commit.

### Remote Synchronization & Branches
* `git clone <url>` — Download an entire remote repository locally.
* `git switch -c <branch-name>` — Create and switch to a new branch.
* `git switch <branch-name>` — Switch to an existing branch.
* `git push origin <branch-name>` — Upload local branch commits to the remote repository.
* `git pull origin <branch-name>` — Fetch and merge remote changes into the current branch.

### Ignoring Files (`.gitignore`)
Used to prevent untracked, sensitive, or generated files from entering version control:
```gitignore
# Secrets and environment variables
.env

# Dependencies
node_modules/
__pycache__/

# Build outputs
dist/
build/


## 4. Standard Feature Branch & Pull Request (PR) WorkflowTo add a new feature safely without modifying main directly:

# 1. Update your local main branch
git switch main
git pull origin main

# 2. Create and switch to a feature branch
git switch -c feature/navbar

# 3. Stage and commit your changes
git add navbar.html styles.css
git commit -m "Add navigation bar"

# 4. Push feature branch to remote repository
git push -u origin feature/navbar

Pull Request (PR): Open a PR on GitHub/GitLab (Base: main $\leftarrow$ Compare: feature/navbar), trigger automated CI checks, complete code reviews, and merge.

## 5. CI/CD Architecture & GitHub Actions
Workflow Triggers
Pipelines are executed automatically when remote events occur (e.g., push, pull_request).

Structure of a Pipeline File (.github/workflows/ci.yml)
on: Defines the triggering events.

jobs: Contains one or more jobs running on isolated virtual environments (e.g., runs-on: ubuntu-latest).

steps: Sequential tasks inside a job that run marketplace actions (uses) or terminal commands (run).

Exit Codes: CI runners determine success or failure via exit codes (0 = Success/Pass, Non-Zero = Failure/Block merge).

Example Workflow Configuration:

name: CI Pipeline

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run automated tests
        run: |
          pytest



