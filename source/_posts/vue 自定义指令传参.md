---
title: vue 自定义指令传参
urlname: zqsu5n
date: 2021-06-18 20:00:00 +0800
tags: [vue]
categories: [前端]
---

#### 钩子函数

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

- bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- update：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新

我们会在[稍后](https://cn.vuejs.org/v2/guide/render-function.html#%E8%99%9A%E6%8B%9F-DOM)讨论[渲染函数](https://cn.vuejs.org/v2/guide/render-function.html)时介绍更多 VNodes 的细节。

- componentUpdated：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- unbind：只调用一次，指令与元素解绑时调用。

<!-- more -->

```javascript
import Vue from "vue";
import $store from "@/store";

Vue.directive("myDrag", {
  bind: function (el) {},
  inserted: function (el, binding) {
    const { fn, toast } = binding.value;

    let switchPos = {
      x: 0,
      y: 0,
      startX: 0,
      startY: 0,
      endX: 0,
      endY: 0,
    };
    el.addEventListener("touchstart", function (e) {
      switchPos.startX = e.touches[0].pageX;
      switchPos.startY = e.touches[0].pageY;
    });
    el.addEventListener("touchend", function (e) {
      if ($store.state.seal.showDelete) {
        return;
      }
      switchPos.x = switchPos.endX;
      switchPos.y = switchPos.endY;
      switchPos.startX = 0;
      switchPos.startY = 0;

      // 移动结束
      // const rect = el.getBoundingClientRect() // 获取元素位置
      fn(el);
    });
    el.addEventListener("touchmove", function (e) {
      if ($store.state.seal.showDelete) {
        toast();
        return;
      }
      if (e.touches.length <= 0) return;
      const pageH = document.documentElement.offsetHeight;
      const pageW = document.documentElement.offsetWidth;
      const tmpH = pageH - document.querySelector(".pdf-box").clientHeight;
      let offsetX = e.touches[0].pageX - switchPos.startX;
      let offsetY = e.touches[0].pageY - switchPos.startY;
      let x = switchPos.x - offsetX;
      let y = switchPos.y - offsetY;
      if (x + el.offsetWidth > pageW) {
        x = pageW - el.offsetWidth;
      }
      if (y + el.offsetHeight > pageH) {
        y = pageH - el.offsetHeight;
      }
      if (x < 10) x = 10;
      if (y < 10 + tmpH) y = 10 + tmpH;
      el.style.right = x + "px";
      el.style.bottom = y + "px";
      switchPos.endX = x;
      switchPos.endY = y;
      if (e.cancelable) {
        e.preventDefault();
      }
      e.stopPropagation();
    });
  },
});
```

```html
<template>
  <div v-myDrag="{ fn: dragMoveEnd, toast: showToast }"></div>
</template>

<script>
  import drag from "../directives/drag";
  export default {
    name: "PdfPreview",
    directives: {
      drag,
    },
    components: {},
    props: {},
    data() {
      return {
        sealList: [],
      };
    },
    created() {},
    mounted() {},
    beforeDestroy() {},
    methods: {
      showToast() {
        this.$toast({ message: "删除模式下禁止移动" });
      },

      dragMoveEnd(el) {},
    },
  };
</script>
```

#### 参考

- [自定义指令](https://cn.vuejs.org/v2/guide/custom-directive.html)
