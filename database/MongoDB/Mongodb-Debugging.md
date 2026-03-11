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

# Notes

This file is designed to help quickly debug MongoDB issues in containerized environments during backend or DevOps development.
