## TRIGGERS
#information_schema 
提供了关于触发程序的信息。必须有 super 权限才能查看该表。
```sql
mysql> SELECT * FROM TRIGGERS LIMIT 1\G
*************************** 1. row ***************************
           TRIGGER_CATALOG: def
            TRIGGER_SCHEMA: sys
              TRIGGER_NAME: sys_config_insert_set_user
        EVENT_MANIPULATION: INSERT
      EVENT_OBJECT_CATALOG: def
       EVENT_OBJECT_SCHEMA: sys
        EVENT_OBJECT_TABLE: sys_config
              ACTION_ORDER: 1
          ACTION_CONDITION: NULL
          ACTION_STATEMENT: BEGIN IF @sys.ignore_sys_config_triggers != true AND NEW.set_by IS NULL THEN SET NEW.set_by = USER(); END IF; END
        ACTION_ORIENTATION: ROW
             ACTION_TIMING: BEFORE
ACTION_REFERENCE_OLD_TABLE: NULL
ACTION_REFERENCE_NEW_TABLE: NULL
  ACTION_REFERENCE_OLD_ROW: OLD
  ACTION_REFERENCE_NEW_ROW: NEW
                   CREATED: 2017-05-27 11:18:43.60
                  SQL_MODE: 
                   DEFINER: mysql.sys@localhost
      CHARACTER_SET_CLIENT: utf8
      COLLATION_CONNECTION: utf8_general_ci
        DATABASE_COLLATION: utf8_general_ci
1 row in set (0.00 sec)
```