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

create image:

    docker run -d -p 27017-27019:27017-27019 --name mongodb mongo

create image with external data storing `/data/databases/mongo/db` and with access to different folred `/home/sergii/Development/projects/research/mongo/learning_mongo` mapped on `/home/learning_mongo` docker inside:

    docker run -d -p 27017-27019:27017-27019 -v /data/databases/mongo/db:/data/db -v /home/sergii/Development/projects/research/mongo/learning_mongo/:/home/learning_mongo --name mongo-db mongo

    docker ps | grep mongo
    
    docker exec -it <mongoId> bash

