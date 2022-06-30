---
title: Vue父子组件通信：将一个对象中的所有属性都作为 props 传递
urlname: cydbit
date: 2020-12-09 20:00:00 +0800
tags: [vue]
categories: [前端]
---

#### 一、使用不带参数的`v-bind`

<!-- more -->

```html
<div id="app">
  <child-component v-bind="newObj"></child>
</div>
// v-bind中没有参数，而组件中的props需要声明对象的每个属性
Vue.component('child', {
  props: ['text','isComplete'],
  template: '<span >{{ text }}  {{isComplete}}</span>'
})
new Vue({
  el: '#app',
  data: {
    newObj: {
      text: 'Learn Vue',
      isComplete: false
    }
  }
})
```

#### 二、使用带参数的`v-bind`

```html
<div id="app">
  <child-component v-bind="newObj"></child>
</div>
// v-bind后跟随参数newObj，组件中的props需要声明该参数，组件变可以通过newObj来访问对象的属性
Vue.component('child', {
  props: ['newObj'],
  template: '<span >{{ newObj.text }}  {{newObj.isComplete}}</span>'
})
new Vue({
  el: '#app',
  data: {
    newObj: {
      text: 'Learn Vue',
      isComplete: false
    }
  }
})
```
