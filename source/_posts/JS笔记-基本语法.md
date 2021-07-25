---
title: JS笔记-基本语法
date: 2021-01-17 09:57:41
tags: JS语法
categories: JavaScript基础 
description: 本文主要介绍了JavaScript数据类型和操作符
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---

## JS数据类型
### typeof操作符

* 功能：检测变量类型
* 返回值：string number boolean object undefined function
```javascript
  console.log(typeof null); // object
```
<!--more-->


### null 与 undefined
* undefined值是派生自null值的，所以undefined == null 的返回结果是true。
  
## Number类型
### Number：表示整数和浮点数
### NaN：非数值 是一个特殊的数值
* 任何涉及NaN的操作都会返回NaN
* NaN与任何值都不相等，包括NaN本身
```javascript
  console.log(NaN == NaN); // false
  console.log(NaN * 1); // NaN
```
### isNaN()
* 功能：检测n是否是“非数值”
* 参数：参数可以是任何类型
> isNaN()对接收的数值，先尝试转换为数值，再检测是否为非数值
```javascript
  console.log(isNaN("abc")); // true
  console.log(isNaN("123")); // false
```

### 数值转换
1. Number()
2. parseInt()
3. parseFloat()

* Number()可以用于任何数据类型
* parseInt()和parseFloat()则专门用于把字符串转换为数值

### parseInt()
* parseInt()：会忽略字符串前面的空格，直到找到第一个非空格字符。

```js
  console.log(parseInt("  123")); // 123
```
* 转换空字符串或空格字符串返回NaN

```js
  console.log(parseInt("  ")); // NaN
  console.log(parseInt("")); // NaN
```
* 此函数提供第二个参数：转换时使用的基数

```js
  let a = "15";
  console.log(parseInt(a,8)); // 13(5+8)
```

### parseFloat()
* parseFloat：从第一个字符开始解析每个字符，直到遇见第一个无效的浮点数字符为止。

```js
  let a = "13.23.21";
  console.log(parseFloat(a)); // 13.23
```

## String类型
### toString()与String()
* 语法：str.toString()
* 功能：将str转换成字符串
* 返回值：str的一个副本
* 参数：str是要转换的内容，可以是数值、布尔值、对象和字符串。
* 说明：在不知道要转换的值是不是null或undefined的情况下，还可以使用String()函数，它能够将任何类型的值转换为字符串。

```js
  let a = null,b = 13;
  b = b.toString();
  a = String(a);
  console.log(a,b); // "null" "13"
```

## Boolean类型
### 类型转换
* 除了0之外的所有数字，转换为布尔型都为true
* 除了""之外的所有字符，转换为布尔型都为true
* null和undefined转换为布尔型都为false

```js
  console.log(Boolean(0)); // false
  console.log(Boolean(1)); // true
  console.log(Boolean("")); // false
  console.log(Boolean("abc")); // true
  console.log(Boolean(null)); // false
  console.log(Boolean(undefined)); // false
```

## 算数操作符
### 递增
* ++a与a++都是对a进行递增的操作
* 区别：
  * ++a先返回递增之后的a的值
  * a++先返回a的原值，再返回递增之后的值

```js
  let a = 1;
  console.log(a++); // 1
  console.log(++a); // 3
```

## 比较操作符
### ==：相等，只比较值是否相等
### ===：相等，比较值的同时比较数据类型是否相等
### !=：不相等，比较值是否不相等
### ！==：不相等，比较值的同时比较数据类型是否不相等

```js
  let a = 1,b = "1";
  console.log(a==b); // true
  console.log(a===b); // false
```

## 逻辑操作符
### 逻辑与&&(只要有一个条件不成立，返回false)
* 说明：在有一个操作数不是布尔值的情况，逻辑与操作就不一定返回值，此时它遵循下列规则：
  * 如果第一个操作数隐式类型转换后为true，则返回后一个操作数
  ```js
    console.log(1 && 0 && "abc"); // 0
  ```

  * 如果第一个操作数隐式类型转换后为false，则返回第一个操作数
  ```js
    console.log(0 && "1" && true); // 0
  ```
  * 如果后面有一个操作数是null，且null之前的操作数隐式类型转换后为true,则返回null
  ```js
    console.log("2" && 1 && null); // null
    console.log("2" && 0 && null); // 0
  ```
  * 如果后面有一个操作数是NaN，且NaN之前的操作数隐式类型转换后为true,则返回NaN
  ```js
    console.log("2" && 1 && NaN); // NaN
    console.log("2" && 0 && NaN); // 0
  ```
  * 如果后面有一个操作数是undefined，且undefined之前的操作数隐式类型转换后为true,则返回undefined
  ```js
    console.log("2" && 1 && undefined); // undefined
    console.log("2" && 0 && 0); // 0
  ```

### 逻辑或||(只要有一个条件成立，返回true)
* 说明：在有一个操作数不是布尔值的情况，逻辑或操作就不一定返回值，此时它遵循下列规则：
  * 如果第一个操作数隐式类型转换后为true，则返回第一个操作数
  ```js
    console.log("1" || 0 || true); // 1
  ```
  * 如果第一个操作数隐式类型转换后为false，则一直往后，若没有隐式转换为true的操作数,则返回最后一个操作数
  ```js
    console.log("" || 0 || true); // true
    console.log(0 || true || ""); // true
  ```
  * 如果两个操作数是null，则返回null
  ```js
    console.log(null || null); // null
  ```
  * 如果两个操作数是NaN，则返回NaN
  ```js
    console.log(NaN || NaN); // NaN
  ```
  * 如果两个操作数是undefined，则返回undefined
  ```js
    console.log(undefined || undefined); // undefined
  ```

### 逻辑非！
* 无论操作数是什么数据类型，逻辑非都会返回一个布尔值
* ！！同时使用两个逻辑非操作符时
  * 第一个逻辑非操作会基于无论什么操作数返回一个布尔值
  * 第二个逻辑非则对该布尔值求反
  ```js
    let a = 0;
    console.log(!!a); // false
  ```