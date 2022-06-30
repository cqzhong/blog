---
title: 安卓手机Charles抓包，返回结果乱码问题
urlname: dv774i
date: 2021-03-09 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

1. 打开 charles 工具->Tools->rewrite->Enable rewrite,勾选.
2. 在 rewrite 界面下方的 sets 中进行添加设置项(sets->add):

<!-- more -->

![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1646970465439-08abd160-d9f1-479e-a594-c4aa14bdb6f8.png#clientId=u71fbb87c-c6c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=262&id=u18c5de9c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=524&originWidth=642&originalType=binary∶=1&rotation=0&showTitle=false&size=90221&status=done&style=none&taskId=u2eca7b8f-5a65-465d-a39a-608cd5505b4&title=&width=321)
注意事项：location 不需要设置
![image.png](https://cdn.nlark.com/yuque/0/2022/png/1028501/1646970606957-2e245ebb-8098-4c2f-8cc5-f9d4dd878441.png#clientId=u71fbb87c-c6c2-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=259&id=u4574b9cd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=517&originWidth=537&originalType=binary∶=1&rotation=0&showTitle=false&size=96400&status=done&style=none&taskId=u80738d69-78be-45fb-ada4-3750abdec21&title=&width=268.5)
