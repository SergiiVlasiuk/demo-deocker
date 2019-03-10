# demo-deocker

## Logs
    docker logs <containerId>

## Maria db

    docker run --name demo-mariadb -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 -d mariadb
