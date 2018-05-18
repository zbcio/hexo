---
title: gson的一些用法
categories:
- 技术
- gson
tags:
- gson
---

#### 1、自定义json中key的名字

可以在Java里面的属性上，加上如下注释：@SerializedName()

例如：  

    @SerializedName("businesscontrolno")

参考：https://www.jianshu.com/p/e740196225a4

#### 2、gson处理null字段

    Gson g = new GsonBuilder().serializeNulls().create();

#### 3、gson处理日期字段

    Gson g = new GsonBuilder().setDateFormat("yyyy-MM-dd hh:mm:ss").create();


