# MySQL Quick Reference (Docker + DevOps Workflow)

This README is a practical cheat sheet for connecting to MySQL running in Docker, performing CRUD operations, and debugging database issues during development and production environments.

---

# 1. Check Running Containers

```bash
docker ps
```

Example:

```
CONTAINER ID   IMAGE        NAME
12ab34cd56ef   mysql:8      devconnect-mysql
```

---

# 2. Enter MySQL Container

```bash
docker exec -it devconnect-mysql bash
```

Login to MySQL shell:

```bash
mysql -u root -p
```

---

# 3. Login Directly from Host

If port is exposed (`3306:3306`):

```bash
mysql -h 127.0.0.1 -P 3306 -u root -p
```

---

# 4. Basic Database Navigation

Show databases:

```sql
SHOW DATABASES;
```

Select database:

```sql
USE devconnect;
```

Show tables:

```sql
SHOW TABLES;
```

Describe table structure:

```sql
DESCRIBE users;
```

---

# 5. CRUD Operations

## Create Table

```sql
CREATE TABLE users (
 id INT AUTO_INCREMENT PRIMARY KEY,
 name VARCHAR(100),
 email VARCHAR(100),
 role VARCHAR(50)
);
```

Insert data:

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

Filter records:

```sql
SELECT * FROM users WHERE role='admin';
```

Count records:

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

Example configuration:

```yaml
version: "3.9"

services:
  mysql:
    image: mysql:8
    container_name: mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: strongpassword
      MYSQL_DATABASE: devconnect
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql

  backend:
    build: .
    container_name: backend
    depends_on:
      - mysql
    environment:
      DB_HOST: mysql
      DB_USER: root
      DB_PASSWORD: strongpassword
      DB_NAME: devconnect

volumes:
  mysql-data:
```

Best practices:

* Use Docker volumes for persistence
* Avoid exposing MySQL publicly in production
* Store credentials in environment variables or secrets

---

# 8. Backup & Restore

## Backup Database

```bash
mysqldump -u root -p devconnect > backup.sql
```

Backup all databases:

```bash
mysqldump -u root -p --all-databases > alldb.sql
```

Backup from Docker container:

```bash
docker exec devconnect-mysql mysqldump -u root -p devconnect > backup.sql
```

---

## Restore Database

```bash
mysql -u root -p devconnect < backup.sql
```

Restore inside container:

```bash
docker exec -i devconnect-mysql mysql -u root -p devconnect < backup.sql
```

---

# 9. Performance Debugging Queries

Show running queries:

```sql
SHOW PROCESSLIST;
```

Check database status:

```sql
SHOW STATUS;
```

Analyze query performance:

```sql
EXPLAIN SELECT * FROM users WHERE email='test@example.com';
```

Check table size:

```sql
SELECT table_name, table_rows
FROM information_schema.tables
WHERE table_schema='devconnect';
```

---

# 10. Common Backend Bugs (Node.js + MySQL)

## 1. Connection refused

Common causes:

* MySQL container not running
* incorrect host

Correct connection example:

```
host: mysql
port: 3306
user: root
password: strongpassword
```

---

## 2. Too many connections

Check active connections:

```sql
SHOW PROCESSLIST;
```

Fix:

* increase connection limit
* use connection pooling

---

## 3. Data not inserting

Possible causes:

* missing commit
* schema mismatch

Debug:

```sql
SELECT * FROM users;
```

---

# 11. Indexing for Faster APIs

Create index:

```sql
CREATE INDEX idx_email
ON users(email);
```

Unique index:

```sql
CREATE UNIQUE INDEX idx_unique_email
ON users(email);
```

Check indexes:

```sql
SHOW INDEX FROM users;
```

Remove index:

```sql
DROP INDEX idx_email ON users;
```

---

# 12. DevOps Monitoring Commands

Check container logs:

```bash
docker logs devconnect-mysql
```

Check container resource usage:

```bash
docker stats
```

Check disk usage inside container:

```bash
docker exec -it devconnect-mysql df -h
```

---

# Notes

This file serves as a quick operational reference for MySQL debugging, development workflows, and DevOps operations in containerized environments.