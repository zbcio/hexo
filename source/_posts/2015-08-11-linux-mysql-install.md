---
title: Linux下安装mysql
category:
- 技术
- MySQL
tags:
- MySQL
---

#### linux安装mysql服务分两种安装方法：
1. 源码安装，优点是安装包比较小，只有十多M，缺点是安装依赖的库多，安装编译时间长，安装步骤复杂容易出错；  
2. 使用官方编译好的二进制文件安装，优点是安装速度快，安装步骤简单，缺点是安装包很大，300M左右。以下介绍linux使用官方编译好的二进制包安装mysql。  

MySQL mirrors地址：<http://dev.mysql.com/downloads/mirrors.html>  

### 一、准备工作：
下载官方编译好的二进制包并解压：  

    wget ftp://ftp.jaist.ac.jp/pub/mysql/Downloads/MySQL-5.6/mysql-5.6.30-linux-glibc2.5-x86_64.tar.gz
    tar -zxvf mysql-5.6.30-linux-glibc2.5-x86_64.tar.gz

复制解压后的mysql目录到系统的本地软件目录：  

    cp mysql-5.6.30-linux-glibc2.5-x86_64 /usr/local/mysql -r

注意：此处建议用如下方法，使用软链过去，不要直接包文件复制，便于系统安装多个版本的mysql：  

    cp mysql-5.6.30-linux-glibc2.5-x86_64 /usr/local/
    ln -s mysql-5.6.30-linux-glibc2.5-x86_64 mysql

### 二、用户和组：
添加系统mysql组和mysql用户：  

    groupadd mysql
    useradd -r -g mysql mysql

进入安装mysql软件目录：  

    cd /usr/local/mysql

修改当前目录所属的组mysql和用户mysql：  

    chgrp -R mysql .
    chown -R mysql:mysql .

### 三、开始安装：
安装数据库：

    scripts/mysql_install_db --user=mysql

将mysql/目录下除了data/目录的所有文件，改回root用户所有，mysql用户只需作为mysql/data/目录下所有文件的所有者：

    chown -R root .
    chown -R mysql data

### 四、开机启动：
复制mysql配置文件：

    cp support-files/my-medium.cnf /etc/my.cnf

首先需要将scripts/mysql.server服务脚本复制到/etc/init.d/，并重命名为mysqld：  

    cp support-files/mysql.server /etc/init.d/mysqld

通过chkconfig命令将mysql服务加入到自启动服务项中，注意服务名称mysql就是我们将mysql.server复制到/etc/init.d/时重命名的名称：  

    chkconfig --add mysqld

查看是否添加成功：  

    chkconfig --list mysqld

重启系统，mysqld就会自动启动了。检查是否启动：  

    netstat -anp|grep mysqld

显示如下：  

    tcp        0      0 0.0.0.0:3306                0.0.0.0:*                   LISTEN      27628/mysqld
    unix  2      [ ACC ]     STREAM     LISTENING     204207 27628/mysqld        /tmp/mysql.sock

如果不想重新启动系统，那就手动启动MySQL服务：  

    service mysqld start

### 五、其他配置：
将/usr/local/mysql/bin/mysql加入环境变量中，在/etc/profile最后加入两行命令：

    MYSQL_HOME=/usr/local/mysql
    export PATH=$PATH:$MYSQL_HOME/bin

修改mysql的root用户密码，root初始密码为空的：

    /usr/local/mysql/bin/mysqladmin -u root password '密码'

### 附录A：

MySQL 5.6官方提供的安装步骤：  

    # Preconfiguration setup
    shell> groupadd mysql
    shell> useradd -r -g mysql -s /bin/false mysql
    # Beginning of source-build specific instructions
    shell> tar zxvf mysql-VERSION.tar.gz
    shell> cd mysql-VERSION
    shell> cmake .
    shell> make
    shell> make install
    # End of source-build specific instructions
    # Postinstallation setup
    shell> cd /usr/local/mysql
    shell> chown -R mysql .
    shell> chgrp -R mysql .
    shell> scripts/mysql_install_db --user=mysql
    shell> chown -R root .
    shell> chown -R mysql data
    shell> bin/mysqld_safe --user=mysql &
    # Next command is optional
    shell> cp support-files/mysql.server /etc/init.d/mysql.server

### 附录B：

MySQL 5.7后安装步骤稍有不同，不过都差不太多，以下是官方提供的安装步骤：  

    # Preconfiguration setup
    shell> groupadd mysql
    shell> useradd -r -g mysql -s /bin/false mysql
    # Beginning of source-build specific instructions
    shell> tar zxvf mysql-VERSION.tar.gz
    shell> cd mysql-VERSION
    shell> cmake .
    shell> make
    shell> make install
    # End of source-build specific instructions
    # Postinstallation setup
    shell> cd /usr/local/mysql
    shell> chown -R mysql .
    shell> chgrp -R mysql .
    shell> bin/mysql_install_db --user=mysql    # Before MySQL 5.7.6
    shell> bin/mysqld --initialize --user=mysql # MySQL 5.7.6 and up
    shell> bin/mysql_ssl_rsa_setup              # MySQL 5.7.6 and up
    shell> chown -R root .
    shell> chown -R mysql data
    shell> bin/mysqld_safe --user=mysql &
    # Next command is optional
    shell> cp support-files/mysql.server /etc/init.d/mysql.server

### 六、参考链接：
<http://jingyan.baidu.com/article/a378c9609eb652b3282830fd.html>  
<http://blog.csdn.net/wendi_0506/article/details/39478369>  
<http://blog.csdn.net/superchanon/article/details/8546254>  
<http://dev.mysql.com/doc/refman/5.7/en/installing-source-distribution.html>
