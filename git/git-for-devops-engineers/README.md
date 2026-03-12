For your **DevOps knowledge repository**, the `git/` section should focus on **Git from a DevOps engineer’s operational perspective**, not just developer usage. This means workflows, CI/CD integration, automation, troubleshooting, and production practices.


# Version Control (Git) for DevOps Engineers

Git is the most widely used **version control system** in modern software development and DevOps workflows.
DevOps engineers rely on Git not only for managing code but also for **infrastructure, CI/CD pipelines, configuration management, and GitOps practices**.

Understanding Git deeply allows DevOps engineers to **automate deployments, manage infrastructure as code, and collaborate effectively with development teams.**

---

# Why Git is Important in DevOps

Git is the backbone of many DevOps practices:

| DevOps Area            | Role of Git                      |
| ---------------------- | -------------------------------- |
| CI/CD                  | Pipeline triggers from commits   |
| Infrastructure as Code | Terraform, Ansible stored in Git |
| GitOps                 | Kubernetes deployments via Git   |
| Collaboration          | Pull requests & code reviews     |
| Rollbacks              | Reverting broken deployments     |

Example workflow:

```
Developer → Git Commit → CI/CD Pipeline → Build → Deploy
```

---

# What is Version Control?

Version control is a system that **tracks changes in files over time**.

Benefits:

* Track history of changes
* Collaborate with teams
* Rollback to previous versions
* Prevent overwriting work
* Manage large projects

Git is a **distributed version control system (DVCS)**.

This means:

Every developer has a **complete copy of the repository**.

---

# Git Architecture

Important components:

```
Working Directory
      ↓
Staging Area
      ↓
Repository
```

### Working Directory

The files you modify on your system.

### Staging Area

Where changes are prepared before committing.

### Repository

Where Git permanently stores version history.

---

# Basic Git Workflow

Typical workflow:

```
git clone
git add
git commit
git push
```

Example:

```bash
git clone https://github.com/user/project.git
cd project
git add .
git commit -m "Added new feature"
git push origin main
```

---

# Important Git Commands

## Initialize Repository

```bash
git init
```

Creates a new Git repository.

---

## Clone Repository

```bash
git clone https://github.com/user/project.git
```

Downloads an existing repository.

---

## Check Status

```bash
git status
```

Shows modified, staged, and untracked files.

---

## Add Files

Stage files before commit.

```bash
git add file.txt
```

Add all files:

```bash
git add .
```

---

## Commit Changes

```bash
git commit -m "Fix login bug"
```

Creates a snapshot of changes.

---

## Push Changes

```bash
git push origin main
```

Uploads commits to remote repository.

---

## Pull Latest Changes

```bash
git pull origin main
```

Fetch and merge changes from remote.

---

# Git Branching

Branches allow developers to work on features independently.

Example:

```
main
 ├── feature-login
 ├── feature-dashboard
 └── bugfix-auth
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

Or modern syntax:

```bash
git switch feature-auth
```

---

## Create and Switch Branch

```bash
git checkout -b feature-auth
```

---

# Merge Branch

After development is completed:

```bash
git checkout main
git merge feature-auth
```

---

# Git Rebase (Important for DevOps)

Rebase creates a **clean commit history**.

Example:

```bash
git rebase main
```

Benefits:

* Cleaner commit history
* Easier debugging
* Better CI/CD tracking

---

# Git Stash

Temporarily save uncommitted work.

```bash
git stash
```

Restore later:

```bash
git stash pop
```

Useful when switching branches quickly.

---

# Git Tags

Tags mark **release versions**.

Example:

```bash
git tag v1.0
git push origin v1.0
```

Used in CI/CD for:

* Production releases
* Deployment triggers

---

# Git Logs

View commit history.

```bash
git log
```

Better format:

```bash
git log --oneline --graph --decorate
```

Example:

```
a34b21c Fix login bug
b12c33a Add dashboard
98ac12d Initial commit
```

---

# Git Reset vs Revert

## Git Reset

Moves branch pointer backwards.

```bash
git reset --hard HEAD~1
```

Dangerous because it rewrites history.

---

## Git Revert

Creates a new commit that undoes changes.

```bash
git revert commit-id
```

Safer for shared repositories.

---

# Git Remote Repositories

Common platforms:

* GitHub
* GitLab
* Bitbucket
* Azure DevOps

Check remote:

```bash
git remote -v
```

Add remote:

```bash
git remote add origin https://github.com/user/repo.git
```

---

# Git Workflows Used in DevOps

## GitHub Flow

Simplest workflow.

```
main → feature branch → pull request → merge
```

Best for:

* Continuous deployment
* SaaS products

---

## GitFlow

Structured workflow.

Branches:

```
main
develop
feature/*
release/*
hotfix/*
```

Used in large enterprise systems.

---

# Git in CI/CD

Most CI/CD pipelines trigger on Git events.

Example triggers:

| Event        | Pipeline Action   |
| ------------ | ----------------- |
| push         | Run build         |
| pull request | Run tests         |
| tag          | Deploy release    |
| merge        | Deploy to staging |

Example:

```
Git Commit → GitHub Actions → Build Docker Image → Deploy
```

---

# Git for Infrastructure as Code

DevOps engineers store infrastructure code in Git.

Example repositories:

```
terraform/
ansible/
kubernetes/
helm/
```

Benefits:

* Infrastructure version control
* Audit trail
* Rollback capability
* Automated deployments

---

# GitOps (Important DevOps Concept)

GitOps means **Git is the single source of truth for infrastructure**.

Workflow:

```
Developer → Git Commit
        ↓
Git Repository
        ↓
GitOps Operator (ArgoCD / Flux)
        ↓
Kubernetes Cluster
```

Changes are deployed automatically.

---

# Useful Git Commands for DevOps Engineers

### Check Differences

```bash
git diff
```

---

### Remove File from Git

```bash
git rm file.txt
```

---

### Rename Branch

```bash
git branch -m new-name
```

---

### Delete Branch

```bash
git branch -d branch-name
```

---

### Force Push (Use Carefully)

```bash
git push --force
```

---

# Git Troubleshooting

## Fix Detached HEAD

```bash
git checkout main
```

---

## Undo Last Commit

Keep changes:

```bash
git reset --soft HEAD~1
```

Remove changes:

```bash
git reset --hard HEAD~1
```

---

## Resolve Merge Conflict

Example conflict:

```
<<<<<<< HEAD
Code from main
=======
Code from feature
>>>>>>> feature
```

Steps:

1. Edit file
2. Remove conflict markers
3. Commit changes

```bash
git add .
git commit
```

---

# Best Practices for DevOps Engineers

Use meaningful commit messages.

Example:

Good:

```
fix: resolve nginx container startup issue
```

Bad:

```
fix stuff
```

---

Keep commits small and focused.

---

Protect main branches.

Use:

* Pull requests
* Code reviews
* CI checks

---

Avoid force pushing to shared branches.

---

Use tags for production releases.

---

# DevOps Git Skills Roadmap

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
* rebasing

Advanced:

* Git workflows
* GitOps
* CI/CD integration
* release management
* monorepo strategies

---

# Conclusion

Git is one of the most critical tools for DevOps engineers. It enables collaboration, automation, infrastructure management, and deployment workflows.

Mastering Git helps DevOps engineers:

* automate CI/CD pipelines
* manage infrastructure as code
* enable GitOps workflows
* maintain reliable production systems


