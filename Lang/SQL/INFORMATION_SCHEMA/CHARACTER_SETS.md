## CHARACTER_SETS
#information_schema 
提供了 mysql 可用字符集的信息。`SHOW CHARACTER SET;` 命令从这个表获取结果。
```sql
mysql> SHOW CHARACTER SET;
+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
...
| eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
| gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 |
+----------+---------------------------------+---------------------+--------+
41 rows in set (0.07 sec)

mysql> SELECT * FROM CHARACTER_SETS;
+--------------------+----------------------+---------------------------------+--------+
| CHARACTER_SET_NAME | DEFAULT_COLLATE_NAME | DESCRIPTION                     | MAXLEN |
+--------------------+----------------------+---------------------------------+--------+
| big5               | big5_chinese_ci      | Big5 Traditional Chinese        |      2 |
| dec8               | dec8_swedish_ci      | DEC West European               |      1 |
| cp850              | cp850_general_ci     | DOS West European               |      1 |
...
| eucjpms            | eucjpms_japanese_ci  | UJIS for Windows Japanese       |      3 |
| gb18030            | gb18030_chinese_ci   | China National Standard GB18030 |      4 |
+--------------------+----------------------+---------------------------------+--------+
41 rows in set (0.00 sec)
```