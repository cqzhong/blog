---
title: vue样式中使用组件中接收的参数
urlname: ytqxd3
date: 2021-07-21 20:00:00 +0800
tags: [vue]
categories: [前端]
---

一、内容简介

> **在使用 vue 开发时，经常会封装很多的组件方便复用，那么难免就有写样式相关组件，比如需要使用时传入颜色、高度等样式参数。**
>
> **那么问题来了：我们要怎么在样式中使用组件中接收的参数呢？或者说，我们要怎么在样式中使用 data 中的变量呢？**

<!-- more -->

## 二、代码演示

这个用法真的简单，没什么其他的知识点，直接上代码：

```html
<template>
  <div class="box" :style="styleVar">  </div>
</template>
<script>
  export default {
    props: {
      height: {
        type: Number,
        default: 54,
      },
    },
    computed: {
      styleVar() {
        return {
          "--box-height": this.height + "px",
        };
      },
    },
  };
</script>
<style scoped>
  .box  {
      height: var(--box-height);
  }
</style>
```

这样我们就在 vue 中实现了在样式里使用 js 变量的方法：
及通过 css 定义变量的方式，将变量在行内注入，然后在`style`中使用`var()`获取我们在行内设置的数据即可。
