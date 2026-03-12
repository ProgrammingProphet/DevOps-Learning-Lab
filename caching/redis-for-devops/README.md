# Redis for DevOps Engineers

This README provides a practical operational reference for Redis in development and production environments. It focuses on how DevOps engineers use Redis for caching, performance optimization, session management, and distributed systems.

---

# 1. What is Redis

Redis (Remote Dictionary Server) is an in-memory data store commonly used as:

* Cache
* Message broker
* Session store
* Real-time analytics engine

Key characteristics:

* Extremely fast (in-memory)
* Key–value data model
* Supports multiple data structures
* Widely used in high-performance systems

---

# 2. Common Redis Use Cases

Redis is typically used for:

1. Application caching
2. Session storage
3. Rate limiting
4. Queue systems
5. Real-time analytics
6. Pub/Sub messaging

Example architecture:

Client → API → Redis Cache → Database

---

# 3. Running Redis with Docker

Pull Redis image:

```bash
docker pull redis:7
```

Run Redis container:

```bash
docker run -d \
  --name redis-server \
  -p 6379:6379 \
  redis:7
```

Check container:

```bash
docker ps
```

---

# 4. Connect to Redis CLI

Enter container:

```bash
docker exec -it redis-server bash
```

Start Redis CLI:

```bash
redis-cli
```

Or connect directly:

```bash
redis-cli -h localhost -p 6379
```

---

# 5. Basic Redis Commands

Set value:

```bash
SET user:1 "Aditya"
```

Get value:

```bash
GET user:1
```

Delete key:

```bash
DEL user:1
```

Check key existence:

```bash
EXISTS user:1
```

List keys:

```bash
KEYS *
```

---

# 6. Working with Data Structures

Redis supports multiple data types.

## Strings

```bash
SET name "DevOps"
GET name
```

## Lists

```bash
LPUSH queue "job1"
LPUSH queue "job2"
LRANGE queue 0 -1
```

## Hashes

```bash
HSET user:1 name "Aditya"
HSET user:1 role "DevOps"
HGETALL user:1
```

## Sets

```bash
SADD tags devops docker kubernetes
SMEMBERS tags
```

---

# 7. Redis Expiration (Caching)

Set key with expiration:

```bash
SET session:123 "user_data" EX 3600
```

Check TTL:

```bash
TTL session:123
```

This is commonly used for:

* sessions
* caching API responses

---

# 8. Docker Compose Production Setup

Example configuration:

```yaml
version: "3.9"

services:

  redis:
    image: redis:7
    container_name: redis
    restart: always
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data

  backend:
    build: .
    container_name: backend
    depends_on:
      - redis
    environment:
      REDIS_HOST: redis
      REDIS_PORT: 6379

volumes:
  redis-data:
```

Best practices:

* use persistent volumes
* restrict public access
* use internal container networking

---

# 9. Caching Strategy

Typical caching workflow:

1. Client requests data
2. Application checks Redis cache
3. If cache hit → return cached data
4. If cache miss → query database
5. Store result in Redis

Pseudo workflow:

```
if data in redis:
    return cache
else:
    data = query database
    store in redis
    return data
```

Benefits:

* reduces database load
* faster response time
* improves scalability

---

# 10. Rate Limiting with Redis

Redis is commonly used to prevent API abuse.

Example logic:

```bash
INCR api:user123
EXPIRE api:user123 60
```

Meaning:

* count requests per minute
* block if limit exceeded

---

# 11. Redis as Message Queue

Redis lists can act as a simple queue.

Producer:

```bash
LPUSH jobs "email_task"
```

Consumer:

```bash
BRPOP jobs 0
```

Used for:

* background jobs
* task processing

---

# 12. Monitoring Redis

Check Redis info:

```bash
INFO
```

Check memory usage:

```bash
INFO memory
```

Check connected clients:

```bash
CLIENT LIST
```

---

# 13. Debugging Redis in Docker

Check container logs:

```bash
docker logs redis-server
```

Check resource usage:

```bash
docker stats
```

Check memory usage inside Redis:

```bash
INFO memory
```

---

# 14. Common Production Problems

## Cache stampede

Many requests hit the database simultaneously.

Solution:

* use cache locking
* stagger cache expiration

---

## Memory overflow

Redis runs out of RAM.

Solutions:

* configure eviction policy
* increase memory

Example configuration:

```
maxmemory 1gb
maxmemory-policy allkeys-lru
```

---

# 15. DevOps Best Practices

1. Never expose Redis publicly
2. Use authentication
3. Monitor memory usage
4. Set eviction policies
5. Use Redis primarily as cache

---

# 16. Typical DevOps Architecture

Client

↓

API Server

↓

Redis Cache

↓

Primary Database

↓

Read Replicas

Monitoring stack:

* Prometheus
* Grafana

---

# Notes

This document provides a practical Redis operational guide for DevOps engineers focusing on caching, system performance, and scalable backend architecture.