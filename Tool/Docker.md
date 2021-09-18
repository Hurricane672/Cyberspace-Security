## Docker
#docker
### 使用docker-compose
`docker-compose -f docker-compose.yml up -d`指定使用的compose模板文件，默认为docker-compose.yml，可以多次指定
`docker-compose ps`列出项目中目前所有的容器
`docker-compose[start/stop/restart]`启动、停止、重启项目中的服务