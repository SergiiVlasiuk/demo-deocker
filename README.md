# demo-docker

## Logs
    docker logs <containerId>

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



