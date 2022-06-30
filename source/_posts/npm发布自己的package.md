---
title: npm发布自己的package
urlname: fu02qt
date: 2020-06-22 10:00:00 +0800
tags: [lerna]
categories: [前端]
---

[npm 官网](https://www.npmjs.com)

- 1、注册 npm 账号
- 2、查看当前的登录源

<!-- more -->

```bash
npm config get registry
```

- 3、修改当前登录的源

```bash
# 修改下载仓库
npm config set registry https://registry.npmjs.org/
# 修改 下载仓库为淘宝镜像
npm config set registry http://registry.npm.taobao.org/
```

- 4、查看是否登录

```bash
# 查看是否登录
npm whoami
# 没有则登录
npm login
```

- 5、新建一个文件夹，初始化一个项目

```bash
mkdir npm-pkg
cd npm-pkg
touch index.js
vim index.js

npm init
# 或者
npm init -y
```

- 6、发布包
  npm 支持以@符开头的包名称，把它叫做有范围的包，示例：@somescope/somepackage。
  但是在发布包的时候，npm 会把这种包当成是你的私有包来进行发布，一般我们是发布不了的，因为 npm 会提示需要登录等等东西，这个时候我们一般就加上一个参数，告诉 npm 我们要发布的这个包是一个公共包：npm publish --access=public，不出意外这个包就可以发布成功了。

```bash
npm publish
```

- 7、删除 NPM 包

```bash
npm unpublish 包名 --force
```

#### 注意事项

- 包名不要包含 demo 字样，会触发垃圾检测，不能有大写字母/空格/下滑线
- npm 镜像源不要连接代理,切换到官网地址
- 私密代码写入.gitignore 或.npmignore 中，上传就会被忽略了
