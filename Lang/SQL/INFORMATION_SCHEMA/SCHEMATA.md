## SCHEMATA
#information_schema 
当前 mysql 实例中所有数据库的信息。`SHOW DATABASES;` 命令从这个表获取数据。
```sql
mysql> SELECT * FROM SCHEMATA;
+--------------+--------------------+----------------------------+------------------------+----------+
| CATALOG_NAME | SCHEMA_NAME        | DEFAULT_CHARACTER_SET_NAME | DEFAULT_COLLATION_NAME | SQL_PATH |
+--------------+--------------------+----------------------------+------------------------+----------+
| def          | information_schema | utf8                       | utf8_general_ci        | NULL     |
| def          | mysql              | latin1                     | latin1_swedish_ci      | NULL     |
| def          | performance_schema | utf8                       | utf8_general_ci        | NULL     |
| def          | sys                | utf8                       | utf8_general_ci        | NULL     |
| def          | test               | utf8                       | utf8_unicode_ci        | NULL     |
+--------------+--------------------+----------------------------+------------------------+----------+
10 rows in set (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
10 rows in set (0.00 sec)
```
