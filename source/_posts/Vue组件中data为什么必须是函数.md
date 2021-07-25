---
title: Vue组件中data为什么必须是函数
date: 2021-04-23 09:50:29
tags: Vue
categories: Vue
# cover: https://z3.ax1x.com/2021/04/22/cOPGIs.jpg 
description: Vue组件中data为什么必须是函数
---

## 前言

在声明式渲染，创建Vue实例时，我们通过mustache语法把data中的message属性渲染到页面上去
<!--more-->
```js
  <div id="app">
    {{message}} 
  </div>

  const app = new Vue({
    el:'#app',
    data{
      message:'hello'
    }
  }) 
```

但是在创建Vue组件时，组件中data却必须是一个函数

## 组件中的data为什么必须是函数

首先我们知道组件的一个特性就是可以被复用的，一个组件被创建好之后可以使用在各个地方，而每个组件内部的**data**数据应该是互不影响的，所以组件每被复用一次，data的数据就应该被复制一次


```html
<template id="cpn">
  <div>
    <h2>按钮被点击了{{counter}}次数</h2>
	<button @click="counter++">点击</button>
  </div>
</template>
```

```js
Vue.component('cpn',{
  template:"#cpn",
  data(){
    return {
      counter:0
    }
  }
})
```

![](https://z3.ax1x.com/2021/04/23/cOaHbT.png)

在上面这个例子中，每个组件的data返回的都是一个新的对象，也就是说每个组件所指向的那个对象都是不同的，因此data中的counter数据没有被共用

```js
data:{
  counter:0
}
```

而如果是仅仅用对象来保存数据，所有组件使用的都会是data对象，无法达到组件复用的目的

![](https://z3.ax1x.com/2021/04/23/cOdpKx.png)



## 总结

组件中的data写成一个函数，数据以对象的形式返回，这样每复用一次组件，都会返回一个新的对象，每个组件指向的内存空间是不一样的，而如果data仅仅以对象的形式创建，所有组件实例共用一份data，无法达到组件复用的目的
