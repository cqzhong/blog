---
title: uniapp 更改输出目录，自定义打包目录
urlname: fxfcki
date: 2021-03-04 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

uniapp 打包配置具体可以参考项目中该路径下的文件`node_modules/@dcloudio/vue-cli-plugin-uni/lib/env.js`

<!-- more -->

- `UNI_INPUT_DIR`指定源代码的绝对路径

```javascript
    "mpaas:before-preview": "cross-env NODE_ENV=production UNI_PLATFORM=mp-alipay UNI_INPUT_DIR=src/app vue-cli-service uni-build"
```

- `UNI_OUTPUT_DIR`指定输出目录

```javascript
    "mpaas:before-compile": "cross-env NODE_ENV=development UNI_PLATFORM=mp-alipay UNI_OUTPUT_DIR=dist/mpaas/mp-alipay vue-cli-service uni-build --watch",
    "mpaas:before-preview": "cross-env NODE_ENV=production UNI_PLATFORM=mp-alipay UNI_OUTPUT_DIR=dist/mpaas/mp-alipay vue-cli-service uni-build",
    "mpaas:beforeu-upload": "cross-env NODE_ENV=production UNI_PLATFORM=mp-alipay UNI_OUTPUT_DIR=dist/mpaas/mp-alipay vue-cli-service uni-build"
```
