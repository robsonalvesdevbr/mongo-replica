# MongoDB Replica Set with Docker Compose

```bash
docker exec -it mongo1 mongosh
mongosh mongodb://localhost:27017,localhost:27018,localhost:27019/?replicaSet=rs0
sudo bash -c 'echo -e "\n127.0.0.1 mongo1\n127.0.0.1 mongo2\n127.0.0.1 mongo3" >> /etc/hosts'
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

```bash
docker exec -it mongo1 mongosh --eval "rs.initiate({
 _id: \"rs0\",
 members: [
   {_id: 0, host: \"mongo1\"},
   {_id: 1, host: \"mongo2\"},
   {_id: 2, host: \"mongo3\"}
 ]
})"

docker exec -it mongo1 mongosh --eval "rs.status()"
docker stop mongo1
docker exec -it mongo2 mongosh --eval "rs.status()"

docker run --rm -v $(pwd)/dump:/dump -it --network=mongo-replica_mongo-cluster mongo mongodump --uri="mongodb://mongo1:27017,mongo2:27017,mongo3:27017/?replicaSet=rs0&authSource=admin&directConnection=true" --out /dump

docker run --rm -v $(pwd)/dump:/dump -it --network=mongo-replica_mongo-cluster mongo mongodump --uri="mongodb://mongo1:27017,mongo2:27017,mongo3:27017/?replicaSet=rs0&authSource=admin&directConnection=true" --gzip --archive=/dump/lotes.20250612.archive
```
