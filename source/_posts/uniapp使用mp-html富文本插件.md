---
title: uniapp使用mp-html富文本插件
urlname: il4req
date: 2022-03-14 18:00:00 +0800
tags: [富文本]
categories: [其它]
---

- 下载 [mp-html](https://github.com/jin-yufeng/mp-html)
- 在 `tools/config.js`文件里配置需要的插件

<!-- more -->

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1652111422380-37576276-b85f-443f-b50f-d04cfe6325c2.png#clientId=ube5af576-8f47-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=144&id=u4d3e6121&margin=%5Bobject%20Object%5D&name=image.png&originHeight=288&originWidth=676&originalType=binary∶=1&rotation=0&showTitle=false&size=35812&status=done&style=none&taskId=u9c64abcf-f4ac-4a07-8b09-02bc66aa061&title=&width=338)

- 参照[uni_modules 规范](https://uniapp.dcloud.io/plugin/uni_modules.html) 导入到 uniapp 的项目中

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1652111515859-5dcda9e3-b8cb-44dc-a9bf-ee11b30ace47.png#clientId=ube5af576-8f47-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=71&id=u7e162180&margin=%5Bobject%20Object%5D&name=image.png&originHeight=142&originWidth=253&originalType=binary∶=1&rotation=0&showTitle=false&size=9538&status=done&style=none&taskId=u0aa4f5b7-45d0-43a8-b170-74f0fc0b372&title=&width=126.5)

- 在对应页面内直接使用 `mp-html`

```vue
<!-- 文章详情 -->
<template>
  <view class="detail-view">
    <mp-html
      :content="html"
      domain="https://www.cqzhong.cn"
      lazy-load
      scroll-table
      selectable
      use-anchor
      show-with-animation
      @ready="ready"
      @load="load"
      @imgtap="imgtap"
      @linktap="linktap"
      @navigate="navigate"
    />
  </view>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { onLoad } from "@dcloudio/uni-app";

import { detailContent } from "@/api/home";

const html = ref<string>("");

onLoad((options) => {
  uni.setNavigationBarTitle({
    title: decodeURIComponent(options.title ?? ""),
  });
  // this.html = `<pre><code class="language-css">p { color: red }</code></pre>`
  const path = decodeURIComponent(options.path ?? "");
  detailContent(path).then((res: any) => {
    const content = res.content;
    html.value = content;
  });
});

const load = () => {
  console.log("dom 树加载完毕");
};

const ready = (e: any) => {
  console.log("ready 事件触发：", e);
};

const imgtap = (e: any) => {
  console.log("imgtap 事件触发：", e);
};

const linktap = (e: any) => {
  console.log("linktap 事件触发：", e);
};

function navigate(href: string, e: any) {
  uni.setClipboardData({
    data: `${href}`,
    success: () => {
      uni.showToast({
        title: "已成功复制链接",
        icon: "none",
      });
    },
  });
}
</script>

<style>
page {
  min-height: 100%;
  background-color: #fff;
}
</style>
<style lang="scss" scoped>
.detail-view {
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  padding: 32rpx;
  animation: fadeIn 1s linear;
}
</style>
```
