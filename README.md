# postgre-cheatsheet

# Service management
```
$ sudo service postgresql status
9.6/main (port 5432): online

$ sudo service postgresql stop
 * Stopping PostgreSQL 9.6 database server                   [ OK ] 

$ sudo service postgresql start
 * Starting PostgreSQL 9.6 database server                   [ OK ] 

$ sudo service postgresql restart
 * Restarting PostgreSQL 9.6 database server                 [ OK ] 
```

# OS user
```
$ getent passwd|grep postgres
postgres:x:122:130:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash

$ cut -d: -f1 /etc/passwd | grep postgres
postgres
```
```
$ sudo su postgres
postgres@egorov:/home/vmuser$
```

# Shell login, whoami and quit
```
postgres@egorov:/home/vmuser$ psql
psql (9.6.11)
Type "help" for help.

postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".

postgres=# select version();

postgres=# \q
postgres@egorov:/home/vmuser$
```

# DB users
## List
```
postgres=# \du
                                     List of roles
   Role name   |                         Attributes                         | Member of
---------------+------------------------------------------------------------+-----------
 postgres      | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 postgres_user |                                                            | {}
```
Use `\du+` for more info
## God mode
```
db.postgresql=# alter role postgres_user superuser;
ALTER ROLE
```
## Admin password
```
postgres=# \password postgres
Enter new password:
```

# Databases
## List
```
postgres=# \l
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)
```
## View current
```
postgres=# select current_database();
 current_database
------------------
 postgres
(1 row)
```
You can also use `\conninfo`
## Create/drop
```
postgres@egorov:/home/vmuser$ createdb db.postgresql
postgres@egorov:/home/vmuser$ dropdb db.postgresql
```
## Connect to db
```
postgres=# \c db.postgresql
You are now connected to database "db.postgresql" as user "postgres".
```

# Backup/restore
## Restore example
```
$ pg_restore -h 127.0.0.1 -U postgres_user -d db.postgresql -c -F t db_tmp.tar
```