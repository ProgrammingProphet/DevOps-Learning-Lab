# PostgreSQL Quick Reference (Docker + DevOps Workflow)

This README is a practical cheat sheet for connecting to PostgreSQL running in Docker, performing CRUD operations, and debugging database issues during development and DevOps workflows.

---

# 1. Check Running Containers

```bash
docker ps
```

Example:

```
CONTAINER ID   IMAGE          NAME
ab12cd34ef56   postgres:16    devconnect-postgres
```

---

# 2. Enter PostgreSQL Container

```bash
docker exec -it devconnect-postgres bash
```

Open PostgreSQL CLI:

```bash
psql -U postgres
```

If a database name is required:

```bash
psql -U postgres -d devconnect
```

---

# 3. Connect Directly From Host

If port is exposed (`5432:5432`):

```bash
psql -h localhost -p 5432 -U postgres -d devconnect
```

---

# 4. Basic Database Navigation

List databases:

```sql
\l
```

Connect to database:

```sql
\c devconnect
```

Show tables:

```sql
\dt
```

Describe table:

```sql
\d users
```

Show current database:

```sql
SELECT current_database();
```

---

# 5. CRUD Operations

## Create Table

```sql
CREATE TABLE users (
 id SERIAL PRIMARY KEY,
 name VARCHAR(100),
 email VARCHAR(100),
 role VARCHAR(50)
);
```

Insert record:

```sql
INSERT INTO users (name,email,role)
VALUES ('Aditya','aditya@example.com','developer');
```

Insert multiple rows:

```sql
INSERT INTO users (name,email,role)
VALUES
('Rahul','rahul@example.com','admin'),
('Priya','priya@example.com','user');
```

---

## Read Data

Fetch all records:

```sql
SELECT * FROM users;
```

Filter data:

```sql
SELECT * FROM users WHERE role='admin';
```

Count rows:

```sql
SELECT COUNT(*) FROM users;
```

---

## Update Data

```sql
UPDATE users
SET role='DevOps Engineer'
WHERE name='Aditya';
```

---

## Delete Data

Delete one record:

```sql
DELETE FROM users WHERE name='Rahul';
```

Delete all records:

```sql
DELETE FROM users;
```

---

# 6. Database Maintenance

Drop table:

```sql
DROP TABLE users;
```

Drop database:

```sql
DROP DATABASE devconnect;
```

---

# 7. Docker Compose Production Setup

Example `docker-compose.yml`:

```yaml
version: "3.9"

services:

  postgres:
    image: postgres:16
    container_name: postgres
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: strongpassword
      POSTGRES_DB: devconnect
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data

  backend:
    build: .
    container_name: backend
    depends_on:
      - postgres
    environment:
      DB_HOST: postgres
      DB_PORT: 5432
      DB_USER: postgres
      DB_PASSWORD: strongpassword
      DB_NAME: devconnect

volumes:
  postgres-data:
```

Best practices:

* Always use Docker volumes for persistence
* Avoid exposing port `5432` publicly in production
* Use internal service name (`postgres`) for connections

---

# 8. Backup & Restore

## Backup Database

```bash
pg_dump -U postgres devconnect > backup.sql
```

Backup all databases:

```bash
pg_dumpall -U postgres > alldb.sql
```

Backup from Docker container:

```bash
docker exec devconnect-postgres pg_dump -U postgres devconnect > backup.sql
```

---

## Restore Database

```bash
psql -U postgres devconnect < backup.sql
```

Restore from container:

```bash
docker exec -i devconnect-postgres psql -U postgres devconnect < backup.sql
```

---

# 9. Performance Debugging Queries

Check active queries:

```sql
SELECT * FROM pg_stat_activity;
```

Check database size:

```sql
SELECT pg_size_pretty(pg_database_size('devconnect'));
```

Analyze query plan:

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email='test@example.com';
```

Check table size:

```sql
SELECT pg_size_pretty(pg_total_relation_size('users'));
```

---

# 10. Common Backend Bugs (Node.js + PostgreSQL)

## 1. Connection refused

Common causes:

* container not running
* wrong host or port

Correct example connection:

```
host: postgres
port: 5432
user: postgres
password: strongpassword
database: devconnect
```

---

## 2. Too many connections

Check connections:

```sql
SELECT * FROM pg_stat_activity;
```

Solutions:

* enable connection pooling
* increase max_connections

---

## 3. Data not inserting

Possible causes:

* transaction not committed
* schema mismatch

Debug:

```sql
SELECT * FROM users;
```

---

# 11. Indexing for Faster APIs

Create index:

```sql
CREATE INDEX idx_users_email
ON users(email);
```

Unique index:

```sql
CREATE UNIQUE INDEX idx_unique_email
ON users(email);
```

Check indexes:

```sql
\di
```

Remove index:

```sql
DROP INDEX idx_users_email;
```

---

# 12. DevOps Monitoring Commands

Check container logs:

```bash
docker logs devconnect-postgres
```

Check container resource usage:

```bash
docker stats
```

Check disk usage inside container:

```bash
docker exec -it devconnect-postgres df -h
```

---

# Notes

This file serves as a quick operational reference for PostgreSQL debugging, development workflows, and DevOps operations in containerized environments.
