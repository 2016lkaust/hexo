title: ubuntu下php环境搭建
categories: Linux
tags: ubuntu
date: 2016-12-14 00:00
---

将要安装的软件：
Apache+mysql+php（+phpMyAdmin用于管理数据库）  
<!-- more -->
# 1.安装 Apache
 ## （1）安装apache命令
   >  sudo apt-get install apache

## (2)测试 Apache安装是否成功

浏览器输入[http://localhost/](http://localhost/)，如果成功：
![安装成功](http://upload-images.jianshu.io/upload_images/1837782-2ffbfc73feb0c6e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 # 2.安装 PHP
 [关于Ubuntu16.04不能安装libapache2-mod-php5](http://www.linuxdiyf.com/linux/20550.html)
>  ubuntu16.04已经选择PHP7.0作为新的基础包了，所以不能再Ubuntu16.04上安装PHP5，如果硬要安装，则会出现依赖问题，而且一般无法解决：
比如，Ubuntu16.04上的软件需要的是一个较高版本的库，而PHP5需要的是一个较低版本的库，系统会提示：libapache2-mod-php5依赖于某某库，但该库不能被安装。因为php5基于较低版本的库文件，如果安装的话有其他软件将不能运行。
故，推荐安装PHP7.0，并且用“libapache2-mod-php”代替“libapache2-mod-php5”。
最后，如果阁下非要使用PHP5，那就只好回到Ubuntu14.04LTC（推荐）了！

简言之
 *  (1)命令行安装php7
> sudo apt-get install php7
sudo apt-get libapache2-mod-php

*  (2)重启一下apache

 > sudo /etc/init.d/apache2 restart

* (3)准备一个测试文件`first.php`放在`/var/www/html/`目录下,内容为
`<?php phpinfo();?>`

* (4)测试php文件是否能被解析。在浏览器输入
> localhost/first.php

到这一步，我不能正常访问，出现了404错误，非常感谢[http://blog.csdn.net/hitabc141592/article/details/23556079](http://blog.csdn.net/hitabc141592/article/details/23556079)这篇文章。
将这两个文件链接到mods-enabled目录下：

 > sudo ln -s /etc/apache2/mods-available/php7.0.load /etc/apache2/mods-enabled/php7.load
 > sudo ln -s /etc/apache2/mods-available/php7.0.conf /etc/apache2/mods-enabled/php.conf

注：`/etc/apache2/mods-available/php7.0.load`是文件路径，可以沿着路径打开去看看，`/etc/apache2/mods-enabled/php7.load`是链接到的路径。意思就是将这两个文件加入apache配置中。
* (5)准备的php文件为什么放在`/var/www/html/`下面？
查看apache配置文件就清楚了，apache2的默认目录配置在/etc/apache2/sites-enabled/00default文件中，里面有一句
```
DocumentRoot /var/www/html
```
说明这个目录(`/var/www/html/`)是默认开发目录。
#  3.安装mysql
> sudo apt-get install mysql-server

 设置用户root及密码。
  # 4.更改`/var/www/html/`的权限
> sudo chmod 777 /var/www/html

 注：我是运行`sudo nautilus`，以管理员身份打开文件夹，直接更改了这个文件的属性中的权限。
   #  5.安装phpMyAdmin，用于管理mysql，属于mysql的可视化界面。
## （1）安装
  > sudo apt-get install phpmyadmin

   这里需要输入root用户的密码（两次）。
 ##（2）把`phpmyadmin中`的`apache.conf`（apache配置文件）复制到`apache2/sites-available`下的`phpmyadmin`文件下。
 > cp /etc/phpmyadmin/apache.conf /etc/apache2/sites-available/phpmyadmin

 ##（3）创建phpMyAdmin链接。
  > sudo ln -s /usr/share/phpmyadmin /var/www/html/

 输入`localhost/phpmyadmin/`,成功访问。
![phpMyAdmin首页](http://upload-images.jianshu.io/upload_images/1837782-05b9901c8976707f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
*****************如果失败了，继续4,5,6步，成功的话就可以结束了。*****************
```
 ## （4）接着输入，进入sites-enabled文件夹下：
  > cd /etc/apache2/sites-enabled/ 

 ##（5）进入之后，要建立一个通往配置文件的链接以便能利用它。
 > sudo ln -s ../sites-available/phpmyadmin

 ##（6）重启apache2
 > sudo /etc/init.d/apache2 restart


 










