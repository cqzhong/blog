---
title: uniapp使用小程序插件
urlname: spxt4m
date: 2020-07-30 20:00:00 +0800
tags: [uni-app]
categories: [前端]
---

#### 1、根目录下创建`wxcomponents`文件夹

<!-- more -->

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1603432795270-60eae846-d573-4343-a6d3-161e245e8a3b.png#crop=0&crop=0&crop=1&crop=1&height=249&id=w7wkQ&margin=%5Bobject%20Object%5D&name=image.png&originHeight=351&originWidth=282&originalType=binary∶=1&rotation=0&showTitle=false&size=80910&status=done&style=none&title=&width=200)

#### 2、将小程序组件拷贝过来放入对应文件夹

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1603432895288-12d93133-3360-4b0c-88ed-320c5e593c6a.png#crop=0&crop=0&crop=1&crop=1&height=188&id=WVPlX&margin=%5Bobject%20Object%5D&name=image.png&originHeight=364&originWidth=542&originalType=binary∶=1&rotation=0&showTitle=false&size=61951&status=done&style=none&title=&width=280)

#### 3、在 uniapp 的 page.json 中添加小程序组件

```json
{
  "path": "pages/repairOrderDetail/driverDetail",
  "style": {
    "navigationBarTitleText": "报修单详情",
    "usingComponents": {
      "driver-skeleton": "/wxcomponents/skeleton/driverDetail/index"
    }
  }
}
```

#### 4、在页面对应的 vue 组件中直接加载使用

```html
<template>
  <view>
    <driver-skeleton />
  </view>
</template>
```
