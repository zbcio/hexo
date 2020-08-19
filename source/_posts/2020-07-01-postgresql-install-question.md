---
layout: post
category: "postgresql"
title: "安装PostgreSQL遇到的问题"
tags: "PostgreSQL"
---

# 安装完成后提示如下信息

    Problem running post-install step. Installation may not complete correctly The database cluster initialisation failed.


## 问题原因

由于所制定的data目录没有写入权限。

## 解决

可在安装之后参考如下步骤进行数据库初始化：

1. 修改data目录权限

    此处省略。

2. 初始化数据库，可使用initdb --help查看详细参数设置：

    initdb -U postgres -W -D "C:\Program Files\PostgreSQL\10\data"

3. 启动pg

    pg_ctl -D "C:\Program Files\PostgreSQL\10\data" -l logfile start
