title: ubuntu常用命令及部分快捷键
categories: Linux
tags: ubuntu
date: 2016-12-11
---


# 查看系统和硬件相关信息的命令简介
### 查看内核/操作系统/CPU信息
` # uname -a `
` # head -n 1 /etc/issue` # 查看操作系统版本

`# cat /etc/redhat-release` # 查看操作系统版本（redhat，centos，fedora）

`# cat /proc/cpuinfo` # 查看CPU信息

`# hostname` # 查看计算机名

`# lspci -tv`# 列出所有PCI设备

`# lsusb -tv `# 列出所有USB设备

<!-- more -->

`# lsmod `# 列出加载的内核模块

`# env` # 查看环境变量资源

`# free -m`# 查看内存使用量和交换区使用量

`# df -h` # 查看各分区使用情况

`# du -sh <目录名>` # 查看指定目录的大小

`# grep MemTotal /proc/meminfo` # 查看内存总量

`# grep MemFree /proc/meminfo` # 查看空闲内存量

`# uptime` # 查看系统运行时间、用户数、负载

`# cat /proc/loadavg` # 查看系统负载磁盘和分区

`# mount | column -t `# 查看挂接的分区状态

`# fdisk -l` # 查看所有分区

`# swapon -s` # 查看所有交换分区

`# hdparm -i /dev/hda` # 查看磁盘参数(仅适用于IDE设备)

`# dmesg | grep IDE` # 查看启动时IDE设备检测状况网络

`# ifconfig` # 查看所有网络接口的属性

`# iptables -L` # 查看防火墙设置

`# route -n` # 查看路由表

`# netstat -lntp` # 查看所有监听端口

`# netstat -antp` # 查看所有已经建立的连接

`# netstat -s` # 查看网络统计信息进程

`# ps -ef` # 查看所有进程

`# top` # 实时显示进程状态用户

`# w` # 查看活动用户

`# id <用户名>` # 查看指定用户信息

`# last` # 查看用户登录日志

`# cut -d: -f1 /etc/passwd` # 查看系统所有用户

`# cut -d: -f1 /etc/group` # 查看系统所有组

`# crontab -l` # 查看当前用户的计划任务服务

`# chkconfig –list` # 列出所有系统服务

`# chkconfig –list | grep on` # 列出所有启动的系统服务程序

`# rpm -qa` # 查看所有安装的软件包

# VI模式切换和退出

## 进入vi编辑文件内容：

vi 文件名

vi 目录/文件名

vi进入编辑模式：

Shift+字母i

## 从编辑模式返回命令模式：

ESC

## 从命令模式vi退出：

:w 保存文件但不退出vi

:w file 将修改另外保存到file中，不退出vi

:w! 强制保存，不推出vi

:wq 保存文件并退出vi

:wq! 强制保存文件，并退出vi

:q 不保存文件，退出vi

:q! 不保存文件，强制退出vi

:e! 放弃所有修改，从上次保存文件开始再编辑

*注意：千万别将英文冒号”:”敲成中文冒号”：”。*

VI前进后退操作

在vi中按u可以撤销一次操作

u 撤销上一步的操作

Ctrl+r 恢复上一步被撤销的操作

注意：

如果你输入“u”两次，你的文本恢复原样，那应该是你的Vim被配置在Vi兼容模式了。

# 重做

如果你撤销得太多，你可以输入CTRL-R（redo）回退前一个命令。换句话说，它撤销一个撤销。要看执行的例子，输入CTRL-R两次。字符A和它后面的空格就出现了：

young intelligent turtle

有一个特殊版本的撤销命令：“U”（行撤销）。行撤销命令撤销所有在前一个编辑行

上的操作。 输入这些命令两次取消前一个“U”：

A very intelligent turtle

xxxx 删除very

A intelligent turtle

xxxxxx 删除turtle

A intelligent

用“U”恢复行

A very intelligent turtle

用“u”撤销“U”

A intelligent

“U”命令自己改变自己，“u”命令撤销操作，CTRL-R命令重做操作。这有点乱，但不用

担心，用“u”和CTRL-R命令你可以切换到任何状态。

流行的文本编辑器通常都有前进和后退功能，可以在文件中曾经浏览过的位置之间来回移动。在 vim 中使用 Ctrl-O 执行后退，使用 Ctrl-I 执行前进。

相关帮助： :help CTRL-O :help CTRL-I :help jump-motions

# VI查找内容

使用vi编辑器编辑长文件时，常常是头昏眼花，也找不到需要更改的内容。

这时，使用查找功能尤为重要。

方法如下：

1、命令模式下输入“/字符串”，例如“/Section 3”。

2、如果查找下一个，按“n”即可。

要自当前光标位置向上搜索，请使用以下命令：

/pattern Enter

其中，pattern表示要搜索的特定字符序列。

要自当前光标位置向下搜索，请使用以下命令：

?pattern Enter

# Linux文件权限介绍

r:read就是读权限 –数字4表示

w:write就是写权限 –数字2表示

x:excute就是执行权限 –数字1表示

权限表示一共需要占9个字符的：— — —

前三个字符表示用户权限，中间三个表示用户所在的组权限，最后三个表示其他用户权限

例子：rw-r–r–

rw-是说用户有读、写权，没有运行权，接着的r–表示用户组只有读权限，没有写入、运行权，最后的r–指其他用户只有读权限，没有写入、运行权。

用数字表示就是644。

例子：777

777就是rwxrwxrwx，意思是用户、用户组和其他用户都有最高权限。
Linux下scp的用法

#scp命令

scp就是secure copy，一个在linux下用来进行远程拷贝文件的命令。

有时我们需要获得远程服务器上的某个文件，该服务器既没有配置ftp服务器，也没有做共享，无法通过常规途径获得文件时，只需要通过简单的scp命令便可达到目的。

## 将本机文件复制到远程服务器上

> #scp /home/administrator/news.txt root@192.168.6.129:/etc/squid

/home/administrator/      本地文件的绝对路径

news.txt                          要复制到服务器上的本地文件

root                                 通过root用户登录到远程服务器（也可以使用其他拥有同等权限的用户）

192.168.6.129                远程服务器的ip地址（也可以使用域名或机器名）

/etc/squid                       将本地文件复制到位于远程服务器上的路径


## 将远程服务器上的文件复制到本机

> #scp remote@www.abc.com:/usr/local/sin.sh /home/administrator

remote                       通过remote用户登录到远程服务器（也可以使用其他拥有同等权限的用户）

www.abc.com              远程服务器的域名（当然也可以使用该服务器ip地址）

/usr/local/sin.sh           欲复制到本机的位于远程服务器上的文件

/home/administrator  将远程文件复制到本地的绝对路径

注意两点：

1.如果远程服务器防火墙有特殊限制，scp便要走特殊端口，具体用什么端口视情况而定，命令格式如下：

> #scp -p 4588 remote@www.abc.com:/usr/local/sin.sh /home/administrator

2.使用scp要注意所使用的用户是否具有可读取远程服务器相应文件的权限。

# 压缩和解压

## zip命令

　　解压1：unzip FileName.zip

　　解压2：unzip FileName.zip -d TargetDir

　　压缩1：zip -r TargetDir/FileName.zip DirName

　　压缩2：zip -r TargetDir/FileName.zip DirName/*

　　压缩3：zip -r TargetDir/FileName.zip File1 File2 File3

　　注意，zip解压适用于解压jar文件

## tar命令

　　解包：tar zxvf FileName.tar

　　打包：tar czvf FileName.tar DirName

## .tar.gz 和 .tgz

　　解压：tar zxvf FileName.tar.gz

　　压缩：tar zcvf FileName.tar.gz DirName

## .tar.bz

　　解压：tar jxvf FileName.tar.bz

## .tar.bz2

　　解压：tar jxvf FileName.tar.bz2

　　压缩：tar jcvf FileName.tar.bz2 DirName

## .tar.Z

　　解压：tar Zxvf FileName.tar.Z

　　压缩：tar Zcvf FileName.tar.Z DirName

## gz命令

　　解压1：gunzip FileName.gz

　　解压2：gzip -d FileName.gz

　　压缩：gzip FileName

## bz2命令

　　解压1：bzip2 -d FileName.bz2

　　解压2：bunzip2 FileName.bz2

　　压缩： bzip2 -z FileName

## bz命令

　　解压1：bzip2 -d FileName.bz

　　解压2：bunzip2 FileName.bz

## Z命令

　　解压：uncompress FileName.Z

　　压缩：compress FileName

## 7z命令(如果没有7z命令请先执行“yum install p7zip”)

解压：7z x FileName.7z -o/home/zhichao

//注意，x参数解压会自动生成与压缩文件名相同的目录，-o表示输出目录，其与目录路径之间没有空格，上面命令会解压FileName.7z到home/zhichao/FileName目录中。

压缩：7z a FileName.7z DirName

# grep命令

grep命令使用正则表达式检索符合条件的内容，一般与其他命令组合使用。

格式：

grep 参数 条件

参数：
```
-a --text #不要忽略二进制的数据。

-A<显示行数> --after-context=<显示行数> #除了显示符合范本样式的那一列之外，并显示该行之后的内容。

-b --byte-offset #在显示符合样式的那一行之前，标示出该行第一个字符的编号。

-B<显示行数> --before-context=<显示行数> #除了显示符合样式的那一行之外，并显示该行之前的内容。

-c --count #计算符合样式的列数。

-C<显示行数> --context=<显示行数>或-<显示行数> #除了显示符合样式的那一行之外，并显示该行之前后的内容。

-d <动作> --directories=<动作> #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。

-e<范本样式> --regexp=<范本样式> #指定字符串做为查找文件内容的样式。

-E --extended-regexp #将样式为延伸的普通表示法来使用。

-f<规则文件> --file=<规则文件> #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。

-F --fixed-regexp #将样式视为固定字符串的列表。

-G --basic-regexp #将样式视为普通的表示法来使用。

-h --no-filename #在显示符合样式的那一行之前，不标示该行所属的文件名称。

-H --with-filename #在显示符合样式的那一行之前，表示该行所属的文件名称。

-i --ignore-case #忽略字符大小写的差别。

-l --file-with-matches #列出文件内容符合指定的样式的文件名称。

-L --files-without-match #列出文件内容不符合指定的样式的文件名称。

-n --line-number #在显示符合样式的那一行之前，标示出该行的列数编号。

-q --quiet或--silent #不显示任何信息。

-r --recursive #此参数的效果和指定“-d recurse”参数相同。

-s --no-messages #不显示错误信息。

-v --revert-match #显示不包含匹配文本的所有行。

-V --version #显示版本信息。

-w --word-regexp #只显示全字符合的列。

-x --line-regexp #只显示全列符合的列。

-y #此参数的效果和指定“-i”参数相同。
```
示例：

从文件info.txt中读取关键词system进行搜索

cat /home/usr/download/info.txt | grep -f system

从文件info.txt中读取关键词system进行搜索并显示行号

cat /home/usr/download/info.txt | grep -nf system

# cat命令

cat命令用来显示文件内容，或者将几个文件连接起来显示，或者从标准输入读取内容并显示，它常与重定向符号配合使用。

格式：

cat 参数 文件

参数：
```
-A, --show-all  等价于 -vET

-b, --number-nonblank 对非空输出行编号

-e   等价于 -vE

-E, --show-ends  在每行结束处显示 $

-n, --number 对输出的所有行编号,由1开始对所有输出的行数编号

-s, --squeeze-blank 有连续两行以上的空白行，就代换为一行的空白行

-t   与 -vT 等价

-T, --show-tabs  将跳格字符显示为 ^I

-u   (被忽略)

-v, --show-nonprinting 使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外
```
示例：

把 log.php 的文件内容加上行号后输出到new_log.php这个文件中

cat -n /data/www/project/log.php /data/www/project/new_log.php

# ps命令

ps命令是process status的缩写，顾名思义就是查看系统中所有进程状态状态。

ps命令查看的程序状态并不是动态连续的，如果想对进程实施监控，请用top命令。

如果直接用ps命令，会显示所有进程的状态，通常结合grep命令查看某进程的状态。

格式：

ps 参数

参数：
```xml
a 显示所有进程

-a 显示同一终端下的所有程序

-A 显示所有进程

c 显示进程的真实名称

-N 反向选择

-e 等于“-A”

e 显示环境变量

f 显示程序间的关系

-H 显示树状结构

r 显示当前终端的进程

T 显示当前终端的所有程序

u 指定用户的所有进程

-au 显示较详细的资讯

-aux 显示所有包含其他使用者的行程
```
示例：

显示java相关程序

ps aux|grep java

显示httpd相关程序

ps aux|grep httpd

查看正在使用8080端口的程序

ps -ef|grep 8080

# fuser 命令

fuser用于查找正在使用指定的某个文件、挂载点、网络端口的进程的详细信息。

格式：

fuser 参数 文件或挂载点或网络端口

参数：
```xml
a 显示所有命令行中指定的文件，默认情况下被访问的文件才会被显示。

-c 和-m一样，用于POSIX兼容。

-d 暗示使用了 -c 和 -x 标志。关于任何与文件系统（自父目录删除的）无链接的打开文件的报告。当与 -V 标志一起使用时，它也会报告被删除文件的节点号和大小。

-f 仅对文件的打开实例报告。

-i 杀掉进程之前询问用户，如果没有-k这个选项会被忽略。

-k 杀掉访问文件的进程。如果没有指定-signal就会发送SIGKILL信号。

-l 列出所有已知的信号名称。

-m name 指定一个挂载文件系统上的文件或者被挂载的块设备（名称name）。这样所有访问这个文件或者文件系统的进程都会被列出来。如果指定的是一个目录会自动转换成,并使用所有挂载在那个目录下面的文件系统。

-n space 指定一个不同的命名空间(space).这里支持不同的空间文件(文件名，此处默认)、tcp(本地tcp端口)、udp(本地udp端口)。对于端口， 可以指定端口号或者名称，如果不会引起歧义那么可以使用简单表示的形式，例如：name/space (即形如:80/tcp之类的表示)。

-s 静默模式，这时候-u,-v会被忽略。-a不能和-s一起使用。

-signal 使用指定的信号，而不是用SIGKILL来杀掉进程。可以通过名称或者号码来表示信号(例如-HUP,-1),这个选项要和-k一起使用，否则会被忽略。

-u 在每个PID后面添加进程拥有者的用户名称。

-v 详细模式。输出似ps命令的输出，包含PID,USER,COMMAND等许多域,如果是内核访问的那么PID为kernel. -V 输出版本号。

-x 与 -c 或 -f 连用，报告除标准 fuser 输出以外的可执行的和可载入的对象。

-4 使用IPV4套接字,不能和-6一起应用，只在-n的tcp和udp的命名存在时不被忽略。

-6 使用IPV6套接字,不能和-4一起应用，只在-n的tcp和udp的命名存在时不被忽略。
```
示例：

查看和关闭正在使用readme.txt文件的程序

fuser -m -v readme.txt

fuser -m -k readme.txt

查看和关闭正在使用80端口的程序

fuser -n -v tcp 80

fuser -n -k tcp 80

chown命令

chown命令将指定文件或文件夹的拥有者改为指定的用户或组，用户可以是用户名或者用户ID，组可以是组名或者组ID，文件是以空格分开的要改变权限的文件列表，支持通配符。

格式：

# chown 参数 用户 文件

参数：
```xml
-c 显示更改的部分的信息

-f 忽略错误信息

-h 修复符号链接

-R 处理指定目录以及其子目录下的所有文件

-v 显示详细的处理信息

-deference 作用于符号链接的指向，而不是链接文件本身
```
示例：

将目录/data/www/project/下的所有文件和目录的拥有者都修改为apache账户。

chown -R apache.apache /data/www/project/

# chmod命令

chmod命令用于改变linux系统文件或目录的访问权限，用它控制文件或目录的访问权限。

Linux系统中的每个文件和目录都有访问许可权限，用它来确定谁可以通过何种方式对文件和目录进行访问和操作。

格式：

chmod 参数 权限模式 文件

参数
```
-c 当发生改变时，报告处理信息

-f 错误信息不输出

-R 处理指定目录以及其子目录下的所有文件

-v 运行时显示详细处理信息
```
示例：

将目录/data/www/project/下的所有文件和目录的访问权限修改为“当前用户、当前用户组和其他用户都可读可写可执行”的权限。

chown -R 777 /data/www/project/

# kill命令

kill命令用于关闭指定进程。

格式：

kill 参数 信号 进程ID

参数：
```xml
-s 指定发送信号

-l 根据信号名显示信号编号，若果不加信号的编号参数，则使用“-l”参数会列出全部的信号名称

-a 当处理当前进程时，不限制命令名和进程号的对应关系

-p 指定kill 命令只打印相关进程的进程号，而不发送任何信号

-u 指定用户
```
# 信号：

## 信号名 信号编号 信号含义

HUP 1 终端断线

INT 2 中断

QUIT 3 退出

TERM 15 终止

KILL 9 强制终止

CONT 18 继续（与STOP相反）

STOP 19 暂停（

示例：

显示信号KILL的信号编号

kill -l KILL

关闭apache用户下所有进程

kill -u apache.apache

强制关闭进程ID为1937的进程，参数s可以省略。

kill -s 9 1937

kill -9 1937

# top命令

top命令查看系统中各个进程的资源占用状况，默认使用cpu占用来排序显示，top命令显示的结果是实时的。

格式：

top 参数

参数：
```xml
-b 批处理

-c 显示完整的治命令

-i<时间> 设置间隔时间

-I 忽略失效过程

-p<进程号> 指定进程

-n<次数> 循环显示的次数

-s 保密模式

-S 累积模式

-u<用户名> 指定用户名
```
示例：

查看所有程序资源占用状况

top

查看apache用户所有程序资源占用状况

top -u apache.apache

# find命令

在指定目录中查找指定条件的文件或文件夹。

格式：

find 目录路径 选项 查找条件

选项：
```xml
-name 按照文件名查找文件。

-perm 按照文件权限来查找文件。

-prune 使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略。

-user 按照文件属主来查找文件。

-group 按照文件所属的组来查找文件。

-mtime -n +n 按照文件的更改时间来查找文件， - n表示文件更改时间距现在n天以内，+ n表示文件更改时间距现在n天以前。find命令还有-atime和-ctime 选项，但它们都和-m time选项。

-nogroup 查找无有效所属组的文件，即该文件所属的组在/etc/groups中不存在。

-nouser 查找无有效属主的文件，即该文件的属主在/etc/passwd中不存在。

-newer file1 ! file2 查找更改时间比文件file1新但比文件file2旧的文件。

-type<类型参数> 查找某一类型的文件，诸如：

b - 块设备文件。

d - 目录。

c - 字符设备文件。

p - 管道文件。

l - 符号链接文件。

f - 普通文件。

-size n：[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。

-fstype：查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。

-mount：在查找文件时不跨越文件系统mount点。

-follow：如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。

-cpio：对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。

-amin n 查找系统中最后N分钟访问的文件

-atime n 查找系统中最后n*24小时访问的文件

-cmin n 查找系统中最后N分钟被改变文件状态的文件

-ctime n 查找系统中最后n*24小时被改变文件状态的文件

-mmin n 查找系统中最后N分钟被改变文件数据的文件

-mtime n 查找系统中最后n*24小时被改变文件数据的文件
```
示例：

查找 “/”目录（Linux系统根目录）下名称为httpd.conf的文件

find / -name httpd.conf

查找 “/data/www/project/”目录下所有php文件

find /data/www/project -name *.php

查找 “.”目录（当前目录）下权限为777的文件

find . -perm 777

查找 “/”目录下48小时内修改过的文件

find / -atime -2

查找 “/”目录下所有文件夹

find / -type d

查找 “/”目录下大于1kb的文件

find / -size +1000c

# whereis命令

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、说明文件（参数-m）和源代码文件（参数-s），如果省略参数，则返回所有信息。

和find相比，whereis查找的速度非常快，这是因为linux系统会将 系统内的所有文件都记录在一个数据库文件中，当使用whereis和下面即将介绍的locate时，会从数据库中查找数据，而不是像find命令那样，通 过遍历硬盘来查找，效率自然会很高。

但是该数据库文件并不是实时更新，默认情况下时一星期更新一次，因此，我们在用whereis和locate 查找文件时，有时会找到已经被删除的数据，或者刚刚建立文件，却无法查找到，原因就是因为数据库文件没有被更新。

whereis常用于查找安装程序的安装目录。

格式：

whereis 参数 程序名

参数：
```xml
-b 定位可执行文件。

-m 定位帮助文件。

-s 定位源代码文件。

-u 搜索默认路径下除可执行文件、源代码文件、帮助文件以外的其它文件。

-B 指定搜索可执行文件的路径。

-M 指定搜索帮助文件的路径。

-S 指定搜索源代码文件的路径。
```
示例：

查找mysql安装目录

whereis mysql

# ls命令

ls命令用来查看目标目录中所有的子目录和文件信息。

格式：

ls 参数 目录名

参数：
```xml
-a, –all 列出目录下的所有文件，包括以 . 开头的隐含文件

-A 同-a，但不列出“.”(表示当前目录)和“..”(表示当前目录的父目录)。

-c 配合 -lt：根据 ctime 排序及显示 ctime (文件状态最后更改的时间)配合 -l：显示 ctime 但根据名称排序否则：根据 ctime 排序

-C 每栏由上至下列出项目

–color[=WHEN] 控制是否使用色彩分辨文件。WHEN 可以是'never'、'always'或'auto'其中之一

-d, –directory 将目录象文件一样显示，而不是显示其下的文件。

-D, –dired 产生适合 Emacs 的 dired 模式使用的结果

-f 对输出的文件不进行排序，-aU 选项生效，-lst 选项失效

-g 类似 -l,但不列出所有者

-G, –no-group 不列出任何有关组的信息

-h, –human-readable 以容易理解的格式列出文件大小 (例如 1K 234M 2G)

–si 类似 -h,但文件大小取 1000 的次方而不是 1024

-H, –dereference-command-line 使用命令列中的符号链接指示的真正目的地

–indicator-style=方式 指定在每个项目名称后加上指示符号<方式>：none (默认)，classify (-F)，file-type (-p)

-i, –inode 印出每个文件的 inode 号

-I, –ignore=样式 不印出任何符合 shell 万用字符<样式>的项目

-k 即 –block-size=1K,以 k 字节的形式表示文件的大小。

-l 除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来。

-L, –dereference 当显示符号链接的文件信息时，显示符号链接所指示的对象而并非符号链接本身的信息

-m 所有项目以逗号分隔，并填满整行行宽

-o 类似 -l,显示文件的除组信息外的详细信息。

-r, –reverse 依相反次序排列

-R, –recursive 同时列出所有子目录层

-s, –size 以块大小为单位列出所有文件的大小

-S 根据文件大小排序

–sort=WORD 以下是可选用的 WORD 和它们代表的相应选项：

extension -X status -c

none -U time -t

size -S atime -u

time -t access -u

version -v use -u

-t 以文件修改时间排序

-u 配合 -lt:显示访问时间而且依访问时间排序

配合 -l:显示访问时间但根据名称排序

否则：根据访问时间排序

-U 不进行排序;依文件系统原有的次序列出项目

-v 根据版本进行排序

-w, –width=COLS 自行指定屏幕宽度而不使用目前的数值

-x 逐行列出项目而不是逐栏列出

-X 根据扩展名排序

-1 每行只列出一个文件

–help 显示此帮助信息并离开

–version 显示版本信息并离开
```
示例：

显示当前目录下所有文件和子目录（包括隐藏文件和隐藏目录）的详细信息

ls -a -l

显示”/data/www/project”目录下所有文件和子目录（不包括隐藏文件和隐藏目录）的详细信息

ls -l /data/www/project

显示”/data/www/project”目录下所有文件和子目录（包括隐藏文件和隐藏目录）的名称

ls -a /data/www/project

# sudo命令

sudo命令是个管理员分配给一些linux用户执行一些没有权限的命令。

如果发现执行linux命令提示没有权限什么balabala的，不管什么命令，前面先加上sudo试试。

格式：无需了解

参数：无需了解

示例：

sudo yum install xxxx

sudo svn up xxxx

sudo rm -r xxxx

# tail命令

tail命令用于显示指定文件的末尾内容，并且使用-f参数可以查看即时的文件内容，常用查看日志文件。

格式：

tail 参数 文件

参数：
```xml
-f 循环读取

-q 不显示处理信息

-v 显示详细的处理信息

-c<数目> 显示的字节数

-n<行数> 显示行数

--pid=PID 与-f合用,表示在进程ID,PID死掉之后结束.

-q, --quiet, --silent 从不输出给出文件名的首部

-s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒
```
示例：

及时显示/data/www/project目录下log.php文件的末尾内容

tail -f /data/www/project/log.php

及时显示/data/www/project目录下log.php文件的500行末尾内容

tail -f -n 500 /data/www/project/log.php

# cp命令

cp命令用于复制文件或目录。

格式：

cp 参数 原文件或原目录 目标文件或目录

参数：
```xml
-a或--archive 等于-dR或-dpR

--backup 为每个已存在的目标文件创建备份

-b 类似--backup但不接受参数

--copy-contents 在递归处理是复制特殊文件内容

-d或--no-dereference或--preserve=links 若源文件为连接文件属性，则复制连接文件指向的文件而非复制文件本身

-s或--symbolic-link 对源文件建立符号链接，而非复制文件

-f或--force 强制操作，如果目标文件无法打开则将其移除并重试(当-n选项存在时则不需再选此项)

-i或--interactive 覆盖前询问(使前面的-n选项失效)

-H 跟随源文件中的命令行符号链接

-l或--link 链接文件而不复制

-L或--dereference 总是跟随符号链接

-n或--no-clobber 不要覆盖已存在的文件(使前面的-i选项失效)

-P或----parents 保留源文件或目录的路径，此路径可以是绝对路径或相对路径，且目的目录必须已经存在

-p等于--preserve 保留源文件或目录的属性，包括所有者、所属组、权限与时间

--preserve[=属性列表 保持指定的属性(默认：模式,所有权,时间戳)，如果可能保持附加属性：环境、链接等

-R或-r或--recursive 递归处理，复制目录及目录内的所有项目

-u或--update 使用这项参数之后，只会在源文件的修改时间(Modification Time)较目的文件更新时，或是名称相互对应的目的文件并不存在，才复制文件

-v或--verbose 显示执行过程

-x或--one-file-system 复制的文件或目录存放的文件系统，必须与cp指令执行时所处的文件系统相同，否则不复制，亦不处理位于其他分区的文件
```
示例：

强行复制文件index.html到目录/data/www/project

cp -f -v /home/usr/download/index.html /data/www/project

同：cp -fv /home/usr/download/index.html /data/www/project

# rm命令

rm命令和cp命令类似。

示例：

删除download目录

rm -rf /home/usr/download

删除download目录下的所有文件和子目录，但保留download目录

rm -rf /home/usr/download/*

# mv命令

mv命令和cp命令类似。

示例：

移动download目录中的所有子目录和文件到/data/www/project 目录，但保留download目录

mv -rf /home/usr/download/* /data/www/project

# rpm命令

rpm是RedHat Package Manager（RedHat软件包管理工具）的简称。

.rpm执行安装包分为二进制包（Binary）以及源代码包（Source）两种，二进制包可以直接安装在计算机中，而源代码包将会由RPM自动编译、安装。源代码包经常以src.rpm作为后缀名。

常用参数：
```xml
－ivh：安装显示安装进度–install–verbose–hash

－Uvh：升级软件包–Update；

－qpl：列出RPM软件包内的文件信息[Query Package list]；

－qpi：列出RPM软件包的描述信息[Query Package install package(s)]；

－qf：查找指定文件属于哪个RPM软件包[Query File]；

－Va：校验所有的RPM软件包，查找丢失的文件[View Lost]；

－e：删除包

-q samba ：查询程序是否安装
```
常用命令：

install/upgrade/remove：安装/更新/卸载

# yum命令

Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

常用参数：
```
-e 静默执行

-t 忽略错误

-R[分钟] 设置等待时间

-y 自动应答yes

–skip-broken 忽略依赖问题

–nogpgcheck 忽略GPG验证
```
# 全部命令：

check-update 检查可更新的包

clearn 清除全部

clean packages 清除临时包文件（/var/cache/yum 下文件）

clearn headers 清除rpm头文件

clean oldheaders 清除旧的rpm头文件

deplist 列出包的依赖

list 可安装和可更新的RPM包

list installed 已安装的包

list extras 已安装且不在资源库的包

info 可安装和可更新的RPM包 信息

info installed 已安装包的信息(-qa 参数相似)

install[RPM包] 安装包

localinstall 安装本地的 RPM包

update[RPM包] 更新包

upgrade 升级系统

search[关键词] 搜索包

provides[关键词] 搜索特定包文件名

reinstall[RPM包] 重新安装包

repolist 显示资源库的配置

resolvedep 指定依赖

remove[RPM包] 卸载包

如果要安装指定版本或指定插件，则需要详细文件名称：

[zhichao@localhost ~]$ sudo yum list java*

java-1.8.0-openjdk.x86_64 1:1.8.0.45-36.b13.fc22 @System

java-1.8.0-openjdk-devel.x86_64 1:1.8.0.45-36.b13.fc22 @System

java-1.8.0-openjdk-headless.x86_64 1:1.8.0.45-36.b13.fc22 @System

sudo yum install java-1.8.0-openjdk-devel.x86_64

apt-get命令

apt-get（全称Advanced Package Tool）的作用同于yum，不过使用该命令的操作系统为Ubuntu和Debian。

经过修改后可以使用apt-rpm处理红帽的Package Manager（RPM）文件。

常用命令：

apt-get install packagename

安装一个新软件包

apt-get remove packagename

卸载一个已安装的软件包（保留配置文档）

apt-get remove –purge packagename

卸载一个已安装的软件包（删除配置文档）

apt-get autoremove packagename

删除包及其依赖的软件包

apt-get autoremove –purge packagname

删除包及其依赖的软件包+配置文件，比上面的要删除的彻底一点

dpkg –force-all –purge packagename

有些软件很难卸载，而且还阻止了别的软件的应用，就能够用这个，但是有点冒险。

dnf命令

dnf是fedora22新的用于替换yum和rpm的命令。

在使用yum时，会提示过时，但仍然可用。

详细了解请看一下这篇文章：[http://www.linuxidc.com/Linux/2015-06/118751.htm](http://www.linuxidc.com/Linux/2015-06/118751.htm)

。

systemctl命令

在cenos7和fedora22中，systemctl替换了service命令。

写法从以前的：service start mysqld

变成了：systemctl start mysqld.service

详细了解请看一下这篇文章：http://www.linuxidc.com/Linux/2015-07/120833.htm

。

或者：

https://wiki.archlinux.org/index.php/Systemd_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

pwd命令

pwd命令可以获取当前目录的全路径。

示例：

$ pwd

/home/zhichao/下载
# 终端下的快捷键

Ctrl+a：光标回到命令行首。 （a：ahead）﻿

Ctrl+e：光标回到命令行尾。 （e：end）

Ctrl+b：光标向行首移动一个字符。 （b：backwards）

Ctrl+f：光标向行尾移动一个字符。 （f：forwards）

Ctrl+w: 删除光标处到行首的字符。

Ctrl+k：删除光标处到行尾的字符。

Ctrl+u：删除整个命令行文本字符。

Ctrl+h：向行首删除一个字符。

Ctrl+d：向行尾删除一个字符。

Ctrl+y:：粘贴Ctrl+u，Ctrl+k，Ctrl+w删除的文本。

Ctrl+p: 上一个使用的历史命令。 （或者直接按向上方向键；p：previous）

Ctrl+n： 下一个使用的历史命令。（或者直接按向下方向键；n：next）

Ctrl+r：快速检索历史命令。（r：retrieve）。

Ctrl+t： 交换光标所在字符和其前的字符。

Ctrl+i：相当于Tab键。

Ctrl+o：相当于Ctrl+m.

Ctrl+m：相当Enter键。

# 其他控制键：

Ctrl+s:使终端发呆，静止，可以使快速输出的终端屏幕停下来。

Ctrl+q：退出Ctrl+s引起的发呆。

Ctrl+z：使正在运行在终端的任务，运行于后台。 （可用fg恢复）

Ctrl+c：中断终端中正在执行的任务。

Ctrl+d: 在空命令行的情况下可以退出终端。

Ctrl+[ ：相当于Esc键。

Esc键：连续按3次显示所有的支持的终端命令。

Tab键：命令、文件名等自动补全命令或目录、文件名。

alt+/：同Tab键
