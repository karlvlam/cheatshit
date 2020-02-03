
#### Create login role, and basic permission setup
```sql
CREATE USER my_user WITH PASSWORD 'abcd1234';

CREATE DATABASE my_db WITH TEMPLATE = template0 ENCODING = 'UTF8' LC_COLLATE = 'en_US.UTF-8' LC_CTYPE = 'en_US.UTF-8';

\c my_db
REVOKE ALL ON SCHEMA public FROM public;

GRANT usage ON SCHEMA public TO my_user;

GRANT select,insert,update ON ALL TABLES IN SCHEMA public TO my_user;
GRANT usage, select ON ALL SEQUENCES IN SCHEMA public TO my_user;

```

#### pg_dump database
```bash
export PGDATABASE=my_db
export PGHOST=127.0.0.1
export PGUSER=my_user
pg_dump > backup.sql
```

#### show pg processes
```sql
select datid,datname,pid,usename,client_addr,state,query from pg_stat_activity;
```

#### show database size
```sql
SELECT datname, pg_size_pretty(pg_database_size(datname)) db_size FROM pg_database where datname <> 'rdsadmin';
```

#### role management
```sql
CREATE USER my_user WITH PASSWORD 'abcd1234';
ALTER USER my_user WITH PASSWORD 'new_password';
```
#### create read-only role, and add login user to it
```sql
CREATE ROLE database_ro;
GRANT connect ON DATABASE my_database TO database_ro;
\c my_database 
GRANT USAGE ON schema public TO database_ro;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO database_ro;
GRANT USAGE , SELECT ON ALL SEQUENCES IN SCHEMA public TO database_ro;


CREATE USER app_user_ro WITH PASSWORD 'abcd1234';
GRANT database_ro TO app_user_ro;
```
