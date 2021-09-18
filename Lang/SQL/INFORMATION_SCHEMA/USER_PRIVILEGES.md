## USER_PRIVILEGES
#information_schema 
用户权限表。内容源自 mysql.user 授权表。是非标准表。
```sql
mysql> SELECT * FROM USER_PRIVILEGES;
+-------------------------+---------------+-------------------------+--------------+
| GRANTEE                 | TABLE_CATALOG | PRIVILEGE_TYPE          | IS_GRANTABLE |
+-------------------------+---------------+-------------------------+--------------+
| 'mysql.sys'@'localhost' | def           | USAGE                   | NO           |
| 'root'@'%'              | def           | SELECT                  | YES          |
| 'root'@'%'              | def           | INSERT                  | YES          |
| 'root'@'%'              | def           | UPDATE                  | YES          |
| 'root'@'%'              | def           | DELETE                  | YES          |
| 'root'@'%'              | def           | CREATE                  | YES          |
| 'root'@'%'              | def           | DROP                    | YES          |
| 'root'@'%'              | def           | RELOAD                  | YES          |
| 'root'@'%'              | def           | SHUTDOWN                | YES          |
| 'root'@'%'              | def           | PROCESS                 | YES          |
| 'root'@'%'              | def           | FILE                    | YES          |
| 'root'@'%'              | def           | REFERENCES              | YES          |
| 'root'@'%'              | def           | INDEX                   | YES          |
| 'root'@'%'              | def           | ALTER                   | YES          |
| 'root'@'%'              | def           | SHOW DATABASES          | YES          |
| 'root'@'%'              | def           | SUPER                   | YES          |
| 'root'@'%'              | def           | CREATE TEMPORARY TABLES | YES          |
| 'root'@'%'              | def           | LOCK TABLES             | YES          |
| 'root'@'%'              | def           | EXECUTE                 | YES          |
| 'root'@'%'              | def           | REPLICATION SLAVE       | YES          |
| 'root'@'%'              | def           | REPLICATION CLIENT      | YES          |
| 'root'@'%'              | def           | CREATE VIEW             | YES          |
| 'root'@'%'              | def           | SHOW VIEW               | YES          |
| 'root'@'%'              | def           | CREATE ROUTINE          | YES          |
| 'root'@'%'              | def           | ALTER ROUTINE           | YES          |
| 'root'@'%'              | def           | CREATE USER             | YES          |
| 'root'@'%'              | def           | EVENT                   | YES          |
| 'root'@'%'              | def           | TRIGGER                 | YES          |
| 'root'@'%'              | def           | CREATE TABLESPACE       | YES          |
+-------------------------+---------------+-------------------------+--------------+
29 rows in set (0.00 sec)
```