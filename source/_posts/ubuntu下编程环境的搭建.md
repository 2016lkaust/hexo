title: ubuntu下编程环境的搭建
categories: Linux
tags: ubuntu
date: 2016-10-31 21:00
---

[安装教程点击此处](http://www.linuxidc.com/Linux/2015-08/122140.htm)
安装好ubuntu之后，第一件事就是更新服务器列表来下载最新的软件
> sudo apt-get update

然后安装vim编辑器
> sudo apt-get install vim

接下来就是搭建Java，c等的开发环境。
<!-- more -->

#一、下载并安装jdk

![QQ截图20161031191627.png](http://upload-images.jianshu.io/upload_images/1837782-5e897bbc07664675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择下载目录`/opt/software`,下载到这里。
下载完成后进行安装。
1、打开终端，进入到该目录
> cd /opt/software

2.解压压缩包
> tar -zxvf jdk-8u111-linux-x64.tar.gz

3.配置环境变量
(1)首先进入profile所在的目录，并打开profile文件
> sudo vim /etc/profile


![进入目录](http://upload-images.jianshu.io/upload_images/1837782-1edf1c05fdd722b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

刚打开文件时的状态：


![1.png](http://upload-images.jianshu.io/upload_images/1837782-d598990eceda58f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



回车后输入`i`进入vim的编辑模式（和windows里面用记事本打开一个道理，只是需要输入i才由命令行进入编辑状态），此时显示插入两字

![插入状态](http://upload-images.jianshu.io/upload_images/1837782-b34a82e040813455.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


(2)将鼠标移动文件末尾，配置环境变量，即在文件末尾加上以下几句：
> export JAVA_HOME=/opt/software/java/jdk1.8.0_111
> export JRE_HOME=/opt/software/java/jdk1.8.0_111/jre
> export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
> export PATH=$JAVA_HOME/bin:$JRE_HOME/bin

(3)按键盘左上角Esc键退出编辑状态，输入
> :wq

回车，即退出vim编辑器并保存修改内容。
(4)使配置生效，输入：
> source /etc/profile

(5)验证，这个相信大家都会。成功后将显示jdk的版本号。
> javac -version

# 二、eclipse
我是在官网稀里糊涂的下载的，就在download的那里，系统会默认下载linux版本的。下载目录选择和jdk的一样，好找。
1、进入目录
> cd /opt/software

2、解压
> tar -zxvf eclipse........
注：省略号部分是完整名称，记不住没事，按Tab键就自动补全了。

3、输入
> ./eclipse

运行成功。
如果常用的话就将其固定在启动器（任务栏），获取按window徽标键，输入eclipse即可运行。

# 三、MySQL
##下载安装mysql
很方便，几句话。
> sudo apt-get install mysql-server
 
> sudo apt-get install mysql-client
 
> sudo apt-get install libmysqlclient-dev

可以参考这篇文章[ubuntu安装mysql和简单操作](http://www.cnblogs.com/zhuyp1015/p/3561470.html)

## 如何启动/停止/重启MySQL
（在终端输入）
1、使用 service 启动：service mysql start

2、使用 service 停止：service mysql stop

3、使用 service 重启：service mysql restart

[CentOS7 64位下MySQL5.7安装与配置（YUM）](http://www.linuxidc.com/Linux/2016-09/135288.htm)

# 四、c语言
(本段系引用别人的原话·[原文在这](http://blog.163.com/zhangjinqing1234@126/blog/static/307302602009538137113/))
linux操作系统上面开发程序， 光有了gcc 是不行的,它还需要一个   `build-essential`软件包。
作用是提供编译程序必须软件包的列表信息，也就是说，编译程序有了这个软件包它才知道头文件在哪， 才知道库函数在哪，还会下载依赖的软件包，最后才组成一个开发环境。
build-essential 安装方法：
> sudo apt-get install build-essential

安装后我测试了一下，很好。
在桌面新建一个c语言文件`first.c`
> cd 桌面
> vim first.c

输入i后进行编辑，输入以下代码
```
#include<stdio.h>
int main(){
printf("hello ubuntu");
return 0;
}
```
按`Esc`退出编辑模式，输入`:wq`回车保存退出。
输入下面一句进行编译
> gcc first.c -o first

再输入
> ./first


# 五、tomcat
官网下载，不多说了，选择tar.gz后缀的下载就行了。仍然放在`/opt/software`目录下。
(1)进入文件所在目录
> cd /opt/software

(2)解压
> tar -zxvf apache-tomcat-7.0.72

(3)修改配置文件
进到`apache-tomcat-7.0.72/bin`目录下
> cd bin

打开`catalina.sh `
> vim catalina.sh 

插入以下代码

```
JAVA_HOME=/opt/software/java/jdk1.8.0_111
 JAVA_OPTS="-server -Xms512m 
-Xmx1024m -XX:PermSize=600M
 -XX:MaxPermSize=600m
  -Dcom.sun.management.jmxremote" 

```
按下`Esc`，输入`:wq`保存退出。
(4)修改端口，习惯了用8080端口，默认是8009。
`tomcat7/conf/server.xml` 文件里的：
```
<Connector port="8009" 
protocol="HTTP/1.1"  connectionTimeout="20000"  redirectPort="8443" /> 
```
将`port="8009"`改为`port="8080"`，保存。
(5)在`apache-tomcat-7.0.72/bin`运行
> sudo ./startup.sh 

运行成功。
(6)以服务方式启动
①. 在/etc/init.d目录下新建文件，命名为tomcat2. 对tomcat文件进行编辑，执行**
 >   cd /etc/init.d/
 >  vi tomcat

将下面代码粘上去
``` 
#!/bin/bash  
# description: Tomcat7 Start Stop Restart  
# processname: tomcat7  
# chkconfig: 234 20 80  
JAVA_HOME=/opt/software/java/jdk1.8.0_111
export JAVA_HOME  
PATH=$JAVA_HOME/bin:$PATH  
export PATH  
CATALINA_HOME=/usr/local/tomcat
case $1 in  
start)  
sh $CATALINA_HOME/bin/startup.sh  
;;   
stop)     
sh $CATALINA_HOME/bin/shutdown.sh  
;;   
restart)  
sh $CATALINA_HOME/bin/shutdown.sh  
sh $CATALINA_HOME/bin/startup.sh  
;;   
esac      
exit 0
```
②. 按ESC退出，并
> ：wq

③. 设置tomcat的文件属性，把tomcat 修改为可运行的文件，命令参考如下
> chmod a+x tomcat

④. 设置服务运行级别
> chkconfig --add tomcat

⑤. 服务就添加成功了
然后用 chkconfig --list 查看，在服务列表里就会出现自定义的服务了
>  chkconfig --list

⑥. 测试
> service tomcat start
service tomcat stop
service tomcat restart
service tomcat status

# 六、jdbc连接桥
[下载地址](http://dev.mysql.com/downloads/connector/j/6.0.html)

下载*.tar.gz格式的文件，解压，将其中的`mysql-connector-java-6.0.5-bin.jar`文件拿出来，就和平常使用的一样了，加载到程序中就行了。
# 七、centOs安装postgreSQL
```
# vi /etc/yum.repos.d/CentOS-Base.repo
# yum localinstall http://yum.postgresql.org/9.5/redhat/rhel-7-x86_64/pgdg-centos95-9.5-2.noarch.rpm
# yum list postgres*
# yum install -y postgresql95-server.x86_64 postgresql95-contrib.x86_64 postgresql95-libs.x86_64 
# /usr/pgsql-9.5/bin/postgresql95-setup initdb
# systemctl enable postgresql-9.5.service
# systemctl start postgresql-9.5.service 
```
详见[CentOS 6.3下PostgreSQL 的安装与配置](http://www.linuxidc.com/Linux/2014-05/101725.htm)
或者[CentOS 7 安装、配置、使用 PostgreSQL 9.5（一）安装及基础配置](http://www.jianshu.com/p/7e95fd0bc91a)
# 小结
总结一下，其实ubuntu下配置这些东西并不难，以前只是在虚拟机上接触了以下ubuntu，当时感觉很难的样子。现在的ubuntu界面很友好，操作比起windows也复杂不太多。
     主要困惑是因为没弄清ubuntu下目录结构和vim部分，其实文件结构和windows类似，而vim就相当于是一个更加强大、功能丰富的记事本（编辑器），凡是用vim操作的部分都可以按照windows下的方式，找到文件，直接打开进行操作。
不过ubuntu下的权限更为苛刻，修改文件都需要管理员权限，可以输入

> sudo nautilus

来以管理员身份打开文件夹，然后复制、剪切、粘贴、删除、移动、修改、安装等等都当做win7、win8来操作即可。
