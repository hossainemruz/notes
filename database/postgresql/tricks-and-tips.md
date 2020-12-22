# Tricks & Tips

## Generating Sample Data

```sql
CREATE TABLE large_test (num1 bigint, num2 double precision, num3 double precision);
```

```sql
INSERT INTO large_test (num1, num2, num3) SELECT round(random()*10), random(), random()*142 FROM generate_series(1, 2000000) s(i);
```

## View Database & table Size

```sql
# all databases size
\l+
# specific database size
\l+ <database_name>
# all table size
\d+
# specific table size
\d+ <table_name>
```

