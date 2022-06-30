---
title: Vuejs生命周期全解
urlname: cpyndd
date: 2020-01-09 20:00:00 +0800
tags: [vue]
categories: [前端]
---

# 生命周期钩子函数

<!-- more -->

![lifecycle.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1612513595538-b1a20f81-8e7f-4ee6-95e7-76df2fba9e79.png#crop=0&crop=0&crop=1&crop=1&height=2603&id=bGKHy&margin=%5Bobject%20Object%5D&name=lifecycle.png&originHeight=2603&originWidth=1028&originalType=binary∶=1&rotation=0&showTitle=false&size=263319&status=done&style=none&title=&width=1028)

## beforeCreate 创建之前

在**实例初始化之后**，数据观测 (data observer) 和 event/watcher 事件配置之前被调用

- 可以在此阶段给每个组件上增加一些特定的属性
- 但是不会挂载 data、计算属性等
- 基本业务逻辑不需要使用。

## created 创建完成

- 该阶段实现了**数据劫持 defineReactive**，把**方法、计算属性**都挂在到了实例上
- 不能获取到真实的 DOM 元素，可以在里边完成 ajax 请求数据，不能操作 DOM

### options 解析原理

- 判断是否存在 el
  - 如果没有** el **选项，会等待 vm.\$mount 挂载 el
  - 如果有，就会跳过上一步
- 然后判断是否有** template 模板**选项
  - 如果有将 template **编译到 render 函数**中。
  - 否则，将 el 外部的 HTML 作为 template 模板进行编译。

## beforeMount 挂载之前

- 在挂载之前调用，会首次调用 **render 函数**，但是一般不会增加业务逻辑。
- el 被新创建的\$el 替换挂载到实例上

## mounted 挂载之后

- 执行完了模板解析，以及挂载
  - 注意  `mounted` **不会**保证所有的子组件也都一起被挂载
  - \*\* **[**vm.\$nextTick\*\*](https://vuejs.bootcss.com/api/#vm-nextTick)用于等待整个视图都渲染完毕
- 这时  `el`  被新创建的  `vm.$el`  替换了
- 将挂载到真实 DOM 上，这个阶段可以操作 DOM。

## beforeUpdate 更新之前

- 数据更新时调用，但是还没有对视图进行重新渲染，这个时候，可以获取视图更新之前的状态。
- 当 data 被修改时，就会先调用 beforeUpdate。
- 然后重新渲染虚拟 DOM 并把应用更新。

## updated   更新完后

- 虚拟 DOM 重新渲染
  - `updated` **不会**保证所有的子组件也都一起被重绘
  - 可以在  `updated`  里使用  [**vm.\$nextTick**](https://vuejs.bootcss.com/api/#vm-nextTick)
- 通过 DOM 操作来获取视图的最新状态。
- 任何数据的更新都会触发这个事件

## beforeDestroy 销毁之前

- 当调用 vm.\$destroy 函数时，先调用 beforeDestroy 函数
- 这个阶段会**绑定销毁子组件**以及**事件监听器、定时器等，防止内存泄漏**。

## destroyed 销毁之后

组件销毁完毕

# 相关方法

​

## errorCaptured()

当捕获一个来自子孙组件的错误时被调用，此钩子会收到三个参数：

1. 错误对象
1. 发生错误的组件实例
1. 以及一个包含错误来源信息的字符串。

此钩子可以返回 `false` 以阻止该错误继续向上传播。

# 异步请求在哪个周期中进行

- 都可以
- created：实例已经创建完成，并且服务端渲染支持 created 方法
- mounted：虚拟 dom 已经挂载完成，可以进行 dom 操作，服务端不支持 mounted 方法

# 生命周期钩子函数是如何实现的

- Vue 的生命周期钩子就是回调函数，当创建组件实例的过程中就会调用对应的回调函数
- 内部主要是使用 callHook 方法来调用对应的方法
- 核心是一个发布订阅模式，将钩子订阅好，在对应的阶段触发
- 内存采用的数组来存储钩子函数

```javascript
function mergeHook(parentVal, childVal) {
  if (childVal) {
    if (parentVal) {
      return parentVal.concat(childVal);
    } else {
      return [childVal]; // 将钩子函数包装为一个数组
    }
  } else {
    return parentVal;
  }
}
// 会做合并操作
function mergeOptions(parent, child) {
  let opt = {};
  for (let key in child) {
    opts[key] = mergeHook(parent[key], child[key]);
  } // 合并钩子方法

  return opt;
}
function callHook(vm, hookName) {
  vm.options[hookName].forEach((fn) => fn());
}

function Vue(options) {
  this.options = mergeOptions(this.constructor.options, options);

  callHook(this, "beforeCreated");
  callHook(this, "created");
}

// 全局组件都会合并进去
Vue.options = {};

new Vue({
  beforeCreate() {
    console.log("before create ok");
  },
});
```
