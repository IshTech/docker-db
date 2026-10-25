- For MariaDB LTS version (11.8 as of Oct-2025)

## Change any settings before run
- To change root password for DB
  - `MYSQL_ROOT_PASSWORD` to new value
- HDD volumes for data and run
  - `/e/mariadb_lts/` to new value

## Charset and collation (utf8mb4)
- On Linux / macOS, can use `my_utf8mb4.cnf` by mounting it read-only, in `docker-compose.yml` under `volumes`
```
- ./my_utf8mb4.cnf:/etc/mysql/conf.d/my_utf8mb4.cnf:ro
```
  - File must not be writable by others, else MariaDB ignores it, i.e. `chmod 644 my_utf8mb4.cnf`
- On Windows this file is ignored, even with `:ro`
  - Docker shows files mounted from Windows as writable by everyone, and MariaDB ignores such config files
  - Logs show `Warning: World-writable config file '/etc/mysql/conf.d/my_utf8mb4.cnf' is ignored`
- So instead `docker-compose.yml` passes them as args, which works on all OS
```
command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

## Build and run
- To run on default port of 23306
```
docker-compose up -d
```

- To run on custom port, e.g. 53306
  - bash / Git Bash
```
DB_PORT=53306 docker-compose up -d
```
  - PowerShell
```
$env:DB_PORT=53306; docker-compose up -d
```
  - cmd
```
set "DB_PORT=53306" && docker-compose up -d
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

## Upgrade to newer LTS image
- Take backup of data folder before upgrade, e.g. copy `/e/mariadb_lts/data` to `/e/mariadb_lts/data-backup-YYYY-MM-DD`
- Pull latest image and recreate container, data in `/e/mariadb_lts/data` is kept
```
docker-compose pull
docker-compose up -d
```
- New image upgrades only the server program, not the system tables (`mysql` DB - users, privileges etc.) in existing data folder
  - Those are upgraded by `mariadb-upgrade`, skipping it shows in logs as
    `MariaDB upgrade (mariadb-upgrade or creating healthcheck users) required, but skipped due to $MARIADB_AUTO_UPGRADE setting`
  - Our own DBs and tables are not affected
- To run upgrade automatically on every start when needed, add in `docker-compose.yml` under `environment`
```
MARIADB_AUTO_UPGRADE: 1
```
- Or run it once manually while container is running
```
docker exec mariadb_lts_container mariadb-upgrade -uroot -pQwer4321
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

## Run without docker-compose
- Prefer `docker-compose up -d` above, it works same from any shell (bash, Git Bash, cmd, PowerShell)
- Change `23306` for custom port of your choice, but ensure port is available
- For PowerShell or cmd use their versions further below (they differ: PowerShell continues lines with `` ` ``, cmd with `^`)

- Linux / macOS / WSL / Git bash
  - If running from Git Bash on Windows, prefix the command with `MSYS_NO_PATHCONV=1`
    - i.e. `MSYS_NO_PATHCONV=1 docker run -d \ ...`
    - Otherwise Git Bash rewrites `/e/...:/var/...` volume args into `E:\...;C:\Program Files\Git\var\...` and docker creates junk folders like `E:\mariadb_lts\data;C`

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
  --health-cmd "mariadb-admin ping --silent" \
  --health-start-period 45s \
  --restart unless-stopped \
  mariadb:lts \
  --character-set-server=utf8mb4 \
  --collation-server=utf8mb4_unicode_ci
```

- PowerShell version
```
docker run -d `
  --name mariadb_lts_container `
  -e MYSQL_ROOT_PASSWORD=Qwer4321 `
  -e MYSQL_PS1="mariadb_lts> " `
  -e LANG=C.UTF-8 `
  -p 23306:3306 `
  -v E:/mariadb_lts/data:/var/lib/mysql `
  -v E:/mariadb_lts/run:/var/run/mysqld `
  -v E:/mariadb_lts/init_scripts/:/docker-entrypoint-initdb.d/ `
  -v E:/mariadb_lts/custom:/opt/custom `
  --health-cmd "mariadb-admin ping --silent" `
  --health-start-period 45s `
  --restart unless-stopped `
  mariadb:lts `
  --character-set-server=utf8mb4 `
  --collation-server=utf8mb4_unicode_ci
```

- cmd version
```
docker run -d ^
  --name mariadb_lts_container ^
  -e MYSQL_ROOT_PASSWORD=Qwer4321 ^
  -e MYSQL_PS1="mariadb_lts> " ^
  -e LANG=C.UTF-8 ^
  -p 23306:3306 ^
  -v E:/mariadb_lts/data:/var/lib/mysql ^
  -v E:/mariadb_lts/run:/var/run/mysqld ^
  -v E:/mariadb_lts/init_scripts/:/docker-entrypoint-initdb.d/ ^
  -v E:/mariadb_lts/custom:/opt/custom ^
  --health-cmd "mariadb-admin ping --silent" ^
  --health-start-period 45s ^
  --restart unless-stopped ^
  mariadb:lts ^
  --character-set-server=utf8mb4 ^
  --collation-server=utf8mb4_unicode_ci
```
