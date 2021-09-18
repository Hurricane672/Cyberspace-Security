## 2021-8-31Web_php_include
#php流 #文件包含 
```php
<?php 
show_source(__FILE__); echo $_GET['hello']; 
$page=$_GET['page']; 
while (strstr($page, "php://")) 
	{ 
		$page=str_replace("php://", "", $page);
} 
include($page); 
?>
```
1. 用大写的[[../Lang/PHP/流#1 php input|PHP://input]]配合POST方法传递参数。![[../attaches/Pasted image 20210901001059.png]]
2. 用[[../Lang/PHP/流#2 data|data://text/plain]]传递数据
