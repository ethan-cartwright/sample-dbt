# sample-dbt

A starter for setting up dbt test cases.

Requirements:

- Docker
- dbt

## Getting started

Unless otherwise specified, run all commands from the repo root.

1. Set up Postgres. We use the [pagila](https://github.com/devrimgunduz/pagila) database.

   ```shell
   docker pull postgres
   
   # start (add -d before the last 'postgres' to run in background)
   docker run --name postgres -p 5432:5432 -e POSTGRES_PASSWORD=secret postgres
   ```

2. Ingest sample data into the database

   ```shell
   # enter psql shell
   docker exec -it postgres psql -U postgres
   ```

   Once in shell, run

   ```sql
   CREATE DATABASE pagila;
   CREATE DATABASE
   \q # quit
   ```

   Now, run the ingestion SQL scripts:

   ```shell
   # schema objects
   cat ./db/pagila-schema.sql | docker exec -i postgres psql -U postgres -d pagila
   # data
   cat ./db/pagila-data.sql | docker exec -i postgres psql -U postgres -d pagila
   ```

   To confirm ingestion, enter psql shell again (same command as before) and run

   ```sql
   # connect to database
   \c pagila
   # list tables
   \dt
   ```

3. Set up dbt:

   ```shell
   # generate ./target/catalog.json and ./target/manifest.json
   dbt run --profiles-dir .
   
   # generate ./target/sources.json
   dbt source snapshot-freshness
   ```

   
