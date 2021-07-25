---
title: Vue中v-show和v-if的异同
date: 2021-04-22 22:13:43
tags: Vue
categories: Vue
# cover: https://z3.ax1x.com/2021/04/22/cOPGIs.jpg 
description: v-show和v-if的异同 
---

* 相同点：两者都控制元素的显示和隐藏

<!--more-->

* 不同点:
  * 对于 v-if 来说当条件为false时，包含v-if指令的元素根本就不会存在dom中
  * 对于 v-show 来说,当条件为false时，只是将dom元素设置display:none; v-show操作的仅仅是css，无论v-show的真假，它都将被渲染

* 性能:对于v-show来说，其对应的元素只会被渲染一次，剩下的就是设置css，而对于v-if来说，需要不停的销毁和创建

* 总结：当切换频率非常高的时候，使用v-show，而只需要切换一次的时候使用v-if。
