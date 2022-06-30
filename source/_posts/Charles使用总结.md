---
title: Charles使用总结
urlname: ot1dis
date: 2020-11-03 18:00:00 +0800
tags: [Charles]
categories: [其它]
---

### 一、下载安装配置

1、下载 [Charles](https://www.charlesproxy.com)
2、安装 dmg、使用设备安装证书

- Help-> SSL Proxying
- 电脑安装证书，钥匙串- > 选择证书->显示简介->始终信任
- 手机安装 ：和电脑处于统一局域网下，手动设置代理-> chls.pro/ssl 安装证书 ->   设置 ->通用->安装描述文件-> 关于本机-> 证书信任设置->打开

<!-- more -->

![WeChatcfab5916ca6c283addc3a8e534d69f36.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604398315830-9614fc35-c298-4769-99e3-b07bff5406e0.png#height=533&id=DwKLg&margin=%5Bobject%20Object%5D&name=WeChatcfab5916ca6c283addc3a8e534d69f36.png&originHeight=533&originWidth=400&originalType=binary∶=1&size=1268947&status=done&style=none&width=400)

3、SSL Proxying Settings 配置(Host 和 Port 配置为通配)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604398196713-f860d09c-2368-414f-ada9-ecd8fb31d6f2.png#height=417&id=oJx90&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1796&originWidth=2584&originalType=binary∶=1&size=613155&status=done&style=none&width=600)

4、在 Charles 的菜单栏上选择“Proxy”->“Proxy Settings”，填入代理端口 8888，并且勾上”Enable transparent HTTP proxying” 就完成了在 Charles 上的设置。

5、如果未出现允许访问网络弹窗的话，需要自己手动设置下
![000001.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1636165279848-32192da1-d8db-483a-aa14-e1369ee50b38.png#clientId=u9dae24e3-b4bf-4&from=drop&id=u38a01f87&margin=%5Bobject%20Object%5D&name=000001.png&originHeight=406&originWidth=1276&originalType=binary∶=1&size=86047&status=done&style=none&taskId=u8291ddcb-b7c0-41f4-88b4-049a99726e0)
![0002.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1636165286189-cc1bfbfb-55cd-495a-8081-c27fccc3335f.png#clientId=u9dae24e3-b4bf-4&from=drop&height=623&id=ue2367363&margin=%5Bobject%20Object%5D&name=0002.png&originHeight=910&originWidth=1096&originalType=binary∶=1&size=147800&status=done&style=none&taskId=u545242ec-6d8c-4890-8666-64b8b41019e&width=750)
​

6、检查网络偏好设置 -> 高级 -> 代理
![截屏2021-11-06 上午10.40.24.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1636166431341-915e18da-318b-4d1e-aa29-842d92449968.png#clientId=u9dae24e3-b4bf-4&from=drop&id=u92581683&margin=%5Bobject%20Object%5D&name=%E6%88%AA%E5%B1%8F2021-11-06%20%E4%B8%8A%E5%8D%8810.40.24.png&originHeight=1042&originWidth=1322&originalType=binary∶=1&size=362721&status=done&style=none&taskId=ub4ead43f-71f5-456c-aa9c-cb20bdd323b)

### **二、Charles 模拟弱网**

1、点击顶部导航 Proxy
2、选择 Throttle Seeings...
![throttle.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604373012710-56d7300a-9abc-494b-8014-fd1a5fbc62d2.png#height=270&id=xY528&margin=%5Bobject%20Object%5D&name=throttle.png&originHeight=373&originWidth=553&originalType=binary∶=1&size=335672&status=done&style=none&width=400)

3、勾选 Enable Throttling
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604373217296-595452e4-0ca1-4b75-8d45-bb7a3efcdb8c.png#height=297&id=uWJ8A&margin=%5Bobject%20Object%5D&name=image.png&originHeight=593&originWidth=535&originalType=binary∶=1&size=154351&status=done&style=none&width=267.5)

### **三、Charles 修改请求返回数据**

1、找到要修改的请求, 并打上断点
![breakpoint.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604373683724-a53cf1b8-9569-4823-932e-4ff16a0a49b6.png#height=725&id=uqTxr&margin=%5Bobject%20Object%5D&name=breakpoint.png&originHeight=725&originWidth=300&originalType=binary∶=1&size=191335&status=done&style=none&width=300)

2、再次发起要修改的请求(进入断点修改请求参数)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604374067045-7b017410-c77b-4155-a9ef-a2a93707cf4a.png#height=361&id=VHo05&margin=%5Bobject%20Object%5D&name=image.png&originHeight=721&originWidth=797&originalType=binary∶=1&size=278198&status=done&style=none&width=398.5)
3、修改服务器返回内容
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1028501/1604374279204-d051730e-2733-4fde-8415-51cd5830d793.png#height=361&id=g6qSZ&margin=%5Bobject%20Object%5D&name=image.png&originHeight=722&originWidth=797&originalType=binary∶=1&size=240004&status=done&style=none&width=398.5)
