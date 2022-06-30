---
title: .xcassets文件说明和解压 Assets.car文件
urlname: nh53sv
date: 2018-11-13 15:06:53 +0800
tags: [Asset.car,cartool,iOS]
categories: [移动端]
---

- Assets.xcassets 是用来存放图像资源文件的。

<!-- more -->

- 如果图片存放在 assets 资源管理器,最终里面所以的图片会被打包成 Assets.car(用 ThemeEngine 可以把图片弄出来),其作用在于
  1、 自动识别@2x，@3x 图片，
  2、 根据不同的设备，不同的分辨率设置相应的图片。
  3、 可以对图片进行剪裁和拉伸处理
  在.car 中的图片是不能通过 imageWithContentsOfFile:来加载 imageName:加载的图片要么是 Assets.car 中的图片,要么是资源包(mainBundle)中直接存放的图片。如果用 imageNamed:从 Images.xcassets 以外的地方加载图片，必须在文件名后加扩展名，例如：
  UIImage \*image=[UIImage imageNamed:@"plus.png"];
- 将图片放在 xcassets 文件中的好处：

      1、组织清晰
      2、不同功用的图片有专门的格式
      3、不同分辨率的图片好管理
      4、工程打包后会对图片进行压缩
      这里我要着重说一下第四点，包的大小，如果将图片直接放在工程目录下面，项目打包后图片文件也是散落在包里面，而且不会对图片进行压缩，而如果放在 xcassets 中，在打包后会将这些图片（除了 AppIcon 和 LaunchImage，这两种图片是直接放在包中的）统一压缩成一个 Assets.car 的文件，大大减小包的大小，具体是几倍的关系我记不清了，但是相当的可观。

- 说完 Assets.xcassets，那么说说由它生成的 Assets.car 文件，这个文件是一种压缩文件。 我们在开发过程中肯定会遇到一种情况就是把一个 ipa 的包解压出来看看里面有哪些图片，不管是不是自己的项目，总可能会有这种需求，那如果图片都在 Assets.car 中该怎么获取呢，直接解压是不行的，这时候就需要用到一个命令行工具叫 cartool，这是一个开源软件，可以从 github 下载，这里给出 github 地址：[https://github.com/steventroughtonsmith/cartool](https://github.com/steventroughtonsmith/cartool)
- 解压方法
  1、打开终端
  2、拖入 cartool 文件 空格 Assets.car 空格 目标文件路径
