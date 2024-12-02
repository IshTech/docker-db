To launch PostgreSQL docker from CLI

```
docker run --name test-postgres-container \
 -e POSTGRES_DB=testdb \
 -e POSTGRES_USER=testdbuser \
 -e POSTGRES_PASSWORD=testdbpass \
 -p 15432:5432 \
 postgres:17

```
