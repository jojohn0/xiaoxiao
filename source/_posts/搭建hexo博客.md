---
title: 搭建+美化hexo博客(让你拥有自己的博客)
date: 2021-02-25 11:35:14
tags: hexo
categories: hexo
description: 本文介紹利用hexo框架+github搭建博客
# cover: https://s3.ax1x.com/2021/02/25/yjc3rR.jpg 
---

## 安裝必备软件

### 软件安装

首先我们需要安装git、node.js、npm、hexo

<!--more-->


#### git的安装

1. 首先更新ubuntu存储库的软件包索引号

```
sudo apt-get update
```

2. 安装git

```
sudo apt install git
```

3. 验证并检查git的安装版本

```
git --version
```

4. 配置git

此时我们需要配置自己的用户信息

```
git config --global user.name "...."
git config --global user.email "...."
```

#### node.js的安装

1. 执行检查可更新的软件

```
sudo apt-get update
```

2. apt工具安装低版本的node、然后升级

```
sudo apt-get install nodejs
sudo apt install nodejs-legacy
sudo apt install npm
```

3. 更换淘宝的镜像

```
sudo npm config set registry https://registry.npm.taobao.org
```

4. 安装更新版本工具N，执行

```
sudo npm install n -g
```

5. 更新node版本

```
sudo n stable
```

#### 安装hexo

1. 执行

```
npm install hexo-cli -g
```

2. 检查hexo是否安装成功

```
hexo -v
```

## 搭建博客

1. 首先初始化hexo文件夹

```
hexo init xxx文件夹
```

2. 本地运行博客

```
hexo server
```

3. 部署博客到github

首先注册github账号,然后再创建仓库

**注意**
仓库的名字必须是你github的用户名.github.io

创建成功后,将博客文件_config.yml更改

![](https://s3.ax1x.com/2021/02/25/yjceaV.png)

更改完成后,执行

```
hexo clean
hexo deploy
```

此时博客成功部署到github上

