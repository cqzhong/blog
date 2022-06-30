---
title: vue-hooks
urlname: pc108b
date: 2020-11-29 20:00:00 +0800
tags: [vue]
categories: [前端]
---

1、我们平时使用一些第三方组件，或者注册一些全局事件的时候，都需要在`mounted`中声明，在`destroyed`中销毁。但由于这个是写在两个生命周期内的，很容易忘记，而且大部分在创建阶段声明的内容都会有副作用，如果你在组件摧毁阶段忘记移除的话，会造成内存的泄漏，而且都不太容易发现。如下代码

<!-- more -->

```html
mounted() { window.addEventListener('resize', this.resizeHandler) },
beforeDestroy() { window.removeEventListener('resize', this.resizeHandler) }
```

使用 hook 后我们就能将之前的代码用更简单和清楚地方式实现了。

```html
mounted() { window.addEventListener('resize', this.resizeHandler)
this.$on('hook:beforeDestroy', () => { window.removeEventListener('resize',
this.resizeHandler) }) }
```

2、在父组件监听子组件的生命周期方法

```html
// 子组件这样写 mounted() { this.$emit('mounted') } // 父组件内这样写
<edit @mounted="doSomething" />
```

使用 hook 可以这样实现

```html
// 父组件中这样写
<rl-child @hook:mounted="handleChildMounted" />
// 子组件中不用写东西 mounted () {}
```
