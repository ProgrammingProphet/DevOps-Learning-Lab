This guide includes:

* Kafka fundamentals
* Architecture
* Step-by-step Docker setup
* CLI usage
* Producer/Consumer examples
* Monitoring
* Production best practices

It is written so someone can **actually run Kafka locally and learn it step-by-step**, which is excellent for your **DevOps portfolio**.

---

# Kafka for DevOps Engineers

## Introduction

Apache Kafka is a **distributed event streaming platform** used for building real-time data pipelines and streaming applications.

Kafka is designed for:

* high throughput
* horizontal scalability
* fault tolerance
* distributed architectures

Many large systems rely on Kafka, including companies like:

* Netflix
* LinkedIn
* Uber
* Airbnb

Kafka is widely used in:

* event-driven architectures
* microservices communication
* log aggregation
* real-time analytics
* IoT data pipelines

---

# Why DevOps Engineers Must Understand Kafka

Kafka is often deployed as a **clustered infrastructure component**.

DevOps engineers are responsible for:

* deploying Kafka clusters
* managing brokers
* configuring replication
* monitoring consumer lag
* scaling partitions
* ensuring fault tolerance

Kafka is commonly integrated with:

* Kubernetes
* Docker
* Prometheus
* Grafana
* data pipelines

---

# Kafka Core Architecture

Kafka uses a **distributed log architecture**.

```
Producer → Topic → Partition → Consumer Group
```

### Producer

Application that **sends messages** to Kafka.

Examples:

* web services
* logging systems
* analytics pipelines
* IoT devices

---

### Broker

A **Kafka server** responsible for storing messages.

A Kafka cluster consists of multiple brokers.

Example:

```
Kafka Cluster
 ├─ Broker 1
 ├─ Broker 2
 └─ Broker 3
```

---

### Topic

A **category of messages**.

Example topics:

```
orders
payments
user-events
logs
```

---

### Partition

Topics are divided into **partitions for scalability**.

```
orders topic
 ├─ partition 1
 ├─ partition 2
 └─ partition 3
```

Each partition enables **parallel processing**.

---

### Consumer Group

Consumers can work together to process messages.

```
Consumer Group
 ├─ Consumer A
 ├─ Consumer B
 └─ Consumer C
```

Each consumer processes different partitions.

---

# Kafka Message Flow

```
Producer
   │
   ▼
Kafka Topic
   │
   ▼
Partitions
   │
   ▼
Consumer Group
```

Benefits:

* high scalability
* fault tolerance
* asynchronous processing

---

# Step 1 — Install Docker

Kafka is easiest to run using Docker.

Check Docker installation:

```
docker --version
```

Check Docker Compose:

```
docker compose version
```

---

# Step 2 — Create Project Directory

```
mkdir kafka-devops
cd kafka-devops
```

---

# Step 3 — Create docker-compose.yml

Create file:

```
docker-compose.yml
```

Add the following configuration.

```yaml
version: '3'

services:

  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    container_name: zookeeper
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
    ports:
      - "2181:2181"

  kafka:
    image: confluentinc/cp-kafka:latest
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```

---

# Step 4 — Start Kafka

Run:

```
docker compose up -d
```

Verify containers:

```
docker ps
```

Expected output:

```
zookeeper
kafka
```

---

# Step 5 — Access Kafka Container

```
docker exec -it kafka bash
```

Now you can run Kafka CLI tools.

---

# Step 6 — Create a Topic

Inside the container:

```
kafka-topics.sh \
--create \
--topic orders \
--bootstrap-server localhost:9092
```

Verify topic:

```
kafka-topics.sh \
--list \
--bootstrap-server localhost:9092
```

Output:

```
orders
```

---

# Step 7 — Produce Messages

Start producer:

```
kafka-console-producer.sh \
--topic orders \
--bootstrap-server localhost:9092
```

Example messages:

```
order1
order2
order3
```

---

# Step 8 — Consume Messages

Open another terminal.

Enter container again:

```
docker exec -it kafka bash
```

Run consumer:

```
kafka-console-consumer.sh \
--topic orders \
--bootstrap-server localhost:9092 \
--from-beginning
```

You will see:

```
order1
order2
order3
```

---

# Kafka Consumer Groups

Consumers can scale horizontally using **consumer groups**.

Example:

```
Consumer Group: order-workers

Consumer 1 → partition 1
Consumer 2 → partition 2
Consumer 3 → partition 3
```

Check consumer groups:

```
kafka-consumer-groups.sh \
--bootstrap-server localhost:9092 \
--list
```

---

# Kafka Persistence

Kafka stores messages **on disk**.

This allows:

* message replay
* fault tolerance
* durability

Retention policies control how long data stays.

Example:

```
7 days
24 hours
100GB limit
```

---

# Kafka Monitoring

Monitoring Kafka is critical for production systems.

Important metrics:

Consumer lag
Messages waiting to be processed.

Throughput
Messages processed per second.

Partition distribution
Load balancing across brokers.

Broker health
CPU, memory, disk usage.

---

# Monitoring Tools

Common monitoring stack:

```
Kafka → Prometheus → Grafana
```

Other tools:

* Confluent Control Center
* Burrow
* Kafka Manager

---

# Common Kafka Failure Scenarios

### Broker Failure

If a broker fails:

* partitions are reassigned
* replicas take over

Replication ensures **data availability**.

---

### Consumer Lag

Occurs when consumers process messages slower than producers.

Solution:

* scale consumers
* increase partitions
* optimize processing logic

---

### Message Duplication

Kafka provides **at-least-once delivery** by default.

Solution:

```
Design idempotent consumers
```

---

# Kafka Scaling Strategies

### Add Brokers

```
Kafka Cluster
 ├─ Broker 1
 ├─ Broker 2
 └─ Broker 3
```

Improves:

* storage
* throughput
* fault tolerance

---

### Increase Partitions

More partitions enable more parallel consumers.

---

### Scale Consumers

Add more consumers to consumer groups.

---

# Production Architecture Example

Example microservices system:

```
API Service
     │
     ▼
Kafka Topic (user-events)
     │
 ┌──────────────┬──────────────┬──────────────┐
 ▼              ▼              ▼
Email Service  Analytics     Fraud Detection
```

Benefits:

* asynchronous communication
* scalable architecture
* independent services

---

# Kafka Best Practices

Use **replication factor ≥ 3** in production.

Monitor **consumer lag continuously**.

Use **schema registry for event validation**.

Separate topics by domain.

Avoid extremely large messages.

---

# Summary

Kafka is a **core infrastructure component for modern distributed systems**.

It provides:

* high-throughput event streaming
* fault-tolerant messaging
* scalable microservice communication
* durable event storage

DevOps engineers must understand:

* Kafka cluster architecture
* topic and partition management
* monitoring and scaling
* failure recovery strategies

---

