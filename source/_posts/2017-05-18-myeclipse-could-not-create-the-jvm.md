---
title: MyEclipse启动服务报错：Could not create the Java virtual machine
categories:
- 技术
- Eclipse
tags:
- Eclipse
---

最近遇到一个问题，在MyEclipse中启动weblogic时，偶尔会报Could not create the Java virtual machine。开始也没找到什么规律，只知道刚开机的时候就能启动成功。后来感觉不是个办法，还是找找原因吧。原因肯定是和参数设置有关系，但是反复改了好多次参数，尝试了好多次才成功，所以记录一下。

首先，我的MyEclipse设置如下：

    -Xmx2048m
    -XX:MaxPermSize=256m
    -XX:ReservedCodeCacheSize=64m

weblogic的启动参数设置如下：

    -Xms1024m -Xmx1024m -XX:CompileThreshold=8000 -XX:PermSize=128m -XX:MaxPermSize=512m -Xverify:none -da

电脑的总内存是12GB，内存绝对是够用的。

后来无意中发现，我把非堆内存的值调低，就可以启动了：

    -Xms1024m -Xmx1024m -XX:CompileThreshold=8000 -XX:PermSize=128m -XX:MaxPermSize=256m -Xverify:none -da

