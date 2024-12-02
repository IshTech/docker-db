- For MariaDB version 10.6
- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value

- HDD volumes for data and run
  - `/e/mariadb/` to new value

- To build and run (this will start mysql containers in background)
  - `docker-compose -f mariadb-10-docker-compose.yml up -d`

- To stop it
  - `docker-compose -f mariadb-10-docker-compose.yml stop`

- To connect to docker using bash
  - `docker exec -it mariadb_10_container bash`

- To connect using mysql cli
  - From container bash
    - `mariadb -u MYUSER -pMYPASSWORD`
  - From local bash
    - `mariadb -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1`
    - `mariadb -u root -pQwer4321 -P 23306 -h 127.0.0.1`
