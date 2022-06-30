---
title: iTunes安装（回退到有App Store）
urlname: zit3ds
date: 2018-03-21 11:46:52 +0800
tags: [iTunes]
categories: [其它]
---

#### 1、退出 iTunes

#### 2、使用终端强制删除

<!-- more -->

终端输入：sudo rm -rf /Applications/iTunes.app 回车，

#### 3、删除错误文件

打开 Finder，搜索 iTunes 文件(可能存在相同文件夹，右键 iTunes 文件夹选择“在上层文件夹显示”，找到根文件夹为音乐文件夹的 iTunes 文件，删除下图中选中的文件)，如下图删除选中文件:
`[https://support.apple.com/kb/DL1934?viewlocale=zh_CN&locale=zh_CN](https://support.apple.com/kb/DL1934?viewlocale=zh_CN&locale=zh_CN)`
补充
command+R 进入 recover 打开终端
csrutil disable
csrutil enable
错误参考；`[https://apple.stackexchange.com/questions/299842/how-to-fix-itunes-error-45076](https://apple.stackexchange.com/questions/299842/how-to-fix-itunes-error-45076)`

```bash
sudo rm -rf /Library/Documentation/Applications/iTunes/Acknowledgements.rtf
sudo rm -rf /Library/Documentation/iPod/Acknowledgements.rtf
sudo rm -rf /Library/Frameworks/iTunesLibrary.framework/
sudo rm -rf /Applications/iTunes.app/
sudo rm -rf /System/Library/PrivateFrameworks/iTunesAccess.framework/
sudo rm -rf /System/Library/LaunchDaemons/com.apple.fpsd.plist
sudo rm -rf /System/Library/PrivateFrameworks/CoreFP.framework/
sudo rm -rf /System/Library/PrivateFrameworks/CoreADI.framework/
sudo rm -rf /System/Library/LaunchDaemons/com.apple.adid.plist
sudo rm -rf /System/Library/CoreServices/UAUPlugins/ADIUserAccountUpdater.bundle/
sudo rm -rf /System/Library/CoreServices/CoreTypes.bundle/Contents/Library/MobileDevices.bundle/
sudo rm -rf /System/Library/LaunchDaemons/com.apple.usbmuxd.plist
sudo rm -rf /System/Library/PrivateFrameworks/AirTrafficHost.framework/
sudo rm -rf /System/Library/PrivateFrameworks/DeviceLink.framework/
sudo rm -rf /System/Library/PrivateFrameworks/MobileDevice.framework/
sudo rm -rf /System/Library/Extensions/AppleMobileDevice.kext/
sudo rm -rf /System/Library/Extensions/AppleUsbEthernetHost
```
