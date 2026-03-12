# MongoDB Quick Reference (Docker + DevOps Workflow)

This README is a personal cheat sheet for connecting to MongoDB running in Docker, performing CRUD operations, and debugging database issues during development.

---

# 1. Check Running Containers

```bash
docker ps
```

Example:

```
CONTAINER ID   IMAGE     NAME
979bf1d0898d   mongo:6   devconnect-mongodb
```

---

# 2. Enter MongoDB Container

Using container name:

```bash
docker exec -it devconnect-mongodb bash
```

Or directly open Mongo shell:

```bash
docker exec -it devconnect-mongodb mongosh
```

---

# 3. Login With Authentication (If Enabled)

```bash
mongosh -u <username> -p <password> --authenticationDatabase admin
```

Example:

```bash
mongosh -u root -p rootpassword --authenticationDatabase admin
```

---

# 4. Basic MongoDB Navigation

Show databases:

```javascript
show dbs
```

Select database:

```javascript
use devconnect
```

Check current database:

```javascript
db
```

Show collections:

```javascript
show collections
```

---

# 5. CRUD Operations

## Create

Insert one document:

```javascript
db.users.insertOne({
  name: "Aditya",
  email: "aditya@example.com",
  role: "developer"
})
```

Insert multiple documents:

```javascript
db.users.insertMany([
 {name:"Rahul",role:"admin"},
 {name:"Priya",role:"user"}
])
```

---

## Read

Fetch all documents:

```javascript
db.users.find()
```

Pretty output:

```javascript
db.users.find().pretty()
```

Find specific record:

```javascript
db.users.find({name:"Aditya"})
```

Find single document:

```javascript
db.users.findOne({name:"Rahul"})
```

Count documents:

```javascript
db.users.countDocuments()
```

---

## Update

Update one record:

```javascript
db.users.updateOne(
 {name:"Aditya"},
 {$set:{role:"DevOps Engineer"}}
)
```

Update multiple records:

```javascript
db.users.updateMany(
 {role:"user"},
 {$set:{role:"member"}}
)
```

---

## Delete

Delete one document:

```javascript
db.users.deleteOne({name:"Rahul"})
```

Delete multiple documents:

```javascript
db.users.deleteMany({role:"member"})
```

Delete all documents:

```javascript
db.users.deleteMany({})
```

---

# 6. Database Maintenance

Drop collection:

```javascript
db.users.drop()
```

Drop database:

```javascript
db.dropDatabase()
```

---

# 7. Debugging MongoDB in Docker

Check container logs:

```bash
docker logs devconnect-mongodb
```

Check container details:

```bash
docker inspect devconnect-mongodb
```

Check environment variables:

```
MONGO_INITDB_ROOT_USERNAME
MONGO_INITDB_ROOT_PASSWORD
```

---

# 8. Connect From Host Machine

If port is exposed:

```bash
mongosh mongodb://localhost:27017
```

With authentication:

```bash
mongosh mongodb://username:password@localhost:27017
```

---

# 9. Useful Debugging Queries

Check data:

```javascript
db.users.find().pretty()
```

Check indexes:

```javascript
db.users.getIndexes()
```

Check collection stats:

```javascript
db.users.stats()
```

---

# 10. Quick DevOps Debug Workflow

1. Check container

```bash
docker ps
```

2. Check logs

```bash
docker logs devconnect-mongodb
```

3. Enter MongoDB

```bash
docker exec -it devconnect-mongodb mongosh
```

4. Verify database

```javascript
show dbs
```

5. Verify data

```javascript
db.users.find().pretty()
```

---

# 11. MongoDB Docker Compose Production Setup

Example `docker-compose.yml` for a typical Node.js + MongoDB stack:

```yaml
version: "3.9"

services:
  mongodb:
    image: mongo:6
    container_name: mongodb
    restart: always
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: strongpassword
    volumes:
      - mongo-data:/data/db

  backend:
    build: .
    container_name: backend
    restart: always
    depends_on:
      - mongodb
    environment:
      MONGO_URI: mongodb://admin:strongpassword@mongodb:27017/devconnect?authSource=admin

volumes:
  mongo-data:
```

Key production practices:

* Always use **Docker volumes** to persist data
* Avoid exposing port `27017` publicly in production
* Use internal Docker network (`mongodb` service name as hostname)
* Store credentials in **environment variables or secrets**

---

# 12. Backup & Restore (Critical for Production)

## Create Backup (mongodump)

Backup database from container:

```bash
mongodump --db devconnect --out /backup
```

Backup entire MongoDB:

```bash
mongodump --out /backup
```

Backup with authentication:

```bash
mongodump -u admin -p password --authenticationDatabase admin --out /backup
```

---

## Restore Backup (mongorestore)

Restore full backup:

```bash
mongorestore /backup
```

Restore specific database:

```bash
mongorestore --db devconnect /backup/devconnect
```

Restore with authentication:

```bash
mongorestore -u admin -p password --authenticationDatabase admin /backup
```

---

# 13. Performance Debugging Queries

These commands help analyze database performance issues.

Check current operations:

```javascript
db.currentOp()
```

Check slow queries:

```javascript
db.setProfilingLevel(1)
```

Check database stats:

```javascript
db.stats()
```

Check collection stats:

```javascript
db.users.stats()
```

Check query execution plan:

```javascript
db.users.find({email:"test@example.com"}).explain("executionStats")
```

---

# 14. Common Backend Bugs (Node.js + MongoDB)

## 1. Database not connecting

Typical issue: wrong connection string.

Example correct connection string:

```javascript
mongodb://admin:password@mongodb:27017/devconnect?authSource=admin
```

Common mistakes:

* using `localhost` instead of service name inside Docker
* wrong authentication database
* missing port mapping

---

## 2. Data not saving

Possible causes:

* API not awaiting database call
* validation error in schema
* incorrect collection name

Debug query:

```javascript
db.users.find().pretty()
```

---

## 3. Duplicate data problem

Solution: create unique index

```javascript
db.users.createIndex({email:1},{unique:true})
```

---

## 4. API slow due to large collection

Cause:

* missing index

Check indexes:

```javascript
db.users.getIndexes()
```

---

# 15. MongoDB Indexing for Faster APIs

Indexes significantly improve query performance.

## Create Index

```javascript
db.users.createIndex({email:1})
```

Compound index:

```javascript
db.users.createIndex({email:1, role:1})
```

Unique index:

```javascript
db.users.createIndex({email:1},{unique:true})
```

Text search index:

```javascript
db.posts.createIndex({title:"text",content:"text"})
```

---

## Check Existing Indexes

```javascript
db.users.getIndexes()
```

---

## Remove Index

```javascript
db.users.dropIndex("email_1")
```

---

# 16. Recommended DevOps Monitoring Commands

Check container resource usage:

```bash
docker stats
```

Check Mongo container logs:

```bash
docker logs mongodb
```

Check disk usage inside container:

```bash
docker exec -it mongodb df -h
```

---

# Notes

This file is designed to help quickly debug MongoDB issues in containerized environments during backend or DevOps development. It also serves as a quick operational reference for common database tasks in production environments.