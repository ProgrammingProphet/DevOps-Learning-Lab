This guide focuses on **how DevOps engineers debug problems both locally and in production systems**. It covers practical workflows, commands, and diagnostic techniques used during real incidents.

---

# Local and Production Debugging Guide for DevOps Engineers

Debugging is a core responsibility for DevOps engineers. Production systems can fail due to infrastructure issues, application bugs, networking problems, or configuration errors.

DevOps engineers must be able to **quickly diagnose problems, identify root causes, and restore system stability**.

This guide explains **systematic debugging methods for both local development environments and production infrastructure**.

---

# Debugging Philosophy

Effective debugging follows a structured approach.

### Basic Debugging Flow

```text
Problem Detected
      ↓
Collect Logs
      ↓
Check Metrics
      ↓
Identify Root Cause
      ↓
Apply Fix
      ↓
Monitor System
```

Never jump directly to fixes without understanding the root cause.

---

# Common Debugging Areas in DevOps

Production issues usually fall into these categories:

| Area           | Example Issues           |
| -------------- | ------------------------ |
| Application    | Code errors, crashes     |
| Infrastructure | Server failures          |
| Containers     | Docker container crashes |
| Kubernetes     | Pod restart loops        |
| Networking     | DNS failures             |
| Database       | Slow queries             |
| CI/CD          | Pipeline failures        |

---

# Local Debugging

Local debugging focuses on **developer machines or staging environments**.

---

# 1. Debugging Application Errors

Check application logs.

Example Node.js:

```bash
npm run dev
```

Example Java:

```bash
mvn spring-boot:run
```

Look for:

* stack traces
* error messages
* dependency failures

---

# 2. Debugging Docker Containers Locally

Check running containers:

```bash
docker ps
```

View container logs:

```bash
docker logs container_id
```

Access container shell:

```bash
docker exec -it container_id bash
```

Inspect container:

```bash
docker inspect container_id
```

---

# 3. Debugging Docker Compose

Check services:

```bash
docker-compose ps
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

# 4. Checking Local Ports

Sometimes applications fail because ports are already used.

Check open ports:

```bash
lsof -i :3000
```

or

```bash
ss -tulpn
```

---

# 5. Debugging Environment Variables

Many failures occur due to missing `.env` variables.

Check environment variables:

```bash
env
```

Check specific variable:

```bash
echo $DATABASE_URL
```

---

# Production Debugging

Production debugging focuses on **live systems running in cloud or servers**.

---

# 1. Server Health Check

First step is verifying server status.

Check CPU and memory:

```bash
top
```

or

```bash
htop
```

Check memory usage:

```bash
free -m
```

Check disk space:

```bash
df -h
```

Check folder sizes:

```bash
du -sh *
```

---

# 2. Check Running Processes

List processes:

```bash
ps aux
```

Find specific process:

```bash
ps aux | grep node
```

Kill process if necessary:

```bash
kill -9 PID
```

---

# 3. Network Debugging

Check connectivity:

```bash
ping google.com
```

Check DNS resolution:

```bash
dig google.com
```

Check route:

```bash
traceroute google.com
```

Test API endpoint:

```bash
curl http://service:3000
```

---

# 4. Debugging Logs

Logs are the **most important debugging tool**.

Common log locations:

```text
/var/log/syslog
/var/log/nginx
/var/log/application
```

View logs:

```bash
tail -f /var/log/syslog
```

View recent logs:

```bash
tail -100 logfile.log
```

---

# 5. Debugging Docker in Production

Check running containers:

```bash
docker ps
```

Check logs:

```bash
docker logs container_id
```

Inspect container:

```bash
docker inspect container_id
```

Check container resources:

```bash
docker stats
```

---

# 6. Debugging Kubernetes

Check pods:

```bash
kubectl get pods
```

Describe pod:

```bash
kubectl describe pod pod-name
```

Check logs:

```bash
kubectl logs pod-name
```

Access container shell:

```bash
kubectl exec -it pod-name -- bash
```

Check services:

```bash
kubectl get svc
```

---

# 7. Debugging Pod CrashLoopBackOff

Example error:

```text
CrashLoopBackOff
```

Steps:

1. Check logs

```bash
kubectl logs pod-name
```

2. Check pod description

```bash
kubectl describe pod pod-name
```

3. Verify configuration and environment variables.

---

# 8. Debugging CI/CD Pipelines

Check pipeline logs.

Example with GitHub Actions.

Look for errors such as:

* dependency install failure
* build errors
* authentication problems

Example debugging step:

```bash
docker build .
```

Run locally to replicate pipeline issue.

---

# 9. Database Debugging

Check database connections.

Example MySQL:

```bash
mysql -u root -p
```

Check active queries:

```sql
SHOW PROCESSLIST;
```

Check slow queries.

---

# 10. Performance Debugging

High CPU usage:

```bash
top
```

Memory issues:

```bash
free -m
```

Network usage:

```bash
iftop
```

Disk I/O:

```bash
iotop
```

---

# Observability Tools

Modern production systems rely on monitoring platforms.

Common tools:

| Tool       | Purpose             |
| ---------- | ------------------- |
| Prometheus | Metrics             |
| Grafana    | Visualization       |
| ELK Stack  | Logging             |
| Jaeger     | Distributed tracing |
| Datadog    | Monitoring          |

---

# Example Production Incident Workflow

Example situation: **API is down**

Steps:

1. Check server health

```bash
top
```

2. Check container status

```bash
docker ps
```

3. Check logs

```bash
docker logs container
```

4. Test API

```bash
curl localhost:3000
```

5. Verify database connection.

---

# DevOps Debugging Best Practices

Always collect logs before restarting services.

---

Reproduce issue locally whenever possible.

---

Use monitoring dashboards.

---

Document incidents and root causes.

---

Implement alerting systems.

---

# Common DevOps Debugging Tools

Essential tools:

```text
top
htop
curl
ping
dig
ss
netstat
lsof
docker logs
kubectl logs
```

Advanced tools:

```text
tcpdump
wireshark
iftop
mtr
strace
```

---

# Conclusion

Debugging is one of the most critical skills for DevOps engineers. Most production incidents require quick diagnosis across multiple layers including infrastructure, containers, networking, and applications.

Strong debugging skills help engineers:

* resolve incidents faster
* minimize downtime
* maintain reliable systems
* improve system observability

A systematic debugging approach ensures that DevOps engineers can **quickly identify and resolve issues in both local development environments and production systems**.
