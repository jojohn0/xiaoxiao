---
title: JS笔记-DOM事件流
date: 2020-12-06 10:52:40
tags: DOM
description: 本文主要介绍了JavaScript的DOM事件流的原理
categories: JavaScript基础
# cover: https://s3.ax1x.com/2021/02/11/yBP99P.jpg
---
#### 事件流
事件流描述的是从页面中接收事件的顺序。
事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即 DOM 事件流。
<!--more-->


#### DOM事件流
DOM事件流分为三个阶段：
1.捕获阶段
2.当前目标阶段
3.冒泡阶段


![在这里插入图片描述](https://img-blog.csdnimg.cn/20201206104218771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rhbnl1YW5h,size_16,color_FFFFFF,t_70#pic_center)

**注意**
1. JS 代码中只能执行捕获或者冒泡其中的一个阶段。
2. onclick 和 attachEvent 只能得到冒泡阶段。
3. addEventListener(type, listener[, useCapture])第三个参数**如果是 true**，表示在**事件捕
获阶段**调用事件处理程序；**如果是 false**（不写默认就是false），表示在**事件冒泡阶段**调用事件处理
程序。
4. 实际开发中很少使用事件捕获，而是更关注事件冒泡。

举个例子：

```html
<div class="father">
	<div class="son"></div>
</div>
```

```js
let son = document.querySelector('.son');
let father = document.querySelector('.father');

son.addEventListener('click',function(){
	console.log('son');
},false);

father.addEventListener('click',function(){
	console.log('father');
},false);
```

由于addEventListener函数的第三个参数是false，因此在事件冒泡阶段才调用事件处理程序，因此当点击son盒子时，
出现了以下结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201206105056317.png#pic_center)

**使用stopPropagation() 来阻止时间冒泡**

```js
son.addEventListener('click',function(e){
	console.log('son');
	e.stopPropagation();
},false);
```

将son的事件监听函数改为上述，即可阻止事件冒泡，原理就是事件对象使用了stopPropagation() 函数。

**利用冒泡，通过事件委托实现给父元素增加监听器，影响子元素**

```html
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</ul>
```

```js
let ul = document.querSelector('ul');
ul.addEventListener('click',function(e){
	e.target.style.backgroundColor = 'green';
},false);
```
上述代码实现点击每个li，每个li背景颜色改变。其原理就是通过给li的父元素ul设置监听器，通过事件委托而实现的。
