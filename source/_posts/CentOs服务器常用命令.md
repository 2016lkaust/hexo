title: Centos服务器常用命令
categories: Linux
tags:
- note
- Centos
date: 2017-4-14 15:23
---


首先要进入root用户
> su

<!-- more -->

# ps
查看某端口是否被占用(如查看8088）
> ps -ef|grep 8088

显示java相关程序
> ps aux|grep java

# kill

kill命令用于关闭指定进程。

格式：

kill 参数 信号 进程ID

参数：

-s 指定发送信号

-l 根据信号名显示信号编号，若果不加信号的编号参数，则使用“-l”参数会列出全部的信号名称

-a 当处理当前进程时，不限制命令名和进程号的对应关系

-p 指定kill 命令只打印相关进程的进程号，而不发送任何信号

-u 指定用户
信号：

信号名 信号编号 信号含义

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
# 查看端口是否被占用 
[参考文章](http://www.cnblogs.com/ggjucheng/archive/2012/01/08/2316661.html)
### 1.显示所有端口和所有对应的程序
> netstat -tunlp

### 2.用grep管道可以过滤出想要的关键字段。
> netstat -tunlp |grep 5432

*参数*
```xml
-a (all)显示所有选项，默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化成数字。
-l 仅列出有在 Listen (监听) 的服務状态

-p 显示建立相关链接的程序名
-r 显示路由信息，路由表
-e 显示扩展信息，例如uid等
-s 按各个协议进行统计
-c 每隔一个固定时间，执行该netstat命令。
```
![测试](http://upload-images.jianshu.io/upload_images/1837782-8b4c29d48645f4e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.查看某一端口的占用情况：
> lsof -i:端口号

![测试](http://upload-images.jianshu.io/upload_images/1837782-ff6376cd592a8a5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# tomcat
修改端口为8088端口。
`tomcat7/conf/server.xml` 文件里的：
```xml
<Connector port="8080" 
protocol="HTTP/1.1"  connectionTimeout="20000"  redirectPort="8443" /> 
```
将`port="8080"`改为`port="8088"`，保存。
** 如果想要添加远程访问的话，需要修改防火墙设置。修改方法参看下面防火墙部分**
`-A INPUT -m state --state NEW -M tcp -p tcp --dport 8088 -j ACCEPT#开放tomcat端口`

**安装好后的启动和关闭方式：**
> sh /opt/apache-tomcat-8.0.23/bin/startup.sh
sh /opt/apache-tomcat-8.0.23/bin/shutdown.sh

可以看出，路径太长，不好启动，所以将其设置成**以服务方式启动**，以后再启动就用⑥中的方式操作。
①. 在/etc/init.d目录下新建文件，命名为tomcat2. 对tomcat文件进行编辑，执行
 >   cd /etc/init.d/
 >  vi tomcat

将下面代码粘上去
```xml
#!/bin/bash
# description: Tomcat7 Start Stop Restart
# processname: tomcat7
# chkconfig: 234 20 80
# JAVA_HOME要根据jdk的具体位置修改

JAVA_HOME=/opt/jdk1.7.0_80
export JAVA_HOME
PATH=$JAVA_HOME/bin:$PATH
export PATH
CATALINA_HOME=/opt/apache-tomcat-8.0.23
case $1 in
start)
sh $CATALINA_HOME/bin/startup.sh
;;
stop)
sh $CATALINA_HOME/bin/shutdown.sh
;;
restart)
sh $CATALINA_HOME/bin/shutdown.sh
sh $CATALINA_HOME/bin/startup.sh
;;
esac
exit 0
```
②. 按ESC退出，并
> ：wq

③. 设置tomcat的文件属性，把tomcat 修改为可运行的文件，命令参考如下
> chmod a+x tomcat

④. 添加服务
> chkconfig --add tomcat

⑤. 服务就添加成功了
然后用 chkconfig --list 查看，在服务列表里就会出现自定义的服务了
>  chkconfig --list

⑥. 测试
> service tomcat start
service tomcat stop
service tomcat restart
service tomcat status

# mysql
安装后需要更改初始密码（初始密码很复杂）
> grep 'temporary password' /var/log/mysqld.log

看到密码后进入mysql修改密码
> mysql -uroot -p（回车后输入密码）

进入后修改密码为123456
> mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

默认只允许root帐户在本地登录，如果要在其它机器上连接mysql，必须修改root允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，添加一个新的帐户：
> mysql> GRANT ALL PRIVILEGES ON *.* TO 'look'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;

修改防火墙设置（*参照“防火墙-更改防火墙配置”部分*）
`-A INPUT -m state --state NEW -M tcp -p tcp --dport 3306 -j ACCEPT#开放mysql端口`
启动mysql
> systemctl start mysqld

或者
>  service mysqld start

关闭mysql
> systemctl stop mysqld

或者
>  service mysqld stop

常用sql语句
```sql
进入
mysql -u root -p
创建数据库
create database test;
use test;
create table a(
id int(4) not null,
name varchar(10),
primary ker(id)
) default charset=utf8;
```
# postgreSQL
**常用命令**
启动
> systemctl start postgresql-9.5.service

登录
> su - postgres
psql -U postgres


安装过程
1.添加RPM    
> yum install https://download.postgresql.org/pub/repos/yum/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm

2.安装PostgreSQL 9.5    
> yum install postgresql95-server postgresql95-contrib

3.初始化数据库   
>  /usr/pgsql-9.5/bin/postgresql95-setup initdb

4.设置开机自启动(有时候没反应，但是继续向下还是可以进行)    
> systemctl enable postgresql-9.5.service

**5.启动服务**
> systemctl start postgresql-9.5.service

6.修改用户密码    
切换用户，执行后提示符会变为 '-bash-4.2$' 
> su - postgres  
 
登录数据库
> psql -U postgres

输入 `ALTER USER postgres WITH PASSWORD '123456'  `，设置用户`postgres `密码 `123456`  ，\q  退出数据库


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1837782-7d1a96228fbe6ba0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


7.开启远程访问   
> vi /var/lib/pgsql/9.5/data/postgresql.conf    

修改`#listen_addresses = 'localhost'  `为 ` listen_addresses='*'   ` 当然，此处‘*’也可以改为任何你想开放的服务器IP

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1837782-275cea19c66876b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8.信任远程连接   
> vi /var/lib/pgsql/9.5/data/pg_hba.conf    

修改如下内容，信任指定服务器连接    
`# IPv4 local connections:    `
`host    all            all      127.0.0.1/32      trust`  
` host    all            all      192.168.1.155/32  trust`

**注：**
```
192.168.1.155是指定这个ip可以连接到postgreSQL
     32 -> 192.168.1.155/32 表示必须是来自这个IP地址的访问才合法；
     24 -> 192.168.1.0/24 表示只要来自192.168.1.0 ~ 192.168.1.255的都合法；
     16 -> 192.168.0.0/16 表示只要来自192.168.0.0 ~ 192.168.255.255的都合法；
     8   -> 192.0.0.0/16 表示只要来自192.0.0.0 ~ 192.255.255.255的都合法；
     0   -> 0.0.0.0/0 表示全部IP地址都合法，/左边的IP地址随便了只要是合法的IP地址即可

trust
无条件地允许联接。这个方法允许任何可以与PostgreSQL 数据库服务器联接的用户以他们期望的任意 PostgreSQL 数据库用户身份进行联接，而不需要口令。
reject
联接无条件拒绝。常用于从一个组中"过滤"某些主机。
md5
要求客户端提供一个 MD5 加密的口令进行认证。
crypt
要求客户端提供一个 crypt() 加密的口令用于认证。 7.2 以前的客户端只能支持 crypt。 对于 7.2 以及以后的客户端，我们建议使用 md5。
password
要求客户端提供一个未加密的口令进行认证。 因为口令是以明文形式在网络上传递的， 所以我们不应该在不安全的网络上使用这个方式。
krb4
用 Kerberos V4 认证用户。只有在进行 TCP/IP 联接的时候才能用。  （译注：Kerberos，"克尔波洛斯"，故希腊神话冥王哈得斯的多头看门狗。 Kerberos 是 MIT 开发出来的基与对称加密算法的认证协议和/或密钥交换方法。 其特点是需要两个不同用途的服务器，一个用于认证身份， 一个用于通道两端用户的密钥交换。同时 Kerberos 对网络时间同步要求比较高，以防止回放攻击，因此通常伴随 NTP 服务。）
krb5
用 Kerberos V5 认证用户。只有在进行 TCP/IP 联接的时候才能用。  （译注：Kerberos V5 是上面 V4 的改良，主要是不再依赖 DES 算法， 同时增加了一些新特性。）
ident
获取客户的操作系统名（对于 TCP/IP 联接，用户的身份是通过与运行在客户端上的 ident 服务器联接进行判断的，对于本地联接，它是从操作系统获取的。） 然后检查一下，看看用户是否允许以要求的数据库用户进行联接， 方法是参照在 ident 关键字后面声明的映射。 
pam
使用操作系统提供的可插入的认证模块服务 （Pluggable Authentication Modules） （PAM）来认证。
```


远程连接配置完成，由于系统原因，还需要在防火墙中打开相应的端口。
9.修改防火墙配置，加上（*参照“防火墙-更改防火墙配置”部分*）
`-A INPUT -m state --state NEW -M tcp -p tcp --dport 5432 -j ACCEPT#开放postgreSQL端口`

**10. 重启PostgreSQL数据服务**
> systemctl restart postgresql-9.5.service


# 防火墙
查询防火墙状态:
> [root@localhost ~]# service   iptables status<回车>
 
停止防火墙:
> [root@localhost ~]# service   iptables stop <回车>
 
启动防火墙:
> [root@localhost ~]# service   iptables start <回车>
 
重启防火墙:
> [root@localhost ~]# service   iptables restart <回车>
 
永久关闭防火墙:
> [root@localhost ~]# chkconfig   iptables off<回车>
 
永久关闭后启用:
> [root@localhost ~]# chkconfig   iptables on<回车>

更改防火墙配置
> vi /etc/sysconfig/iptables 

```xml
	# sample configuration for iptables service
	# you can edit this manually or use system-config-firewall
	# please do not ask us to add additional ports/services to this default 	configuration
	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [0:0]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p icmp -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
	-A INPUT -m state --state NEW -M tcp -p tcp --dport 8088 -j ACCEPT#开放tomcat端口
	-A INPUT -m state --state NEW -M tcp -p tcp --dport 3306 -j ACCEPT#开放mysql端口
	-A INPUT -m state --state NEW -M tcp -p tcp --dport 5432 -j ACCEPT#开放	postgreSQL端口
	-A INPUT -j REJECT --reject-with icmp-host-prohibited
	-A FORWARD -j REJECT --reject-with icmp-host-prohibited
	COMMIT
```
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
