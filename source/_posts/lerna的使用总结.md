---
title: lerna的使用总结
urlname: ucpmi6
date: 2020-06-21 10:00:00 +0800
tags: [lerna]
categories: [前端]
---

Lerna 是一个工具，它优化了使用 git 和 npm 管理多包存储库的工作流。

- [Lerna 官网](https://lernajs.io/)
- [Lerna 仓库](https://github.com/lerna/lerna)
- [npm 官网](https://www.npmjs.com)

<!-- more -->

#### 1、注册 npm 账号

#### 2、查看当前的登录源

```bash
npm config get registry
```

#### 3、修改当前登录的源

```bash
# 修改下载仓库
npm config set registry https://registry.npmjs.org/
# 修改 下载仓库为淘宝镜像
npm config set registry http://registry.npm.taobao.org/
```

#### 4、查看是否登录

```bash
# 查看是否登录
npm whoami
# 没有则登录
npm login
```

#### 5、安装 lerna

```bash
npm install --global lerna
# 或者
npm install lerna -g
```

#### 6、查看 lerna 版本

```bash
lerna --version
```

#### 7、初始化

（使用 lerna 管理项目时，可以选择两种模式。默认的为固定模式(Fixed mode)，当使用 lerna init 命令初始化项目时，就默认为固定模式，也可以使用 lerna init --independent 命令初始化项目，这个时候就为独立模式(Independent mode)。固定模式中，packages 下的所有包共用一个版本号(version)，会自动将所有的包绑定到一个版本号上(该版本号也就是 lerna.json 中的 version 字段)，所以任意一个包发生了更新，这个共用的版本号就会发生改变。而独立模式允许每一个包有一个独立的版本号，在使用 lerna publish 命令时，可以为每个包单独制定具体的操作，同时可以只更新某一个包的版本号。）

- 固定模式

```bash
# 初始化项目
lerna init
```

- 独立模式

```bash
# 初始化项目（独立模式）
lerna init --independent
```

- 初始化后的目录结构

```md
my-lerna-demo
|--- packages
|--- .gitignore
|--- lerna.json #lerna 配置
|--- package.json
|--- README.md
```

其中：packages 目录用来存放我们需要拆分的各种公共代码库。lerna.json 文件里面记录了 lerna 的相关配置信息

```json
{
  "version": "0.0.1",
  "npmClient": "npm",
  "command": {
    "publish": {
      "ignoreChanges": ["ignored-file", "*.md"],
      "message": "chore(release): publish"
    },
    "bootstrap": {
      "ignore": "component-*",
      "npmClientArgs": ["--no-package-lock"]
    }
  },
  "packages": ["packages/*"]
}
```

分别介绍每个配置项的功能：
version: 记录当前项目的版本号
npmClient: 你可以指定使用 npm, cnpm 或 yarn 来执行命令
command.publish.ignoreChanges: 忽略特定的项
command.publish.npmClientArgs: 当执行 lerna bootstrap 命令时，传给 npm install 的参数
command.publish.message: 发布模块的时候，填写的 commit 信息
packages: 模块包默认所在的地址

```bash
# package.json 文件加入
 "private": true,
  "workspaces": [
    "packages/*"
  ],

# lerna.json 文件加入
"useWorkspaces": true,
"npmClient": "yarn",
```

#### 8、创建一个包

我们在 packages 新建目录 lerna-module

````bash
# cd 到lerna-module 初始化
npm init
# 或者
npm init -y
``
- 目录结构

```md
my-lerna-demo
|--- packages
     |--- lerna-module
          |--- package.json
          |--- index.js
|--- .gitignore
|--- lerna.json #lerna配置
|--- README.md
````

- 添加依赖（类似 npm install)
  lerna add babel , 该命令会在 package-1 和 package-2 下安装 babel
  lerna add react --scope=package-1 ,该命令会在 package-1 下安装 react
  lerna add package-2 --scope=package-1，该命令会在 package-1 下安装 package-2

```bash
lerna add
# 给a, b 包中加入Lodash，会同时改变a,b模块中packages.json文件
lerna add lodash packages/a packages/b
# 给a 包中加入jquery, 使用--dev参数是使依赖加入到devDependencies中
lerna add jquery packages/a --dev
# 你也可以使用通配符, 下面这命令，会往所有re开头的模块包中加入依赖
lerna add jquery packages/re-*
# 指定特定的范围，要使用--scope参数，如下：给b包安装a模块
lerna add a --scope=b
```

- 发版 (因为 lerna publish 是集合了 git push 和 npm publish 的操作，因此我们需要将项目文件夹连接到 git 仓库和登录 npm 仓库)

```bash
lerna publish
```

运行 lerna updated 来决定哪一个包需要被 publish
如果有必要，将会更新 lerna.json 中的 version
将所有更新过的的包中的 package.json 的 version 字段更新
将所有更新过的包中的依赖更新
为新版本创建一个 git commit 或 tag
将包 publish 到 npm 上

同时，该命令也有许多的参数，例如--skip-git 将不会创建 git commit 或 tag，--skip-npm 将不会把包 publish 到 npm 上。

```bash
# 新建包
lerna create <name> [loc]
# 自动构建项目
lerna bootstrap
# 列出当前项目所有包,显示packages下的各个package的version
lerna ls
# 清理node_modules文件夹
lerna clean
# 将本地包链接起来，可以直接引用。链接互相引用的库
lerna link
# 对包是否发生过变更
lerna updated/lerna diff
# 运行npm script，可以指定具体的package
lerna run
```
