- For MySQL LTS version, image `mysql:lts` (9.7.x as of Oct-2026)

## Change any settings before run
- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value
- HDD volumes for data and run
  - `/e/mysql_lts/` to new value

## Build and run
- To run on default port of 13306
```
docker-compose up -d
```

- To run on custom port, e.g. 53306
```
DB_PORT=53306 docker-compose up -d
```

- To run containers in background vs foreground
  - Adding option of `-d` makes it run in background
    - `docker-compose up -d`
  - And removing makes it run in foreground with logs visible and keep window occupied
    - `docker-compose up`

- To stop it
```
docker-compose stop
```

## Upgrade from 8.4 LTS to 9.7 LTS
- In-place upgrade from an LTS to next LTS is supported, but there is no going back to 8.4 on same data, so take backup first
  - [Upgrade Paths](https://dev.mysql.com/doc/refman/9.7/en/upgrade-paths.html)
- Risk: `mysql_native_password` is removed in 9.0, users still on it cannot login after upgrade
  - [Changes in MySQL 9.0.0 - Deprecation and Removal Notes](https://dev.mysql.com/doc/relnotes/mysql/9.0/en/news-9-0-0.html)
  - [Caching SHA-2 Pluggable Authentication](https://dev.mysql.com/doc/refman/9.7/en/caching-sha2-pluggable-authentication.html)

### Steps
1. With 8.4 container still running, find users on `mysql_native_password`
```
docker exec -it mysql_lts_container mysql -uroot -pQwer4321 -e "SELECT user, host, plugin FROM mysql.user WHERE plugin = 'mysql_native_password';"
```
2. Switch each of them to `caching_sha2_password` (needs their password)
```
ALTER USER 'MYUSER'@'%' IDENTIFIED WITH caching_sha2_password BY 'MYPASSWORD';
```
3. Optionally run MySQL Shell upgrade checker, it also checks auth methods (`authMethodUsage`) and other incompatibilities
  - [Upgrade Checker Utility](https://dev.mysql.com/doc/mysql-shell/9.7/en/mysql-shell-utilities-upgrade.html)
```
mysqlsh -- util checkForServerUpgrade root@127.0.0.1:13306 --target-version=9.7.4
```
4. Stop container and backup data folder, e.g. copy `/e/mysql_lts/data` to `/e/mysql_lts/data-backup-YYYY-MM-DD`
```
docker-compose stop
```
5. Pull latest LTS and start, MySQL upgrades data on first start
```
docker-compose pull
docker-compose up -d
docker logs -f mysql_lts_container
```
6. Apps connecting without SSL using `caching_sha2_password` may need `allowPublicKeyRetrieval=true` in JDBC url (MySQL Connector/J)
7. If anything goes wrong, stop container, restore data folder from backup and use `image: mysql:8.4` in `docker-compose.yml`

## To connect
- To connect to docker using bash
```
docker exec -it mysql_lts_container bash
```

- To connect using mysql cli
  - From container bash
```
mysql -u MYUSER -pMYPASSWORD
```

  - From local bash
```
mysql -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1
mysql -u root -pQwer4321 -P 13306 -h 127.0.0.1
```

## Other options to set in env
- E.g. Timezone
  - `TZ: Atlantic/Reykjavik`
