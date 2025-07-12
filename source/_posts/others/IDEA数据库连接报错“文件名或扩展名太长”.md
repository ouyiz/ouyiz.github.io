---
title: IDEA数据库连接报错“文件名或扩展名太长”
categories:
  - IntelliJ IDEA
tags:
  - bug
mathjax: true
date: 2025-07-07 15:01:18
---
## 问题
在IDEA中连接MYSQL数据库时报错如下
```
DBMS: MySQL (no ver.)
Case sensitivity: plain=mixed, delimited=exact
Cannot run program "D:\IDEA\IntelliJ IDEA 2023.1.2\jbr\bin\java" (in directory "D:\college\javaWebStudy\hello-mp"): CreateProcess error=206, 文件名或扩展名太长。
```

## 解决方法
卸载当前IDEA版本，更换新的IDEA版本。
本人：2023.1.2 ->2024.3.6

**曾尝试过的无用方法：**
1.更换项目位置
将项目放在D盘下，没用

2.更改IDEA位置
卸载IDEA，更改IDEA的安装路径，使路径变短，没用。

3.配置 IntelliJ IDEA 的“Shorten command line”选项
没用
![](file-20250707145907064.png)
4.配置 IntelliJ IDEA 使用短 Classpath
到 `Runner` 选项卡，勾选 `Delegate IDE build/run actions to Maven`，没用。
![](file-20250707145726442.png)
