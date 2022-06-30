---
title: .env 文件配置说明及新增配置文件
urlname: fqd09t
date: 2021-02-18 20:00:00 +0800
tags: [vue]
categories: [前端]
---

## 文件说明

.env：全局默认配置文件，无论什么环境都会加载合并。
.env.development：开发环境的配置文件
.env.production：生产环境的配置文件

> 注意：三个文件的文件名必须按上面方式命名，不能乱起名，否则读取不到文件。

<!-- more -->

## 内容格式

> 注意：属性名必须以 VUE_APP 开头，如：**VUE_APP_XXX**

## 加载

vue 会根据启动命令自动加载相对应的环境配置文件。
开发环境加载 `.env` 和 `.env.development` 。
生成环境加载 `.env` 和 `.env.production` 。

## 优先级

**环境配置文件 > 全局配置文件**
当全局的配置文件和环境的配置文件有相同配置项时，环境的配置项会覆盖全局的配置项
如：

开发环境
![](https://upload-images.jianshu.io/upload_images/1790502-859a70558043912b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1200/format/webp#id=FSnJI&originHeight=125&originWidth=1200&originalType=binary∶=1&status=done&style=none)
打印 process.env 属性
![](https://upload-images.jianshu.io/upload_images/1790502-9d74c2ad6380469e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/647/format/webp#id=aga8A&originHeight=167&originWidth=647&originalType=binary∶=1&status=done&style=none)

> 从上面图片中可知，**.env** 中的全局属性 **VUE_APP_PREVIEW** 与 **VUE_APP_API_BASE_URL** 被覆盖。
>
> **.env** 中的全局属性 **VUE_APP_AGE** 被保留。

## 项目中的使用

在配置文件中定义的属性在其它文件中如何访问呢？？
可以使用 `process.env.xxx` 来访问属性。
如：
![](https://upload-images.jianshu.io/upload_images/1790502-e49bdb33a7b5378c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/524/format/webp#id=ZwgrD&originHeight=174&originWidth=524&originalType=binary∶=1&status=done&style=none)
![](https://upload-images.jianshu.io/upload_images/1790502-90f1c5644a9a54ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/632/format/webp#id=f6B8f&originHeight=133&originWidth=632&originalType=binary∶=1&status=done&style=none)
​

## 新增环境 staging

- `package.json` 同级目录下新增 `.env.staging` 文件

```bash
.
├── README.md
├── babel.config.js
├── build
├── .env.production
├── .env.development
├── .env.staging
├── package.json
├── public
├── src
├── static
└── vue.config.js
```

- `.env.staging `文件

```vue
NODE_ENV = production # just a flag ENV = 'staging' # base api
VUE_APP_WEBSOCKET_URL = 'http://...' VUE_APP_ENV = 'staging'
#(此处根据测试环境域名动态变化, 此处是默认值) VUE_APP_BASE_API = 'http://...'
VUE_APP_CIUCUIT_PREVIEW_BASE = 'https://...'
```

- `package.json` 文件修改

```json
{
  "scripts": {
    "build:prod": "vue-cli-service build",
    "build:stage": "vue-cli-service build --mode staging",
    "dev": "vue-cli-service serve"
  },
  "license": "MIT"
}
```

## 参考

- [.env 文件配置详解](https://blog.csdn.net/cezlz/article/details/108100460)
