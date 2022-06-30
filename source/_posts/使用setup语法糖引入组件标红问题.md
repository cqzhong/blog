---
title: 使用setup语法糖引入组件标红问题
urlname: sru7uz
date: 2022-01-22 20:00:00 +0800
tags: [setup]
categories: [前端]
---

- 使用`setup`语法糖注册组件：

<!-- more -->

```typescript
<script setup lang="ts">
  import MineTopView from './components/MineTopView.vue' import MineCenterView
  from './components/MineCenterView.vue' import MineBottomView from
  './components/MineBottomView.vue'
</script>
```

- `MineTopView、MineCenterView、MineBottomView`标红，原因是因为语法检测的`Vetur`插件不适用了，需禁用掉当前插件，安装并启用`Volar`插件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1652082057470-604c1b6e-9d02-47df-b8a5-dd6bd508eab2.png#clientId=u0bfe2a93-cbb1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=550&id=ubb693b7f&margin=%5Bobject%20Object%5D&name=image.png&originHeight=550&originWidth=1880&originalType=binary∶=1&rotation=0&showTitle=false&size=390771&status=done&style=none&taskId=u37896c86-ed47-4b83-80a8-bae2b303be9&title=&width=1880)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1652082029018-30cbdbed-5098-455a-94da-289d6e54f9a6.png#clientId=u0bfe2a93-cbb1-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=596&id=u6886d410&margin=%5Bobject%20Object%5D&name=image.png&originHeight=596&originWidth=2244&originalType=binary∶=1&rotation=0&showTitle=false&size=613444&status=done&style=none&taskId=ua73c355c-037e-4788-99f7-e28d838020a&title=&width=2244)
