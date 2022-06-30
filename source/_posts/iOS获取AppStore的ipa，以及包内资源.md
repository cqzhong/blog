---
title: iOS获取AppStore的ipa，以及包内资源
urlname: fby365
date: 2019-09-13 21:41:04 +0800
tags: [cartool,iOS,ipa]
categories: [移动端]
---

1、获取 [Apple Configurator 2](https://apps.apple.com/cn/app/apple-configurator-2/id1037126344?mt=12)
2、打开 Apple Configurator 2，并连接 iPhone，点击 Apple Configurator 2 菜单中->账户->登录（用连接设备的 Apple ID）

<!-- more -->

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269494-19bacd8e-5d80-4e30-9e3a-8b5db57e1cb8.png#align=left&display=inline&height=326&margin=%5Bobject%20Object%5D&originHeight=326&originWidth=646&size=0&status=done&style=none&width=646)

3、选中界面中的手机画面，这时候“添加”按钮变为可点击状态
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937270549-cfef8933-029d-45f5-a598-b2dab67bb832.png#align=left&display=inline&height=1450&margin=%5Bobject%20Object%5D&originHeight=1450&originWidth=2004&size=0&status=done&style=none&width=2004)

4、点击上图中的应用，选择你要下载的 app
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269556-79f86f4b-946c-4d40-8c0c-ee64167d448b.png#align=left&display=inline&height=1430&margin=%5Bobject%20Object%5D&originHeight=1430&originWidth=1904&size=0&status=done&style=none&width=1904)

5、等待下载完成
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269684-872bba53-97f0-4255-80fe-76bdf8d04080.png#align=left&display=inline&height=1430&margin=%5Bobject%20Object%5D&originHeight=1430&originWidth=1962&size=0&status=done&style=none&width=1962)

6、当下载出现下图提示时候，不要进行操作，直接进入以下目录查找已经下载的 ipa

```bash
~/Library/Group Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Assets/TemporaryItems/MobileApps/
```

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269467-ad2a96ba-a10b-49cb-bba2-d8bc3c398c08.png#align=left&display=inline&height=721&margin=%5Bobject%20Object%5D&originHeight=721&originWidth=655&size=0&status=done&style=none&width=655)

7、将 ipa 拷贝出来，关闭 Apple Configurator 2

8、解压获取 ipa 内的内容。
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269563-522d01b4-d1b3-4184-b09a-32473bca2c57.png#align=left&display=inline&height=570&margin=%5Bobject%20Object%5D&originHeight=570&originWidth=886&size=0&status=done&style=none&width=886)
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269576-6c286e38-47a6-4419-93ef-c20c99a5977c.png#align=left&display=inline&height=318&margin=%5Bobject%20Object%5D&originHeight=318&originWidth=1086&size=0&status=done&style=none&width=1086)

9、找到.car 文件
![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937269481-8eea5ba4-d8f8-4001-b768-473bfba51efb.png#align=left&display=inline&height=540&margin=%5Bobject%20Object%5D&originHeight=540&originWidth=392&size=0&status=done&style=none&width=392)

10、解压 Assets.car
下载一个 [cartool](https://github.com/steventroughtonsmith/cartool)

11、执行编译获取 cartool 文件

12、打开终端 依次拖入 （1）cartool 文件 （2）Assets.car 文件 （3）解压至图片的文件夹. 回车执行，获取所有图片
