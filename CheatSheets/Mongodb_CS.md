 

# Connect DB

```bash
# Basic connection
mongosh "mongodb://localhost:27017"
mongosh --host localhost --port 27017

# With authentication
mongosh "mongodb://username:password@localhost:27017/database"

# Specify host and port
mongosh --host localhost --port 27017

# Connect to specific database
mongosh "mongodb://localhost:27017/mydatabase"

# With SSL/TLS (if configured)
mongosh "mongodb://localhost:27017/?tls=true"
```


# Check databases 

```powershell

# show dbs
show dbs
show databases

# View database stats
db.stats()

# Select database 
use database_name



# 
```


# Collections

```powershell

# List collections
show collections

# count 
db.collectionName.countDocuments()

# Collection quick look
db.collection.find() # finds all
db.collectionName.findOne() # finds one
db.collection.find().limit(5) # finds five


```