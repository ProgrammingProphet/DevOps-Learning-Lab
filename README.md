# 🚀 Docker Networking, Security & Image Optimization – DevOps Learning Lab

## 📌 Overview

This project documents core DevOps concepts learned while building a multi-container application using:

* Docker
* Docker Compose
* Nginx Reverse Proxy
* Multi-stage builds
* Distroless images
* Container security best practices

The goal was to deeply understand:

* Container networking
* Image optimization
* Security hardening
* Production-ready Docker practices

---

# 🧱 Architecture

## 🏗 Application Setup

We built a simple multi-container app:

* **Frontend container** (Static HTML via Nginx)
* **Backend container** (Node.js + Express API)
* **Nginx reverse proxy container**
* **Custom Docker bridge network**

### Traffic Flow

```
Browser → Nginx (port 80)
               ↓
        Docker Internal Network
               ↓
     Frontend Container (port 80)
               ↓
     Backend Container (port 5000)
```

Only Nginx exposes port `80` to the host.

---

# 🌐 Docker Networking Concepts

## Default Behavior

If no network is defined in `docker-compose.yml`, Docker automatically creates:

```
<project_name>_default
```

This is a **custom bridge network**, not the global default bridge.

---

## Custom Network Definition

```yaml
networks:
  devops_network:
    driver: bridge
```

Each service is attached explicitly:

```yaml
networks:
  - devops_network
```

### Why Use Custom Network?

* Clear architecture
* Better isolation
* Professional production setup
* Easier debugging
* Enables multi-network segmentation

---

# 📦 Dockerfile Concepts

## Single Stage Build

```dockerfile
FROM node:18
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

### Important Optimizations

* Copy `package.json` first for layer caching
* Install dependencies before copying source code
* Reduce unnecessary rebuilds

---

# 🚀 Multi-Stage Build

Used to reduce final image size.

```dockerfile
# Stage 1 - Builder
FROM node:18 AS builder
WORKDIR /app
COPY package.json ./
RUN npm install --production
COPY . .

# Stage 2 - Production
FROM node:18-slim
WORKDIR /app
COPY --from=builder /app /app
EXPOSE 5000
CMD ["node", "server.js"]
```

### Benefits

* Smaller final image
* No build dependencies in production
* Cleaner runtime image
* Better CI/CD performance

---

# 🐧 node:18 vs node:18-alpine vs Distroless

| Image          | Size               | Compatibility        | Security |
| -------------- | ------------------ | -------------------- | -------- |
| node:18        | Large (~900MB)     | High                 | Medium   |
| node:18-alpine | Small (~150MB)     | Medium (musl issues) | Better   |
| Distroless     | Very Small (~50MB) | High (runtime-only)  | Best     |

---

# 🔐 Distroless Images

Distroless images:

* No shell
* No package manager
* No extra OS tools
* Only runtime + app

### Why It’s More Secure

* Smaller attack surface
* No bash/sh access
* Harder to install malicious tools
* Reduced CVEs

Example:

```dockerfile
FROM gcr.io/distroless/nodejs18
WORKDIR /app
COPY --from=builder /app /app
USER nonroot
EXPOSE 5000
CMD ["server.js"]
```

---

# 🔐 Container Security Best Practices

## 1️⃣ Run as Non-Root

```dockerfile
USER nonroot
```

Why?

* Follows Principle of Least Privilege
* Reduces damage in case of RCE
* Prevents privileged port binding
* Limits file system access

---

## 2️⃣ Avoid Privileged Ports (<1024)

Non-root cannot bind to:

* 80
* 443
* 22

Instead:

* Run app on 3000 or 5000
* Map host port → container port

```yaml
ports:
  - "80:3000"
```

---

## 3️⃣ Avoid Dangerous Volume Mounts

❌ Never mount:

```
/:/
```

❌ Avoid mounting:

```
/var/run/docker.sock
```

Why?

* Breaks container isolation
* Allows host takeover
* Enables privilege escalation

---

# 🔥 Security Comparison

| Setup               | Security Level |
| ------------------- | -------------- |
| Root container      | Weak           |
| Distroless root     | Medium         |
| Slim non-root       | Good           |
| Distroless non-root | Best           |

---

# ⚡ Why Smaller Images Matter

Smaller images improve:

* Faster CI builds
* Faster image push/pull
* Faster Kubernetes scaling
* Lower bandwidth costs
* Reduced attack surface

---

# 🧠 Key DevOps Concepts Learned

* Docker bridge networking
* Service name DNS resolution
* Internal vs exposed ports
* Reverse proxy architecture
* Multi-stage builds
* Layer caching
* Alpine vs Debian tradeoffs
* Distroless security model
* Privileged ports in Linux
* Least privilege principle
* Container attack surface reduction
* Volume-based security risks

---

# 🎯 Interview Talking Points

If asked:

**Why multi-stage builds?**

> Reduces image size, removes build dependencies, improves security and CI/CD performance.

**Why run containers as non-root?**

> Follows least privilege principle and reduces blast radius during compromise.

**Why not always use Alpine?**

> Alpine uses musl libc which may cause compatibility issues with native Node modules.

**What is Distroless?**

> A minimal container image containing only runtime and application, without shell or package manager, reducing attack surface.

---

# 🚀 Final Takeaway

This project was not just about running containers.

It focused on understanding:

* How Docker networking works internally
* How to design secure containers
* How image size impacts CI/CD and scaling
* How to reduce attack surface
* How to think like a DevOps/SRE engineer

---

If you want, next we can:

* Add Kubernetes security layer
* Add read-only root filesystem
* Drop Linux capabilities
* Add real production hardening checklist

You’re moving into serious DevOps territory now 🔥
