## KEY_COLUMN_USAGE
#information_schema 
描述了具有约束的键列。
```sql
mysql> SELECT * FROM KEY_COLUMN_USAGE;
+--------------------+-------------------+--------------------+---------------+--------------+---------------------------+--------------------+------------------+-------------------------------+-------------------------+-----------------------+------------------------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME    | TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME                | COLUMN_NAME        | ORDINAL_POSITION | POSITION_IN_UNIQUE_CONSTRAINT | REFERENCED_TABLE_SCHEMA | REFERENCED_TABLE_NAME | REFERENCED_COLUMN_NAME |
+--------------------+-------------------+--------------------+---------------+--------------+---------------------------+--------------------+------------------+-------------------------------+-------------------------+-----------------------+------------------------+
| def                | mysql             | PRIMARY            | def           | mysql        | columns_priv              | Host               |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | columns_priv              | Db                 |                2 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | columns_priv              | User               |                3 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | columns_priv              | Table_name         |                4 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | columns_priv              | Column_name        |                5 |                          NULL | NULL                    | NULL                  | NULL                   |
...
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_leap_second     | Transition_time    |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_name            | Name               |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_transition      | Time_zone_id       |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_transition      | Transition_time    |                2 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_transition_type | Time_zone_id       |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | time_zone_transition_type | Transition_type_id |                2 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | user                      | Host               |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | mysql             | PRIMARY            | def           | mysql        | user                      | User               |                2 |                          NULL | NULL                    | NULL                  | NULL                   |
| def                | sys               | PRIMARY            | def           | sys          | sys_config                | variable           |                1 |                          NULL | NULL                    | NULL                  | NULL                   |
+--------------------+-------------------+--------------------+---------------+--------------+---------------------------+--------------------+------------------+-------------------------------+-------------------------+-----------------------+------------------------+
278 rows in set (0.03 sec)
```