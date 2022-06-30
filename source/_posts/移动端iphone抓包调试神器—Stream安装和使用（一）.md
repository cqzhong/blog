---
title: 移动端iphone抓包调试神器—Stream安装和使用（一）
urlname: axggl5
date: 2022-04-15 18:00:00 +0800
tags: [抓包]
categories: [其它]
---

[本文转载自 王大力测试进阶之路](https://blog.csdn.net/qq_36502272/article/details/117341718)
之前已经给大家分享了很多[抓包工具](https://so.csdn.net/so/search?q=%E6%8A%93%E5%8C%85%E5%B7%A5%E5%85%B7&spm=1001.2101.3001.7020)的文章了，如果觉得有用，记得分享！！！

<!-- more -->

[Fiddler 抓取 APP 请求（环境搭建）之 mama 再也不用担心抓不到包了](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030eabdf74763ab60912516b5cc2c53135de2c190f7340fb2b0148c47b605a7254b2d5d3992&idx=1∣=2247483801&scene=21&sn=86d8d96a7fbb8ad80e562fa6db5a80c3#wechat_redirect)

[Fiddler 抓包神器带你遨游网络，叱咤风云，为所欲为](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030ea6ef7476378b3280cd50c5632291ce186d2936fe2fffc9cdb3bb3f12022fd41cdb5a6c8&idx=1∣=2247483722&scene=21&sn=71d6dad6266ec9d94aed56928e63bad2#wechat_redirect)

[【Fiddler 篇】FreeHttp 无限篡改 http 报文数据调试和 mock 服务](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030e957f74760416a388138e344b5ab18ac1a785ad6960ea01d4b49dcd39337ebfa9275a908&idx=1∣=2247484019&scene=21&sn=2ee8fb792a8ab02253d2c2672443a1c9#wechat_redirect)

[【Fiddler 篇】抓包工具之 Filters（过滤器）进行会话过滤](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030e9e9f74760ff8185ccb6c96b75a80622d5037d9f564a3f4c99f12691a951c94d3344894a&idx=1∣=2247484109&scene=21&sn=2b6a905281284273b955c45c94c8e2ec#wechat_redirect)

[【Fiddler 篇】Stave 插件之环境映射](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030e9f2f74760e41e7a58ac197de0212a8e55a74e68ca3137980fda7f952ce726745a03f0a0&idx=1∣=2247484118&scene=21&sn=13a11e410080e745143b047c9da23bb5#wechat_redirect)

[Fiddler Everywhere 全平台抓包调试工具安装和使用（一）](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030e348f7476a5e9a2d36dbfb41cfcff894bbe330a4ee7df11d713e13b0ec753422ea0f72ae&idx=1∣=2247485548&scene=21&sn=fdf7c4b36858f7c38afd228df95ad08a#wechat_redirect)

[【Jmeter 篇】你有 Fiddler Charles，我有 Jmeter 录制 Web 和 App](http://mp.weixin.qq.com/s?__biz=Mzg5MzI1NTI0Mw%3D%3D&chksm=c030e886f74761904a7f66344e20cfd9b763682a9827de65f851bbe1cc90c8768ba36e4e8c58&idx=1∣=2247484322&scene=21&sn=07b2790fbaf04ce46debd340b52697b9#wechat_redirect)

[stream](https://so.csdn.net/so/search?q=stream&spm=1001.2101.3001.7020)是一款免费轻量级移动端 ios 抓包调试工具，配置方便无需设置代理，集成了 HTTP 抓包、构建请求、Hosts 设置、常用工具、数据导出等功能。

1、苹果手机 appstore 搜 stream 并下载

![](https://img-blog.csdnimg.cn/img_convert/597eb512a532854a1aa58863c84b8023.png#crop=0&crop=0&crop=1&crop=1&id=arsnG&originHeight=340&originWidth=306&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

2、进入抓包工具，允许配置 VPN，下载 CA 证书

![](https://img-blog.csdnimg.cn/img_convert/9549255ab7549cdf69a9a32bf1d91dd7.png#crop=0&crop=0&crop=1&crop=1&id=kgPWX&originHeight=443&originWidth=272&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/img_convert/92fef9d1f0dbce13af80283ae34ba539.png#crop=0&crop=0&crop=1&crop=1&id=jXpcI&originHeight=412&originWidth=273&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

3、设置-通用-描述文件，找到下载好的证书，安装成功

![](https://img-blog.csdnimg.cn/img_convert/072ffcade9e9f77342965e6e6cda72f4.png#crop=0&crop=0&crop=1&crop=1&id=w2NbC&originHeight=371&originWidth=297&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/img_convert/e3db2d42065a427432c3c960550d72ec.png#crop=0&crop=0&crop=1&crop=1&id=ONO2l&originHeight=331&originWidth=286&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

4、设置-通用-关于手机-证书信任设置，开启信任

![](https://img-blog.csdnimg.cn/img_convert/31bfc013e27417c86e2cb3e7d5aacd04.png#crop=0&crop=0&crop=1&crop=1&id=PWpaO&originHeight=430&originWidth=273&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/img_convert/60d926188b4b4fa67a6fbc526e77e807.png#crop=0&crop=0&crop=1&crop=1&id=MlrGg&originHeight=383&originWidth=301&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

5、点开始[抓包](https://so.csdn.net/so/search?q=%E6%8A%93%E5%8C%85&spm=1001.2101.3001.7020)，进入要抓包的 app 美团外卖，抓好包后 停止抓包

![](https://img-blog.csdnimg.cn/img_convert/144d462e6d3f821d5dec721bd7edc9f9.png#crop=0&crop=0&crop=1&crop=1&id=D9wyc&originHeight=550&originWidth=290&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

6、进入抓包历史，查看抓包信息

![](https://img-blog.csdnimg.cn/img_convert/cbf1be09fe6558e8804b036aff828285.png#crop=0&crop=0&crop=1&crop=1&id=WL3vx&originHeight=293&originWidth=313&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/img_convert/b152e81b772231a04349dfd11b3135ff.png#crop=0&crop=0&crop=1&crop=1&id=tkCgg&originHeight=604&originWidth=291&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/img_convert/36d1cdca1512db73df813f6f06e5f3d1.png#crop=0&crop=0&crop=1&crop=1&id=Pw9VL&originHeight=607&originWidth=285&originalType=binary∶=1&rotation=0&showTitle=false&status=done&style=none&title=)
