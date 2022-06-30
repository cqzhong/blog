---
title: VueX（Vue状态管理)
urlname: ud0236
date: 2020-08-18 20:00:00 +0800
tags: [vue,vuex]
categories: [前端]
---

### 一、初识 VueX

#### 1.1 关于 VueX

Vue 官网提供的状态管理器名为 Vuex，Vuex，用于管理分散在 Vue 各个组件中的数据。
对于小型应用来说，完全没有必要引入状态管理，因为这会带来更多的开发成本。然而当应用的复杂度逐渐提高，状态管理也越发重要起来。
对于组件化开发来说，大型应用的状态往往跨越多个组件。在多层嵌套的父子组件之间传递状态已经十分麻烦，而 Vue 更是没有为兄弟组件提供直接共享数据的办法。基于这个问题，许多框架提供了解决方案——使用全局的状态管理器，将所有分散的共享数据交由状态管理器保管，Vue 也不例外。

<!-- more -->

#### 1.2 安装

- Npm 安装 Vuex

```bash
npm i vuex -s
```

- 在项目的根目录下新增一个`store`文件夹，在该文件夹内创建`index.js`

```
.
├── App.vue
├── components
├── main.js
└── store
    └── index.js
```

#### 1.3 使用

##### 1.3.1 初始化`store`下 i`ndex.js`中的内容

- State 用于维护所有应用层的状态，并确保应用只有唯一的数据源（SSOT, Single Source of Truth）。
- Getter 维护由 State 派生的一些状态，这些状态随着 State 状态的变化而变化。与计算属性一样，Getter 中的派生状态在被计算之后会被缓存起来，当重复调用时，如果被依赖的状态没有变化，那么 Vuex 不会重新计算派生状态的值，而是直接采用缓存值。
- Mutation 提供修改 State 状态的方法。
- Action 类似 Mutation，不同在于：
  1.  Action 不能直接修改状态，只能通过提交 mutation 来修改。
  1.  Action 可以包含异步操作。在组件中，我们可以直接使用 store.dispatch 来分发 action.

```javascript
import Vue from "vue";
import Vuex from "vuex";

//挂载Vuex
Vue.use(Vuex);

//创建VueX对象
const store = new Vuex.Store({
  state: {
    //存放的键值对就是所要管理的状态
    name: "helloVueX",
    count: 1,
    version: "1.0.0",
    qiniu: {
      token: "",
      baseUrl: "",
    },
  },
  mutations: {
    // 成员操作
    setProjectVersion(state, payload) {
      state.version = payload;
    },
    setQiniuToken(state, data) {
      state.qiniu.token = data.token;
      state.qiniu.baseUrl = data.bucketUrl;
    },
  },
  getters: {
    // 加工state成员给外界
    tenTimesCount(state) {
      // vuex 为其注入state对象
      return state.count * 10;
    },
  },
  actions: {
    // 异步操作
    getQiniuToken(context, payload) {
      const data = {
        token: "",
        bucketUrl: "",
      };
      context.commit("setQiniuToken", data);
    },
  },
  // modules 模块化状态管理
});

export default store;
```

##### 1.3.2 将 store 挂载到当前项目的 Vue 实例当中去

打开 main.js

```javascript
import Vue from "vue";
import App from "./App";
import router from "./router";
import store from "./store";

Vue.config.productionTip = false;

/* eslint-disable no-new */
new Vue({
  el: "#app",
  router,
  store, //store:store 和router一样，将我们创建的Vuex实例挂载到这个vue实例中
  render: (h) => h(App),
});
```

##### 1.3.3 在组件中使用 Vuex

```html
<template>
  <div id="app">
    name:
    <h1>{{ $store.state.name }}</h1>
    <span>{{ $store.getters.tenTimesCount }}</span>
  </div>
</template>
```

```vue
methods:{ getName(){ console.log(this.$store.state.name)
console.log(this.$store.getters.tenTimesCount)
this.$store.dispatch('getQiniuToken') this.$store.commit('setProjectVersion',
'1.0.1') } }
```

```
Vue.set 为某个对象设置成员的值，若不存在则新增
例如对state对象中添加一个age成员
Vue.set(state,"age",15)

Vue.delete 删除成员
将刚刚添加的age成员删除
Vue.delete(state,'age')
```

#### modules

当项目庞大，状态非常多时，可以采用模块化管理模式。`Vuex` 允许我们将 `store` 分割成模块（module）。每个模块拥有自己的 `state、mutation、action、getter、`甚至是嵌套子模块——从上至下进行同样方式的分割。

##### store 拆分

```
.
├── actions.js
├── index.js
├── mutations.js
└── state.js
```

- 示例代码

`index.js`文件

```javascript
import state from "./state";
import * as mutations from "./mutations";
import * as actions from "./actions";

const store = new Vuex.Store({
  state,
  mutations,
  actions,
});
export default store;
```

`state.js`文件

```javascript
export default {
  name: "tony",
};
```

`mutations.js`文件

```javascript
export const setQiniu = (state, data) => {
  state.qiniu = data;
};
```

##### modules 拆分

```
.
├── index.js
└── modules
    ├── common.js
    ├── order.js
    └── user.js
```

- 示例代码

`index.js`文件

```javascript
import Vue from "vue";
import Vuex from "vuex";

Vue.use(Vuex);
const files = require.context("./modules", false, /\.js$/);
let modules = {
  state: {},
  mutations: {},
  actions: {},
};

files.keys().forEach((key) => {
  Object.assign(modules.state, files(key)["state"]);
  Object.assign(modules.mutations, files(key)["mutations"]);
  Object.assign(modules.actions, files(key)["actions"]);
});
const store = new Vuex.Store(modules);
export default store;
```

`common.js`文件

```javascript
import Vue from "vue";

export const state = {
  //登录弹窗状态
  loginPopupShow: false,
};
export const mutations = {
  //登录弹窗状态
  setLoginPopupShow(state, data) {
    state.loginPopupShow = data;
  },
};
export const actions = {};
```

- [参考来源](https://www.jianshu.com/p/2e5973fe1223)
