This document focuses on **real Git issues that DevOps engineers encounter in production environments** such as CI/CD failures, broken repositories, lost commits, permission issues, and merge conflicts.

---

# Real Production Git Problems & Fixes (DevOps Guide)

In real production environments, Git issues can break deployments, CI/CD pipelines, and infrastructure automation. DevOps engineers must be able to **quickly diagnose and resolve Git problems**.

This guide covers **common Git production issues and their fixes.**

---

# 1. Detected Dubious Ownership

### Error

```
fatal: detected dubious ownership in repository
```

This usually happens when:

* Repository owner differs from current user
* Common in **Docker containers, CI/CD runners, or servers**

Example scenario:

```
repo owned by root
user running git = deploy
```

### Fix

Add safe directory:

```bash
git config --global --add safe.directory /path/to/repo
```

Example:

```bash
git config --global --add safe.directory /home/deploy/project
```

---

# 2. Detached HEAD State

### Problem

Git shows:

```
You are in 'detached HEAD' state
```

This happens when you checkout a **specific commit instead of a branch**.

Example:

```bash
git checkout 3f2c1d
```

### Fix

Return to branch:

```bash
git checkout main
```

Or create a branch:

```bash
git checkout -b new-branch
```

---

# 3. Merge Conflict

### Problem

Occurs when two branches modify the same lines.

Example conflict:

```
<<<<<<< HEAD
code from main
=======
code from feature
>>>>>>> feature
```

### Fix

1. Edit file
2. Remove conflict markers
3. Choose correct code

Then run:

```bash
git add .
git commit
```

---

# 4. Push Rejected (Non Fast Forward)

### Error

```
failed to push some refs
non-fast-forward
```

This happens when remote branch has new commits.

### Fix

Pull latest changes first:

```bash
git pull origin main
```

Then push again:

```bash
git push origin main
```

---

# 5. Accidentally Committed Sensitive Data

Example:

* API keys
* passwords
* tokens

### Fix

Remove file:

```bash
git rm --cached secret.txt
```

Commit removal:

```bash
git commit -m "remove secret"
```

Add to `.gitignore`.

---

# 6. Undo Last Commit

Keep changes:

```bash
git reset --soft HEAD~1
```

Remove commit and changes:

```bash
git reset --hard HEAD~1
```

---

# 7. Lost Commits Recovery

Sometimes commits disappear after reset or rebase.

### Fix

Use reflog:

```bash
git reflog
```

Example output:

```
a34c21 HEAD@{0}: reset: moving to HEAD~1
b52c10 HEAD@{1}: commit: add login
```

Restore commit:

```bash
git checkout b52c10
```

Or create branch:

```bash
git checkout -b recovered-branch b52c10
```

---

# 8. Force Push Recovery

Someone runs:

```bash
git push --force
```

and overwrites history.

### Recovery

Use reflog:

```bash
git reflog
```

Find lost commit and restore:

```bash
git reset --hard commit-id
```

Then push:

```bash
git push origin main --force
```

---

# 9. Large Repository Slow Clone

Problem:

```
git clone very slow
```

Cause:

* Huge repo history
* Large files

### Solution

Shallow clone:

```bash
git clone --depth 1 repo-url
```

Clone specific branch:

```bash
git clone --single-branch --branch main repo-url
```

---

# 10. Fix Corrupted Repository

Symptoms:

```
fatal: bad object
fatal: loose object corrupted
```

### Fix

Run:

```bash
git fsck
```

Clean repository:

```bash
git gc --prune=now
```

If still broken:

Re-clone repository.

---

# 11. Remove File from Git History

Example: accidentally committed large file.

Use:

```bash
git filter-branch
```

Or modern tool:

```
git filter-repo
```

Example:

```bash
git filter-repo --path secrets.txt --invert-paths
```

---

# 12. Fix Permission Issues on Server

Example error:

```
permission denied
```

Fix ownership:

```bash
sudo chown -R deploy:deploy repo-folder
```

Or correct permissions:

```bash
chmod -R 755 repo-folder
```

---

# 13. Git Pull Strategy Conflict

Error:

```
fatal: Need to specify how to reconcile divergent branches
```

### Fix

Set default strategy:

Merge:

```bash
git config pull.rebase false
```

Rebase:

```bash
git config pull.rebase true
```

---

# 14. CI/CD Pipeline Git Failures

Example errors:

```
repository not found
authentication failed
```

Common causes:

* expired tokens
* wrong SSH keys
* permission issues

### Fix

Check authentication.

Example SSH test:

```bash
ssh -T git@github.com
```

Or update token in pipeline secrets.

---

# 15. Fix .gitignore Not Working

Problem:

File already tracked before `.gitignore`.

### Fix

Remove from cache:

```bash
git rm -r --cached .
```

Then re-add files:

```bash
git add .
git commit -m "apply gitignore"
```

---

# 16. Rename Default Branch

Example:

```
master → main
```

Rename locally:

```bash
git branch -m master main
```

Push new branch:

```bash
git push origin main
```

Delete old branch:

```bash
git push origin --delete master
```

---

# 17. Delete All Local Changes

Discard everything:

```bash
git reset --hard
```

Remove untracked files:

```bash
git clean -fd
```

---

# 18. View Who Changed Code

Useful for debugging.

```bash
git blame file.txt
```

Shows:

* author
* commit
* timestamp

---

# 19. Debugging Production Bugs

Find which commit introduced bug:

```bash
git bisect start
git bisect bad
git bisect good commit-id
```

Git will automatically identify problematic commit.

---

# 20. Recover Deleted Branch

Check reflog:

```bash
git reflog
```

Restore:

```bash
git checkout -b recovered-branch commit-id
```

---

# DevOps Best Practices for Git

Protect main branch.

Use:

* Pull Requests
* Code reviews
* CI validation

---

Avoid force pushes in shared repos.

---

Store infrastructure code in Git.

Example:

```
terraform/
kubernetes/
helm/
ansible/
```

---

Use tags for releases.

Example:

```
v1.0
v1.1
v2.0
```

---

# Real DevOps Production Workflow

Example pipeline:

```
Developer Commit
       ↓
Git Repository
       ↓
CI Pipeline (GitHub Actions / Jenkins)
       ↓
Build Docker Image
       ↓
Push to Registry
       ↓
Deploy Kubernetes
```

Git is the **single source of truth**.

---

# Conclusion

Git issues frequently occur in production environments, especially in automated DevOps pipelines.

DevOps engineers must be comfortable with:

* recovering lost commits
* resolving merge conflicts
* fixing CI/CD Git failures
* managing repository history
* debugging deployment problems

Strong Git troubleshooting skills can **save hours of downtime and broken deployments**.

