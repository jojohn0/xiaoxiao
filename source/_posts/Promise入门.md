---
title: Promise入门
date: 2021-05-22 19:44:45
tags: Promise
catergories: JavaScript基础
description: Promise简单入门
# cover: https://z3.ax1x.com/2021/05/22/gLrzsf.jpg

---

## Promise是做什么的

* Promise是异步编程的解决方案
* 当网络请求非常复杂时，就会出现回调地狱
<!--more-->
## 一个例子

```js
$.ajax("url1", function (data1) {
        $.ajax(data1["url2"], function (data2) {
          $.ajax(data2["url3"], function (data3) {
            $.ajax(data3["url3"], function (data4) {
              console.log(data4);
            });
          });
        });
      });
```

* 上面这种情况，正常时不会有什么问题的
* 但是这样的代码难看且不易维护
* 我们更加期望的是一种更加优雅的方式来进行异步操作
* Promise可以以一种非常优雅的方式来解决这个问题

## Promise的基本使用

```js
new Promise((resolve, reject) => {
        setTimeout(() => {
          // 成功的时候调用resolve
          resolve("hello world");

          //   失败的时候调用reject
          reject("error message");
        }, 1000);
      })
        .then((data) => {
          console.log(data);
          console.log(data);
        })
        .catch((err) => {
          console.log(err);
        });
```

* Promise接收一个参数，参数里面分别是resolve，reject
* resolve和reject是本身也是一个函数
* 如果发送网络请求成功，则使用then方法，接收的参数是resolve传入的参数
* 如果发送网络请求失败，则使用catch方法，接收的参数是reject传入的参数

## Promise三种状态

异步操作之后会有三种状态

* pending：等待状态，比如正在进行网络请求，或者定时器没有到时间
* fulfill：满足状态，当我们主动回调resolve时，就处于该状态，并且会回调.then()
* reject：拒绝状态，当我们主动回调reject时，就处于该状态，并且会回调.catch()

## Promise的另外处理方式

可以只写then方法，then方法里面传入两个函数，一个成功的回调，一个失败的回调

```js
new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve("hello world");
          reject("error message");
        }, 1000);
      }).then(
        (res) => {
          console.log(res);
        },
        (err) => {
          console.log(err);
        }
      );
```

## Promise的链式调用

1. 链式调用的简写

```js
// new Promise(resolve=> resolve(结果))简写 Promise.resolve(结果)
new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve("aaa");
        }, 1000);
      })
        .then((res) => {
          //   处理10行代码
          console.log(res, "第一层的10行代码");
          // 对结果进行第一次处理
          return Promise.resolve(res + "111");
        })
        .then((res) => {
          console.log(res, "第二层的10行代码");
          return Promise.resolve(res + "222");
        })
        .then((res) => {
          console.log(res, "第三层的10行代码");
        });
```

2. 省略掉Promise.resolve/Promise.reject

```js
new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve("aaa");
        }, 1000);
      })
        .then((res) => {
          //   处理10行代码
          console.log(res, "第一层的10行代码");
          // 对结果进行第一次处理
          return res + "111";
        })
        .then((res) => {
          console.log(res, "第二层的10行代码");
          return res + "222";
        })
        .then((res) => {
          console.log(res, "第三层的10行代码");
        });
```

## Promise的all方法使用

在有些时候，我们可能出现这种情况，某个需求依赖两次网络请求，但是你无法判断网络请求的先后顺序，于是只能写这样的代码

```js
let isRes1 = false;
let isRes2 = false;
// 请求1
$.ajax({
    url:'url1',
    success(){
        isRes1 = true;
        handleRes();
    }
})

// 请求2
$.ajax({
    url:'url2',
    success(){
        isRes2 = true;
        handleRes();
    }
})

function handleRes(){
    if(isRes1 && isRes2){

    }
}

```

使用Promise.all可以进行包装

```js
Promise.all([
        new Promise((resolve, reject) => {
            $.ajax({
              url: "url1",
              success(data) {
                resolve(data);
              },
            });
        }),

        new Promise((resolve, reject) => {
            $.ajax({
              url: "url2",
              success(data) {
                resolve(data);
              },
            });
        }),
      ]).then((results) => {
        // 获取两次请求的结果
        console.log(results[0]);
        console.log(results[1]);
      });
```