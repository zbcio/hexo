---
title:  "树莓派无显示器安装步骤"
date: "2017-03-09 10:15:00"
categories: 
- 技术
- 硬件
tags: "树莓派"
comments: true
---

### 准备

1、树莓派
2、SD卡（8G+）
3、Android通用充电器（5V2A）
4、路由器
5、网线一根
6、电脑一台

### 安装

下载官方提供的Raspberry Pi专用Debian——Raspbian，将下载后的.zip文件解压，得到一个.img文件。打开Win32DiskImager，选择.img文件和SD卡，点击Write按钮开始安装系统到SD卡上。

<!-- more -->

### 启动

安装完成后，把SD卡插到树莓派的卡槽上，接上电源，网线（连接路由器），树莓派会自动启动。

### 登录

登录路由器的管理页面（通常为192.168.1.1），找到叫做Raspberrypi的设备的IP，这便是树莓派的局域网IP。

打开SSH工具，连接树莓派的局域网IP，端口22，默认用户名：pi，密码：raspberry

### 配置

登录SSH后，需要配置一下，输入：sudo raspi-config，选择Expand Filesystem，这步是为了把整个系统的可用空间扩展到SD卡的大小。

选择Finish，然后重启并生效。


### 安装远程桌面

远程桌面控制需要安装vncserver，执行命令：sudo apt-get install tightvncserver。

安装完成后可以使用vncpasswd命令来设置密码，然后询问是否设置一个view-only密码，根据自己需要决定是否要设置。

第一次启动图形界面的服务时也会提示进行设置。

启动图形界面的命令：

  vncserver :1 -geometry 800x600

命令中的:1表示的是1号桌面，我们也可以输入:2创建2号桌面。然后-geometry 800x600当然就是设置分辨率。

可以使用vncserver -kill :1这个命令来杀死1号桌面。

然后就可以使用VNC连接远程桌面了。

### 连接远程桌面

下载VNC，安装并打开。

输入树莓派的IP以及桌面的号码（例如：192.168.1.22:1），点connect后输入密码即可登录。

### 参考：

http://www.eeboard.com/bbs/thread-27029-1-1.html
