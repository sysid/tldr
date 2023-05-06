# pg_dumpall

> Extract a PostgreSQL database cluster into a script file or other archive file.
> More information: <https://www.postgresql.org/docs/current/app-pg-dumpall.html>.

- Dump all databases:

`pg_dumpall > {{path/to/file.sql}}`

- Dump all databases using a specific username:

`pg_dumpall --username={{username}} > {{path/to/file.sql}}`

- Same as above, customize host and port:

`pg_dumpall -h {{host}} -p {{port}} > {{output_file.sql}}`

- Dump all databases into a custom-format archive file with moderate compression:

`pg_dumpall -Fc > {{output_file.dump}}`

- Dump only database data into an SQL-script file:

`pg_dumpall --data-only > {{path/to/file.sql}}`

- Dump only schema (data definitions) into an SQL-script file:

`pg_dumpall -s > {{output_file.sql}}`


# Custom ...........................................................................................
https://www.postgresql.org/docs/current/app-pg-dumpall.html
- connect as a database superuser in order to produce a complete dump.
- Also, you will need superuser privileges to execute the saved script in order to be allowed to add users and groups and to create databases
- “pg_dumpall needs to connect several times to the PostgreSQL server (once per database). If you use password authentication it will ask for a password each time.”
- It is not important to which database you connect here since the script file created by pg_dumpall will contain the appropriate commands to create and connect to the saved databases.
- handle the entire cluster, backing up information on roles, tablespaces, users, permissions, etc...
