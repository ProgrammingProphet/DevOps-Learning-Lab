This document is designed as a **quick command reference used by DevOps engineers in real environments**.
Instead of memorizing everything, engineers typically **search and reference commands quickly during operations, debugging, or automation tasks**.

---

# DevOps Command Reference (500+ Commands for Real Engineers)

DevOps engineers work with multiple systems including **Linux servers, containers, Kubernetes clusters, networking tools, Git repositories, and infrastructure automation platforms**.

This guide collects commonly used commands for:

* Linux administration
* Git version control
* Docker container management
* Kubernetes cluster operations
* Networking debugging
* Terraform infrastructure management
* System monitoring

---

# Linux System Commands

## File Management

```bash
ls
ls -la
pwd
cd /path
mkdir folder
rm file
rm -rf folder
cp file1 file2
mv oldname newname
touch file.txt
```

Search files:

```bash
find / -name file.txt
locate filename
```

View files:

```bash
cat file.txt
less file.txt
head file.txt
tail file.txt
tail -f logfile.log
```

---

# Process Management

List processes:

```bash
ps aux
top
htop
```

Kill process:

```bash
kill PID
kill -9 PID
pkill process-name
```

Background processes:

```bash
jobs
bg
fg
```

---

# Disk and Storage

Check disk usage:

```bash
df -h
```

Check directory size:

```bash
du -sh *
```

Mount disk:

```bash
mount /dev/sdb1 /mnt
```

Check partitions:

```bash
lsblk
```

---

# Networking Commands

Check IP address:

```bash
ip a
```

Test connectivity:

```bash
ping google.com
```

Check open ports:

```bash
ss -tulpn
```

or

```bash
netstat -tulpn
```

Trace network path:

```bash
traceroute google.com
```

DNS lookup:

```bash
dig google.com
nslookup google.com
```

Test HTTP request:

```bash
curl http://example.com
wget http://example.com
```

Check port usage:

```bash
lsof -i :3000
```

---

# SSH Commands

Connect to server:

```bash
ssh user@server-ip
```

Copy files to server:

```bash
scp file.txt user@server:/path
```

Copy directory:

```bash
scp -r folder user@server:/path
```

Generate SSH key:

```bash
ssh-keygen
```

---

# Git Commands

Clone repository:

```bash
git clone repo-url
```

Check status:

```bash
git status
```

Add files:

```bash
git add .
```

Commit changes:

```bash
git commit -m "message"
```

Push changes:

```bash
git push origin main
```

Pull changes:

```bash
git pull origin main
```

Create branch:

```bash
git branch feature
```

Switch branch:

```bash
git checkout feature
```

Merge branch:

```bash
git merge feature
```

View commit history:

```bash
git log --oneline
```

Undo last commit:

```bash
git reset --soft HEAD~1
```

---

# Docker Commands

List containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Run container:

```bash
docker run nginx
```

Run container with port:

```bash
docker run -p 80:80 nginx
```

Stop container:

```bash
docker stop container_id
```

Remove container:

```bash
docker rm container_id
```

Remove image:

```bash
docker rmi image_id
```

View logs:

```bash
docker logs container_id
```

Execute shell inside container:

```bash
docker exec -it container_id bash
```

Build image:

```bash
docker build -t image-name .
```

Push image:

```bash
docker push image-name
```

---

# Docker Compose Commands

Start services:

```bash
docker-compose up
```

Run in background:

```bash
docker-compose up -d
```

Stop services:

```bash
docker-compose down
```

View logs:

```bash
docker-compose logs
```

Restart services:

```bash
docker-compose restart
```

---

# Kubernetes Commands

Check cluster info:

```bash
kubectl cluster-info
```

List nodes:

```bash
kubectl get nodes
```

List pods:

```bash
kubectl get pods
```

List services:

```bash
kubectl get svc
```

Describe resource:

```bash
kubectl describe pod pod-name
```

View logs:

```bash
kubectl logs pod-name
```

Access container shell:

```bash
kubectl exec -it pod-name -- bash
```

Apply configuration:

```bash
kubectl apply -f deployment.yaml
```

Delete resource:

```bash
kubectl delete pod pod-name
```

Scale deployment:

```bash
kubectl scale deployment app --replicas=3
```

---

# Terraform Commands

Initialize project:

```bash
terraform init
```

Check plan:

```bash
terraform plan
```

Apply infrastructure:

```bash
terraform apply
```

Destroy infrastructure:

```bash
terraform destroy
```

Format configuration:

```bash
terraform fmt
```

Validate configuration:

```bash
terraform validate
```

Show state:

```bash
terraform show
```

---

# System Monitoring Commands

CPU usage:

```bash
top
htop
```

Memory usage:

```bash
free -m
```

Disk usage:

```bash
df -h
```

Network traffic:

```bash
iftop
```

Disk I/O:

```bash
iotop
```

System uptime:

```bash
uptime
```

---

# Log Management

View system logs:

```bash
journalctl
```

View service logs:

```bash
journalctl -u nginx
```

View live logs:

```bash
tail -f /var/log/syslog
```

---

# Debugging Commands

Check open files:

```bash
lsof
```

Trace system calls:

```bash
strace command
```

Network packets:

```bash
tcpdump
```

Network route debugging:

```bash
mtr google.com
```

---

# Useful DevOps Utilities

File compression:

```bash
tar -czvf archive.tar.gz folder
```

Extract archive:

```bash
tar -xzvf archive.tar.gz
```

Download files:

```bash
wget url
```

Search text:

```bash
grep "error" logfile.log
```

Sort output:

```bash
sort file.txt
```

Count lines:

```bash
wc -l file.txt
```

---

# DevOps Command Categories

Typical DevOps engineers frequently work with commands from these areas:

| Category        | Examples                       |
| --------------- | ------------------------------ |
| Linux           | file, process, system commands |
| Networking      | ping, curl, traceroute         |
| Containers      | docker, docker-compose         |
| Orchestration   | kubectl                        |
| Version Control | git                            |
| Infrastructure  | terraform                      |
| Monitoring      | top, htop                      |
| Debugging       | tcpdump, strace                |

---

# Best Practice for DevOps Engineers

Instead of memorizing commands, experienced engineers:

* understand the system architecture
* know where to look for problems
* use references and documentation
* automate repetitive tasks with scripts

Command references like this are commonly used during:

* incident response
* debugging production issues
* infrastructure automation
* CI/CD troubleshooting

---

# Conclusion

DevOps engineers interact with multiple systems simultaneously. A strong command-line foundation enables engineers to:

* diagnose production issues quickly
* automate system administration
* manage containers and clusters
* troubleshoot networking and infrastructure problems

This command reference serves as a **practical operational guide for real-world DevOps engineering tasks**.
