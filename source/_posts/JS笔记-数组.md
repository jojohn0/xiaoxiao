---
title: JS笔记-数组
date: 2021-01-19 10:22:39
tags: JS数组
description: 本文主要介绍了JavaScript数组的使用与相关方法
categories: JavaScript基础
# cover: https://s3.ax1x.com/2021/02/11/yB9GMF.jpg
---
### **创建数组**
1.  使用Array构造函数
      * 语法：new Array()
      * 小括号说明：
        * 预先知道数组要保存的长度
        * 向Array构造函数传递数组应包含的项
  ```js
    let arr = new Array(3);
    let colors = new Array("blue","red");
  ```
<!--more-->

2. 使用数组字面量表示法
  ```js
    let arr = [1,2,3,{name:"mike",age:18}];
  ```
### **数组长度**
1. 语法：array.length
2. 功能：获取数组array的长度
3. 返回值：number
4. 说明：
  	* 通过设置length可以从数组的末尾移除项或向数组中添加新项。
 	 * 把一个值放在超出当前数组大小的位置上时，会重新计算数组长度值，长度值等于最后一项索引加1
```js
  let arr = [1,2,3,4];
  console.log(arr.length); // 4
  arr[99] = 1;
  console.log(arr.length); // 100
```

### **数组的栈方法**
1. push()
   * 语法：array.push(newele1,newele2,...);
   * 功能：把它的参数顺序添加到array的尾部
   * 返回值：把指定的值添加到数组后的长度
  ```js
    let arr = [1,2,3];
    let newLen = arr.push(4);
    console.log(arr); // [1,2,3,4];
    console.log(newLen); // 4
  ```
2. unshift()
   * 语法：array.unshift(newele1,newele2,...);
   * 功能：把它的参数顺序添加到array的开头
   * 返回值：把指定的值添加到数组的新长度
  ```js
    let arr = [1,2,3];
    let newLen = arr.unshift(4);
    console.log(arr,newLen); // [1,2,3,4],4
  ```
3. pop()
   * 语法：array.pop()
   * 功能：删除array的最后一个元素
   * 返回值：被删除的元素
  ```js
    let arr = [1,2,3];
    let delEmt = arr.pop();
    console.log(arr,delEmt); // [1,2],3
  ```
4. shift()
   * 语法：array.shift()
   * 功能：删除array的第一个元素
   * 被删除的元素
  ```js
    let arr = [1,2,3];
    let delEmt = arr.shift();
    console.log(arr,delEmt); // [2,3],1
  ```
### **数组的转换方法**
1. join()
   * 语法：array.join(separator)
   * 功能：用于把数组中的所有元素放入一个字符串
   * 返回值：字符串
  ```js
    let arr = [2,3,4];
    let str = arr.join();
    console.log(str); // 2,3,4
    let colors = ["red","green","blue"];
    let colorStr = colors.join("-");
    console.log(colorStr); // red-green-blue
  ```
2. reverse()
    * 语法：array.reverse()
    * 功能：用于颠倒数组中元素的顺序
    * 返回值：数组
  ```js
    let arr = [2,3,4];
    let newArr = arr.reverse();
    console.log(newArr); // [4,3,2];
  ```
3. sort()
    * 语法：array.sort(sortby)
    * 功能：用于对数组的元素进行排序
    * 返回值：数组
    * 说明
      * 即使数组中的每一项都是数值，sort()方法比较的也是字符串
      * sort()方法可以接收一个比较函数作为参数
  ```js
    let arr = ["color","border","ab"];
    console.log(arr.sort()); // ["ab","border","color"]
    let nums = [1,4,9,81];
    console.log(nums.sort()); // [1,4,81,9]
    console.log(nums.sort(
        function(a,b){
                return a-b
            })
        ); // [1,4,9,81];
  ```
4. concat()
    * 语法：array.concat(arr1,arr2,...);
    * 功能：用于连接两个或多个数组
    * 返回值：数组
  ```js
    let arr1 = [1,2,3], arr2 = [4,5,6];
    arr3 = arr1.concat(arr2,[7,8,9]);
    console.log(3); // [1,2,3,4,5,6,7,8,9]
  ```
5. slice()
   * 语法：array.slice(start,end)
    >start和end指的时数组中索引值，
    >截取从start和end的元素，即从start和end-1的元素
   * 功能：从已有的数组中返回选定的元素
   * 返回值：数组
   * 参数：
     * start(必需)规定从何处开始选取，如果时负数，则规定从数组尾部开始算起的位置
     * end(可选)规定从何处结束选取，该参数是数组片段结束处的数组下标
   * 说明：
     * 如果未指定end，切分的数组包含从start到数组结束的所有元素
     * 如slice()方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置
  ```js
    let arr = ["red","green","blue","yellow"];
    console.log(arr.slice(2)); // ["blue","yellow"]
    console.log(arr.slice(1,4)); 
    // ["green,","blue","yellow"]
    console.log(arr.slice(-2,4)); 
    // ["blue","yellow"]
  ```
6. **splice()**
 	1. 删除：
    	1. 语法：array.splice(index,count)
    	2. 功能：删除从index处开始的零个或多个元素
    	3. 返回值：含有被删除的元素的数组
    	4. 说明：count是删除的项目数量，如果设置为0，则不会删除项目。如果不设置，则删除从index开始的所有值
```js
    let arr = ["a","b","c","d","f"];
    let delArr = arr.splice(2,2);
    console.log(delArr); // ["c","d"]
    console.log(arr.splice(2,0)); // []
```
  2. 插入：
    	1. 语法：array.splice(index,0,item1,...itemx)
    	2. 功能：在指定位置插入值
    	3. 参数：
      			1. index：起始位置
      			2.	0：要删除的项数
       			3. item1,...itemx：要插入的项
    	4. 返回值：数组
```js
    let arr = ["a","b","c","d","f"];
    let insertArr = arr.splice(3,0,1,2);
    console.log(arr);
    // ["a","b","c",1,2,"d","f"]
```
3. 替换
    * 语法：array.splice(index,count1,item1,..,itemx)
    * 功能：在指定位置插入值，且同时删除任意数量的项
    * 参数：
      * index：起始位置
      * count：要删除的项数
      * item1,...,itemx：要插入的项
    * 返回值：从原始数组中删除的项(如果没有删除任何项，则返回空数组)
```js
    let arr = ["a","b","c","d","f"];
    let reArr = arr.splice(1,2,"x","y");
    console.log(arr); 
    // ["a","x","y","d","f"]
    console.log(reArr);
    // ["b","c"]
  ```
### **位置方法**
1. indexOf()
   * 语法：array.indexOf(searchvalue,startIndex)
   * 功能：从数组的开头(位置0)开始向后查找
   * 参数：
     * searchvalue：必需，要查找的项
     * startIndex：可选，起点位置的索引
     * 返回值：number，查找的项在数组中的位置，没有找到的情况下返回-1
  ```js
      let nums = [1,7,5,7,8,1,6,9];
      var pos = nums.indexOf(7);
      console.log(pos); // 2
      console.log(nums.indexOf(99)); // -1
      console.log(nums.indexOf(7,3)); // 3
  ```
2. lastIndexOf()
   * 语法：array.lastIndexOf(searchvalue,startIndex)
   * 功能：从数组的末尾开始向后查找
   * 参数：
     * searchvalue：必需，要查找的项
     * startIndex：可选，起点位置的索引
     * 返回值：number，查找的项在数组中的位置，没有找到的情况下返回-1
  ```js
     let nums = [1,7,5,7,8,1,6,9];
     var pos = nums.lastIndexOf(7);
     console.log(pos); // 3
     console.log(nums.indexOf(99)); // -1
     console.log(nums.indexOf(1,3)); // 5
  ```
