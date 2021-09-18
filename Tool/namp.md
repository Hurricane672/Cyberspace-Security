## 32. nmap
#nmap

-   检测主机是否在线
-   端口扫描
-   版本检测
-   系统监测
-   探测脚本的缩写

### 32.1 常用参数

```shell
namp
-P #指定端口（若扫描的时候不指定端口，会默认扫描top1000端口）
-sn #仅探测主机，不进行端口扫描
-sP #ping扫描（nmap在扫描端口的时候会默认使用ping扫描）
-sT #TCP（全）连接扫描
-sS #TCP（半）SYN扫描
-sU #UDP扫描
-sN/sF/sX #隐蔽扫描
-sN #NULL扫描，标识位全为0
-sF #FIN扫描，标识位中的FIN=1
-sX #Xmas扫描，标识位中的FIN=1，PSH=1，URG=1
-sV #探测端口服务版本
-O #探测目标主机版本（存在误报）
-A #全面系统探测、启用脚本探测、扫描等
-oA #保存扫描报告到所有格式
-oN #保存扫描报告成txt纯文本文档格式
-oX #保存扫描报告成xml格式
-oG #保存扫描报告成grepable格式
-v #显示扫描过程
-T4 #针对TCP端口禁止动态扫描延迟超过10ms
-Pn #不进行主机存活探测
#以下皆可绕防火墙
-PS #ping TCP SYN
-PA #只扫描ACK包
-PU #UDP ping扫描
-PP #ICMP时间戳ping扫描
-PE #ICMP在指定系统上输出ping
-Pn #不采用ping方式
-sA #发现防火墙规则
–traceroute #路由追踪
namp 192.168.1,100,199.1-255 #单个用逗号隔开，段用横杠连接
192.168.1.1/24 #与192.168.1.1-255等效
```

### 32.2 脚本

```shell
### nmap/scripts
nmap --script 
```


