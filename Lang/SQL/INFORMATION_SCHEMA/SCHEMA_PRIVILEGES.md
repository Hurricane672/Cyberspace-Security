## SCHEMA_PRIVILEGES
#information_schema 
方案权限表。给出了关于方案（数据库）权限的信息。内容来自 mysql.db 授权表。是非标准表。
```sql
mysql> SELECT * FROM SCHEMA_PRIVILEGES;
+-------------------------+---------------+--------------+-------------------------+--------------+
| GRANTEE                 | TABLE_CATALOG | TABLE_SCHEMA | PRIVILEGE_TYPE          | IS_GRANTABLE |
+-------------------------+---------------+--------------+-------------------------+--------------+
| 'mysql.sys'@'localhost' | def           | sys          | TRIGGER                 | NO           |
| 'root'@'%'              | def           | mysql        | SELECT                  | YES          |
| 'root'@'%'              | def           | mysql        | INSERT                  | YES          |
| 'root'@'%'              | def           | mysql        | UPDATE                  | YES          |
| 'root'@'%'              | def           | mysql        | DELETE                  | YES          |
| 'root'@'%'              | def           | mysql        | CREATE                  | YES          |
| 'root'@'%'              | def           | mysql        | DROP                    | YES          |
| 'root'@'%'              | def           | mysql        | REFERENCES              | YES          |
| 'root'@'%'              | def           | mysql        | INDEX                   | YES          |
| 'root'@'%'              | def           | mysql        | ALTER                   | YES          |
| 'root'@'%'              | def           | mysql        | CREATE TEMPORARY TABLES | YES          |
| 'root'@'%'              | def           | mysql        | LOCK TABLES             | YES          |
| 'root'@'%'              | def           | mysql        | EXECUTE                 | YES          |
| 'root'@'%'              | def           | mysql        | CREATE VIEW             | YES          |
| 'root'@'%'              | def           | mysql        | SHOW VIEW               | YES          |
| 'root'@'%'              | def           | mysql        | CREATE ROUTINE          | YES          |
| 'root'@'%'              | def           | mysql        | ALTER ROUTINE           | YES          |
| 'root'@'%'              | def           | mysql        | EVENT                   | YES          |
| 'root'@'%'              | def           | mysql        | TRIGGER                 | YES          |
+-------------------------+---------------+--------------+-------------------------+--------------+
19 rows in set (0.00 sec)
```
