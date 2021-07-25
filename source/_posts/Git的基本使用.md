---
title: Git的基本使用
date: 2021-02-12 16:56:29
tags: Git
categories: Git
description: 本文主要介绍了Git的基本使用以及分支等相关概念
# cover: https://s3.ax1x.com/2021/02/12/yD5cLT.jpg
---
## ubuntu下 安装git

1. 打开终端，输入sudo apt-get install git
2. 安装完成后，查看版本号 git --version
3. 配置git
    1. 配置用户名 git config --global user.name ""
    2. 配置邮箱 git config --global user.email ""
<!--more-->

## git的使用

1. 提交步骤
    1. git init **初始化git仓库**
    2. git status **查看文件状态**
    3. git add 文件列表 **追踪文件**
    4. git commit -m 提交信息 **向仓库中提交代码**
    5. git log **查看提交信息**
2. 撤销
    * 用暂存区中的文件覆盖工作目录中的文件： git checkout -- 文件
    * 将文件从暂存区中删除： git rm --cached 文件
    * 将git仓库中指定的更新记录恢复出来，并且覆盖暂存区和工作目录： git reset --hard commitID
    * 可以把暂存区的修改撤销掉,重新放回工作区： git reset HEAD file 

## 分支

使用分支，可以使开发者从开发主线分离出来，以免影响主线

### 主分支

主分支(master)就是第一次向git仓库提交更新记录时自动产生的一个分支

### 开发分支

开发分支(develop)作为开发的分支，基于master分支创建

### 功能分支

功能分支(feature)作为开发具体功能的分支，基于开发分支创建

## 分支命令

* 查看分支：`git branch`
* 创建分支：`git branch <name>`
* 切换分支：`git checkout <name>`或者`git switch <name>`6
* 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`
* 合并某分支到当前分支：`git merge <name>`
* 删除分支：`git branch -d <name>`
* 删除远程分支：`git push origin -d <name>`

## 暂时保存更改

在git中，可以暂时提取分支上所有的改动并存储，让开发人员得到一个干净的副本，临时转向其他工作。

使用场景：分支临时切换

* 存储临时变动：git stash
* 恢复改动：git stash pop

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

## 关联远程仓库

* 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`

* 使用命令`git push -u origin master`第一次推送master分支的所有内容
* 以后使用命令`git push origin master`推送最新修改

## 克隆远程仓库

* 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

## 标签

- `git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- `git tag`可以查看所有标签。

- `git push origin <tagname>`可以推送一个本地标签；
- `git push origin --tags`可以推送全部未推送过的本地标签；
- `git tag -d <tagname>`可以删除一个本地标签；
- `git push origin :refs/tags/<tagname>`可以删除一个远程标签。

