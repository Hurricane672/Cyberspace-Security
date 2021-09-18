## TABLE_PRIVILEGES
#information_schema 
表权限表。给出了关于表权限的信息。内容源自 mysql.tables_priv 授权表。是非标准表。
```sql
mysql> SELECT * FROM TABLE_PRIVILEGES;
+-------------------------+---------------+--------------+------------+----------------+--------------+
| GRANTEE                 | TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | PRIVILEGE_TYPE | IS_GRANTABLE |
+-------------------------+---------------+--------------+------------+----------------+--------------+
| 'mysql.sys'@'localhost' | def           | sys          | sys_config | SELECT         | NO           |
+-------------------------+---------------+--------------+------------+----------------+--------------+
1 row in set (0.00 sec)
```