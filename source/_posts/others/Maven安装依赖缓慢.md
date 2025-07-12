---
title: Maven安装依赖缓慢
categories:
  - IntelliJ IDEA
tags:
  - bug
mathjax: true
date: 2025-07-03 12:19:25
---
## 步骤一.更改settings.xml文件
找到maven安装路径下的settings.xml文件，一般位于conf\setting.xml。
更改镜像配置如下：
```
<mirrors>
        <mirror>
          <id>aliyun</id>
          <mirrorOf>central</mirrorOf>
          <name>aliyun Maven</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
 
        <mirror>
            <id>uk</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://uk.maven.org/maven2/</url>
        </mirror>
 
        <mirror>
            <id>CN</id>
            <name>OSChina Central</name>
            <url>http://maven.oschina.net/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
 
        <mirror>
            <id>nexus</id>
            <name>internal nexus repository</name>
            <!-- <url>http://192.168.1.100:8081/nexus/content/groups/public/</url>-->
            <url>http://repo.maven.apache.org/maven2</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
 
    </mirrors>
```

## 步骤二.Idea更改
然后还要更改idea中maven的设置，选择自己maven所在位置，如下：
Maven home path选择自己的maven路径
User settings file选择自己的maven下的setting.xml
Local repository为本地仓库位置，一般会跟踪上述自动配置
![](file-20250703121619592.png)


## 步骤三.刷新
![](file-20250703121213440.png)