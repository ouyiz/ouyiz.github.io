---
title: 将本地项目上传到 GitHub 仓库的步骤
categories:
  - git
tags:
  - github
mathjax: true
date: 2025-07-12 15:21:36
---
### 首次上传新项目到 GitHub
如果你有一个全新的本地项目，还没有任何 Git 初始化或关联，可以按照以下步骤操作：
1. **在本地项目文件夹中初始化 Git 仓库：**
    打开项目文件夹，右键点击空白处，选择“Git Bash Here”或者打开终端/命令提示符，导航到项目目录。然后运行：
    ```
    git init
    ```
    这会在项目文件夹中创建一个名为 `.git` 的隐藏文件夹，表示本地仓库已初始化。
    
2. **将项目文件添加到暂存区：**
    接下来，需要告诉 Git 哪些文件需要被追踪并提交。
    ```
    git add .
    ```
    `git add .` 会将当前目录下所有文件（包括子文件夹中的文件）添加到暂存区。如果只想添加特定文件，可以将 `.` 替换为文件名或文件夹名。
    
3. **提交到本地仓库：**
    现在，将暂存区的文件提交到本地 Git 仓库，并附上一条有意义的提交信息。    
    ```
    git commit -m "Initial commit of my project"
    ```
    将 `"Initial commit of my project"` 替换为你自己的提交信息。
    
4. **在 GitHub 上创建新的远程仓库：**
    - 登录 GitHub 账号。
    - 点击页面右上角的 `+` 号，选择 "New repository"（新建仓库）。
    - 填写仓库名称，可以添加描述，选择公共或私有。
    - 点击 "Create repository"（创建仓库）。
5. **关联本地仓库与远程仓库：**
    创建成功后，GitHub 会显示一些指令，需要复制并运行其中的两行命令。它们通常是：
    ```
    git remote add origin <你的GitHub仓库地址>
    git branch -M main # 或者 git branch -M master，取决于你希望默认分支叫什么
    ```
    `<你的GitHub仓库地址>` 通常是 `https://github.com/你的用户名/你的仓库名.git`。
    `git remote add origin <你的GitHub仓库地址>`：给 GitHub 仓库地址起一个名叫 `origin` 的别名，并将其添加到你本地 Git 仓库的远程配置中。
    `git branch -M main`：将当前你所在的本地分支（通常是刚初始化的 `master` 分支）强制重命名为 `main`
    
    
6. **将本地代码推送到 GitHub：**
    最后，将本地 main（或 master）分支的代码推送到 GitHub 远程仓库。
    ```
    git push -u origin main
    ```
    将本地仓库的 `main` (或 `master`) 分支上的提交推送到名为 `origin` 的远程仓库。
    `-u` 选项会在第一次推送时设置上游分支，这样以后只需要运行 `git push` 即可。

### 后续代码更新后的推送步骤
1. **提交本地更改：** 
	在对项目进行了修改、新增文件或删除了文件之后，需要将这些更改提交到本地 Git 仓库中。
    - **添加更改到暂存区：**
        ```
        git add .
        ```
        这条命令会将所有修改过的、新增的或删除的文件添加到暂存区。如果只想添加特定文件，可以将 `.` 替换为相应的文件名或文件夹名。
    - **提交更改到本地仓库：**
        ```
        git commit -m "Your meaningful commit message here"
        ```
        将 `"Initial commit of my project"` 替换为你自己的提交信息。
        
2. **将本地提交推送到 GitHub：**
	完成本地提交后，就可以将这些新的提交推送到 GitHub 远程仓库了。
    ```
    git push
    ```
    由于在第一次推送时使用了 `-u` 选项（`git push -u origin main`），Git 已经记住了本地 `main` 分支应该推送到 `origin`（您的 GitHub 仓库）的 `main` 分支。所以，后续只需要运行 `git push` 即可。
    
