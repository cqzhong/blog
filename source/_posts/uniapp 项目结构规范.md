---
title: uniapp 项目结构规范
urlname: mi3xeq
date: 2021-06-16 17:00:00 +0800
tags: [规范,uniapp]
categories: [其它]
---

## 一、项目分包

### 1、整体分包

- 分包目录结构如下

<!-- more -->

```bash
.
├── pages
│   └── home
│       ├── components
│       └── home.vue
├── packageA-subpackage
│   ├── components
│   ├── pages
│   │   └── list
│   │       ├── components
│   │       └── list.vue
│   └── static
│       ├── css
│       ├── image
│       └── js
├── static
│   ├── css
│   ├── image
│   └── js
├── service
├── components
├── utils
├── main.js
├── App.vue
├── manifest.json
├── pages.json
└── uni.scss
```

- pages.json 中填写

```json
{
    "pages": [{
        "path": "pages/home/home",
        "style": { ...}
    }],
    "subPackages": [{
        "root": "packageA-subpackage",
        "pages": [{
            "path": "pages/list/list",
            "style": { ...}
        }]
    }],
    "preloadRule": {
        "packageA-subpackage/pages/list/list": {
            "network": "all",
            "packages": ["__APP__"]
        }
    }
}
```

- 分包命名规范:
  - 模块名后面跟`-subpackage` 示例 `packageA-subpackage` packageA 为模块名
- 打包原则
  - 声明 subpackages 后，将按 subpackages 配置路径进行打包，- subpackages 配置路径外的目录将被打包到 app（主包） 中
  - app（主包）也可以有自己的 pages（即最外层的 pages 字段)
  - subpackage 的根目录不能是另外一个 subpackage 内的子目录
  - tabBar 页面必须在 app（主包）内
- 引用原则
  - packageA-subpackage 无法 require packageB-subpackage JS 文件，但可以 require app、自己 package 内的 JS 文件
  - packageA-subpackage 无法 import packageB-subpackage 的 template，但可以 require app、自己 package 内的 template
  - packageA-subpackage 无法使用 packageB-subpackage 的资源，但可以使用 app、自己 package 内的资源

### 2、service 划分

```bash
.
├── api
│   ├── config
│   │   ├── httpConfig.js
│   │   └── index.js
│   ├── home.js
│   ├── index.js
│   ├── mine.js
│   ├── packageA-subpackage
│   │   └── list.js
│   └── packageB-subpackage
│       └── list.js
└── store
    ├── index.js
    └── modules
        ├── packageA-subpackage.js
        └── packageB-subpackage.js
```

## 二、utils 结构

- 划分原则
  - 判断是否有需要独立为文件夹
  - 判断是否有需要独立为 js 文件
- 非特殊情况下 utils 文件下入口文件必须为 index.js

```bash
.
└── utils
    ├── base64.js
    ├── index.js
    └── monitor
        ├── miniProLoggerMixins.js
        ├── monitor.js
        └── wxLogger.js
```

## 三、注意事项

- 图片等媒体资源统一放置于七牛云，并注意按功能区分文件夹，eg:`[https://static4.51gonggui.com/GGZJ_dataService/materialFolder/folder.png](https://static4.51gonggui.com/GGZJ_dataService/materialFolder/folder.png)`,`GGZJ_dataService` 代表该项目，`materialFolder` 代表模块
- 区分环境 不用取反操作, 需罗列出平台

```javascript
// #ifdef MP-WEIXIN || MP-ALIPAY ||  H5
    ...判断内容, 当运行环境为 微信小程序、支付宝小程序、h5时候
// #endif
```

- 如区分环境条件注释内代码较多, 需独立出方法, 并将区分环境的注释代码放在方法内

```javascript
test() {
  // #ifdef MP-WEIXIN || MP-ALIPAY ||  H5
    ...判断内容, 当运行环境为 微信小程序、支付宝小程序、h5时候
  // #endif
}
```

- 和 eslint 冲突, 禁单个规则

```javascript
api.sendEvent({
  // eslint-disable-line
  name: "acOpenNewFrm",
  extra: {
    name: "picList" + item.fileNo,
    url,
  },
});
```

## 推荐阅读

- [uniapp 分包](https://uniapp.dcloud.io/collocation/pages?id=subpackages)

- [关于分包优化的说明](https://uniapp.dcloud.io/collocation/manifest?id=%E5%85%B3%E4%BA%8E%E5%88%86%E5%8C%85%E4%BC%98%E5%8C%96%E7%9A%84%E8%AF%B4%E6%98%8E)

- [微信小程序使用分包](https://developers.weixin.qq.com/miniprogram/dev/framework/subpackages/basic.html)
