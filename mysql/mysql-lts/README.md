- For MySQL LTS version (8.4 as of Oct-2025)

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
