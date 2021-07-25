---
title: Vuex入门
date: 2021-05-24 23:27:15
tags: Vuex
catergories: 
- Vue
description: Vuex简单入门
# cover: https://z3.ax1x.com/2021/05/24/gxBjz9.jpg
---
## 什么是Vuex

Vuex 是一个专为 Vue.js 应用程序开发的状态管理插件。它采用集中式存储管理应用的所有组件的状态。这是官方文档的解释，但是什么是状态管理？
<!--more-->
## 状态管理

状态可以理解为数据，那么什么是状态管理。举一个简单的例子，如果多个组件之间需要相互使用数据，在这个过程中如果涉及的组件过多，代码逻辑将会变得复杂。Vuex就是一个管家，将所有状态也就是所有数据放在一起进行管理，组件之间相互使用数据就变得方便了。


## 什么时候使用Vuex

* 多个组件依赖于同一状态时。
* 来自不同组件的行为需要变更同一状态。

## 使用Vuex

* 安装依赖```npm install vuex --save```
* 在项目目录的src中创建store文件夹
* 在store中创建index.js 写入

```js
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
//创建Vuex实例对象 用于保存在多个组件中共享的状态
const store = new Vuex.Store({
    state:{
    },
    getters:{
    },
    mutations:{
    },
    actions:{
    },
    modules:{

    }
})
export default store;
```

* 再在main.js中引入Vuex

```js
import Vue from 'vue';
import App from './App.vue';
import store from './store';

const vm = new Vue({
    store:store,
    render: h => h(App)
}).$mount('#app')
```

## Vuex的5个核心属性

分别是state getters mutations actions modules

## Vuex的过程

组件通过dispatch将异步处理的代码在actions里面处理，处理后再commit到mutations里面进行同步代码的处理，此时有浏览器插件Devtools对mutations进行监听

![](https://z3.ax1x.com/2021/05/24/gxBoq0.png)

> 如果没有异步代码可以直接commit到mutations里面

## Vuex的基本使用

* 通过this.$store.state.属性的方式来访问状态
* 通过this.$store.commit("mutations中的方法")来修改状态

```html
<!--APP.vue -->
<template>
  <div id="app">
    <h3>
      {{ $store.state.counter }}
    </h3>
    <button @click="addition">+</button>
    <button @click="subtraction">-</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
    };
  },
  components: {
  },
  methods: {
    addition() {
    
      this.$store.commit("increment");
    },
    subtraction() {
      this.$store.commit("decrement");
    },
  },
};
</script>

<style>
</style>

<!-- index.js -->
<script>
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
const store = new Vuex.Store({
    state:{
        counter:100
    },
    getters:{
    },
    mutations:{
    },
    actions:{
    },
    modules:{

    }
})
export default store;
</script>
```
* 注意：我们是通过提交mutations的方式，而非直接改变store.state.count，这是因为Vuex可以更准确的追踪状态的变化

## Vuex的state单一状态树

* 如果状态信息是保存到多个Store对象中，那么以后维护代码会非常困难
* Vuex使用单一状态树来管理应用层级的全部状态
* 单一状态树能让我们最直接找到某个状态的片段，更利于代码的维护

## Vuex的getters属性

getters属性类似于计算属性

```html
<template>
  <div id="app">

    <h2>app内容：getters相关信息</h2>
    <h2>
      {{ $store.getters.powerCounter }}
    </h2>

    <h2>
      {{ $store.getters.more20Player }}
    </h2>
    <h2>
      {{ $store.getters.more20PlayerLength }}
    </h2>

    <h2>
      {{ $store.getters.moreIdPlayer(1) }}
    </h2>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      message: "hello",
    };
  },
  components: {
    HelloVuex,
  },
  methods: {
  }
};
</script>

<style>
</style>


```

```js
const store = new Vuex.Store({
    state:{
        counter:100
    },
    getters:{
        powerCounter(state) {
             return state.counter * state.counter;
        },
        more20Player(state) {
             return state.players.filter(s => s.id > 20);
        },
        more20PlayerLength(state, getters) {
            return getters.more20Player.length;
        },
        moreIdPlayer(state) {
            return id => {
                return state.players.filter(s => s.id > id);
            };
        }
    },
    mutations:{
    },
    actions:{
    },
    modules:{

    }
})

```

## Vuex中的mutations

* Vuex的store状态的更新唯一方式，提交mutation
* mutations包含两部分
  * 字符串的事件类型(type)
  * 一个回调函数，该回调函数的第一个参数就是state

```js
mutations:{
    increment(state){
        state.count++
    }
}
```

* 通过mutations更新
```js
increment:function(){
    this.$store.commit("increment")
}
```

* 在通过mutation更新数据的时候，有可能我们会希望携带一些额外的参数
  * 参数被称为是mutation的payload
  * 如果是多个参数，payload就是一个对象

```js
incrementCount(state, payload) {
      console.log(payload);
      state.counter += payload.count;
},
```

* 两种提交风格
```js
 addCount(count) {
      // 1.普通的提交风格
      //   this.$store.commit("incrementCount", count);
      //   2.特殊的提交风格
      this.$store.commit({
        type: "incrementCount",
        count,
      });
    },
```

* 哪些方式修改属性是响应式的
```js
updateInfo(state) {
      /* 直接修改属性是响应式的 因为Vue已经提前在store中初始化好所需要的信息，
      state中的内容已经被加入到了响应式系统中 */
      state.info.name = "james";
      //   这种方式不是响应式的，新加入的属性虽然成功添加到对象中，但是没有加入到响应式系统中
      //   state.info["address"] = "gsw";
      // 响应式添加属性的解决方案，使用Vue.set()
      //   Vue.set(state.info, "address", "gsw");
      // 要想响应式的删除属性，使用Vue.delete()
      //   Vue.delete(state.info, "age");
    }
  },
```

## Vuex中的actions

* 跳过actions 直接提交 在没有异步操作的时候是可以的
* 存在异步操作的时候 需要使用dispatch到actions里面
* 如果你需要在请求成功后，得到请求结果 有两种解决方案


### 方案一(不够优雅)

```js
        // App.vue
        this.$store.dispatch("aUpdateInfo", {
          message: "我是携带的信息",
          success() {
            console.log("调用成功");
          },
        });
```

```js
    // index.js
    aUpdateInfo(context, payload) {
      setTimeout(() => {
        // 此时的context上下文是store对象
        context.commit("updateInfo");
        payload.success();
      }, 1000);
    }
```

### 方案二 通过Promise

```js
      // App.vue
      this.$store.dispatch("aUpdateInfo").then((res) => {
        console.log(res);
      });
```

```js
    // index.js
    aUpdateInfo(context) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          context.commit("updateInfo");
        }, 1000);
        resolve("调用成功");
      });
    }
```

## Vuex中的modules

* Vue使用单一状态树，那么也意味着很多状态都会交给Vuex来管理
* 当应用变得非常复杂时，store对象就会变得很臃肿
* 为了解决这个问题，Vuex允许我们将store对象分割成多个模块，而每个模块又有自己的核心属性

```js
const moduleA = {
    state:{},
    mutations:{},
    getters:{},
    actions:{},
}

const store = new Vue.Store({
    modules:{
        a:moduleA
    }
})

store.state.a // moduleA的状态
```

## 每个module中的actions的写法

* 局部状态通过context.state暴露出来，根节点状态则为context.rootStae
* getters也需要全局状态的话，可以接受更多的参数

```js
const moduleA = {
    state:{
        count:0
    },
    mutations:{},
    getters:{
        sumRootCount(state,getters,rootState){
            state.count+rootState.count
        }
    },
    actions:{
        incrementOnRootSum(context){
            if(context.state.count!=context.rootState.count)
            commit('increment')
        }
    },
}
```

## Vuex的项目结构

* 当使用Vuex进行项目管理过多的内容时，好的项目结构可以使得项目进行维护
* 通过对mutations getters actions modules进行抽离，使得项目结构变得清晰

![](https://z3.ax1x.com/2021/05/24/gxBgIS.png)

```js
import Vue from "vue";
import Vuex from "vuex";
import mutations from "./mutations";
import actions from "./actions";
import getters from "./getters";
import moduleA from "./modules/moduleA";
```
