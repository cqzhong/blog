---
title: uniapp 蒙板双滚动问题
urlname: yu5sdx
date: 2020-04-25 20:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

```html
<template>
  <view class="new-drop" @touchmove.stop.prevent="scrollViewDidScroll"> </view>
</template>

<script>
  export default {
    data() {},
    methods: {
      scrollViewDidScroll() {},
    },
  };
</script>
```
