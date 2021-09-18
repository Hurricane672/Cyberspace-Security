## uid
#uid 
在讨论这个setuid函数之前，我们首先要了解的一个东西就是内核为每个进程维护的三个UID值。这三个UID分别是实际用户ID(real uid)、有效用户ID(effective uid)、保存的设置用户ID(saved set-user-ID)。

### 实际用户ID
首先说这个实际用户ID，就是我们当前以哪个用户登录了，我们运行的程序的实际用户ID就是这个用户的ID。

### 有效用户ID
有效用户ID就是当前进程是以哪个用户ID来运行的，一般情况下是实际用户ID，如果可执行文件具有了SUID权限，那么它的有效用户ID就是可执行文件的拥有者。保存的设置用户ID就是有效用户ID的一个副本，与SUID权限有关。

### 保存的设置用户ID
关于SUID，最经典的例子莫过于passwd命令。passwd这个可执行文件的所有者是root，但是其他用户对于它也有执行权限，并且它自身具有SUID权限。那么当其他用户来执行passwd这个可执行文件的时候，产生的进程的就是以root用户的ID来运行的。如下图所示：

　　![](https://images0.cnblogs.com/blog/693341/201501/151048122148031.png)

这个passwd的所有者是root，并且在权限那一栏中s代表的是SUID权限。我以普通用户michael来运行这个命令，产生的进程如下：

　　![](https://images0.cnblogs.com/blog/693341/201501/151049196984883.png)

倒数第二行这个进程其命令是passwd，但是其USER却是root。这就是SUID权限。