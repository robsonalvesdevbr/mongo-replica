# MongoDB Replica Set with Docker Compose

```bash
docker exec -it mongo1 mongosh
mongosh mongodb://localhost:27017,localhost:27018,localhost:27019/?replicaSet=rs0
```

```javascript
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "mongo1:27017" },
    { _id: 1, host: "mongo2:27017" },
    { _id: 2, host: "mongo3:27017" },
  ],
});

rs.status();
```
