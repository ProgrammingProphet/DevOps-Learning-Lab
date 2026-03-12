# MySQL Advanced DevOps Reference

This document contains advanced operational practices used in production environments with MySQL. These topics are typically handled by backend engineers, DevOps engineers, and database administrators.

Topics included:

* MySQL Replication (Master–Replica Setup)
* Query Optimization Techniques
* Connection Pooling with Node.js
* Slow Query Log Debugging
* Database Migration Strategy

---

# 1. MySQL Replication (Master–Replica Setup)

Replication is used to improve scalability and reliability.

Benefits:

* Read scaling
* Backup safety
* High availability

Architecture:

Master → Handles writes
Replica → Handles reads

Example Docker Compose setup:

```yaml
version: '3.9'

services:

  mysql-master:
    image: mysql:8
    container_name: mysql-master
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
    ports:
      - "3306:3306"

  mysql-replica:
    image: mysql:8
    container_name: mysql-replica
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
    ports:
      - "3307:3306"
```

Master configuration (`my.cnf`):

```
server-id=1
log_bin=mysql-bin
```

Replica configuration:

```
server-id=2
relay-log=mysql-relay-bin
```

Start replication:

```sql
CHANGE MASTER TO
MASTER_HOST='mysql-master',
MASTER_USER='replica_user',
MASTER_PASSWORD='password',
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=  4;

START SLAVE;
```

Check status:

```sql
SHOW SLAVE STATUS\G
```

---

# 2. Query Optimization Techniques (Production)

Slow queries are one of the biggest performance bottlenecks.

### Use EXPLAIN

```sql
EXPLAIN SELECT * FROM users WHERE email='test@example.com';
```

This shows:

* index usage
* scanned rows
* join strategy

---

### Avoid SELECT *

Bad:

```sql
SELECT * FROM users;
```

Better:

```sql
SELECT id,name,email FROM users;
```

---

### Use Indexes

```sql
CREATE INDEX idx_email ON users(email);
```

---

### Pagination for large data

```sql
SELECT * FROM users LIMIT 50 OFFSET 0;
```

---

# 3. Connection Pooling with Node.js

Creating a new DB connection for every request is inefficient.

Use connection pooling.

Example using `mysql2`:

```javascript
const mysql = require('mysql2');

const pool = mysql.createPool({
  host: 'mysql',
  user: 'root',
  password: 'password',
  database: 'devconnect',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});

module.exports = pool;
```

Benefits:

* reuse connections
* prevent DB overload
* faster query response

---

# 4. MySQL Slow Query Log Debugging

Enable slow query logging.

Inside MySQL:

```sql
SET GLOBAL slow_query_log = 'ON';
```

Set threshold (seconds):

```sql
SET GLOBAL long_query_time = 2;
```

Check location:

```sql
SHOW VARIABLES LIKE 'slow_query_log_file';
```

Typical analysis tools:

* mysqldumpslow
* pt-query-digest

Example:

```bash
mysqldumpslow /var/log/mysql/mysql-slow.log
```

---

# 5. Database Migration Strategy

Schema changes should not be done manually in production.

Use migration tools.

Common tools:

* Flyway
* Liquibase

---

## Flyway Example

Install Flyway and create migration script.

Migration file:

```
V1__create_users_table.sql
```

Example migration:

```sql
CREATE TABLE users (
 id INT PRIMARY KEY AUTO_INCREMENT,
 name VARCHAR(100),
 email VARCHAR(100)
);
```

Run migration:

```bash
flyway migrate
```

---

## Liquibase Example

Liquibase uses XML or YAML change logs.

Example:

```xml
<changeSet id="1" author="dev">

<createTable tableName="users">

<column name="id" type="INT" autoIncrement="true">
<constraints primaryKey="true"/>
</column>

<column name="name" type="VARCHAR(100)"/>

</createTable>

</changeSet>
```

Run migration:

```bash
liquibase update
```

---

# DevOps Best Practices

1. Never run schema changes manually in production
2. Always version control database migrations
3. Monitor slow queries regularly
4. Use read replicas for scaling
5. Always use connection pooling

---

# Notes

This document focuses on advanced MySQL operational practices commonly used in production infrastructure and large-scale backend systems.