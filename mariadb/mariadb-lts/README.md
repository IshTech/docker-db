- For MariaDB LTS version (11.8 as of Oct-2025)

## Change any settings before run
- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value
- HDD volumes for data and run
  - `/e/mariadb_lts/` to new value

## Build and run
- To run on default port of 23306
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

## To connect
- To connect to docker using bash
```
docker exec -it mariadb_lts_container bash
```

- To connect using mysql cli
  - From container bash
```
mariadb -u MYUSER -pMYPASSWORD
```

  - From local bash
```
mariadb -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1
mariadb -u root -pQwer4321 -P 23306 -h 127.0.0.1
```

```
docker run -d \
  --name mariadb_lts_container \
  -e MYSQL_ROOT_PASSWORD=Qwer4321 \
  -e MYSQL_PS1="mariadb_lts> " \
  -e LANG=C.UTF-8 \
  -p ${DB_PORT:-23306}:3306 \
  -v /e/mariadb_lts/data:/var/lib/mysql \
  -v /e/mariadb_lts/run:/var/run/mysqld \
  -v /e/mariadb_lts/init_scripts/:/docker-entrypoint-initdb.d/ \
  -v /e/mariadb_lts/custom:/opt/custom \
  -v ./my_utf8mb4.cnf:/etc/mysql/conf.d/my_utf8mb4.cnf \
  --restart unless-stopped \
  mariadb:lts

```