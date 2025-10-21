### To launch PostgreSQL docker from CLI

```
docker run -d \
  --name postgres_dev_db \
  -e POSTGRES_DB=devdb \
  -e POSTGRES_USER=devuser \
  -e POSTGRES_PASSWORD=devpass \
  -p 15432:5432 \
  -v "/e/postgres/data:/var/lib/postgresql/data" \
  --restart always \
  postgres:latest

```
