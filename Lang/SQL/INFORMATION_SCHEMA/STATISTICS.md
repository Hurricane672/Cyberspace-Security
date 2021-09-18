## STATISTICS
#information_schema 
表索引的信息。`SHOW INDEX FROM schemaname.tablename;` 命令从这个表获取结果。
```sql
mysql> SHOW INDEX FROM szhuizhong.users;
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table | Non_unique | Key_name      | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| users |          0 | PRIMARY       |            1 | UserID      | A         |        1460 |     NULL | NULL   |      | BTREE      |         |               |
| users |          0 | Account_index |            1 | Account     | A         |        1460 |     NULL | NULL   |      | BTREE      |         |               |
| users |          1 | CorpID        |            1 | FromID      | A         |           2 |     NULL | NULL   | YES  | BTREE      |         |               |
+-------+------------+---------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
3 rows in set (0.00 sec)
```