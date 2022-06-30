---
title: Vue要点
urlname: gvvgua
date: 2020-11-09 20:00:00 +0800
tags: [vue]
categories: [前端]
---

# MVVM

- Model-view-viewModel，是一个数据驱动视图的框架
- Model 并不直接影响视图 View
- viewModel 层中有一套数据响应式机制
- Model 与 view 通过 viewModel 层双向监听
  - 当 Model 层数据修改后，中间层 viewModel 监听到变化就反映到 view
  - 当 view 层调用 DOM 事件时，同样 viewModel 也会触发修改 Model 层的数据
- 这样在 ViewModel 中就减少了大量 DOM 操作代码

<!-- more -->

## 发展历程

1. 早期时没有前端的概念，以 JSP 为代表的服务端模板引擎，前后端不分离，也没有高效的开发模式
1. **mvc**开发模式，View、Controller 和 Model，逻辑分层架构容易维护
   1. **Model**：负责保存应用数据，与后端数据进行同步
   1. **Controller**：负责业务逻辑，根据用户行为对 Model 数据进行控制
   1. **View**：负责数据展示
   1. 问题是只限于服务端，前端页面开发仍然不高效，前后端仍然不分离
   1. 数据流混乱
1. **MVP** ：MVP 与 MVC 很接近
   1. P 指的是 Presenter，presenter 可以理解为一个中间人
   1. 它负责着 View 和 Model 之 间的数据流动，防止 View 和 Model 之间直接交流
1. **mvvm**

**​**

# 父子组件生命周期的调用顺序

1. 父组件 created
1. 子组件 created
1. 子组件 mounted
1. 父组件 mounted

# v-if 和 v-show

- v-if 在编译的时候会被转化为三元表达式，v-show 会被编译为指令
- v-if 不会渲染之后的内容，适用于切换不频繁的场景
- v-show 会切换 display:none，适用于切换频繁的场景，所以初次渲染更耗费性能

# v-if 与 v-for

1. v-for 优先于 v-if 被解析生成，
1. 如果同时出现，每次渲染都会先执行循环再判断条件，会给每一个都加上 v-if，无论如何循环都不可避免，浪费了性能
1. 要避免出现这种情况，则在外层嵌套 template，在这一层进行 v-if 判断，然后在内部进行 v-for 循环
1. 如果条件出现在循环内部，可通过计算属性提前过滤掉那些不需要显示的项

# 为什么 data 必须是个函数形式

源码中找到：**src\core\instance\state.js - initData()**
**​**

每次使用组件时会对组件进行实例化操作，并调用 data 函数返回一个对象作为组件的数据源，使得每个组件的数据源相互独立

- Vue 组件可能存在多个实例，如果使用对象形式定义 data，则会导致它们**共用一个 data 对象**，那么状态 变更将会影响所有组件实例，这是不合理的；
- 采用函数形式定义，在**initData 时**会将其**作为工厂函数返** 回全新 data 对象，有效规避多实例之间状态污染问题
- 而在 Vue 根实例创建过程中则不存在该限制，也 是因为**根实例只能有一个**，不需要担心这种情况

# watch 和 computed 的区别

- computed 更简单/更高效，有缓存，优先使用，适用于购物车统计价格的时候使用
- watch 需要在数据变化时执行异步或开销较大的操作时使用，简单讲，当一条数据影响多条数据的时候，例如 搜索数据

# 组件写 name 的好处

- 可通过 name 找到对应的组件（组件通讯，路由跳转，递归组件）
- 可通过 name 属性实现缓存功能（keep-alive）

# 组件修饰符及其原理

- capture
- once
- passive
- stop：**\$event.stopPropergation()**
- self
- prevent: **\$event.preventDefault()**

##

# keep-alive 组件

只会在第一次切换组件的时候调用生命周期中的 created 函数
并且**不再调用 destroyed 函数**销毁组件

属性

- include 属性：字符串或者正则表达式，只有匹配的命名组件会被缓存
- exclude 属性：相反
- 使用正则和数组时需要配合 v-bind

# 组件通讯

通信核心：找到数据交互之间的桥梁。如果两者之间能直接沟通，那就直接沟通，如果不能，则找一个两者都能说得上话的人。

- props/\$emit（父子通信）
- \$refs/ref（父子通信）
- children/parent（父子通信）
- attrs/listeners（父子通信）
- provide/inject（父子通信、跨级通信）
- eventBus（父子通信、跨级通信、兄弟通信）
- vuex（父子通信、跨级通信、兄弟通信、路由视图级别通信）
- localStorage/sessionStorage 等基于浏览器客户端的存储（父子通信、跨级通信、兄弟通信、路由视图级别通信）

## props/\$emit（父子通信）

父级

1. 通过给子级标签添加**动态属性**传值
1. 子集通过`props`参数接收父级数据

```html
<template>
  <div>
    <h2>MyParent1</h2>
    <MyChild1 :data="name" @sendData="getChildData"></MyChild1>
    <p>我接收到了子级的数据-{{childData}}</p>
  </div>
</template>

<script>
  import MyChild1 from "@/components/MyChild1";
  export default {
    name: "MyParent1",
    data() {
      return {
        name: "MyParent1-Name",
        childData: "",
      };
    },
    components: {
      MyChild1,
    },
    methods: {
      getChildData(data) {
        this.childData = data;
      },
    },
  };
</script>
```

子组件

1. 子集通过`$emit`方法提交给父级对应事件名传值
1. 父级对应事件名触发自身方法

```html
<template>
  <div>
    <h2>MyChild1</h2>
    <p>我接收来自父级传入的数据：{{ data }}</p>
    <button @click="sendDataToParent">把数据传递给父级</button>
  </div>
</template>

<script>
  export default {
    name: "MyChild1",
    props: ["data"],
    data() {
      return {
        name: "MyChid1-Name",
      };
    },
    methods: {
      sendDataToParent() {
        this.$emit("sendData", this.name);
      },
    },
  };
</script>
```

## \$refs/ref（父子通信）

- 与 props/emit 不同的是
- ref 不需要子集事先向父级提交
- 而是父级直接拿到子组件的实例中的数据

父组件

```html
<template>
  <div>
    <h2>MyParent1</h2>
    <MyChild1 :data="name" ref="myChild"></MyChild1>
    <button @click="getChildData">通过ref获取子集数据</button>
    <p>我接收到了子级的数据-{{childData}}</p>
  </div>
</template>

<script>
  import MyChild1 from "@/components/MyChild1";
  export default {
    name: "MyParent1",
    data() {
      return {
        name: "MyParent1-Name",
        childData: "",
      };
    },
    components: {
      MyChild1,
    },
    methods: {
      getChildData() {
        console.log(this.$refs.myChild); // 拿到子级的组件实例对象
        this.childData = this.$refs.myChild.name;
      },
    },
  };
</script>
```

子组件

```html
<template>
  <div>
    <h2>MyChild1</h2>
    <p>我接收来自父级传入的数据：{{ data }}</p>
  </div>
</template>

<script>
  export default {
    name: "MyChild1",
    props: ["data"],
    data() {
      return {
        name: "MyChid1-Name",
      };
    },
  };
</script>
```

## $children/$parent（父子通信）

```javascript
console.log(this.$children); // 所有子组件实例的数组
```

![](https://cdn.nlark.com/yuque/0/2020/png/1522594/1597580915323-d8724ba5-43da-423f-8270-35c21bd41b33.png#crop=0&crop=0&crop=1&crop=1&height=85&id=fobeK&originHeight=85&originWidth=480&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=480)
获取子组件数据就更方便了

```javascript
    methods: {
        getChildData() {
            console.log(this.$children) // 所有子组件的数组
            this.childData = this.$children[0].name;
        }
    }
```

同理，子组件可通过`this.$parent`获取父组件数据

```javascript
	methods: {
		getParentData() {
			console.log(this.$parent);
			this.parentData = this.$parent.name;
		}
	},
```

## attrs/listeners（父子通信）

attrs 获取父级在子组件标签上写的非 props 属性（非动态属性）
![](https://cdn.nlark.com/yuque/0/2020/png/1522594/1597580915435-cc7a16ab-482a-40f0-9159-42cff141170a.png#crop=0&crop=0&crop=1&crop=1&height=96&id=s5WEc&originHeight=96&originWidth=789&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=789)

```javascript
	methods: {
		getAttrs() {
			console.log(this.$attrs);	// 获取非props属性
		}
	},
```

但是不能获取事件，要获取事件可通过`$listeners`获取
![](https://cdn.nlark.com/yuque/0/2020/png/1522594/1597580915326-de477de0-ef82-4cea-973b-7e6f1a0aba64.png#crop=0&crop=0&crop=1&crop=1&height=60&id=dhoBi&originHeight=60&originWidth=519&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=519)

```javascript
		getListeners() {
			console.log(this.$listeners);	// 获取事件
		},
```

## provide/inject（父子通信、跨级通信）

组件与组件之间是隔离的，没有所谓的作用域链
所以如何实现爷爷组件和孙子组件之间的数据传递呢？

### provide/inject 跨级通信

提供数据/注入数据
爷爷组件 provide 选项

```javascript
    provide() {
        return {
            pName: this.name
        }
    },
```

孙子组件`inject`选项

```javascript
export default {
  name: "MyChidl1Child2",
  inject: ["pName"],
};
```

### 命名冲突的问题解决

如果自己本身有同样的命名，就会出现命名冲突的问题

```javascript
export default {
  name: "MyChidl1Child2",
  inject: {
    parentPname: {
      from: "pName",
    },
  },
};
```

## eventBus（父子通信、跨级通信、兄弟通信）

就是一个全局对象，例如挂载到`windows`下

通过 Vue 实例化一个 eventBus 对象

```javascript
import Vue from "vue";

let eventBus = new Vue({});
export default eventBus;
```

组件引入`eventBus`并注册一个事件

```javascript
import eventBus from "@/eventBus";
export default {
  name: "MyChild2",
  data() {
    return {
      pName: "",
    };
  },
  created() {
    eventBus.$on("bus", (data) => {
      this.pName = data;
    });
  },
};
```

另一个组件通过`eventBus`触发这个事件，实现数据存储到 Vue 全局，数据传递

```javascript
import eventBus from "@/eventBus";

export default {
  name: "MyChidl1Child2",
  inject: {
    parentPname: {
      from: "pName",
    },
  },
  data() {
    return {
      pName: "我是兄弟组件的pName",
    };
  },
  methods: {
    saveDataToEventBus() {
      eventBus.$emit("bus", this.pName);
    },
  },
};
```

![](https://cdn.nlark.com/yuque/0/2020/png/1522594/1597580915361-e2441704-cf43-4cb1-ae01-9ce4ded3296b.png#crop=0&crop=0&crop=1&crop=1&height=212&id=Yfgs7&originHeight=212&originWidth=281&originalType=binary∶=1&rotation=0&showTitle=false&size=0&status=done&style=none&title=&width=281)
**​**

#

# key 的作用和原理

1. key 的作用主要是为了高效的更新虚拟 DOM
1. 当正在更新已经渲染过的元素列表时会默认采用**就地复用策略，**如果数据项顺序被改变时将不会移动 DOM 元素
1. 其原理是 vue 在 patch 过程中通过 key 可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个 patch 过程更加高效，减少 DOM 操作量，提高性能。
1. vue 中在使用相同标签名元素的过渡切换时，也会使用到 key 属性，其目的也是为了让 vue 可以区分它们，否则 vue 只会替换其内部属性而不会触发过渡效果。
1. 最好也不要使用 index 作为 key 值，因为当遍历对象被删除时会产生数组塌陷的问题，index 将会递减，这样就失去了 key 的意义，所以在不能保证 index 不会改变时别用 index。

# .\$nextTick()

它可以在 DOM 更新完毕之后执行一个回调

- 将**回调延迟**到下次 DOM 更新循环之后执行。
- 与 生命周期函数中的**updated**有些类似
  - this.\$nextTick() 可以用作局部的数据更新后 DOM 更新结束后的操作
  - 全局的可以用 **updated**`生命周期函数。

## 理解 MutationObserver

- MutationObserver 是**HTML5 新增的属性**，用于**监听 DOM 修改事件**
- 能够监听到节点的属性、文本内容、子节点等的改动

```javascript
//MutationObserver基本用法
var observer = new MutationObserver(function () {
  //这里是回调函数
  console.log("DOM被修改了！");
});
var article = document.querySelector("article");
observer.observer(article);
```

## vue 的源码中实现 nextTick：

​

```javascript
//vue@2.2.5 /src/core/util/env.js
if (
  typeof MutationObserver !== "undefined" &&
  (isNative(MutationObserver) ||
    MutationObserver.toString() === "[object MutationObserverConstructor]")
) {
  var counter = 1;
  var observer = new MutationObserver(nextTickHandler);
  var textNode = document.createTextNode(String(counter));
  observer.observe(textNode, {
    characterData: true,
  });
  timerFunc = () => {
    4;
    counter = (counter + 1) % 2;
    textNode.data = String(counter);
  };
}
```

## 回答范式

基本

1. nextTick 是一个全局 API，用法是：
   1. 当组件修改了 state 之后，由于 vue 组件更新是异步的，无法立即获取修改后的 state 以及 DOM 状态
   1. 这个时候就可以使用这个 nextTick 来把获取 state 的代码放入到其中的回调函数中去
   1. 回调函数会等待 DOM 渲染完毕之后再去执行
2. 我也有了解过 nextTick 的源码实现，其中有一个 callbacks 回调函数数组，会使用 timerFunc 异步的方式调用他们
   1. 这个 timerFunc 函数中首选 Promise，然后是 MutationObserverAPI， 最后才是 setTimeOut()
   1. 这其实是 vue 做的一个兼容性处理

原理

1. vue 用异步队列的方式来控制 DOM 更新和 nextTick 回调先后执行
1. microtask 因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
1. 因为兼容性问题，vue 不得不做了 microtask 向 macrotask 的降级方案

# vue 的数据响应式原理

Vue 的数据响应的设计思想为观察者模式。

- 所谓数据响应式就是 MVVM 模式的机制

## 添加动态响应式属性

- vue 不允许在已经创建的实例对象上动态添加新的根级响应式属性

可以使用**Vue.set(obj, key, value)全局方法**来添加响应式数据到嵌套对象上
或者使用**vm.$set实例方法this.$set(this, key, value)**
**​**

## defineProperty 对比 Proxy

- **defineProperty**
  - 只能劫持对象的属性，需要进行深度遍历
  - 不能监控到数组下标的变化，需要特殊处理
  - 不能监听动态增加的属性
- Proxy
  - 可以**代理整个对象，不需要深度遍历监听**
  - **代理数组**
  - 可以代理**动态增加的属性**

# 函数式组件的优点

- 无状态
- 无实例（this）
- 无生命周期
- 渲染性能好
- 组件便于复用
