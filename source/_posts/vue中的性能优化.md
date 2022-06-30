---
title: vue中的性能优化
urlname: lgb0vg
date: 2020-11-09 20:00:00 +0800
tags: [vue]
categories: [前端]
---

# 路由懒加载

```html
const router = new VueRouter({ routes: [ { path: '/foo', component: () =>
import('./Foo.vue') } ] })
```

<!-- more -->

# keep-alive 缓存页面

```html
<template>
   
  <div id="app">
       <keep-alive>      <router-view /> </keep-alive>  
  </div>
</template>
```

# 使用 v-show 复用 DOM

# v-for  遍历避免同时使用  v-if

# 长列表性能优化

如果列表是纯粹的数据展示，不会有任何改变，就不需要做响应化
利用**Object.freeze**冻结对象，保护不被修改

```html
export default {  data: () => ({    users: [] }),  async created() {    const
users = await axios.get("/api/users"); this.users = Object.freeze(users); } };
```

如果是**大数据长列表**，可采用**虚拟滚动**，**只渲染少部分区域的内容**
[vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller)库

```html
<recycle-scroller class="items" :items="items" :item-size="24">
   <template v-slot="{ item }">
    <FetchItemView :item="item" @vote="voteItem(item)" />
     </template
  >
</recycle-scroller>
```

# 事件的销毁

注意在 beforeDestroy 周期中销毁绑定的事件，定时器等

# 图片懒加载

vue-lazyload 库

```html
<img v-lazy="/static/img/1.png" />
```

# 第三方插件按需引入

不必要引入整个插件，利用对象解构的方式按需引入

```html
import Vue from 'vue'; import { Button, Select } from 'element-ui';
Vue.use(Button) Vue.use(Select)
```

# 无状态的组件标记为**函数式组件**

functional 属性转化为函数式组件

```html
<template functional>
   
  <div class="cell">
       
    <div v-if="props.value" class="on"></div>
    <section v-else class="off"></section>
     
  </div>
</template>

<script>
  export default {
    props: ["value"],
  };
</script>
```

# 子组件分割

```html
<template>
   
  <div>   <ChildComp /></div>
</template>

<script>
  export default {
    components: {
      ChildComp: {
        methods: {
          heavy() {
            /* 耗时任务 */
          },
        },
        render(h) {
          return h("div", this.heavy());
        },
      },
    },
  };
</script>
```

#   变量本地化

比如将计算属性**通过 const 变量存储**，而不是反复调用 this.base，反复调用就会反复触发计算

```html
<template>
   
  <div :style="{ opacity: start / 300 }">   {{ result }}  </div>
</template>
<script>
  import { heavy } from "@/utils";

  export default {
    props: ["start"],
    computed: {
      base() {
        return 42;
      },
      result() {
        const base = this.base; // 不要频繁引用this.base
        let result = this.start;
        for (let i = 0; i < 1000; i++) {
          result += heavy(base);
        }
        return result;
      },
    },
  };
</script>
```

# SSR

利用 ssr 减少首屏渲染时间
