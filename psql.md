# psql

## About psql

PostgreSQL command-line client

## OC specifics

```bash
# access shell in container (execute in devenv-maker directory)
./dcompose.sh onecareer exec db bash
```

## Basics

```bash
# start postgres cli session (-h : host, -U : user, -d : database,)
# postgres is default superuser for PostgreSQL
psql -h localhost -U postgres -d onecareer_development

# show help
\?

# show all databases
\l

# connect to specific database
\c onecareer_development

# show all data tables (relations)
\dt

# show all tables with name having user in it
\dt *user*

# SELECT statement
SELECT * from activities limit 10;
```
