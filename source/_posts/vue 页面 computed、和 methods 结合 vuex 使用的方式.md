---
title: vue 页面 computed、和 methods 结合 vuex 使用的方式
urlname: xnoiyu
date: 2020-08-19 19:00:00 +0800
tags: [vue,vuex]
categories: [前端]
---

`store`下 `index.js`中的内容

<!-- more -->

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

// 挂载Vuex
Vue.use(Vuex)

// 创建VueX对象
const store = new Vuex.Store({
  state: {
    // 存放的键值对就是所要管理的状态
    count: 10,
    countNumber: 0,
    qiniu: {
      token: '',
      baseUrl: ''
    }
  },
  mutations: {
    inc(state) {
      state.count++
    },
    dec(state) {
      state.count--
    },
    updateCount(state,value) {
      state.count = value
    },
    upNumber(state) {
      state.countNumber++
    },
    // 成员操作
    setQiniuToken(state, data) {
      state.qiniu.token = data.token
      state.qiniu.baseUrl = data.bucketUrl
    }
  },
  getters: {
    // 加工state成员给外界
    count(state) {
      // vuex 为其注入state对象
      state.count
    },
    countNumber(state) {
      state.countNumber
    },
    square(state) {
      state.count*state.count
    }
  },
  actions: {
    // 异步操作
    inc(context) {
      context.commit("inc")
    },
    dec(context) {
      context.commit("dec")
    },
    updateCount(context, payload) {
      context.commit("updateCount",payload)
    },
    upNumber:function(context) {
      context.commit("upNumber")
    }
    getQiniuToken(context, payload) {
      const param = ''
      context.commit('setQiniuToken', param)
    }
  }
  // modules 模块化状态管理
  modules: {}
})

export default store
```

vue 页面内

```javascript
import { mapState, mapGetters, mapActions } from 'vuex'

computed: {
  ...mapState({
    count: state => state.count,
    countNumber: state => state.countNumber
  }),
  square:mapGetters({
    square:"square"
    }).square
  },
methods: {
  ...mapActions({
    inc:"inc",
    dec:"dec"
  })
},
created() {
  this.$store.dispatch("getQiniuToken")
}
```
