---
title: 手写AJAX
date: 2021-04-09 16:06:56
tags: AJAX
categories: JavaScript基础
# cover: https://z3.ax1x.com/2021/04/09/cNhkan.jpg
description: 介绍AJAX，手写原生AJAX
--- 
## 什么是AJAX

* AJAX(Asynchronous JavaScript and XML) 是一种用于创建快速动态网页的技术。
* 通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
<!--more-->

### 创建 XMLHttpRequest 对象

* XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

* 创建XMLHttpRequest对象的语法

```js
let xhr = new XMLHttpRequest();
```

### XMLHttpRequest 对象用于和服务器交换数据。

* 如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法

#### open()方法接收三个参数 (method,url,async)

* method：请求的类型；GET 或 POST
* url：文件在服务器上的位置，该文件可以是任何类型的文件，或者服务器脚本文件
* async：true（异步）或 false（同步）

#### 简单的GET请求

```js
xmlHttp.open("GET","demo_get.asp",true);
xmlHttp.send();
```

#### 简单的POST请求

* 如果需要像 HTML 表单那样 POST 数据，请使用 setRequestHeader() 来添加 HTTP 头。然后在 send() 方法中规定您希望发送的数据

```js
xhr.open("POST","ajax_test.asp",true);
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xhr.send("fname=Bill&lname=Gates");
```

> Content-Type（内容类型），一般是指网页中存在的 Content-Type，用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，

* setRequestHeader(header,value)方法，用来向请求添加HTTP头

### async

* XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true

```js
xhr.open("GET","ajax_test.asp",true);
```

#### async=true

* 当使用 async=true 时，需要规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数

```js
xhr.onreadystatechange=function(){
  if (xhr.readyState == 4 && xhr.status == 200){
      document.getElementById("myDiv").innerHTML = xhr.responseText;
    }
  }
xhr.open("GET","test1.txt",true);
xhr.send();
```

### onreadystatechange事件

* 当请求被发送到服务器时，我们需要执行一些基于响应的任务。每当readyState 改变时，就会触发onreadystatechange事件。

#### readyState属性存有XMLHttpRequest的状态信息。

### XMLHttpRequest对象的三个重要的属性

1. readyState：存有XMLHttpRequest对象的状态。从0到4发生变化。    
   * 0：请求未初始化
   * 1：服务器连接已建立
   * 2：请求已接收
   * 3：请求处理中
   * 4：请求已完成，且响应已就绪
2. onreadystatechange：每当readyState属性改变时，就会调用该函数。
3. status：表示状态

* 当readyState等于4且状态为200时，表示响应已就绪：

```js
xhr.onreadystatechange=function(){
  if (xhr.readyState == 4 && xhr.status == 200){
    document.getElementById("myDiv").innerHTML = xhr.responseText;
    }
  }
```

### 服务器响应

* 如果需要获得来自服务器的响应，使用XMLHttpRequest对象的responseText 或responseXML属性。
* responseText获得字符串形式的响应数据，而responseXML获得XML形式的响应数据。



## 手写AJAX

### 一般实现

```js
const url = "/server";
let btn = document.querySelector("button");
btn.addEventListener("click", function () {
  send();
})
function send() {
  // 1.创建对象
  let xhr = new XMLHttpRequest();
  // 2.打开连接
  xhr.open("GET", url);
  // 3.发送请求
  xhr.send(null);
  // 4.获取响应
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
      document.querySelector(".box1").innerHTML = xhr.responseText
    }
  }
}
```

### Promise封装实现

```js
function AJAX(url){
  return new Promise((resolve,reject) => {
    // 新建xhr对象
    let xhr = new XMLHttpRequest();
    // 新建http请求
    xhr.open("GET",url,true);
    // 设置状态监听函数
    xhr.onreadystatechange = function(){
      if (this.readyState !== 4)
        return;
      if (this.status === 200){
        resolve(this.responseText);
      } else {
        reject(new Error(this.statusText));
      }
    }
    // 设置响应的数据类型
    xhr.responseType = "json"
    // 设置请求头信息
    xhr.setRequestHeader("Accept","application/json");
    // 发送http请求
    xhr.send(null);
  })
}
```