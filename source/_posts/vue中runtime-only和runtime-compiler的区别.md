---
title: Vue中runtime-only和runtime-complier的区别
date: 2021-05-17 17:19:56
tags: Vue-cli
categories: Vue
description: runtime-only和runtime-complier的区别
# cover: https://z3.ax1x.com/2021/05/17/gRRXNT.jpg
---

### 前言

最近在学习vue，在创建vue脚手架的时候，会让你选择runtime-only和runtime-compiler，提示显示runtime-only体积更小，两者有啥区别

<!--more-->


<img src="https://z3.ax1x.com/2021/05/17/gRgnvn.png">

### 两者的区别

左侧是使用runtime-compiler，右侧是使用runtime-only，这两者的区别在于能否解析template模板，前者能够解析，而后者不能解析。
runtime-compiler在遇到.vue文件时，就是当作vue使用，所以能够解析template。但是runtime-only在遇到.vue文件时，解析为.js文件，其中的tempalte会转化为render函数。具体步骤如下所示:

* runtime-complier: template->ast(抽象语法树)->render->visual dom->ui
* runtime-only: render->visual dom->ui
* 与runtime-complier相比，runtime-only性能更好，体积更小

**在runtime-only中，它的template是由vue-template-complier处理的**


