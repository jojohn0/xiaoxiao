---
title: 理解JS中的构造函数
date: 2021-03-26 20:39:18
tags: 构造函数
categories: JavaScript基础
description: 介绍JS创建对象模式之构造函数模式，同时理解new一个对象的执行过程
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---

## 理解构造函数模式

### 为什么要使用构造函数模式

* 首先我们看看使用工厂模式创建对象

```js
  function createPerson(name,age){
    let o = new Object();
    o.name = name;
    o.age = age;
    o.sayName = function(){
        console.log(this.name)
      }
    return o;
  }
```
<!--more-->

* 在这个例子中，使用工厂模式创建对象，每次可以用不同的参数调用此函数，每次都会返回两个属性和一个方法的对象
* 但是通过工厂模式无法解决对象标识问题(即新创建的对象是什么类型)，但**构造函数模式**可以解决这个问题

### 通过构造函数模式创建对象

```js
  function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = function(){
      console.log(this.name);
    }
  }

  let person1 = new Person('simple',20);
  let person2 = new Person('zywoo',19);

  person1.sayName(); //simple
  person2.sayName(); //zywoo
```

### 构造函数模式与工厂模式的区别

* 构造函数没有显示地创建对象
* 属性和方法直接赋值给this
* 没有return
* 注意：函数名Person的**首字母为大写**，按照惯例，构造函数名称的首字母都是要大写的，非构造函数则以小写字母开头
* 创建构造函数的实例前，需要使用new操作符

### new一个对象的过程

```js
  let player = new Person("curry",33);
```

上述代码的过程如下：

1.  在内存中创建一个新对象
2. 这个新对象的内部 [[Prototype]] 特性被赋值为构造函数的prototype属性：
```js
  player.__proto__ = Person.prototype
``` 

3.  构造函数内部的this被赋值给新的对象 也就是新对象和函数调用的this会绑定起来

```js
  Person.call(player,"curry",30);
```

4. 执行构造函数内部的代码 即给新对象添加属性和方法

```js
  player.name;
  player.age;
  player.sayName();
```

5. 如果构造函数返回非空对象，则返回该对象；否则，返回刚创建的新对象


## 构造函数与普通函数

* 构造函数与普通函数唯一的区别就是**调用方式不同**。除此之外，**构造函数也是函数**。并没有把某个函数定义为构造函数的特殊语法。任何函数只要使用 new 操作符调用就是构造函数，而不使用 new 操作符调用的函数就是普通函数

```js
  // 作为构造函数
  let person = new Person('simple',20);
  person.sayName(); // simple

  // 作为普通函数调用
  Person('zywoo',19); // 添加到window对象
  window.sayName(); // zywoo
```

* 在上述例子中，作为普通函数调用时，结果会将属性和方法添加到window对象
* 在调用一个函数而没有明确设置 this 值的情况下（即没有作为对象的方法调用，或者没有使用 call() / apply() 调用），this 始终指向 Global 对象（在浏览器中就是 window 对象）

## 构造函数的问题

* 通过构造函数定义的方法会在每个实例上都创建一遍，但它们做的都是同一件事情
* 可以通过把函数定义转移到构造函数外部

```js
  function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayName = sayName;
  }

  function sayName(){
    console.log(this.name);
  }
```

* 在上面这个例子中，sayName() 被定义在了构造函数外部。在构造函数内部， sayName 属性等于全局 sayName()函数。因为这一次 sayName 属性中包含的只是一个指向外部函数的指针
* 这样虽然解决了相同逻辑的函数重复定义的问题，但全局作用域也因此被搞乱了，因为那个函数实际上只能在一个对象上调用
* 要想解决这个问题，可以使用**原型模式**来解决