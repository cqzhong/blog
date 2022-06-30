---
title: vue中$attrs和$listeners
urlname: dag0ow
date: 2020-11-07 20:00:00 +0800
tags: [vue]
categories: [前端]
---

`$attrs`:包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (`class` 和 `style` 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (`class`和 `style` 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件——在创建高级别的组件时非常有用。
`$listeners`:包含了父作用域中的 (不含 `.native` 修饰器的) `v-on` 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件——在创建更高层次的组件时非常有用。

<!-- more -->

这两个属性是  `vue 2.4`  版本之后提供的，`$attrs`与`$listeners`的主要应用是实现多层嵌套传递。它简直是二次封装组件或者说写高阶组件的神器。在我们平时写业务的时候免不了需要对一些第三方组件进行二次封装。比如我们需要基于`el-select`分装一个带有业务特性的组件。但`el-select`这个第三方组件支持几十个配置参数，我们当然可以适当的挑选几个参数通过 props 来传递，但万一哪天别人用你的业务组件的时候觉得你的参数少了，那你只能改你封装的组件了，亦或是哪天第三方组件加入了新参数，你该怎么办？
其实我们的这个组件只是基于`el-select`做了一些业务的封装，比如添加了默认的`placeholder`，封装了远程 ajax 搜索请求等等，总的来说它就是一个中间人组件，只负责传递数据而已。
这时候我们就可以使用`v-bind="$attrs"`：传递所有属性、`v-on="$listeners"`传递所有方法。如下所示：

```html
<!-- 二次封装el-select -->
<template>
  <div class="w-select">
    <el-select
      v-model="customSelect"
      :placeholder="placeholder"
      v-bind="$attrs"
      v-on="$listeners"
      @change="valueChange"
    >
      <el-option
        v-for="item in options"
        :key="item.label"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
  </div>
</template>

<script>
  export default {
    name: "WomSelect",
    props: {
      type: {
        type: String,
        default: "",
      },
      options: {
        type: Array,
        default: () => [],
      },
      placeholder: {
        type: String,
        default: "",
      },
      size: {
        type: Number,
        default: 14,
      },
    },
    data() {
      return {
        customSelect: "",
      };
    },
    mounted() {},
    methods: {
      /**
       * 选择器的值发生变化
       * @param {String} e
       */
      valueChange(e) {},
    },
  };
</script>

<style lang="scss" scoped></style>
```

这样，我们没有在`$props`中声明的方法和属性，会通过`$attrs`、`$listeners`直接传递下去。这两个属性在我们平时分装第三方组件的时候非常有用！
