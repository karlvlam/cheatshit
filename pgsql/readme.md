
#### Create login role, and basic permission setup
```sql
CREATE USER my_user WITH PASSWORD 'abcd1234';

CREATE DATABASE my_db WITH TEMPLATE = template0 ENCODING = 'UTF8' LC_COLLATE = 'en_US.utf8' LC_CTYPE = 'en_US.utf8';

\c my_db
REVOKE ALL ON SCHEMA public FROM public;

GRANT usage ON SCHEMA public TO my_user;

GRANT select,insert,update ON ALL TABLES IN SCHEMA public TO my_user;

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
