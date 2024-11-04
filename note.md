# 1. shell

## 1.1 shell 简介



## 1.2 命令行提示符：

username@hostname:directon$

用户名			主机名	 当前目录

```shell
whoami
```

可以打印用户名

```shell
hostname
```

可以打印主机名

```shell
pwd
```

可以打印当前路径

## 1.3 shell命令格式

- 一条命令的三要素之间用空格隔开
- 多个命令在一行书写，用分号`;`将他们隔开
- 如果一条命令不能在一行写完，在行尾使用反斜杠`\ `表明该条命令未结束

## 1.4 命令行操作

- 一次tab使用文件名补齐
- 两次tab使用命令补齐

## 1.5 shell中的特殊字符

### 1.5.1 shell中的通配符

当用户需要用命令处理一组文件，例如一堆 .txt 文件，用户不必一一输入文件名，可以使用shell 通配符

`*`

`?`

`[xxx]`

`[x-y]`

`[^yyy]`

### 1.5.2 管道

> 补充 wc 命令: 一个可以用于 统计文件/目录的 命令
>
> ```shell
> man wc
> ```
>
> 打开wc命令的使用手册,查阅更多做法

管道符 `|` 将左边程序的输出，作为右边程序的输入

`sudo` do as su ,su super user

结合wc -w 命令可以干这件事

```shell
ls -l | wc -w 
```

可以统计当前目录下有多少文件数目

原理是：ls -l 的输出是文件名列表，wc -w是统计输入中的单词数，文件名列表中的单词数显而易见就是文件数目咯

**grep 命令和 | 的结合**

场景1：将某个目录中的文件名进行过滤，滤出你想要的带有那个文件名的那些文件	

​	做法1：将目标目录作为参数输入到grep命令，然后使用“linux”进行过滤

```shell
grep "linux" /etc/passwd 
```

​	做法2：使用cat 对目标目录/etc/passwd进行输出，并将其输出作为grep的输入，接着用“linux”进行过滤

```shell
cat /etc/passwd | grep "linux"
```

场景2：使用`ps -ef` 命令，来筛选出正在进行的bash进程(`ps -ef` 可以输出到终端当前计算机正在执行的进程)

```shell
ps -ef | grep "bash"
```

### 1.5.3 输出输入重定向

每个程序都有两个主要的流，input stream & output stream 

默认情况下，input stream 来自你的键盘

shell 提供了一些改变“流”向的方法

`>`输出

`<`读取输入

如果我想复制一个文件，但是我不想使用 `cp` 命令，这时我可以使用 `cat`

```shell
cat < hello.txt > hello2.txt
```

这一段的意思就是， 从 hello.txt 中读取输入， 然后输出到 hello2.txt 中

`>>` 追加

> 在终端中依次输入这两条命令

```shell
# 验证效果
cat < hello.txt >> hello2.txt
cat < hello.txt >> hello2.txt
```

> 你将会得到这个输出
>
> 第一次：hello
>
> 第二次：hello
> 			    hello

2> 或 &> : 将由命令产生的错误信息输入到文件中

场景：假设abc.txt 是一个不存在的文件

```shell
# 使用 ls abc.txt 一定会报错
# 而如果我们使用
ls abc.txt > wrong.txt
# 会发现仍旧输出在终端而非wrong.txt中
# 需要使用 2>
ls abc.txt 2> wrong.txt
# 如果我们想要同时保存正确信息和错误信息，那就得使用 &> (假设 cde.txt 是存在的文件)
ls abc.txt cde.txt &> msg.txt
```

### 1.5.4 命令置换

将一个命令的输出作为另一个命令的 参数

```shell
echo "Today is `date`"
```

和

```shell
echo "Today is date"
```

## 1.6 shell基本系统维护命令

### 1.6.1 su

su 命令用于临时改变用户身份，具有其他用户的权限。普通用户可以使用su命令临时具有超级用户的权限；

```shell
su # 仅切换到超级用户身份，但是不改变当前工作目录，不加载目标用户的环境变量和shell配置文件（如.profile 或 .bashrc）
su - # 切换用户身份，并将当前工作目录切换到目标用户的主目录，加载目标用户的环境变量和shell配置文件，完全模拟目标用户的登录环境
```

当想要退出时，输入

```shell
exit
```

### 1.6.2 echo

加和不加双引号

```shell
echo "Hello     world"
# 输出Hello     world
echo Hello      world
# 输出Hello world
```

### 1.6.3 date

```shell
date
```

打印当前时间

### 1.6.4 man

### 1.6.5 df

### 1.6.6 du

## 1.7 进程管理命令

- 程序的一次执行就是一个进程

### 1.7.1 ps 命令

进程的状态标志：

R：正在执行中；S：阻塞状态；T：暂停执行；

Z：不存在但暂时无法消除---一种“僵尸态“

D：不可中断的静止

<：高优先级的进程

N：低优先级的进程

L：有内存分页分配并锁在内存中

### 1.7.2 kill 命令

使用kill命令终止进程

kill [-signal] PID**(process id)**

signal 是信号，PID是进程号

> keten@Keten:~$ kill -l
>
> 1)SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
>
> 6)SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
>
> 11)SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
>
> 16)SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
>
> 21)SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
>
> 26)SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
>
> 31)SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
>
> 38)SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
>
> 43)SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
>
> 48)SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
>
> 53)SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
>
> 58)SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
>
> 63)SIGRTMAX-1	64) SIGRTMAX

kill 命令向指定的进程发出一个信号signal，在默认的情况下，kill命令向指定进程发出信号15，正常情况下，将杀死那些不捕捉或不忽略这个信号的进程

如果发送-18 那将会发出一个把被挂起进程恢复的 信号

比如一个处于R+ 的进程，使用 Ctrl + Z 可以将它暂停，此时它为 T ,然后

```shell
kill -18 [pid]
```

它就位于 R ，即后台运行状态

接着在当前终端使用

```shell
fg 
```

可以将它恢复为 R+

当然对应的，输入

```shell
bg
```

就可以将其转换为后台作业

fg 和 bg 通常后面要接 作业号 ，可以在当前终端 输入jobs 来查看作业号

### 1.7.3 top 命令

动态显示进程占用情况，跟win下的任务管理器差不多

### 1.7.4 pstree命令

将所有行程以树状图显示，树状图会以 pid 或是以init 这个基本进程为根，如果有指定使用者id，则树状图会只显示该使用者所拥有的进程

---



shell 实际上是一种编程语言

环境变量：

```shell
echo $PATH 
```

这将显示机器上的所有路径,路径是一种命名计算机上文件位置的方法

/ 做开始，表示从根目录开始寻找

`pwd` :print working dirtory

`. `表示current directory

`.. `表示 parent directory

`ls ` will show you all files in your current directory

`~ `总是扩展到主目录

`-`将会扩展到你之前所在的目录

`-L` 使用长列表模式,使用` ls -l`将会在列出所有文件的同时，打印出详细信息

每三个字符为一组，分别是 读取、写入和执行

r read 读取

w write 写入

x execute 执行

对于目录，这些权限的含义有点不同

read “你是否被允许看到这个目录中的文件”，“你是否被允许列出它的内容”

write “你是否被允许在该目录中重命名、创建或删除文件”

> 但请注意，如果你对一个文件有写入权限，但你没有对它的目录有写入权限，你就不能删除该文件，你可以清空它，但是你不能删除它，因为这需要写入到目录本身

execute "目录上的执行权限是所谓的搜索权限"，即“你是否被允许进入该目录”，就是说如果你要cd 进一个目录，你必须拥有它及它的父目录的执行权限

还有一件重要事情：如果只有一个`-`，那就意味着你没有该权限

> e-g : `r-x`,这意味着你有读和执行权限，但没有写权限

`linux` 不能递归执行删除命令，所以不能使用`rm` 删除一个目录

`rm -r` 进行递归删除，给出一个路径，它将删除路径下的所有内容

`rmdir` 可以让你删除一个目录，但是这个目录必须是一个空目录

`mkdir` 创建一个新目录，注意不能这样写：`mkdir My Photo ` 这会创建两个目录 一个My 一个Photo，正确做法是： `mkdir My\ Photo `

`man` 可以查看手册页，如 `man ls` 可以查看关于 command ls 的所有用法（通过键盘上的q来退出手册模式）

Ctrl + L 快捷键，清除终端并回到顶部

管道符 `|` 将左边程序的输出，作为右边程序的输入

`sudo` do as su ,su super user

```shell
cd /sys/
ls 
```

> block  bus  class  dev  devices  firmware  fs  hypervisor  kernel  module  power 

这些实际上不是你计算机上的文件，而是各种内核参数

`# ` 可以表示`sudo` ，当它位于命令之前时

> keten@Keten:~$

`$` 表示你现在并不是位于root用户

sudo su 

su 可以让你用super user的身份获取一个shell,然后你就会返现

> root@Keten:/sys# 

当然你想退出时，你可以使用 `exit`

tee 会将输入内容写入一个文件，同时也将它输出到标准输出

因此，如果你想将一个日志文件保存起来以备之后查看，但同时也想在屏幕上看到它，就可以使用 tee 命令，这将会把日志输出到那个你指定的文件并且输出到终端

open to file 

`xdg-open ` 可以打开文件

`""` &` '' `对于字符串的处理

1. 对于纯文本字符串，两者没有区别
2. 对于使用了 $参数的，"" 可以将参数替换， '' 会直接原封不动保留

> ```shell
> foo=bar
> echo "Value is $foo"
> ```
>
> ​		Value is bar
>
> ```shell
> echo 'Value is $foo'
> ```
>
> ​		Value is $foo

# 2. 软件包管理机制

## 2.1 通识及 dpkg 管理器

deb软件包	

rpm软件包

软件包的命名：

```shell
Filename_Version-Reversion_Architecture.deb
软件包名称 软件版本 修订版本    体系架构
```

软件包管理工具分类

1. 命令行
2. 文本窗口界面
3. 图形界面

`dpkg` 相关命令

`dpkg -i  <package> `	安装一个在本地文件系统上存在的Debian软件包

`dpkg -r  <package>` 	移除一个已经安装的软件包

`dpkg -P <package>`		移除已安装软件包以及配置文件

`dpkg -L <package>`		列出安装的软件包清单

`dpkg -s <package>`	    显示软件包的安装状态

> package 就是软件包包名

dpkg 和 apt 软件包管理器有什么区别？

dpkg 由于无互联网功能，没有考虑到软件包的各个依赖关系，apt 有了互联网功能，所以它可以把这个软件包及其所有依赖都装好

## 2.2 APT 工作管理

Ubuntu 采用集中式的软件仓库机制，将各式各样的软件包分门别类地存放在软件仓库中，进行组织和管理；然后将软件仓库置于许许多多的镜像服务器中，并保持一致。因此，对于用户而言，这些镜像服务器就是他们的软件源（reposty）

/etc/apt/sources.list 软件源配置文件 ，列出最合适访问的镜像源站点地址

而 软件源配置文件 只是告知Ubuntu系统可以访问的镜像站点地址，但那些镜像站点拥有什么软件资源是不清楚的，所以为了拒绝这种每安装一个软件包就去寻找一遍的低效率，就有必要来为这些软件资源列个清单（建立索引文件）；

**这就是APT软件包管理器的工作原理**

所以谈到建立索引文件，就是`sudo apt-get update`了。这个 命令会去扫描每一个软件源服务器，并**为该服务器所具有软件包资源建立索引文件**，存放在本地的/var/lib/apt/lists/ 目录中

---

**修复软件包依赖关系**

`apt-get check ` 检查软件包依赖关系

`apt-get -f install` 修复依赖关系

在处理依赖关系上，apt-get 会自动下载并安装具有依赖关系（depends）的软件包，但不会处理与安装软件包存在推荐（recommends）和建议（suggests）关系的软件包

---

`sudo apt-get upgrade ` 所有软件包一键升级

---

`sudo apt-get install ` 下载软件包

> 在准备好软件源并连通网络后，用户只需告知安装软件的名称，apt-get install 命令就可以轻松完成整个安装过程，而无需考虑软件包的版本、优先级、依赖关系

大体分为4个STEP

1. 扫描本地存放的软件包更新列表（由apt-get update 命令刷新更新列表），找到最新版本的软件包
2. 进行软件包依赖关系检查，找到支持该软件正常运行的所以软件包
3. 从软件源所指的镜像站点中，下载相关软件包
4. 解压软件包，并自动完成应用程序的安装和配置

---

**重新安装软件包**

若用户不小心损坏了已安装的软件包，而需要修复；或者想要更新，可以重新安装软件包

`sudo apt-get install <package> --reinstall`

---

**卸载软件包**

不完全卸载：`apt-get remove <package>`

完全卸载： 	`apt-get --purge remove <package>`

---

**清理软件包缓冲区**

缓冲区干什么的？如果将来你在一个无网络的环境下想要装某个软件包（一个你之前装过的），那这个时候你就可以进入到缓冲区把对应软件包提取出来，进行安装；这里面也有可能有旧版本之类的东西（如果安装过多个版本）

`apt-get clean `

如果用户希望缓冲区中只保留最新版本的软件包，多余版本全部删除，可以使用 `apt-get autoclean`

---

**查询软件包信息**

`apt-cache subcommands <package>`

`apt-cache show <package>`获取指定软件包的详细信息，包括软件包安装状态、优先级、适用架构、版本、存在依赖关系的软件包，以及功能描述

`apt-cache policy <package>` 可以获取当前软件包的安装状态

`apt-cache depends <package>` 可以获取某个软件包的依赖关系

`apt-cache rdepends <package>` 可以知道该软件包被什么软件包所依赖

---

回顾：下列文件有什么作用？

/etc/apt/sources.list 软件源配置文件

/var/lib/apt/lists/* 服务器资源列表存放位置

/var/cache/apt/archives 本地的软件包缓存目录

# 3. Linux 文件系统

用于组织和管理计算机存储设备上的大量文件，并提供用户交互接口

## 3.1 Linux 文件系统的类型

## 3.2 Linux文件系统的结构

## 3.3 文件系统命令

# 4. shell脚本

**前言：编译型语言 & 解释型语言**

编译型语言：受编译器和处理器限制，简单举例：在x86 架构使用gcc ，在 arm 架构使用 arm -gcc

解释型语言：shell是解释型语言

shell脚本的本质：shell命令的有序集合

基本过程：

 	1. 建立shell 文件，包含任意多行操作系统命令或shell命令的文本文件
 	1. 赋予shell文件执行权限，用chmod命令修改权限(初始文本文件没有可执行权限)
 	1. 执行shell文件，直接在命令行上调用shell

## 4.1 shell 变量

shell允许用户建立变量存储数据，但不支持数据类型（整型、字符、浮点型），将任何赋给变量的值都解释为一串字符

```shell
Variable=value
```

注意不能加空格！！！

Bourne Shell 有如下四种变量

- 用户自定义变量
- 位置变量即命令行参数
- 预定义变量
- 环境变量

### 4.1.1 用户自定义变量

建议使用全大写变量，方便识别

变量的调用：在变量前 + $`

变量从右向左赋值

使用unset命令可以 删除变量的赋值

### 4.1.2 位置变量

`$0` 与键入的命令行一样，包含脚本文件名

`$1` ~ `$9` 分别表示第一个到第9个命令行参数

超过个位数的，要用`{}`进行包含,例如使用 `${11}`表示第11个命令行参数

`$#` 包含命令行参数的个数

`$@` 包含所有命令行参数 1~9

`$?` 包含前一个命令的退出状态

`$*` 包含所有命令行参数

`$$` 包含正在执行进程的ID号

### 4.1.3 预定义变量

### 4.1.4 环境变量

## 4.2 功能语句

不换行，/bin/bash echo -n

或者 ，/bin/sh "\c"

表达式为真是 0，为假是1

## 4.3 分支语句

### 4.3.1 条件分支语句

### 4.3.2 多路分支语句

