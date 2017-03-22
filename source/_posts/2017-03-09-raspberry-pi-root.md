---
title:  "树莓派启用root账户"
categories:
- RaspberryPi
tags: "树莓派"
comments: true
---

树莓派使用的linux是debian系统，所以树莓派启用root和debian是相同的。

debian里root账户默认没有密码，但账户锁定。

当需要root权限时，由默认账户经由sudo执行，Raspberry pi系统中的Raspbian默认用户是pi 密码为raspberry

<!-- more -->

重新开启root账号，可由pi用户登录后，在命令行下执行

  sudo passwd root

执行此命令后系统会提示输入两遍的root密码，输入你想设的密码即可，然后在执行

  sudo passwd --unlock root

这样就可以解锁root账户了。

参考：http://outofmemory.cn/code-snippet/2897/shumeipai
