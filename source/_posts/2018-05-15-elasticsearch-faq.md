---
title: ElasticSearch报错汇总
categories:
- 技术
- ElasticSearch
tags:
- ElasticSearch
---

### 1、error='Cannot allocate memory' (errno=12)

#### 原因：

这种情况是因为内存不足

#### 解决办法： 

使用如下方式修改jvm启动内存参数：

    [es@CentOS ~]$ vi elasticsearch/config/jvm.options
    
    -Xms512m
    -Xmx512m


### 2、max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

#### 解决办法：
切换到root用户修改配置sysctl.conf

    vi /etc/sysctl.conf

添加下面配置：

    vm.max_map_count=655360

并执行命令：

    sysctl -p

然后，重新启动elasticsearch，即可启动成功。


### 3、[es@CentOS elasticsearch-head]# npm install  

npm ERR! Error: CERT_UNTRUSTED

#### 解决办法： 

这是因为ssl验证问题，使用下面的命令取消ssl验证即可解决：

    npm config set strict-ssl false


### 4、[es@CentOS elasticsearch-head]# npm install  

SyntaxError: Use of const in strict mode.

#### 解决办法： 

1) Clear NPM's cache:

    sudo npm cache clean -f

2) Install a little helper called 'n'

    sudo npm install -g n

3) Install latest stable NodeJS version

    sudo n stable


### 5、max file descriptors [65535] for elasticsearch process is too low, increase to at least [65536]

#### 解决办法： 

切换到root用户修改配置sysctl.conf

    vi /etc/sysctl.conf

添加下面配置：

    fs.file-max=655350

并执行命令：

    sysctl -p

然后，重新启动elasticsearch，即可启动成功。


### 6、max number of threads [1024] for user [es] is too low, increase to at least [4096]

#### 解决办法： 

1、切换到root用户修改配置

    vi /etc/security/limits.d/90-nproc.conf

添加下面配置：

    * soft nproc 4096

2、修改配置

    vi /etc/security/limits.conf

添加下面配置：

    * soft nproc 2048
    * hard nproc 4096

并执行命令：

    sysctl -p

然后，重新启动elasticsearch，即可启动成功。


### 7、Could not reliably determine the server's fully qualified domain name, using 10.117.235.227 for ServerName

#### 原因：

此问题是在用httpd服务器做端口转发时遇到的问题。域名解析到此服务器，然后把9200端口转发到80端口，访问搜索引擎时就不用再域名后加端口了。

#### 解决办法： 

修改/etc/apache2/httpd.conf

    vi /etc/apache2/httpd.conf

找到如下配置，去掉注释：

    ServerName localhost:80

重启httpd即可。


### 8、system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk

#### 原因：

CentOS6.X 不支持SecComp，而ES5.2.0默认bootstrap.system_call_filter为true进行检测，所以导致检测失败，失败后直接导致ES不能启动。

#### 解决办法： 

修改/opt/es/elasticsearch-5.5.0/config/elasticsearch.yml中配置

    vi /opt/es/elasticsearch-5.5.0/config/elasticsearch.yml

修改如下配置：

    bootstrap.memory_lock: false
    bootstrap.system_call_filter: false


