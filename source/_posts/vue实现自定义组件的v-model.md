---
title: vue实现自定义组件的v-model
urlname: me1pbc
date: 2020-02-19 20:00:00 +0800
tags: [vue]
categories: [前端]
---

编写自定义组件

<!-- more -->

```html
<template>
  <div>{{title}}</div>
</template>
<script>
  export default {
    name: "MyComponents",
    model: {
      prop: "modelVal", //指向props的参数名
      event: "change", //事件名称
    },
    props: {
      modelVal: "",
    },
    data() {
      return {
        title: "",
      };
    },
    watch: {
      //监听值变化，再赋值给modelVal
      title(value) {
        this.$emit("change", value);
      },
    },
  };
</script>

<style scoped></style>
```

#### **使用组件，设置 v-model**

**​**

```html
<template>
  <my-components v-model="title" />
</template>

<script>
  import MyComponents from "./MyComponents";

  export default {
    name: "Test",
    components: {
      MyComponents,
    },
    data() {
      return {
        title: "",
      };
    },
  };
</script>

<style scoped></style>
```
