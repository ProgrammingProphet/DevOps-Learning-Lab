This guide is critical because **Linux is the foundation of almost all DevOps infrastructure**, including Docker, Kubernetes, CI/CD servers, and cloud systems.

---

# Linux for DevOps Engineers

## Introduction

Linux is an open-source operating system widely used to run servers, cloud infrastructure, and container platforms.

Most DevOps tools and platforms such as:

* Docker
* Kubernetes
* Jenkins
* Git
* Nginx
* Apache

run on Linux systems.

Because of this, Linux knowledge is considered **one of the most essential skills for DevOps engineers**.

Linux provides powerful tools for:

* system administration
* automation
* networking
* process management
* infrastructure monitoring

---

# Why DevOps Engineers Must Learn Linux

DevOps engineers frequently interact with Linux systems when managing infrastructure.

Common tasks include:

* configuring servers
* managing processes
* troubleshooting performance issues
* monitoring system resources
* configuring networking
* managing file permissions

Linux is used extensively in:

* cloud platforms (AWS, GCP, Azure)
* container platforms (Docker, Kubernetes)
* CI/CD systems
* production servers

---

# Linux File System Structure

Linux uses a hierarchical file system structure.

Example structure:

```
/
├── bin
├── etc
├── home
├── var
├── usr
└── tmp
```

Important directories:

| Directory | Purpose                     |
| --------- | --------------------------- |
| `/`       | Root directory              |
| `/home`   | User home directories       |
| `/etc`    | System configuration files  |
| `/var`    | Log files and variable data |
| `/usr`    | User applications           |
| `/tmp`    | Temporary files             |

Understanding this structure is essential for system administration.

---

# Basic Linux Commands

## File and Directory Commands

List files:

```
ls
```

List files with details:

```
ls -l
```

Change directory:

```
cd /path/to/directory
```

Show current directory:

```
pwd
```

Create directory:

```
mkdir myfolder
```

Remove directory:

```
rm -r myfolder
```

---

# File Management

Create file:

```
touch file.txt
```

Copy file:

```
cp source.txt destination.txt
```

Move or rename file:

```
mv old.txt new.txt
```

Delete file:

```
rm file.txt
```

View file contents:

```
cat file.txt
```

View large file:

```
less file.txt
```

---

# File Permissions

Linux uses permissions to control access to files.

Example permission:

```
-rwxr-xr--
```

Permission breakdown:

| Symbol | Meaning |
| ------ | ------- |
| r      | Read    |
| w      | Write   |
| x      | Execute |

User categories:

* Owner
* Group
* Others

Example permission change:

```
chmod 755 script.sh
```

Example ownership change:

```
chown user:group file.txt
```

Permissions are critical for **security and system management**.

---

# Process Management

Linux allows monitoring and controlling running processes.

List processes:

```
ps aux
```

Real-time process monitoring:

```
top
```

Enhanced monitoring:

```
htop
```

Kill a process:

```
kill PID
```

Force kill:

```
kill -9 PID
```

Process management is important for diagnosing **performance issues and crashes**.

---

# System Monitoring

DevOps engineers frequently monitor system performance.

Check CPU and memory usage:

```
top
```

Check disk usage:

```
df -h
```

Check directory size:

```
du -sh folder
```

View system uptime:

```
uptime
```

View system logs:

```
journalctl
```

Monitoring helps identify **resource bottlenecks**.

---

# Networking Commands

Networking is critical for managing servers.

Check IP address:

```
ip a
```

Test connectivity:

```
ping google.com
```

Check open ports:

```
netstat -tuln
```

Show listening services:

```
ss -tuln
```

Check DNS resolution:

```
nslookup example.com
```

Networking commands help troubleshoot **service connectivity issues**.

---

# Package Management

Linux distributions use package managers to install software.

Examples:

| Distribution | Package Manager |
| ------------ | --------------- |
| Ubuntu       | apt             |
| CentOS       | yum             |
| Fedora       | dnf             |

Example install command:

```
sudo apt install nginx
```

Update packages:

```
sudo apt update
sudo apt upgrade
```

Package managers simplify software installation and updates.

---

# User Management

Linux supports multiple users.

Create user:

```
sudo useradd username
```

Set password:

```
sudo passwd username
```

Delete user:

```
sudo userdel username
```

Add user to group:

```
sudo usermod -aG group username
```

User management is essential for **access control and security**.

---

# Log Management

Linux stores logs that help diagnose system problems.

Common log directory:

```
/var/log
```

Example logs:

```
/var/log/syslog
/var/log/auth.log
```

View logs:

```
tail -f /var/log/syslog
```

Logs help identify:

* application errors
* system failures
* security events

---

# Shell and Bash Scripting

Linux allows automation using shell scripts.

Example script:

```
#!/bin/bash

echo "Hello DevOps"
date
```

Run script:

```
bash script.sh
```

Shell scripts automate tasks such as:

* deployments
* backups
* monitoring

Automation is a key DevOps practice.

---

# SSH (Secure Shell)

SSH allows secure remote access to servers.

Example connection:

```
ssh user@server-ip
```

SSH is commonly used for:

* server management
* infrastructure configuration
* automation scripts

SSH key authentication improves security.

---

# Linux in DevOps Infrastructure

Linux servers run many DevOps tools.

Example infrastructure:

```
Linux Server
│
├── Docker
├── Kubernetes
├── Nginx
├── CI/CD Tools
└── Monitoring Tools
```

Linux acts as the **foundation of cloud-native infrastructure**.

---

# Linux Automation

DevOps engineers automate tasks using tools such as:

* Bash scripts
* Cron jobs
* Ansible
* Terraform

Example cron job:

```
0 2 * * * backup.sh
```

This runs a script every day at 2 AM.

Automation reduces manual work and improves reliability.

---

# Example DevOps Server Setup

Example Linux-based infrastructure:

```
Cloud Server (Linux)
│
├── Docker Engine
├── Kubernetes Node
├── Nginx Reverse Proxy
├── Monitoring Stack
└── CI/CD Runner
```

Linux provides the platform for running **production infrastructure**.

---

# Linux Best Practices

Use SSH keys instead of passwords.

Keep systems updated regularly.

Monitor system resources continuously.

Limit root access.

Use automation tools for repetitive tasks.

Secure servers with firewalls and permissions.

---

# Linux Section in This Repository

This repository includes multiple DevOps topics.

```
linux/
│
└── linux-for-devops-engineers
```

This complements other sections such as:

```
docker/
kubernetes/
ci-cd/
gitops/
security/
observability/
architecture/
messaging/
databases/
caching/
```

Together these guides provide a **complete DevOps learning roadmap**.
