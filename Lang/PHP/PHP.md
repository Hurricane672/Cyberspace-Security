## 1. PHP基础
#php
### 1.1 session&cookie
```php
setcookie(name,value,timeout,path,area);#设置cookie
session_start();#开启session
session_id();#查询phpsessid
setcookie("PHPSESSID",session_id(),time()+100,"/","localhost");#设置phpsessid所有以localhost域名结尾的二级域名都在作用域内
```

### 1.2 文件上传

```ini
;php.ini
file_uploads = on
;文件上传开关
upload_tmp_dir = "./tmp"
;文件上传临时路径
upload_max_filesize = 2M
;上传文件的最大大小
max_file_uploads = 20
;单词上传文件的最大数目
```
创建[[HTML+CSS|html]]表单
```html
<html>
    <body>
        <form action="upload_file.php" method="post" enctype="multipart/form-data">
            <label for="file">Filename:</label>
            <input type="file" name="file" id="file"><br>
            <input type="submit" name="submit" value="Submit">
        </form>
    </body>
</html> 
```

创建上传脚本

```php
<?php
if ($_FILES["file"]["error"] > 0)
{
    echo "Error: " . $_FILES["file"]["error"] . "<br>";
}
else
{
    echo "Upload: " . $_FILES["file"]["name"] . "<br>";
    echo "Type: " . $_FILES["file"]["type"] . "<br>";
    echo "Size: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
    echo "Stored in: " . $_FILES["file"]["tmp_name"];
}
# move_uploaded_file($_FILES["file"]["tmp_name"],"upload/" . $_FILES["file"]["name"]);保存文件
?> 
```

- $_FILES\["file"\]\["name"\] - 被上传文件的名称

- $_FILES\["file"\]\["type"\] - 被上传文件的类型

- $_FILES\["file"\]\["size"\] - 被上传文件的大小，以字节计

- $_FILES\["file"\]\["tmp_name"\] - 存储在服务器的文件的临时副本的名称

- $_FILES\["file"\]\["error"\] - 由文件上传导致的错误代码

### 1.3 可变变量

  ```php
$a = "test";
$b = "a";
$$b = "test";
  ```

