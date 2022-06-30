---
title: JSBridge在uniapp中的使用
urlname: lvq0kk
date: 2020-12-28 20:00:00 +0800
tags: [JavaScript]
categories: [前端]
---

## 什么是 JSBridge

---

    主要是给 JavaScript 提供调用 Native 功能的接口，让混合开发中的前端部分可以方便地使用 Native 的功能（例如：地址位置、摄像头）。

而且 JSBridge 的功能不止调用 Native 功能这么简单宽泛。实际上，JSBridge 就像其名称中的 Bridge 的意义一样，是 Native 和非 Native 之间的桥梁，它的核心是构建 Native 和非 Native 间消息通信的通道，而且这个通信的通道是双向的。

<!-- more -->

### 1、创建 jsBridge.js 文件

```javascript
// #ifdef H5
const u = navigator.userAgent;
// Android终端
const isAndroid = u.indexOf("Android") > -1 || u.indexOf("Adr") > -1;
// iOS 终端
const isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);

function setupWebViewJavascriptBridge(callback) {
  // Android使用
  if (isAndroid) {
    if (window.WebViewJavascriptBridge) {
      callback(window.WebViewJavascriptBridge);
    } else {
      document.addEventListener(
        "WebViewJavascriptBridgeReady",
        function () {
          callback(window.WebViewJavascriptBridge);
        },
        false
      );
    }
    // sessionStorage.phoneType = 'android'
  }
  if (isiOS) {
    if (window.WebViewJavascriptBridge)
      return callback(window.WebViewJavascriptBridge);
    if (window.WVJBCallbacks) return window.WVJBCallbacks.push(callback);
    window.WVJBCallbacks = [callback];
    var WVJBIframe = document.createElement("iframe");
    WVJBIframe.style.display = "none";
    WVJBIframe.src = "wvjbscheme://__BRIDGE_LOADED__";
    document.documentElement.appendChild(WVJBIframe);
    setTimeout(function () {
      document.documentElement.removeChild(WVJBIframe);
    }, 0);
    // sessionStorage.phoneType = 'ios'
  }
}

// 注册回调函数，第一次连接时调用 初始化函数(android需要初始化,ios不用)
setupWebViewJavascriptBridge((bridge) => {
  if (isAndroid) {
    // 初始化
    bridge.init((message, responseCallback) => {
      var data = {
        "Javascript Responds": "Wee!",
      };
      responseCallback(data);
    });
  }
});
// #endif

export default {
  // js调APP方法 （参数分别为:app提供的方法名  传给app的数据  回调）
  callHandler(name, data, callback) {
    // #ifdef H5
    setupWebViewJavascriptBridge((bridge) => {
      bridge.callHandler(name, data, callback);
    });
    // #endif
  },
  // APP调js方法 （参数分别为:js提供的方法名  回调）
  registerHandler(name, callback) {
    // #ifdef H5
    setupWebViewJavascriptBridge((bridge) => {
      bridge.registerHandler(name, (data, responseCallback) => {
        callback(data, responseCallback);
      });
    });
    // #endif
  },
};
```

### 2、main.js 中引入

```javascript
import jsBridge from "@/utils/jsBridge";
Vue.prototype.$jsBridge = jsBridge;
```

### 3、项目中使用

```javascript
// js调用原生方法
this.$jsBridge.callHandler(define.GZH5_TO_NATIVE_NAVIGATE_TO, url, res => {
   // uni.showToast({
   //   title: `回调结果: ${JSON.stringify(res)}`,
   //   duration: 3000
   // })
 })

// 原生调用js方法
mounted() {
    this.$jsBridge.registerHandler('alertMessageBox', (datas, responseCallback) => {
      uni.showToast({
        title: '原生调用js方法成功',
        duration: 2000
      })
      responseCallback('js回调给原生值')
    })
 },
```

### 4、注意事项

- 方法名要和 App 开发人员沟通好

### 参考链接

- [JSBridge 的原理及使用](https://blog.csdn.net/yuzhengfei7/article/details/93468914)
- [使用 JSBridge 与原生 IOS、Android 进行交互](https://blog.csdn.net/zgd826237710/article/details/95518433)
