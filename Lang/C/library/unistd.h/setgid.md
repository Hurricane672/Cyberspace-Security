## setgid( )
#uid
setgid()用来将目前进程的真实组识别码(real gid)设成参数gid 值. 如果是以超级用户身份执行此调用, 则real、effective 与savedgid 都会设成参数gid。

### 函数原型
```c
int setgid(gid_t gid);
```
  
### 返回值
设置成功则返回0, 失败则返回-1, 错误代码存于errno 中。
错误代码：EPERM：并非以超级用户身份调用, 而且参数gid并非进程的effective gid 或saved gid 值之一.