# Example MediaWiki using MariaDB using Docker Compose

## Starting the instance

```bash
# Start containers in background
$ docker-compose up -d

# Get name of database container
$ docker container ls -f "ancestor=mariadb"

# Import initial database (replace CONTAINER with name found above)
$ cat initdb.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=mariadb_secret my_wiki
```

## Connecting to instance

Browse to: http://localhost:8000/

Login using user **Admin** and password **supersecret**
*(Should be changed in production)*

## Persist local volumes

The following directories and file are mounted in the respective containers and persist:

- ./db
-- MariaDB's database
-- In container: */var/lib/mysql*
- ./images
-- MediaWiki's uploads directory
-- In container: */var/www/html/images*
- ./LocalSettings.php
-- MediaWiki's installation settings
-- In container: */var/www/html/LocalSettings.php*

## Backing-up and restoring the database
Replace CONTAINER with running MariaDB container
#### Backup
```bash
docker exec -i CONTAINER /usr/bin/mysqldump -u root --password=mariadb_secret my_wiki > backup.sql
```
#### Restore
```bash
cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=mariadb_secret my_wiki
```
