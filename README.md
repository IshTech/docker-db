# docker-mysql
- To pull
    - For latest
        - `docker pull mysql`
        - `docker pull mysql:latest`
    - For specific version
        - `docker pull mysql:8.4.0`

- To pull and run
    - `docker run --name you_can_choose_a_name -d mysql:latest`
        - `-d` is for detached mode
        - `--name` for container name is optional, but makes it convinient
        - `-e` to set environemnt vairables
            - You can set multiple evn vars
            - `-e NAME_1=VALUE_1 -e NAME_2=VALUE=2`

- To stop
    - `docker stop <container_name_or_id>`

- To restart
    - `docker start <container_name_or_id>`
    - This will use all settings done in run

- Options
    - Root password, set using `-e` and one of the following options
        - MYSQL_ROOT_PASSWORD
        - MYSQL_ALLOW_EMPTY_PASSWORD
        - MYSQL_RANDOM_ROOT_PASSWORD
        - You can set like below
            - `-e MYSQL_ROOT_PASSWORD=todo_set_a_password`
            - `-e MYSQL_ALLOW_EMPTY_PASSWORD=true`
            - `-e MYSQL_RANDOM_ROOT_PASSWORD=true`
        - If you use MYSQL_RANDOM_ROOT_PASSWORD, you can find password using following
            - ` docker logs <container_name_or_id> 2>&1 | grep 'GENERATED ROOT PASSWORD'`    
    - Port
        - `-p YOUR_CHOICE_OF_PORT:3306`
        - Without above option other applications cannot access mysql
    - Volumes
        - Advantage of data persitence, migration / portability, separation etc.
        - For following options
            - `/var/lib/mysql`
            - `/var/lib/data`
            - `/var/log/mysql`
            - `/var/run/mysqld`
            - `/etc/mysql` for `my.cnf` etc configuration files path
            - `/docker-entrypoint-initdb.d/`
        - e.g. `-v /path/on/host:/var/lib/mysql`

- To connect using mysql cli
    - using docker
        - `docker exec -it <container_name_or_id> mysql -uroot -p`
    - From container bash
        - `mysql -u MYUSER -pMYPASSWORD`
    - From local bash
        - `mysql -u MYUSER -pMYPASSWORD -P MYPORT -h 127.0.0.1`

- UI tools for connecting
    - PhpMyAdmin
        - `docker run --name you_can_choose_a_name --link <container_name_or_id_of_mysql>:db -p YOUR_CHOICE_OF_PORT:80 -d phpmyadmin`
        - http://localhost:YOUR_CHOICE_OF_PORT
    - Adminer
        - `docker run --name you_can_choose_a_name --link <container_name_or_id_of_mysql>:db -p YOUR_CHOICE_OF_PORT:8080 adminer`
