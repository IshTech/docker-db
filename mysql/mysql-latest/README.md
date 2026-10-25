- For MySQL latest innovation version, image `mysql:latest` (26.x as of Oct-2026)
- For testing new features only, use `mysql-lts` for real data
  - Innovation releases come about every quarter and are supported only until next one
  - Data upgraded by a newer innovation version cannot be used by older version or LTS, so keep separate data folder
  - [MySQL Releases: Innovation and LTS](https://dev.mysql.com/doc/refman/9.7/en/mysql-releases.html)

## Change any settings before run
- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value
- HDD volumes for data and run
  - `/e/mysql_latest/` to new value

## Charset and collation (utf8mb4)
- On Linux / macOS, `my_utf8mb4.cnf` can be used by mounting it read-only, in `docker-compose.yml` under `volumes`
```
- ./my_utf8mb4.cnf:/etc/mysql/conf.d/my_utf8mb4.cnf:ro
```
  - File must not be writable by others, else MySQL ignores it, i.e. `chmod 644 my_utf8mb4.cnf`
- On Windows this file is ignored, even with `:ro`
  - Docker shows files mounted from Windows as writable by everyone, and MySQL ignores such config files
  - Logs show `[Warning] World-writable config file '/etc/mysql/conf.d/my_utf8mb4.cnf' is ignored.`
- So `docker-compose.yml` also passes them as args under `command`, which works on all OS
```
command:
  - --character-set-server=utf8mb4
  - --collation-server=utf8mb4_unicode_ci
```

## Build and run
- To run on default port of 14306 (different from LTS port 13306, so both can run together)
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

## Upgrade to newer innovation version
- Take backup of data folder, then pull and start, MySQL upgrades data on first start
```
docker-compose pull
docker-compose up -d
```

## To connect
- To connect to docker using bash
```
docker exec -it mysql_latest_container bash
```

- To connect using mysql cli
  - From container bash
```
mysql -u MYUSER -pMYPASSWORD
```

  - From local bash
```
mysql -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1
mysql -u root -pQwer4321 -P 14306 -h 127.0.0.1
```

## Other options to set in env
- E.g. Timezone
  - `TZ: Europe/Helsinki`
