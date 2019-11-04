
#### Create login user
```sql
CREATE USER 'my_user'@'%' IDENTIFIED BY 'abcd1234';

GRANT ALL PRIVILEGES ON *.* TO 'my_user'@'%';

GRANT select ON my_db.* TO 'my_user'@'%';

GRANT insert, update, delete ON my_db.* TO 'my_user'@'%';

FLUSH PRIVILEGES;

```


