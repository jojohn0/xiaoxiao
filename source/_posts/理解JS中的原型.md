---
title: 理解JS中的原型
date: 2021-03-27 15:42:48
tags: 原型
categories: JavaScript基础
description: 介绍JS创建对象的原型模式与使用原型模式
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---


## 为什么使用原型模式

* 与构造函数不同的是，使用原型模式定义的属性和方法是每个实例化对象共享的
* 每个函数都会创建一个prototype属性，这个属性是一个对象，包含应该由特定引用类型的实例共享的属性和方法
<!--more-->

## 理解原型

```js
  function Person(){}
  Person.prototype.name = 'simple';
  Person.prototype.age = 20;
  Person.prototype.sayName = function(){
    console.log(this.name);
  }

  let person1 = new Person();
  let person2 = new Person();
```

* 无论何时，只要创建一个函数，就会按照特定的规则为这个函数创建一个 prototype 属性（指向原型对象）

```js
  Person.prototype
```

* 所有**原型对象**自动获得一个名为 constructor 的属性，指回与之关联的构造函数, **constructor属性只存在于原型对象**
* 在上面这个例子中，Person.prototype 指向原型对象，而 Person.prototype.constructor 指回 Person 构造函数

```js
  console.log(Person.prototype.constructor === Person); // true
```

* 每个实例化的对象都通过__proto__属性链接到原型对象上，但是它实际上指向隐藏特性 [[prototype]]

```js
  console.log(person1.__proto__=== Person.prototype); // true
```

## 原型层级

* 在通过对象访问属性时，会按照这个属性的名称开始搜索。搜索开始于对象实例本身。如果在这个实例上发现了给定的名称，则返回该名称对应的值。如果没有找到这个属性，则搜索会沿着指针进入原型对象，然后在原型对象上找到属性后，再返回对应的值

```js
  person1.sayName(); // simple
```

![](https://z3.ax1x.com/2021/03/27/6zAUt1.png)

* 虽然可以通过实例读取原型对象上的值，但不可能通过实例重写这些值。如果在实例上添加了一个与原型对象中同名的属性，那就会在实例上创建这个属性，这个属性会遮住原型对象上的属性。只要给对象实例添加一个属性，这个属性就会遮蔽原型对象上的同名属性，也就是虽然不会修改它，但会屏蔽对它的访问

```js
  person1.name = 'curry';
  console.log(person1.name); // curry
  console.log(person2.name); // simple
```

* 下图是在为person1实例对象添加name属性时


![](https://z3.ax1x.com/2021/03/27/6zAsne.png)


## in 操作符 和 hasOwnProperty()

* 只要通过对象可以访问， in 操作符就返回 true ，而 hasOwnProperty() 只有属性存在于实例上时才返回 true 

```js
  console.log('name' in person1); // true;
  console.log('name' in person2); // true;
  console.log(hasOwnProperty(person1,'name')); // true
  console.log(hasOwnProperty(person2,'name')); // false
```

## 其他原型语法

```js
  function Person(){}
  Person.prototype = {
    name:'simple',
    age:20,
    sayName:function(){
      console.log(this.name);
    }
  }
```

* 在上面这个例子中，Person.prototype 被设置为一个字面量创建的对象，这样会有一个问题， Person.prototype.constructor 的 constructor 属性就不指向 Person构造函数
* 如果 constructor 的值很重要，则可以像下面这样在重写原型对象时专门设置一下它的值

```js
  Person.prototype = {
    // 将constructor属性指回原来的构造函数
    constructor:Person,
    name:'simple',
    age:20,
    sayName:function(){
      console.log(this.name);
    }
  }
```

* 重写整个原型会切断最初原型与构造函数的联系，但实例引用的仍然是最初的原型。重写构造函数上的原型之后再创建的实例才会引用新的原型。而在此之前创建的实例仍然会引用最初的原型。

```js
  function Person(){}
  let person1 = new Person();
  Person.prototype = {
    constructor:Person,
    name:'simple',
    age:20,
    sayName:function(){
      console.log(this.name);
    }
  }
  person1.sayName(); // error

  let person2 = new Person();
  person2.sayName(); // 'simple'
```

![](https://z3.ax1x.com/2021/03/27/6zVIoj.png)

* 可以看到person1指向的仍然时原来的原型对象，而person2作为改变原型对象后的实例对象，它的__proto__属性指向的后面创建的原型对象

## 原型的问题

* 它弱化了向构造函数传递初始化参数的能力，会导致所有实例默认都取得相同的属性值。虽然这会带来不便，但还不是原型的最大问题。原型的最主要问题源自它的共享特性。

```js
  function Person(){}
  Person.prototype = {
    constructor:Person,
    name:'simple',
    age:20,
    friends:['curry','paul'],
    sayName:function(){
      console.log(this.name);
    }
  }

  let person1 = new Person();
  let person2 = new Person();
  person1.friends.push('james');
  console.log(person1.friends); // ['curry','paul','james']
  console.log(person2.friends); // ['curry','paul','james']
```

* 由于这个 friends 属性存在于 Person.prototype 而非 person1 上，新加的这个字符串也会在（指向同一个数组的） person2.friends 上反映出来。如果这是有意在多个实例间共享数组，那没什么问题。但一般来说，不同的实例应该有属于自己的属性副本。这就是实际开发中通常不单独使用原型模式的原因

## 总结

1. constructor属性的含义就是指向该对象的构造函数，所有函数（此时看成对象了）最终的构造函数都指向Function，只有prototype才有constructor属性
2. \__proto__的作用就是需要访问一个对象的属性时，如果实例对象中不存在该属性，就会通过__proto__属性所指向的原型对象里面去找。
3. prototype属性指向原型对象
4. 在重写原型对象时，要考虑constructor属性的指向问题，以及之前创建的对象的__proto__属性无法指向新的原型对象
5. 原型对象的属性包含引用类型数据，它们实例化对象将会共享该数据，有一个实例更改该属性，所有实例和原型对象的该属性都会被修改