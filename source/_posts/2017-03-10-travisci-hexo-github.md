---
title:  "使用Travis CI自动部署Hexo博客到Github"
categories:
- Hexo
tags: ["Travis CI", "Hexo", "Github"]
comments: true
---

在网上找了些教程，写的都不尽相同，这里只介绍Travis CI部分，默认认为已掌握Hexo和GitHub的基本使用。

### Travis CI

首先，要使用Travis CI，必须要GIthub账号。在[Travis CI官网](https://travis-ci.org)使用GitHub账号登录。

登录后创建一个新的Repositories，选择并开启有Hexo源码的仓库。

然后进行一些设置，在General Settings中，打开如下两项即可：

Build only if .travis.yml is present：是只有在.travis.yml文件中配置的分支改变了才构建

Build pushes：当推送完这个分支后开始构建

<!-- more -->

### 在GitHub上生成Access Token

Setting-->Personal access tokens-->Generate new token

随便填个名称，然后下面根据需要进行勾选。

然后回到Travis CI中，在刚建的那个Repositories中设置Environment Variables：

  name：GH_TOKEN
  value：刚刚在GitHub中生成的key

### .travis.yml

接下来在源代码的根目录添加.travis.yml配置文件，内容如下：

    language: node_js
    node_js: stable
    
    # S: Build Lifecycle
    install:
      - npm install
    
    
    #before_script:
     # - npm install -g gulp
    
    script:
      - hexo g
    
    after_script:
      - cd ./public
      - git init
      - git config user.name "xxx"
      - git config user.email "xxx@gmail.com"
      - git add .
      - git commit -m "Update docs"
      - git push --force --quiet "https://${GH_TOKEN}@${GH_REF}" master:master
    # E: Build LifeCycle
    
    branches:
      only:
        - master
    env:
     global:
       - GH_REF: github.com/xxx/xxx.github.io.git

把xxx替换为自己的名字或者账号即可。

参考：[手把手教你使用Travis CI自动部署你的Hexo博客到Github上](http://blog.csdn.net/woblog/article/details/51319364)
