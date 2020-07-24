# Tips & Tricks

## Locking Database

### How to make a MySQL server read only to all users?

In order to make all the databases of a MySQL server read only to all users including super user, set `super_read_only` variable to `ON`.

```bash
mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL super_read_only = ON;
```

In order to make the database writable again set `super_read_only` variable to `OFF`.

```bash
mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL super_read_only = OFF;
```

### How to make a MySQL server read only to all user except the super user?

In order to make all the databases of a MySQL server read only to all users except the super user, set `read_only` variable to `ON`.

```bash
mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL read_only = ON;
```

In order to make the database writable again set `read_only` variable to `OFF`.

```bash
mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL read_only = OFF;
```

### How to lock all tables?

In order to lock all tables user `FLUSH TABLES WITH READ LOCK;` syntax.

```bash
FLUSH TABLES WITH READ LOCK;

// do work here

UNLOCK TABLES;
```

### How to lock a single table table?

In order to lock a table to make sure that no other client update it during query, use `LOCK TABLES` syntax.

```bash
LOCK TABLES tablename WRITE;

# Do other queries here

UNLOCK TABLES;
```





