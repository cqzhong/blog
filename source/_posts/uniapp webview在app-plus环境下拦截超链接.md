---
title: uniapp webview在app-plus环境下拦截超链接
urlname: py8bbn
date: 2021-05-30 20:27:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

需求: web-view 组件加载一个网页, 可以拦截到在网页内点击一个链接的动作, 然后进行其它一些操作.

<!-- more -->

```html
<template>
  <view class="web-view"></view>
</template>

<script>
  export default {
    data() {
      return {
        webSrc: "",
      };
    },
    onLoad(options) {
      const src = decodeURIComponent(options.src);
      this.webSrc = src;

      // #ifdef APP-PLUS
      const webview = plus.webview.create("", src, {
        // plusrequire: 'none',
        // 'uni-app': 'none', 会导致 h5内调用uni方法失效
        top: uni.getSystemInfoSync().statusBarHeight + 44,
        bottom: "0px",
      });
      webview.overrideUrlLoading({ mode: "reject", match: ".*" }, (e) => {
        console.log("------overrideUrlLoading-----: ", e.url);
        // 拦截超链接并做跳转、打开新窗口
        uni.navigateTo({
          url: `/pages/webView/webView?src=${e.url}`,
        });
      });
      // 监听页面加载完成后修改导航栏标题
      webview.addEventListener(
        "loaded",
        function (e) {
          uni.setNavigationBarTitle({
            title: webview.getTitle() || "共轨之家",
          });
        },
        false
      );
      // embed.removeEventListener('loaded', embedLoaded);
      webview.loadURL(this.webSrc);
      // webview.show()
      const currentWebview = this.$scope.$getAppWebview();
      currentWebview.append(webview);
      // #endif
    },
    methods: {},
  };
</script>

<style lang="scss" scoped>
  .web-view {
    width: 100%;
    height: 100%;
  }
</style>
```
