---
title: vue组件通信
urlname: gxvt38
date: 2020-02-16 20:00:00 +0800
tags: [vue]
categories: [前端]
---

### 1、组件通信

主要有 4 种 Vue 组件通信方式：父子组件的通信、非父子组件的 eventBus 通信、利用本地缓存实现组件通信、Vuex 通信，后两种在这里不做重点介绍，感兴趣的读者可以在网上查找资料。

<!-- more -->

#### 1.1 　父组件向子组件通信

父组件向子组件传递数据，在这里介绍两种方式，一种需使用 props，另一种要通过\$parent。props 默认是单向绑定：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意中修改了父组件的状态。
​

**（1）使用 props 属性父组件向子组件传值可以使用如下代码：**

```html
<child-component v-bind:子组件属性="父组件数据属性" />
```

具体步骤：父组件在 template 里使用子组件，子组件使用 props 接收数据。

```html
// 父组件在template里使用子组件 <child v-bind:"parentMsg" /> //
子组件中用props接收数据 props: ['myMessage']
```

完整代码如下：
[   ](http://rn.mobile.ant.design)
[      ](http://rn.mobile.ant.design)![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1596442472786-edce9c76-ad32-4639-9a24-4d58756b1337.png#crop=0&crop=0&crop=1&crop=1&height=78&id=Tz9t9&originHeight=199&originWidth=815&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=320)
[    ](http://rn.mobile.ant.design)![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1596442489191-d2fe0199-4730-427b-aa73-b94a06c14b30.png#crop=0&crop=0&crop=1&crop=1&height=308&id=Cv7EM&originHeight=784&originWidth=815&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=320)

props 默认是单向绑定，如果需要使用双向绑定可以使用.sync 显式地指定双向绑定，这使得子组件的数据修改会回传给父组件。

```html
<my-component v-bind:my-name.sync="name" v-bind:my-age.sync="age" />
```

如果单次绑定，可以使用.once 显式地指定单次绑定，单次绑定在建立之后不会同步之后的变化，这意味着即使父组件修改了数据，也不会传导给子组件。
​

```html
<my-component v-bind:my-name.once="name" v-bind:my-age.once="age" />
```

**（2）直接在子组件中通过 this.\$parent 调用其父组件，但并不建议使用。**
**​**

#### 1.2 　子组件向父组件通信

子组件向父组件通信也有两种方式：一种使用自定义事件，另一种使用\$refs。

1.使用自定义事件
（1）在父组件中调用子组件的时候，绑定一个自定义事件和对应的处理函数。

```html
<template>
  <detail-info
    ref="detailInfo"
    @reload="changeData"
    @showMask="showMaskAlert"
  />
</template>
<script>
  import DetailInfo from "./components/detail-info.vue";
  export default {
    components: {
      DetailInfo,
    },
    methods: {
      changeData() {},
    },
  };
</script>
```

（2）在子组件中把要发送的数据通过触发自定义事件传递给父组件。
使用一个自定义事件实现子组件与父组件直接的通信，其中 this.$emit("customEvent"，"参数")中$emit()的意思是把事件沿着作用域链向上派送。

```vue
this.$emit('reload')
```

2.使用\$refs
步骤 1：在调用子组件的时候，可以制定 refs 属性。

```html
<child-component refs="xiaoming" />
```

步骤 2：通过\$refs 得到指定引用名称对应的组件实例。

```javascript
this.$refs.xiaoming;
```

#### 1.3 　任意组件及平行组件通信

eventBus 这种通信方式针对的是非父子组件之间的通信，它的原理还是通过事件的触发和监听。但是因为是非父子组件的关系，它们需要有一个中间组件来连接。可以在根组件，也就是#app 组件上定义一个所有组件都可以访问到的组件，具体方式如下。
使用 eventBus 传递数据的 3 个步骤如下。实例代码如例 5-18 所示。
（1）创建一个 Vue 实例，作为事件绑定触发的公共属性。
（2）在发送方的组件触发自定义事件。

```html
// 1、创建一个vue实例 bus : new Vue() // 2、在子组件触发自定义的事件 $emit()
————把事件沿着作用域向上派送 bus.$emit('changeMsgEvent', '需要传递的数据') //
3、在接收组件监听事件，接收数据 mounted() { bus.$on('changeMsgEvent',
function(msg) { // msg是通过事件传递来的数据 }) }
```

使用 Vue 实例作为中间事件总线，实现组件之间的数据通信
