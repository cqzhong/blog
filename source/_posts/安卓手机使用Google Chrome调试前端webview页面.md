---
title: 安卓手机使用Google Chrome调试前端webview页面
urlname: hcwsmu
date: 2021-04-15 18:00:00 +0800
tags: [webview]
categories: [其它]
---

### 一、环境准备

- 在安卓 APP 中启用 WebView 调试，开启调试后，Chrome DevTools 才能对 WebView 进行远程调试；

```java
WebView.setWebContentsDebuggingEnabled(true);　
```

- 电脑端安装 Google Chrome
- 手机打开开发者模式、允许**USB**调试

<!-- more -->

​

### 二、开始

- 电脑 Google Chrome 打开 `chrome://inspect/#devices`, 若手机连接电脑成功则出现设备号，详见下图:

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1619429573942-98753fa4-2043-4ee1-b323-5113a5b8f0b2.png#height=174&id=IPvTM&margin=%5Bobject%20Object%5D&name=image.png&originHeight=348&originWidth=567&originalType=binary∶=1&size=35682&status=done&style=none&width=283.5)

- 手机端打开浏览器、输入调试网址
- 或者打开 App 应用、选择使用原生 webview 加载的页面

![image.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1619429823100-a7e7f4fc-0fe2-4896-8ac1-0a5738d9ce3b.png#height=331&id=uXmzE&margin=%5Bobject%20Object%5D&name=image.png&originHeight=662&originWidth=1091&originalType=binary∶=1&size=102494&status=done&style=none&width=545.5)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1619429905283-7e2aa14d-31a7-4ccc-b0f4-818fe09e0b47.png#height=384&id=A9CwK&margin=%5Bobject%20Object%5D&name=image.png&originHeight=767&originWidth=864&originalType=binary∶=1&size=248908&status=done&style=none&width=432)

### 参考

- [Android WebView 调试方法](https://www.cnblogs.com/wmhuang/p/7396150.html)​
