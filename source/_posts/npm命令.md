---
title: npm命令
urlname: umtmyx
date: 2020-06-23 10:00:00 +0800
tags: [lerna]
categories: [前端]
---

- 先初始化一个 package.json 文件

```bash
npm init --yes
```

<!-- more -->

- npm 安装模块

```bash
npm install xxx利用 npm 安装xxx模块到当前命令行所在目录
npm install -g xxx利用npm安装全局模块xxx
```

- 本地安装时将模块写入 package.json 中：

```bash
npm install xxx             安装但不写入package.json
npm install xxx --save  安装并写入package.json的”dependencies”中
npm install xxx --save-dev 安装并写入package.json的”devDependencies”中
```

- npm 删除模块

```bash
npm uninstall xxx      删除xxx模块
npm uninstall -g xxx   删除全局模块xxx
```

- 如果想使用淘宝源下载, 主要为了速度，建议不要用 cnpm，而是直接修改源地址

```bash
npm i axios --save  --registry=http://registry.npm.taobao.org
```
