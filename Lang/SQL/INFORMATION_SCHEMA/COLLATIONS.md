## COLLATIONS
#information_schema 
提供了关于各字符集的对照信息。`SHOW COLLATION;` 命令从这个表获取结果。
```sql
mysql> SELECT * FROM COLLATIONS;
+--------------------------+--------------------+-----+------------+-------------+---------+
| COLLATION_NAME           | CHARACTER_SET_NAME | ID  | IS_DEFAULT | IS_COMPILED | SORTLEN |
+--------------------------+--------------------+-----+------------+-------------+---------+
| big5_chinese_ci          | big5               |   1 | Yes        | Yes         |       1 |
| big5_bin                 | big5               |  84 |            | Yes         |       1 |
| dec8_swedish_ci          | dec8               |   3 | Yes        | Yes         |       1 |
| dec8_bin                 | dec8               |  69 |            | Yes         |       1 |
...
| gb18030_bin              | gb18030            | 249 |            | Yes         |       1 |
| gb18030_unicode_520_ci   | gb18030            | 250 |            | Yes         |       8 |
+--------------------------+--------------------+-----+------------+-------------+---------+
222 rows in set (0.03 sec)
```