---
title: JS笔记-字符串
date: 2021-01-19 10:33:38
tags: JS字符串
categories: JavaScript基础
description: 本文主要介绍了JavaScript的字符串的使用和相关方法
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---
### **位置方法**
1. charAt()
   1. 语法：string.charAt(index)
   2. 功能：返回string中index位置的字符
  ```js
    let str = "hello world";
    console.log(str[1]); // "e"
    console.log(str.charAt(0)); // "h"
    console.log(str.charCodeAt(4));
    // 111 charCodeAt()返回的是字符的编码 
  ```
<!--more-->

2. indexOf()
   1. 语法：string.indexOf(value);
   2. 功能：从一个字符串中搜索给定的子字符串，返回子字符串的位置
   3. 返回值：数值
   4. 说明：如果没有找到该子字符串，则返回-1
  ```js
    let arr = "marry.@sohu.com";
    console.log(arr.indexOf("rr")); // 2
    console.log(arr.indexOf(".")); // 5
  ```
3. lastIndexOf()
   1. 语法：string.lastIndexOf(value);
   2. 功能：从一个字符串中搜索给定的子字符串，返回子字符串的位置
   3. 返回值：数值
   4. 说明：如果没有找到该子字符串，则返回-1
  ```js
    let arr = "marry.@sohu.com";
    console.log(arr.lastIndexOf(".")); // 12
  ```
### **截取方法**
1. slice()
   1. 语法：string.slice(start,end)
   2. 功能：截取子字符串
   3. 参数说明：
      1. start：必需，指定子字符串的开始位置
      2. end：可选，表示子字符串到哪里结束，end本身不在截取范围之内，省略时截取至字符串的末尾
      3. 当参数为负数时，会将传入的负值与字符串的长度相加
   ```js
    let str = "hello wolrd"
    console.log(str.slice(7,10));
    // olr
    console.log(str.slice(1,3));
    // el
    console.log(str.slice(-3));
    // rld 
    // 相当于console.log(str.slice(11-3));
   ```
2. substring()
   1. 说明：语法及功能同slice()完全一样
   2. 区别：
      1. 当参数为负数时，自动将参数转换为0
      2. substring()会将较小的数作为开始位置，将较大的数作为结束位置
   ```js
    let str = "hello world";
    console.log(str.substring(-2,-2)); // ""
    console.log(str.substring(2,-2)); // 相当于(0,2)
   ```
3. substr()
   1. 语法：string.substr(start,len)
   2. 功能：截取子字符串
   3. 参数说明：
      1. start：必需，指定子字符串的开始位置
      2. len：可选，表示截取的字符总数，省略时截取至字符串的末尾
      3. 当start为负数时，会将传入的负值与字符串的长度相加
      4. 当len为负数时，返回空字符串
   ```js
    let str = "hello world";
    console.log(str.substr(6,3));
    // wor
    // 等价于 str.substring(6,9)
    console.log(str.substr(-5,4));
    // worl
    // 相当于 等价于 str.substr(11-5,4)
    console.log(3,-4); // ""
   ```
### **其他方法**
1. split()
   1. 语法：string.split(separator)
   2. 功能：把一个字符串分割成字符串数组
   3. 返回值：Array
   4. 说明：separator：必需，分隔符
  ```js
    let str = "hello-world-c";
    let arr = str.split("-");
    console.log(arr); // ["hello","world","c"];
  ```
2. replace()
   1. 语法：string.replace(regexp/substr,replacement)
   2. 功能：在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串
   3. 返回值：String
   4. 参数：
      1. regexp：必需。规定子字符串或要替换的模式的RegExp对象
      2. replacement：必需。一个字符串值
  ```js
    let str = "010-62921312,400-100-9087,010-3421412";
    let newStr = str.replace(","," ");
    console.log(newStr); // "010-62921312 400-100-9087 010-3421412"
  ```
### **转换大小写方法**
1. toUpperCase()
   1. 语法：string.toUpperCase()
   2. 功能：把字符串转换为大写
2. toLowerCase()
   1. 语法：string.toLowerCase()
   2. 功能：把字符串转换为小写


### **实例：将字符串转为驼峰命名**
```js
	funciton camelBack(str){
		//首先将字符串转换为数组
		let arr = str.split("-"),newStr = arr[0];
		for(let i = 0; i < arr.length; i++){
			let word = arr[i];
			newStr += word.charAt(0).toUpperCase() + word.substr(1);
			console.log(newStr)
		}
	}
	
	let str = "border-left-color"
```
