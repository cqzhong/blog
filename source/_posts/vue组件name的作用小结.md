---
title: vue组件name的作用小结
urlname: qqkdnv
date: 2020-07-21 19:00:00 +0800
tags: [vue]
categories: [前端]
---

[本文转载自 Qin\_\_](https://blog.csdn.net/Uookic/article/details/80420472)

我们在写 vue 项目的时候会遇到给组件命名  
这里的 name 非必选项，看起来好像没啥用处，但是实际上这里用处还挺多的

```javascript
export default {
  name: "xxx",
};
```

<!-- more -->

#### 1、当项目使用 keep-alive 时，可搭配组件 name 进行缓存过滤  

举个例子： 
我们有个组件命名为 detail,其中 dom 加载完毕后我们在钩子函数 mounted 中进行数据加载

```javascript
export default {
    name:'Detail'
}，
mounted(){
   this.getInfo();
}，
methods:{
   getInfo(){
          axios.get('/xx/detail.json',{
              params:{
                id:this.$route.params.id
              }
          }).then(this.getInfoSucc)
     }
 }
```

因为我们在 App.vue 中使用了 keep-alive 导致我们第二次进入的时候页面不会重新请求，即触发 mounted 函数。
有两个解决方案,一个增加 activated()函数,每次进入新页面的时候再获取一次数据。
还有个方案就是在 keep-alive 中增加一个过滤，如下图所示：

```vue
<div id="app"> 
    <keep-alive exclude="Detail">
      <router-view/>
    </keep-alive>
  </div>
```

#### 2、DOM 做递归组件时  

比如说 detail.vue 组件里有个 list.vue 子组件，递归迭代时需要调用自身 name

list.vue:

```vue
  <div>
        <div v-for="(item,index) of list" :key="index">
            <div>
                <span class="item-title-icon"></span>
                {{item.title}}
            </div>
            <div v-if="item.children" >
               <detail-list :list="item.children"></detail-list>
            </div>
        </div>
  </div>

<script>
export default {
    name:'DetailList',//递归组件是指组件自身调用自身
    props:{
        list:Array
    }
}
</script>
 list数据：
  const list = [{
          "title": "A",
          "children": [{
            "title": "A-A",
            "children": [{
              "title": "A-A-A"
            }]
          },{
                "title": "A-B"
          }]
        }, {
          "title": "B"
        }, {
          "title": "C"
        }, {
          "title": "D"
        }]
```

迭代的结果如下
![20180523152904570.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604052101420-9938a3ae-9bb1-49e6-9e7a-08151c0f3bc1.png#crop=0&crop=0&crop=1&crop=1&height=272&id=YMfSy&margin=%5Bobject%20Object%5D&name=20180523152904570.png&originHeight=272&originWidth=200&originalType=binary∶=1&rotation=0&showTitle=false&size=2168&status=done&style=none&title=&width=200)
​

#### 3、**当你用 vue-tools 时** 

vue-devtools 调试工具里显示的组见名称是由 vue 中组件 name 决定的  
​

![20180523153448582.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604052131153-43aae29b-18ec-4941-bf3f-efb601e667fb.png#crop=0&crop=0&crop=1&crop=1&height=60&id=Vw5W6&margin=%5Bobject%20Object%5D&name=20180523153448582.png&originHeight=60&originWidth=238&originalType=binary∶=1&rotation=0&showTitle=false&size=15160&status=done&style=none&title=&width=238)
