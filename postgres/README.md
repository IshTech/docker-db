### To launch PostgreSQL docker from CLI

```
docker run -d \
  --name postgres_dev \
  -e POSTGRES_DB=ishtech_dev_db \
  -e POSTGRES_USER=ishtech_dev_user \
  -e POSTGRES_PASSWORD=ishtech_dev_pass \
  -p 15432:5432 \
  -v "/e/postgres/data:/var/lib/postgresql/data" \
  -v "/e/postgres/init_scripts/:/docker-entrypoint-initdb.d/" \
  --restart unless-stopped \
  postgres:latest

```

### To connect from cli

```
psql -U ishtech_dev_user -d ishtech_dev_db -W

```
