## setuid( )
#uid 
setuid函数设置[[uid#实际用户ID|实际用户ID]]和[[uid#有效用户ID|有效用户ID]]。Linux的setuid函数和Unix中的setuid函数的行为是不同的。在Linux中， setuid(uid)函数的执行步骤为:(1)如果由普通用户调用,将当前进程的有效ID设置为uid. (2)如果由有效用户ID符为0的进程调用,则将真实,有效和已保存用户ID都设置为uid.

### 函数原型
```c
int setuid(uid_t uid);
```

在Unix中，setuid(uid)函数的行为为： (1)如果进程没有超级用户特权,且uid等于实际用户ID或已保存用户ID,则将有效的用户ID设置为uid.否则返回错误.(2)如果进程是有超级用户特权,则将真实、有效和已保存用户表示符都设置为uid.如果两个条件都不满足，则设置errno为EPERM。

函数在执行成功的时候返回0，在出错的时候返回-1.