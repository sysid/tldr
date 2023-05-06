# pg_dump

> Extract a PostgreSQL database into a script file or other archive file.
> More information: <https://www.postgresql.org/docs/current/app-pgdump.html>.

- Dump database into an SQL-script file:

`pg_dump {{db_name}} > {{output_file.sql}}`

- Same as above, customize username:

`pg_dump -U {{username}} {{db_name}} > {{output_file.sql}}`

- Same as above, customize host and port:

`pg_dump -h {{host}} -p {{port}} {{db_name}} > {{output_file.sql}}`

- Dump a database into a custom-format archive file:

`pg_dump -Fc {{db_name}} > {{output_file.dump}}`

- Dump only database data into an SQL-script file:

`pg_dump -a {{db_name}} > {{path/to/output_file.sql}}`

- Dump only schema (data definitions) into an SQL-script file:

`pg_dump -s {{db_name}} > {{path/to/output_file.sql}}`


# Custom ....................................................................................................
- can back up a running, active database
- caveat: does not dump roles or other database objects including tablespaces, only a single database
- To include the necessary DDL CREATE DATABASE command and a connection in the backup file, include the -C option
```bash
createdb -O postgres psycopgtest  # specify DB owner

# Backup/Restore plain text format (-Fp)
pg_dump dbname > dump.sql
psql dbname < dump.sql

# custom format: Fc compresses ala zip
pg_dump -Fc mydb > db.dump
pg_dump -Fc -f db.dump URL

# restore into mydb (-d postgres needed to issue create database command)
pg_restore  -C -d postgres db.dump  # required for non-plaint text archives
pg_restore --clean -d 'postgresql://USER:qfdvr2ata7opidy4@HOST:PORT/DATABASE?sslmode=require' --jobs 2 database.dump

# VACUUM ANALYZE all databases;
vacuumdb -a -z

# backup/restore entire cluster
pg_dumpall > cluster.dump
psql -f cluster.dump postgres  # default db to kickstart commands

pg_stat_activity
pg_stat_database connections
```
