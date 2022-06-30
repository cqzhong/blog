---
title: App多环境配置
urlname: novxo8
date: 2018-03-06 11:46:52 +0800
tags: [iOS]
categories: [移动端]
---

1、project 里 Configurations 增加环境

<!-- more -->

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726447-ed696aba-b1fe-4d59-b412-203e4ec9b04b.png#align=left&display=inline&height=467&margin=%5Bobject%20Object%5D&originHeight=467&originWidth=753&size=0&status=done&style=none&width=753)

2、project 里 build settings 里 Preprocessor Macros 里的 value 里修改名字

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726365-b77d9c56-2a91-433f-a5c2-988f0fbe2a6a.png#align=left&display=inline&height=353&margin=%5Bobject%20Object%5D&originHeight=353&originWidth=902&size=0&status=done&style=none&width=902)

3、target 里  build settings 里 Preprocessor Macros 再设置下环境名字

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726380-98fe0d68-3465-4a2d-b18f-0a864a5b09f6.png#align=left&display=inline&height=349&margin=%5Bobject%20Object%5D&originHeight=349&originWidth=944&size=0&status=done&style=none&width=944)

4、target 里  build settings 里 点击+按钮增加一个宏 BUNDLE_DISPLAY_NAME
在里面给不同环境，设置不同名字
最后在 info 里 Bundle display name 设置名字 \${BUNDLE_DISPLAY_NAME}

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931727378-762eb7c4-ec89-4771-8e6a-dfd2af9f4e78.png#align=left&display=inline&height=252&margin=%5Bobject%20Object%5D&originHeight=252&originWidth=866&size=0&status=done&style=none&width=866)
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726369-7ebc066b-ae71-45e4-abe0-e6ebcbea0578.png#align=left&display=inline&height=561&margin=%5Bobject%20Object%5D&originHeight=561&originWidth=856&size=0&status=done&style=none&width=856)

5、target 里  build settings 里  Asset Catalog App Icon Set Name 给不同环境设置不同 icon
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726543-bad06342-9744-47f4-8fe6-5a62e6ab77f2.png#align=left&display=inline&height=243&margin=%5Bobject%20Object%5D&originHeight=243&originWidth=886&size=0&status=done&style=none&width=886)
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931726582-669050d0-4e4b-4fd2-a62d-8d9364449ba7.png#align=left&display=inline&height=203&margin=%5Bobject%20Object%5D&originHeight=203&originWidth=670&size=0&status=done&style=none&width=670)
