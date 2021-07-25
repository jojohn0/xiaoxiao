---
title: JavaScript箭头函数与普通函数
date: 2021-02-12 17:14:13
tags: 箭头函数
categories: JavaScript基础
description: 本文主要介绍了JS中箭头函数与普通函数的区别,以及用法
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---
### 语法格式

* 普通函数：function(){}
* 箭头函数: ()=>{}

```js
    let words = ['a','b','c']

    let words_normal = words.map(function(item){
        return item += 'd';
    });

    // 箭头函数只有一个参数无需写括号
    // 如果只有一个表达式的时候可以不添加大括号
    let words_arrow = words.map(item => item += 'd');
    console.log(words_arrow);
```
<!--more-->

### new 与 原型

箭头函数不能创建new函数的实例

```js
    function Star(){}
    Star.prototype.name = 'a';

    const normal = new Star();
    console.log(normal);

    const Arrow = () => {};
    const arrow = new Arrow();
```
**得到如下结果**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210212171324243.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rhbnl1YW5h,size_16,color_FFFFFF,t_70#pic_center)



### arguments

箭头函数无arguments对象

但仍然可以让箭头函数看起来有arguments的效果

```js
    function normal(){
        return ()=> arguments.length;
    }

    let arrow = normal(1,2,3);
    // 由于作用域链的关系，箭头函数保存了arguments的长度
    console.log(arrow()); // 3
```

### this指向

箭头函数的this值取决于外部普通函数里的this值

箭头函数不能通过call apply bind来改变this的值，
但箭头函数仍然可以调用call apply bind方法

### 总结

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210212171228809.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Rhbnl1YW5h,size_16,color_FFFFFF,t_70#pic_center)

