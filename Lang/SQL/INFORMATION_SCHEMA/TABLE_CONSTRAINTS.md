## TABLE_CONSTRAINTS
#information_schema 
描述了存在约束的表。以及表的约束类型。
```sql
mysql> SELECT * FROM TABLE_CONSTRAINTS;
+--------------------+-------------------+--------------------+--------------+---------------------------+-----------------+
| CONSTRAINT_CATALOG | CONSTRAINT_SCHEMA | CONSTRAINT_NAME    | TABLE_SCHEMA | TABLE_NAME                | CONSTRAINT_TYPE |
+--------------------+-------------------+--------------------+--------------+---------------------------+-----------------+
| def                | mysql             | PRIMARY            | mysql        | columns_priv              | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | db                        | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | engine_cost               | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | event                     | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | func                      | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | gtid_executed             | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | help_category             | PRIMARY KEY     |
| def                | mysql             | name               | mysql        | help_category             | UNIQUE          |
| def                | mysql             | PRIMARY            | mysql        | help_keyword              | PRIMARY KEY     |
| def                | mysql             | name               | mysql        | help_keyword              | UNIQUE          |
| def                | mysql             | PRIMARY            | mysql        | help_relation             | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | help_topic                | PRIMARY KEY     |
| def                | mysql             | name               | mysql        | help_topic                | UNIQUE          |
| def                | mysql             | PRIMARY            | mysql        | innodb_index_stats        | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | innodb_table_stats        | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | ndb_binlog_index          | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | plugin                    | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | proc                      | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | procs_priv                | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | proxies_priv              | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | server_cost               | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | servers                   | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | slave_master_info         | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | slave_relay_log_info      | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | slave_worker_info         | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | tables_priv               | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | time_zone                 | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | time_zone_leap_second     | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | time_zone_name            | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | time_zone_transition      | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | time_zone_transition_type | PRIMARY KEY     |
| def                | mysql             | PRIMARY            | mysql        | user                      | PRIMARY KEY     |
| def                | sys               | PRIMARY            | sys          | sys_config                | PRIMARY KEY     |
| def                | zentao            | PRIMARY            | zentao       | zt_action                 | PRIMARY KEY     |
...
| def                | zentao            | account            | zentao       | zt_usergroup              | UNIQUE          |
| def                | zentao            | PRIMARY            | zentao       | zt_userquery              | PRIMARY KEY     |
| def                | zentao            | PRIMARY            | zentao       | zt_usertpl                | PRIMARY KEY     |
+--------------------+-------------------+--------------------+--------------+---------------------------+-----------------+
213 rows in set (0.37 sec)
```