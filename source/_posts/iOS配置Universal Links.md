---
title: iOS配置Universal Links
urlname: wd1moa
date: 2021-01-03 20:00:00 +0800
tags: [Universal Links,iOS]
categories: [移动端]
---

#### 1、配置 App ID 支持 Associated Domains

登录 [苹果开发者中心](https://developer.apple.com/) 找到对应的 App ID，在 Application Services 列表里有 Associated Domains 一条，把它变为 Enabled 就可以了。

<!-- more -->

![link_001.jpeg](https://cdn.nlark.com/yuque/0/2021/jpeg/1028501/1609669244663-b9467ba4-7e19-4078-9780-562b685ed5b6.jpeg#align=left&display=inline&height=395&margin=%5Bobject%20Object%5D&name=link_001.jpeg&originHeight=790&originWidth=800&size=74236&status=done&style=none&width=400)

#### 2、配置 iOS App 工程

工程配置中相应功能：targets->Signing&Capabilites->Capability->Associated Domains，在其中的 Domains 中填入你想支持的域名，也必须必须以 applinks:为前缀。例如: applinks:www.baidu.com、applinks:*.baidu.com
![link_002.jpeg](https://cdn.nlark.com/yuque/0/2021/jpeg/1028501/1609669545217-db898db5-21e1-492c-8204-1f4551762aa8.jpeg#align=left&display=inline&height=274&margin=%5Bobject%20Object%5D&name=link_002.jpeg&originHeight=608&originWidth=1332&size=74891&status=done&style=none&width=600)

#### 3、创建、配置和上传 apple-app-association

##### 3.1 创建一个 apple-app-site-association 文件

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "9JA89QQLNQ.com.apple.wwdc",
        "paths": ["/wwdc/news/", "/videos/wwdc/2015/*"]
      },
      {
        "appID": "ABCD1234.com.apple.wwdc",
        "paths": ["*"]
      }
    ]
  }
}
```

##### 3.2 配置

- 不要追加.json 到 apple-app-site-association 文件名。
- appID：组成方式是 teamId.yourapp’s bundle identifier。如上面的 9JA89QQLNQ 就是 teamId。登陆开发者中心，在 Account -> Membership 里面可以找到 Team ID。
- paths：设定你的 app 支持的路径列表，只有这些指定的路径的链接，才能被 app 所处理。星号的写法代表了可识 别域名下所有链接。

##### 3.3 上传

- 上传指定文件:上传该文件到你的域名所对应的根目录或者.well-known 目录下，这是为了苹果能获取到你上传的文件。上传完后,自己先访问一下,看看是否能够获取到，当你在浏览器中输入这个文件链接后，应该是直接下载 apple-app-site-association 文件。

#### 4、验证 Universal link 生效

- 可以使用 iOS 自带的备忘录程序，输入链接，长按链接，如果弹出菜单中有”在‘xxx’中打开”，即表示配置生效。
- 或者将要测试的网址在 Safari 中打开，在出现的网页上方下滑，可以看到有在”xxx”应用中打开, 出现菜单

![](https://cdn.nlark.com/yuque/0/2021/webp/1028501/1609672004914-fa29a077-cc00-4cf4-90b9-62b18ee184a8.webp#align=left&display=inline&height=711&margin=%5Bobject%20Object%5D&originHeight=1182&originWidth=665&size=0&status=done&style=none&width=400)

#### 5、在`AppDelegate`里中实现代理方法，官方链接：[Handling Universal Links](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.apple.com%2Fdocumentation%2Fuikit%2Finter-process_communication%2Fallowing_apps_and_websites_to_link_to_your_content%2Fhandling_universal_links)

参考链接:

- [iOS Universal link 入门指南](https://www.jianshu.com/p/49bae73b8e72)
- [苹果文档](https://developer.apple.com/library/ios/documentation/General/Conceptual/AppSearch/UniversalLinks.html)
- [苹果检测链接有效性](https://search.developer.apple.com/appsearch-validation-tool/)
- [apple-app-site-association 文件](https://wx.51gonggui.com/apple-app-site-association)
