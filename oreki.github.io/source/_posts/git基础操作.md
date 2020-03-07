---
title: git基础操作
toc: true
mathjx: true
date: 2018-08-06 11:43:36
update: 2018-09-01 06:42:24
tags: [Git]
categories: Git
cover: /2018/08/06/git基础操作/添加sshkey.png
indexing: true
---

### 用户信息配置
> git config --global user.name "xxx"
> git config --global user.email "xxx@eamil.com"

### 新建git仓库
> git init

### 添加远程仓库
> git remote add origin 仓库链接

### 上传修改文件
> git add

### 上传文件描述
> git commit -m "xxx"

### 创建新分支
> git branch new

### 切换到新分支
> git checkout new

### 上传分支
> git push origin master

### 拉去更新分支
> git pull origin master

### 更新分支
>git add
>git commit -m "xxx"
>git push origin master

### 添加ssh key
>ssh-keygen -t rsa -C "your_email@mail.com"

t 指定密钥类型，默认是 rsa ，可以省略。 -C 设置注释文字，比如邮箱或其他。
登录github,点击Settings,然后点击 SSH keys ,在这个页面你可以管理你所有的ssh keys
然后点击Add SSH key
用文本编辑器打开刚刚添加的key文件id_rsa.pub,复制里面的所有的内容
回到github页面,将复制的内容粘贴到刚刚那个页面的key对应的文本框里面,title 可以随便填写
![](添加sshkey.png)
