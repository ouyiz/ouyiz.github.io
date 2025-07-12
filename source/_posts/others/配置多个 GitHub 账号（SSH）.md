---
title: 配置多个 GitHub 账号（SSH）
categories:
  - 问题解决
tags:
  - 编程
  - 笔记
  - github
mathjax: true
date: 2025-05-30 15:34:43
---
假设一个名字为aaa，一个名字为bbb

## 1.为每个账号生成不同的 SSH 密钥
```bash
# 为账号 aaa
ssh-keygen -t ed25519 -C "aaa@example.com" -f ~/.ssh/id_ed25519_aaa

# 为账号 bbb
ssh-keygen -t ed25519 -C "bbb@example.com" -f ~/.ssh/id_ed25519_bbb
```

## 2.将公钥添加到对应 GitHub 账户
进行下述操作，得到`.pub` 文件内容
```bash
cat ~/.ssh/id_ed25519_aaa.pub
cat ~/.ssh/id_ed25519_bbb.pub
```
进入各自的 GitHub 账号页面：
- 打开 GitHub → **Settings → SSH and GPG keys → New SSH key**
- 把你生成的 `.pub` 文件内容粘贴进去

## 3.配置 SSH 配置文件 `~/.ssh/config`
打开 SSH 配置文件：
```bash
notepad ~/.ssh/config
```

写入以下内容（每个账户一个 Host）：
```ssh
# aaa 账号配置
Host github-aaa
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_aaa

# bbb 账号配置
Host github-bbb
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_bbb
```


## 4.项目中使用不同的 SSH 地址
假设你有两个项目分别归属于不同账号：
aaa 账号的项目使用下述
```bash
git remote set-url origin git@github-aaa:aaa/aaa-project.git
```

bbb 账号的项目使用下述
```bash
git remote set-url origin git@github-bbb:bbb/bbb-project.git
```

注意：此处 `github-aaa` 和 `github-bbb` 是你在 `~/.ssh/config` 中定义的别名，替代了默认的 `github.com`。

