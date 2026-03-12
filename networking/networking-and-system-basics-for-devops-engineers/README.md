# Networking & System Basics for DevOps Engineers

Networking and system fundamentals are essential for DevOps engineers. Every modern infrastructure component—containers, Kubernetes clusters, CI/CD pipelines, cloud services—relies on networking and operating system behavior.

Understanding these concepts helps engineers **debug production issues, design scalable systems, and maintain secure infrastructure.**

---

# Why Networking Matters in DevOps

DevOps engineers regularly work with:

* Kubernetes clusters
* Docker containers
* Load balancers
* Reverse proxies
* Cloud infrastructure
* CI/CD pipelines
* Microservices communication

Almost every production issue eventually relates to:

* DNS resolution
* Network latency
* Firewall rules
* TLS certificates
* Port conflicts
* Load balancing
* Service discovery

Without networking fundamentals, troubleshooting becomes extremely difficult.

---

# Core Networking Concepts

## IP Address

Every machine connected to a network has an **IP address**.

Example:

```
192.168.1.10
```

Two main types:

### Private IP

Used inside internal networks.

Examples:

```
192.168.x.x
10.x.x.x
172.16.x.x – 172.31.x.x
```

### Public IP

Accessible from the internet.

Example:

```
34.125.82.101
```

---

## Ports

Ports identify which **application** is receiving traffic on a machine.

Examples:

| Port | Service    |
| ---- | ---------- |
| 22   | SSH        |
| 80   | HTTP       |
| 443  | HTTPS      |
| 3306 | MySQL      |
| 5432 | PostgreSQL |
| 6379 | Redis      |

Example:

```
http://server-ip:3000
```

---

## TCP vs UDP

### TCP (Transmission Control Protocol)

Reliable communication.

Features:

* Connection based
* Error checking
* Guaranteed delivery

Used by:

* HTTP
* HTTPS
* SSH
* MySQL

---

### UDP (User Datagram Protocol)

Faster but less reliable.

Used by:

* DNS
* Video streaming
* Gaming
* VoIP

---

# DNS (Domain Name System)

DNS converts **domain names → IP addresses**.

Example:

```
google.com → 142.250.183.206
```

Common DNS tools:

```
nslookup google.com
dig google.com
```

---

# HTTP vs HTTPS

## HTTP

Standard web communication protocol.

Example:

```
http://example.com
```

Port:

```
80
```

---

## HTTPS

Secure version of HTTP.

Uses:

* TLS encryption
* Certificates

Example:

```
https://example.com
```

Port:

```
443
```

---

# Load Balancing

Load balancing distributes traffic across multiple servers.

Example architecture:

```
Users
  |
Load Balancer
  |
|--------|--------|
Server1  Server2  Server3
```

Benefits:

* High availability
* Fault tolerance
* Scalability

Common tools:

* Nginx
* HAProxy
* AWS ALB
* Traefik

---

# Reverse Proxy

A reverse proxy sits between users and backend servers.

Example:

```
User → Nginx → Application Server
```

Responsibilities:

* SSL termination
* Load balancing
* Caching
* Security filtering

Example:

```
Nginx
Traefik
Envoy
```

---

# NAT (Network Address Translation)

NAT allows **private networks to access the internet** using one public IP.

Example:

```
Private server → NAT Gateway → Internet
```

Common in:

* Cloud infrastructure
* Kubernetes clusters
* Home routers

---

# Firewall

A firewall controls incoming and outgoing traffic.

Example rules:

Allow:

```
Port 22 (SSH)
Port 80 (HTTP)
Port 443 (HTTPS)
```

Block:

```
All other ports
```

Linux firewall tools:

```
ufw
iptables
firewalld
```

---

# SSH (Secure Shell)

SSH allows secure remote server access.

Example:

```
ssh user@server-ip
```

Common DevOps usage:

* Server administration
* Git operations
* Automation scripts
* CI/CD deployment

---

# Important Networking Commands

### Check IP Address

```
ip a
```

or

```
ifconfig
```

---

### Check Open Ports

```
netstat -tulpn
```

or

```
ss -tulpn
```

---

### Test Network Connectivity

```
ping google.com
```

---

### Check DNS

```
dig google.com
```

---

### Test HTTP Request

```
curl http://example.com
```

---

### Trace Network Route

```
traceroute google.com
```

---

# DevOps Networking Examples

## Example 1: Debugging Container Networking

```
docker exec -it container bash
ping database
```

Check if service discovery works.

---

## Example 2: Checking Kubernetes Service

```
kubectl get svc
```

Test connectivity:

```
curl service-name:port
```

---

## Example 3: Debugging Production Server

Check running ports:

```
ss -tulpn
```

Check firewall:

```
ufw status
```

---

# System Basics Every DevOps Engineer Must Know

## Processes

Check running processes:

```
ps aux
```

Kill process:

```
kill -9 PID
```

---

## Disk Usage

Check disk space:

```
df -h
```

Check folder size:

```
du -sh *
```

---

## Memory Usage

```
free -m
```

---

## System Monitoring

```
top
```

or

```
htop
```

---

# Networking in Modern DevOps Stack

Networking is used in:

| Technology    | Networking Role         |
| ------------- | ----------------------- |
| Docker        | Container networking    |
| Kubernetes    | Service networking      |
| CI/CD         | Deployment connectivity |
| Cloud         | VPC networking          |
| Microservices | Service communication   |

---

# Real Production Example

A production request flow:

```
User
 |
DNS
 |
Load Balancer
 |
Reverse Proxy (Nginx)
 |
Application (Node.js)
 |
Database (PostgreSQL)
 |
Cache (Redis)
```

Each layer relies on networking.

---

# Recommended Tools for DevOps Networking

Essential tools:

```
curl
wget
dig
nslookup
netstat
ss
tcpdump
nmap
traceroute
ping
```

Advanced debugging:

```
wireshark
iftop
mtr
```

---

# DevOps Networking Skills Roadmap

Beginner:

* IP
* Ports
* DNS
* HTTP/HTTPS

Intermediate:

* Load balancers
* Reverse proxies
* TLS
* Firewall rules

Advanced:

* Kubernetes networking
* Service mesh
* Network observability
* Zero trust networking

---

# Conclusion

Networking knowledge is one of the most important skills for DevOps engineers. It enables faster troubleshooting, better infrastructure design, and improved system reliability.

Every DevOps engineer should be comfortable diagnosing:

* DNS issues
* Port conflicts
* Network latency
* Service connectivity
* Firewall blocks
* TLS problems

Mastering networking fundamentals makes it significantly easier to work with modern DevOps technologies like Docker, Kubernetes, and cloud platforms.
