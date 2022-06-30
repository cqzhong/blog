---
title: mPaaS IDE调试⽅法
urlname: ygxbcx
date: 2021-02-04 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

- 打开 IDE，在编辑器界⾯按`cmd + shift + p`。
- `windows` 是 `ctrl + shipf + p`
- 选中 `Developer: open background window` 回⻋

<!-- more -->

![11.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1612426653829-3d9e3886-da79-45c5-9e2d-6f29006d8b4e.png#align=left&display=inline&height=852&margin=%5Bobject%20Object%5D&name=11.png&originHeight=852&originWidth=1500&size=257858&status=done&style=none&width=1500)
弹出⼀个 chrome console 窗⼝，在 chome 中输⼊ `bash debugAdaptorWebView.mPaaS.openDevTools()` 回车

- 政钉输⼊ `debugAdaptorWebView['mPaaS-dingtalk'].openDevTools()`

![12.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1612426672259-4fd947d5-917a-4f2d-a1f6-3139749c1def.png#align=left&display=inline&height=852&margin=%5Bobject%20Object%5D&name=12.png&originHeight=852&originWidth=1500&size=468775&status=done&style=none&width=1500)

- ⼜弹出⼀个 chrome 窗⼝，此窗⼝是 node server 的接⼝数据

![13.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1612426683704-f997e21b-3f72-4871-a7c1-649098034f83.png#align=left&display=inline&height=852&margin=%5Bobject%20Object%5D&name=13.png&originHeight=852&originWidth=1500&size=570600&status=done&style=none&width=1500)
