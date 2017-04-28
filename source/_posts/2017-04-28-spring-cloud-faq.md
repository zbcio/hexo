---
title: Spring Cloud常见问题
categories:
- 技术
- Spring Cloud
tags:
- Spring Cloud
---

### Eureka的自我保护模式

如果在Eureka Server的首页看到以下这段提示，则说明Eureka已经进入了保护模式。

    EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.

保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据（也就是不会注销任何微服务）。

详见：https://github.com/Netflix/eureka/wiki/Understanding-Eureka-Peer-to-Peer-Communication

参考：[Spring Cloud中，Eureka常见问题总结](http://blog.csdn.net/jdhanhua/article/details/55002191)
