# demo-docker

    docker exec -it <containerId> bash

## some info
### logs
    docker logs <containerId>
    docker logs <containerId> -f
### environment variables
    docker exec <containerId> env
### clean containers & images:

    docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
    docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
    
[docker-remove-all-images-and-containers](docker-remove-all-images-and-containershttps://techoverflow.net/2013/10/22/docker-remove-all-images-and-containers/)
[to remove old and unused Docker images](https://stackoverflow.com/a/32723127/5728095)

## Maria db

    docker run --name demo-mariadb -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 -d mariadb
    
### [useful](https://hub.docker.com/r/mysql/mysql-server)

### sql (create schema|user|privelegies)
    
	CREATE USER 'batch'@'%' IDENTIFIED BY 'batch';
	CREATE DATABASE IF NOT EXISTS batch;
	GRANT ALL ON batch.* TO 'batch'@'%';
	FLUSH PRIVILEGES;
	SHOW GRANTS FOR 'batch'@'%';
    
    -- DROP USER 'batch'@'*';

## [postgresql](https://hub.docker.com/_/postgres)


    docker run --rm -P --publish 127.0.0.1:5432:5432 --name postgesql__scala-db -e POSTGRES_PASSWORD=p12345 -v /usr/local/data/postgresql:/var/lib/postgresql/data -d postgres

Create a data directory on a suitable volume on your host system, e.g. `/usr/local/data/postgresql`

    docker run -p 5432:5432 --name postgesql__scala-db -e POSTGRES_PASSWORD=p12345 -v /usr/local/data/postgresql:/var/lib/postgresql/data -d postgres

to connect docker you can use `localhost` or `ip` taken from `ifconfig` info.

    docker run -it --rm postgres psql -h 172.17.0.1 -U postgres

in case network is specified, next instruction should be used (also possible to replace `IP` with `docker host name`):

    docker run -it --rm --network docker_gwbridge postgres psql -h 172.17.0.1 -U postgres

## Mongo

    docker run -d -p 27017-27019:27017-27019 --name mongodb mongo

    docker ps | grep mongo
    
    docker exec -it <mongoId> bash

    :/#mongo
```
MongoDB shell version v4.0.10
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("5aa30ef3-646d-409d-b1f1-ae7cdec210e3") }
MongoDB server version: 4.0.10
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2019-06-17T11:56:27.034+0000 I STORAGE  [initandlisten] 
2019-06-17T11:56:27.034+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2019-06-17T11:56:27.034+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2019-06-17T11:56:27.642+0000 I CONTROL  [initandlisten] 
2019-06-17T11:56:27.642+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2019-06-17T11:56:27.642+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2019-06-17T11:56:27.642+0000 I CONTROL  [initandlisten] 
---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display
metrics about your deployment (disk utilization, CPU, operation statistics, etc).

The monitoring data will be available on a MongoDB website with a unique URL accessible to you
and anyone you share the URL with. MongoDB may use this information to make product
improvements and to suggest MongoDB products and deployment options to you.

To enable free monitoring, run the following command: db.enableFreeMonitoring()
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
---
```
    > show dbs
    > db.people.save({ firstname: "Nic", lastname: "Raboy" })
    WriteResult({ "nInserted" : 1 })
    > db.people.save({ firstname: "Maria", lastname: "Raboy" })
    WriteResult({ "nInserted" : 1 })
    > db.people.find({ firstname: "Nic" })
    { "_id" : ObjectId("5d078059eac917e283931d8e"), "firstname" : "Nic", "lastname" : "Raboy" }
    
    > db.people.find()
    { "_id" : ObjectId("5d078059eac917e283931d8e"), "firstname" : "Nic", "lastname" : "Raboy" }
    { "_id" : ObjectId("5d078066eac917e283931d8f"), "firstname" : "Maria", "lastname" : "Raboy" }
    > db.getCollection("people").find()
    { "_id" : ObjectId("5d078059eac917e283931d8e"), "firstname" : "Nic", "lastname" : "Raboy" }
    { "_id" : ObjectId("5d078066eac917e283931d8f"), "firstname" : "Maria", "lastname" : "Raboy" }

