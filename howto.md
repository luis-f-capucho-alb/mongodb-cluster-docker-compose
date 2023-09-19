MongoDB (6.0.1) Sharded Cluster with Docker Compose
=========================================

```bash
docker-compose up -d
```
Run these command one by one:

```bash
docker-compose exec configsvr01 sh -c "mongosh < /scriptsall/init-configserver.js"
```

```bash
docker-compose exec shard01-a sh -c "mongosh < /scriptsall/init-shard01.js"
```
```bash
docker-compose exec shard02-a sh -c "mongosh < /scriptsall/init-shard02.js"
```
```bash
docker-compose exec shard03-a sh -c "mongosh < /scriptsall/init-shard03.js"
```

>Note: Wait a bit for the config server and shards to elect their primaries before initializing the router

```bash
docker-compose exec router01 sh -c "mongosh < /scriptsall/init-router.js"
```

### 👉Enable sharding and setup sharding-key 
```bash
docker-compose exec router01 mongosh --port 27017

// Enable sharding for database `celllock`
sh.enableSharding("celllock")

// Setup shardingKey for collection `cells`**
db.adminCommand( { shardCollection: "celllock.cells", key: { oemNumber: "hashed", zipCode: 1, supplierId: 1 } } )

```
---
### ✔️ Done !!!
#### But before you start inserting data you should verify them first

Btw, here is mongodb connection string if you want to try to connect mongodb cluster with MongoDB Compass from your host computer (which is running docker)

```
mongodb://127.0.0.1:27117,127.0.0.1:27118
```
