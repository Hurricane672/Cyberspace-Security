## 7. CSRF
#CSRF

Cross-site request forgery 跨站请求伪造

利用img标签触发get请求

漏洞修补：

1.  验证HTTP Referer字段
2.  在请求地址中添加token并验证
3.  在HTTP头中自定义属性并验证