---
title: Java Config 注解
categories:
- 技术
- Java
tags:
- Java
---


java config是指基于java配置的spring。传统的Spring一般都是基本xml配置的，后来spring3.0新增了许多java config的注解，特别是spring boot，基本都是清一色的java config。

#### @Configuration

在类上打上这一标签，表示这个类是配置类

#### @ComponentScan

相当于xml的

    <context:componentscan basepakage=>

#### @Bean

bean的定义，相当于xml的

    <bean id="objectMapper" class="org.codehaus.jackson.map.ObjectMapper" /> 

#### @EnableWebMvc

相当于xml的

    <mvc:annotation-driven>

#### @ImportResource

相当于xml的

    <import resource="applicationContext-cache.xml">

#### @PropertySource

spring 3.1开始引入，它是基于java config的注解，用于读取properties文件

#### @Profile

spring3.1开始引入,一般用于多环境配置，激活时可用@ActiveProfiles注解，@ActiveProfiles("dev")等同于xml配置

    <beans profile="dev">
        <bean id="beanname" class="com.pz.demo.ProductRPC"/>
    </beans>

激活该profile spring.profiles.active，也可设置默认值 spring.profiles.default

    <context-param>
        <param-name>spring.profiles.default</param-name>
        <param-value>dev</param-value>
    </context-param>

参考：[Java Config 注解](https://www.cnblogs.com/whx7762/p/7828435.html)
