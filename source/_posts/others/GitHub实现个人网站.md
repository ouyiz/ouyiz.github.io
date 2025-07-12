---
title: GitHub实现个人网站
categories:
  - 网站搭建
tags:
  - 笔记
  - 播客
  - github
mathjax: true
date: 2025-05-30 15:35:22
---
## 1.创建仓库
仓库名字必须是 `<你的用户名>.github.io`
Description可以不写
选择public
![](eaac9f755c400bcc0b7e6940dc95c02.png)
## 2.本地创建文件夹
文件夹内容如下，再让chatgpt随便写的内容用于测试
```
my-website/             ← 项目主文件夹
├── index.html          ← 首页（主 HTML 文件）
├── about.html          ← 其他页面（可选）
├── contact.html        ← 其他页面（可选）
├── css/                ← 所有样式文件
│   └── style.css       ← 主样式表
├── js/                 ← 所有 JavaScript 文件
│   └── script.js       ← 主脚本文件
├── images/             ← 图片资源
│   ├── logo.png
│   └── banner.jpg
├── fonts/              ← 字体资源（可选）
│   └── custom-font.woff
├── assets/             ← 其他资源（视频、PDF 等，可选）
│   └── brochure.pdf
└── README.md           ← 项目说明文件（可选）
```
![](file-20250530150135050.png)

## 3.上传github(安装了Git LFS)
进入你的网站项目文件夹
```
git init
git lfs track "*.jpg"
git lfs track "*.mp4" 
git add .
git commit -m "初始提交，包含网站文件和 LFS 配置"
git remote add origin https://github.com/你的用户名/你的仓库名.git.io
git branch -M main
git push -u origin main
```

## 4.访问网站
提交后如上，使用`https://你的用户名.github.io/)`就可以访问了
![](file-20250530153126862.png)