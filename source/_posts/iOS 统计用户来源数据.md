---
title: iOS 统计用户来源数据
urlname: okgehv
date: 2019-09-19 14:04:11 +0800
tags: [AppStore,iOS]
categories: [其它]
---

用户获取来源包括 App Store 浏览、App Store 搜索、App 引荐来源和网页引荐来源。
当顾客在 App Store 中浏览或搜索时查看了您的 App，“展示次数”和“产品页面查看次数”便归因于“App Store 浏览”或“App Store 搜索”。
当顾客通过轻点某个 App 中或网页中的链接访问您的 App Store 产品页，随即产生的“产品页面查看次数”便归因于引荐的 App 或网页。如果顾客随后首次轻点下载了 App，则产生的“App 购买量”也会归因于引荐的 App 或网页。

<!-- more -->

### 一、使用苹果 ItunesConnect 后台的营销活动

1、 登录 [itunesconnect](https://itunesconnect.apple.com) --->App 分析 --->点击任一款 app --->来源 --->App 引荐来源。通过这几个步骤就能看到不同 app 的引荐来源。另外，还能看到网页的引荐来源：

2、生成一个营销活动链接

- 打开 [itunesconnect](https://itunesconnect.apple.com) ，然后登录开发者账号。然后点击 App 分析
- 点击你要进行营销推广的 App
- 点击来源，然后选中营销活动
- 点击右上角的生成营销活动链接
- 输入营销互动，然后在下面的营销活动链接里面复制此营销活动特有的 AppStore 链接。
- 生辰的链接用于投放，这样，我们就能统计到不同渠道的下载量了。

3、激活 5 个以上才能在 iTunes Connect 上展示，数据展示一般是有 1 天的时间延迟。

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602938263617-68bc5a43-3aa0-414b-8add-06cf9ef7f77a.png#align=left&display=inline&height=1232&margin=%5Bobject%20Object%5D&originHeight=1232&originWidth=2142&size=0&status=done&style=none&width=2142)

### 二、使用 SFSafariViewController 传递参数

```
   SFSafariViewController 是 iOS 9.0 出现的，可以通过 Safari 对应的 cookier 传递参数，跨App与Safari共享数据。但是 openurl 失败率还是很高，并且有系统版本、浏览器等限制，比如微信等第三方 App 的内置浏览器就不能很好实现。
```

### 三、通过 IDFA 进行追踪

[Google Analytics](https://developers.google.cn/analytics/devguides/platform/)

常用的比如谷歌官方的 Google Analytics，它的获取原理就是通过获取设备的 IDFA ，来作为唯一标示符号，然后根据你的渠道来源提供数据，通过比对的方式进行渠道定位。弊端在于，用户重置系统，或者关闭广告跟踪的话，这种方法就会失效。

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602938263648-e75d487f-5c5a-4f2e-b83f-837813d3383f.png#align=left&display=inline&height=300&margin=%5Bobject%20Object%5D&originHeight=300&originWidth=275&size=0&status=done&style=none&width=275)

目前用户的隐私保护意识也在逐渐觉醒，只要用户手握这个开关，IDFA 的统计误差就始终存在。

另一方面，Google Analytics 的 iOS 安装跟踪功能仅适用于通过移动广告网络（例如投放应用内广告的 AdMob）投放的广告。也就是如果渠道是从线下扫二维码或者 web 上的推广链接下载是不能通过这种方法跟踪到的，这时就需要其它工具作为补充。

### 四、采用第三方 SDK 追踪

1、[神策数据：App 渠道追踪](https://www.sensorsdata.cn/manual/app_channel_tracking.html)

2、[openinstall](https://www.openinstall.io)
