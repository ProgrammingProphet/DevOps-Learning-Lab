This guide connects with your other sections such as **Kubernetes, CI/CD, Microservices, and GitOps**, since Docker is the foundation for containerized applications.

---

# Docker for DevOps Engineers

## Introduction

Docker is an open-source platform used to **build, package, and run applications inside containers**.

Containers allow applications to run consistently across different environments by packaging:

* application code
* runtime
* dependencies
* system libraries

into a single container image.

Example workflow:

```
Application Code → Docker Image → Docker Container
```

Docker enables developers and DevOps engineers to **build once and run anywhere**.

---

# Why Docker Is Important for DevOps

Modern software systems rely heavily on containerization.

Docker helps DevOps teams:

* standardize environments
* simplify application deployment
* improve portability
* support microservices architectures
* enable CI/CD automation

Docker is commonly used with:

* Kubernetes
* CI/CD pipelines
* microservices
* cloud-native applications

---

# Virtual Machines vs Containers

Traditional virtualization uses **virtual machines**.

```
Host OS
 │
 ├── VM 1
 ├── VM 2
 └── VM 3
```

Each VM includes a full operating system.

Containers share the host OS kernel:

```
Host OS
 │
 ├── Container 1
 ├── Container 2
 └── Container 3
```

Benefits of containers:

* faster startup
* lower resource usage
* easier deployment

---

# Docker Architecture

Docker uses a client-server architecture.

```
Docker Client
     │
     ▼
Docker Daemon
     │
     ▼
Docker Containers
```

### Docker Client

The Docker client is the command-line interface used to interact with Docker.

Example command:

```
docker run nginx
```

---

### Docker Daemon

The Docker daemon manages Docker objects such as:

* containers
* images
* networks
* volumes

It performs tasks like:

* building images
* running containers
* managing resources

---

### Docker Registry

Docker images are stored in **registries**.

Common registries include:

* Docker Hub
* GitHub Container Registry
* AWS ECR
* Google Container Registry

Example image:

```
nginx:latest
```

---

# Docker Images

A Docker image is a **read-only template used to create containers**.

Images contain:

* application code
* dependencies
* runtime environment

Example image:

```
node:18
nginx:latest
python:3.11
```

Images are built using a **Dockerfile**.

---

# Docker Containers

A container is a **running instance of a Docker image**.

Example:

```
Docker Image → Docker Container
```

Example command:

```
docker run -d nginx
```

Options explained:

```
-d → run container in background
```

---

# Dockerfile

A Dockerfile defines how to build a Docker image.

Example Dockerfile:

```dockerfile
FROM node:18

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["node", "app.js"]
```

Explanation:

```
FROM → base image
WORKDIR → working directory
COPY → copy files into container
RUN → execute command
CMD → default container command
```

---

# Building Docker Images

Build a Docker image using the Dockerfile.

Example command:

```
docker build -t my-app .
```

Options:

```
-t → tag name for image
.  → build context
```

Verify image:

```
docker images
```

---

# Running Containers

Run a container from an image.

Example command:

```
docker run -p 8080:80 nginx
```

Explanation:

```
-p → port mapping
8080 → host port
80 → container port
```

Access application:

```
http://localhost:8080
```

---

# Managing Containers

List running containers:

```
docker ps
```

List all containers:

```
docker ps -a
```

Stop container:

```
docker stop container_id
```

Remove container:

```
docker rm container_id
```

---

# Docker Volumes

Containers are ephemeral, meaning data is lost when containers stop.

Docker volumes provide persistent storage.

Example:

```
docker run -v mydata:/data nginx
```

This creates a persistent volume.

Use cases:

* database storage
* application logs
* shared files

---

# Docker Networking

Docker allows containers to communicate through networks.

Example:

```
docker network create my-network
```

Run container in network:

```
docker run --network my-network nginx
```

This allows containers to communicate with each other.

---

# Docker Compose

Docker Compose allows running **multi-container applications**.

Example architecture:

```
Web App
 │
 ├── Backend Service
 └── Database
```

Example `docker-compose.yml`:

```yaml
version: "3"

services:
  web:
    image: nginx
    ports:
      - "8080:80"

  redis:
    image: redis
```

Run services:

```
docker compose up
```

This starts both containers together.

---

# Docker in CI/CD Pipelines

Docker is widely used in CI/CD pipelines.

Example pipeline:

```
Code Commit
   │
   ▼
Build Docker Image
   │
   ▼
Run Tests
   │
   ▼
Push Image to Registry
   │
   ▼
Deploy to Kubernetes
```

This ensures **consistent deployments across environments**.

---

# Docker Best Practices

Use **small base images**.

Example:

```
alpine
```

Reduce image size with **multi-stage builds**.

Example:

```dockerfile
FROM node:18 AS builder
RUN npm install

FROM node:18-alpine
COPY --from=builder /app /app
```

Additional best practices:

* avoid running containers as root
* use `.dockerignore`
* keep images minimal

---

# Example DevOps Deployment Architecture

Example containerized system:

```
Client
 │
 ▼
API Gateway
 │
 ▼
Docker Containers
 │
 ├── User Service
 ├── Order Service
 └── Payment Service
```

Containers may run inside:

```
Docker Hosts
or
Kubernetes Cluster
```

---

# Docker Monitoring

Containers should be monitored to track performance.

Common monitoring tools:

* Prometheus
* Grafana
* cAdvisor

Metrics include:

* CPU usage
* memory usage
* container restarts

---

# Docker Security

Security best practices include:

* scanning images for vulnerabilities
* limiting container privileges
* using secure base images
* storing secrets securely

Tools used:

* Trivy
* Snyk
* Docker Scout

---

# Docker Workflow for DevOps

Typical DevOps workflow:

```
Developer
   │
   ▼
Write Dockerfile
   │
   ▼
Build Docker Image
   │
   ▼
Push to Container Registry
   │
   ▼
Deploy with Kubernetes
```

Docker acts as the **packaging layer for applications**.

---

# Docker Section in This Repository

This repository contains multiple DevOps guides.

```
docker/
│
└── docker-for-devops-engineers
```

This section complements other topics including:

```
kubernetes/
ci-cd/
gitops/
architecture/
observability/
security/
messaging/
databases/
caching/
```

Together these guides create a **complete DevOps engineering knowledge base**.

