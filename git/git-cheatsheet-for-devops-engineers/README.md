This README focuses on **practical Git commands that DevOps engineers frequently use in real infrastructure, CI/CD, and production troubleshooting scenarios.**

---

# DevOps Git Cheat Sheet

Git is one of the most critical tools used by DevOps engineers for managing source code, infrastructure as code, CI/CD pipelines, and deployment workflows.

This cheat sheet provides **commonly used Git commands for real-world DevOps tasks** such as troubleshooting, automation, release management, and collaboration.

---

# Git Configuration

Configure Git identity.

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Check configuration:

```bash
git config --list
```

---

# Repository Setup

## Initialize a Repository

```bash
git init
```

Creates a new Git repository.

---

## Clone a Repository

```bash
git clone https://github.com/user/repo.git
```

Clone with specific branch:

```bash
git clone -b branch-name repo-url
```

---

# Checking Repository Status

## Check File Status

```bash
git status
```

Shows modified, staged, and untracked files.

---

## View Commit History

```bash
git log
```

Short format:

```bash
git log --oneline
```

Graph view:

```bash
git log --oneline --graph --decorate
```

---

# Staging Changes

## Add File

```bash
git add file.txt
```

Add multiple files:

```bash
git add file1 file2
```

Add all files:

```bash
git add .
```

---

# Commit Changes

Create commit:

```bash
git commit -m "Add login feature"
```

Commit staged changes:

```bash
git commit
```

Commit including staged and modified files:

```bash
git commit -am "Fix bug"
```

---

# Branch Management

## List Branches

```bash
git branch
```

Remote branches:

```bash
git branch -r
```

All branches:

```bash
git branch -a
```

---

## Create Branch

```bash
git branch feature-auth
```

---

## Switch Branch

```bash
git checkout feature-auth
```

Modern command:

```bash
git switch feature-auth
```

---

## Create and Switch Branch

```bash
git checkout -b feature-auth
```

---

## Delete Branch

```bash
git branch -d feature-auth
```

Force delete:

```bash
git branch -D feature-auth
```

---

# Remote Repository Management

## Check Remote

```bash
git remote -v
```

---

## Add Remote

```bash
git remote add origin https://github.com/user/repo.git
```

---

## Remove Remote

```bash
git remote remove origin
```

---

## Change Remote URL

```bash
git remote set-url origin new-repository-url
```

---

# Push & Pull Operations

## Push to Remote

```bash
git push origin main
```

Push branch:

```bash
git push origin feature-auth
```

---

## Pull Changes

```bash
git pull origin main
```

Fetch only:

```bash
git fetch
```

---

# Merge Operations

## Merge Branch

```bash
git checkout main
git merge feature-auth
```

---

## Abort Merge

```bash
git merge --abort
```

---

# Rebase Operations

Rebase current branch onto main.

```bash
git rebase main
```

Interactive rebase:

```bash
git rebase -i HEAD~5
```

Abort rebase:

```bash
git rebase --abort
```

---

# Git Stash (Temporary Save)

Save current work:

```bash
git stash
```

List stash entries:

```bash
git stash list
```

Apply stash:

```bash
git stash apply
```

Apply and remove stash:

```bash
git stash pop
```

Delete stash:

```bash
git stash drop
```

---

# Undo Changes

## Undo Modified File

```bash
git checkout -- file.txt
```

Modern syntax:

```bash
git restore file.txt
```

---

## Unstage File

```bash
git reset file.txt
```

---

## Reset Last Commit

Soft reset (keep changes):

```bash
git reset --soft HEAD~1
```

Hard reset (remove changes):

```bash
git reset --hard HEAD~1
```

---

# Reverting Changes

Safer alternative to reset.

```bash
git revert commit-id
```

Creates a new commit reversing changes.

---

# Tags (Release Management)

Create tag:

```bash
git tag v1.0
```

Push tag:

```bash
git push origin v1.0
```

List tags:

```bash
git tag
```

Delete tag:

```bash
git tag -d v1.0
```

---

# Viewing Differences

## Compare Changes

```bash
git diff
```

Compare staged changes:

```bash
git diff --staged
```

Compare branches:

```bash
git diff branch1 branch2
```

---

# Git Cleanup Commands

Remove untracked files:

```bash
git clean -f
```

Remove directories:

```bash
git clean -fd
```

Preview cleanup:

```bash
git clean -n
```

---

# DevOps Production Troubleshooting Commands

## Show Who Changed a Line

```bash
git blame file.txt
```

---

## Show Specific Commit

```bash
git show commit-id
```

---

## Find When a Bug Was Introduced

```bash
git bisect start
git bisect bad
git bisect good commit-id
```

---

# Git Ignore

Ignore files by adding to `.gitignore`.

Example:

```
node_modules
.env
*.log
dist/
build/
```

---

# Useful DevOps Git Aliases

Add shortcut commands.

Example:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.cm commit
git config --global alias.br branch
```

Usage:

```bash
git st
git co main
```

---

# Git in CI/CD Pipelines

Most pipelines trigger from Git events.

Examples:

| Event        | Action            |
| ------------ | ----------------- |
| push         | Start build       |
| pull request | Run tests         |
| merge        | Deploy to staging |
| tag          | Deploy production |

Example workflow:

```
Developer Commit
      ↓
Git Repository
      ↓
CI/CD Pipeline
      ↓
Build Docker Image
      ↓
Deploy Infrastructure
```

---

# Git Best Practices for DevOps Engineers

Use clear commit messages.

Example:

```
feat: add docker health check
fix: resolve nginx container crash
```

---

Keep commits small and atomic.

---

Protect main branch using pull requests.

---

Avoid force pushing to shared branches.

---

Use tags for releases.

---

Store infrastructure as code in Git.

Examples:

```
terraform/
kubernetes/
helm/
ansible/
```

---

# Essential Git Skills for DevOps Engineers

Beginner:

* clone
* add
* commit
* push
* pull

Intermediate:

* branching
* merging
* stash
* tags
* reset

Advanced:

* rebase
* bisect
* GitOps workflows
* CI/CD integration
* release strategies

---

# Conclusion

Git is a fundamental tool for DevOps engineers. It enables collaboration, automation, infrastructure management, and reliable deployment workflows.

Mastering Git commands and workflows helps engineers:

* debug production issues
* manage infrastructure as code
* automate deployments
* maintain clean version history

