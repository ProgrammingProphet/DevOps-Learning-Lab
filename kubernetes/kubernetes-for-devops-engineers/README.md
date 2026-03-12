Below is the **Kubernetes guide** for your DevOps knowledge repository.

Repository path:

```text
kubernetes/kubernetes-for-devops-engineers/README.md
```

This README connects naturally with your previous sections:

* Docker
* CI/CD
* GitOps
* Microservices
* Observability

---

# Kubernetes for DevOps Engineers

## Introduction

Kubernetes (K8s) is an **open-source container orchestration platform** used to automate deployment, scaling, and management of containerized applications.

Modern applications are often packaged as **Docker containers**, but managing large numbers of containers manually becomes difficult.

Kubernetes solves this by providing a platform to:

* deploy containers
* manage application scaling
* handle failures automatically
* perform rolling updates
* manage networking and storage

Kubernetes is widely used by companies such as:

* Google
* Netflix
* Spotify
* Airbnb

---

# Why DevOps Engineers Must Learn Kubernetes

DevOps engineers manage the infrastructure that runs containerized applications.

Kubernetes helps DevOps teams:

* automate deployments
* scale applications dynamically
* ensure high availability
* manage microservices architectures
* operate cloud-native systems

Kubernetes is now considered a **core skill for DevOps engineers**.

---

# Kubernetes Architecture

Kubernetes uses a **cluster-based architecture**.

```
Kubernetes Cluster
│
├── Control Plane
└── Worker Nodes
```

The control plane manages the cluster, while worker nodes run applications.

---

# Control Plane Components

The **control plane** is responsible for managing the Kubernetes cluster.

### API Server

The API Server is the **entry point for all Kubernetes operations**.

All commands such as:

```
kubectl apply
kubectl get pods
```

communicate with the API server.

---

### etcd

etcd is a **distributed key-value store** used to store cluster state.

Examples of stored data:

* cluster configuration
* running pods
* service information

etcd acts as the **database of Kubernetes**.

---

### Scheduler

The scheduler decides **which node should run a pod**.

It considers factors such as:

* CPU availability
* memory resources
* node constraints

---

### Controller Manager

Controllers continuously monitor cluster state and ensure the system reaches the desired state.

Examples:

* replica controller
* node controller
* deployment controller

---

# Worker Nodes

Worker nodes are machines that **run application containers**.

Each node includes the following components:

```
Worker Node
│
├── Kubelet
├── Container Runtime
└── Kube Proxy
```

---

### Kubelet

The kubelet communicates with the control plane and ensures containers are running correctly.

It performs tasks such as:

* starting containers
* monitoring container health
* reporting node status

---

### Container Runtime

The container runtime runs containers.

Common runtimes:

* containerd
* CRI-O
* Docker (older setups)

---

### Kube Proxy

Kube Proxy manages **network communication between services and pods**.

It ensures traffic is routed correctly inside the cluster.

---

# Kubernetes Objects

Kubernetes manages applications using **objects**.

Common objects include:

* Pods
* Deployments
* Services
* ConfigMaps
* Secrets

These objects are usually defined in **YAML files**.

---

# Pods

A **Pod** is the smallest deployable unit in Kubernetes.

A pod can contain one or more containers.

Example pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

Pods run inside worker nodes.

---

# Deployments

Deployments manage pod replicas and updates.

Example deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: web
          image: nginx
```

Benefits:

* automatic scaling
* rolling updates
* self-healing pods

---

# Services

Pods have dynamic IP addresses, so Kubernetes provides **services** for stable networking.

Example service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
```

Types of services:

* ClusterIP
* NodePort
* LoadBalancer

---

# ConfigMaps

ConfigMaps store **non-sensitive configuration data**.

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  database_url: mysql:3306
```

Applications can read these configurations at runtime.

---

# Secrets

Secrets store **sensitive data** such as:

* API keys
* passwords
* certificates

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: cGFzc3dvcmQ=
```

Secrets should always be handled securely.

---

# Scaling Applications

Kubernetes allows applications to scale automatically.

Example:

```
Deployment
│
├── Pod 1
├── Pod 2
├── Pod 3
```

Scaling command:

```
kubectl scale deployment web-app --replicas=5
```

---

# Horizontal Pod Autoscaler (HPA)

HPA automatically scales pods based on metrics.

Example:

```
CPU usage > 70% → scale pods
```

Example command:

```
kubectl autoscale deployment web-app --cpu-percent=70 --min=2 --max=10
```

---

# Rolling Updates

Kubernetes supports **zero-downtime deployments**.

Example update flow:

```
Old Pods → Gradually replaced → New Pods
```

Command example:

```
kubectl rollout restart deployment web-app
```

This ensures applications stay available during updates.

---

# Kubernetes Networking

Kubernetes provides networking between pods and services.

Networking features include:

* pod-to-pod communication
* service discovery
* load balancing

Example request flow:

```
Client
 │
 ▼
Service
 │
 ▼
Pods
```

---

# Kubernetes Storage

Applications often need persistent storage.

Kubernetes provides **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)**.

Example:

```
Application → PVC → Persistent Volume → Storage
```

Storage backends include:

* cloud storage
* network storage
* local storage

---

# Observability in Kubernetes

Monitoring Kubernetes clusters is essential.

Typical monitoring stack:

```
Kubernetes Cluster
│
├── Prometheus
├── Grafana
├── Loki
└── Jaeger
```

Metrics monitored include:

* pod CPU usage
* memory usage
* request latency
* container restarts

---

# Kubernetes Deployment Workflow

Example DevOps workflow:

```
Developer
   │
   ▼
Git Repository
   │
   ▼
CI Pipeline
   │
   ▼
Docker Image Build
   │
   ▼
Push Image to Registry
   │
   ▼
GitOps Deployment
   │
   ▼
Kubernetes Cluster
```

This integrates Kubernetes with **CI/CD and GitOps workflows**.

---

# Kubernetes Best Practices

Use **declarative configurations (YAML)**.

Separate environments (dev, staging, production).

Implement resource limits for containers.

Use health checks for services.

Monitor cluster performance continuously.

---

# Example Production Architecture

Example modern cloud-native architecture:

```
Internet
   │
   ▼
API Gateway
   │
   ▼
Kubernetes Cluster
│
├── Microservices
├── Message Broker (Kafka)
├── Redis Cache
├── Databases
│
└── Observability Stack
```

This architecture supports **high scalability and reliability**.

---

# Kubernetes Section in This Repository

This repository includes guides for DevOps engineers.

```
kubernetes/
│
└── kubernetes-for-devops-engineers
```

This section complements other topics:

```
architecture/
ci-cd/
gitops/
security/
observability/
messaging/
databases/
caching/
```

Together these sections form a **complete DevOps engineering knowledge base**.

---
