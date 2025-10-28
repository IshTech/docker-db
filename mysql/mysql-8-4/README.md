- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value

- HDD volumes for data and run
  - `/e/mysql_8_4/` to new value

- To build and run (this will start mysql containers in background)
  - `docker-compose -f docker-compose.yml up -d`

- To stop it
  - `docker-compose -f docker-compose.yml stop`

- To connect to docker using bash
  - `docker exec -it mysql_8_4 bash`

- To connect using mysql cli
  - From container bash
    - `mysql -u MYUSER -pMYPASSWORD`
  - From local bash
    - `mysql -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1`
    - `mysql -u root -pQwer4321 -P 13306 -h 127.0.0.1`

- Timezone
  - `TZ: Atlantic/Reykjavik`
