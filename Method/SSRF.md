## 9. SSRF
#SSRF
Server-Side Request Forgery 服务器端请求伪造（内网渗透）
```php
<?php
	$url=$_GET['url'];
	echo file_get_contents($url);
?>
```
对于上述代码构造payload
`http://example.com/ssrf.php?url=http://192.168.252.1:8000`可以检测内网主机HTTP服务8000端口的开放情况
[CSRF](./CSRF)