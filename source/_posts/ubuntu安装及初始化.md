title: ubuntu安装及初始化
categories: Linux
tags: ubuntu
date: 2016-12-11 13:35
---

参考文章[Ubuntu 16.04 U盘安装图文教程](http://www.linuxidc.com/Linux/2016-04/130520.htm)
* 问题

![图1.jpg](http://upload-images.jianshu.io/upload_images/1837782-2729c17d5a1914d9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!-- more -->
按照以上文章结束时，我的电脑提示`无法将grub-efi-amd64-signed安装到/target/`，重装好多次都是卡在这一步。
解决
之前的步骤中电脑会提示电脑是以UEFI启动什么的，不选在UEFI模式中运行，选择后退一项，也能正常安装。（当时没有截图，现在记不清了）
* 后续
1.创建root用户
> sudo passwd
2. 更新系统
> sudo apt update
> sudo apt dist-upgrade

3.配置输入法快捷键
4.设置`系统设置-键盘-快捷键-启动器-启动终端`快捷键为`super+R`
5.安装vim

> sudo apt install vim

配置

> sudo vim /etc/vim/vimrc

插入(用jk代替Esc退出编辑模式)

> :imap jk <Esc> 

6.将文件夹改为英文路径
首先修改现有主文件夹下各文件夹名称：
desktop、 documents、 download、 music、 pictures、 public、  video ……
然后编辑配置文件：

> gedit ~/.config/user-dirs.dirs

把文件夹指向改掉，例如：
```xml
XDG_DESKTOP_DIR="$HOME/desktop"
XDG_DOWNLOAD_DIR="$HOME/download"
XDG_PUBLICSHARE_DIR="$HOME/public"
XDG_DOCUMENTS_DIR="$HOME/documents"
XDG_MUSIC_DIR="$HOME/music"
XDG_PICTURES_DIR="$HOME/pictures"
XDG_VIDEOS_DIR="$HOME/video"
```

7.在系统启动界面，Win8.1系统处于最后一项，如果需要让Win8.1处于第一项，可以这样设置：

1、进入Ubuntu系统。

2、Ctrl+Alt+T打开终端，输入“sudo nautilus”，以root权限打开资源管理器。

3、找到“计算机/etc/grub.d/30_os-prober”文件，将其名称修改为“06_os-prober”即可：

8.ubuntu16.04的软件中心应该是有bug，安装不了第三方.deb文件，我们只有使用dpkg -i 或者gdebi的方式安装，我使用的是后者，因为后者功能更加强大。要使用gdebi命令先要安装它：

> sudo apt install gdebi-core


然后就可以安装.deb文件了。安装过程如下：先切换到你下载的lantern的安装文件目录下，直接使用：

> sudo gdebi xxxxxx.deb

9.安装git
> sudo apt-get install git

10.安装lantern
> git clone [https://github.com/getlantern/lantern.git](https://github.com/getlantern/lantern.git)
> cd lantern
> make lantern
> ./lantern
