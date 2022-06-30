---
title: VSCode workspace 地址修改
urlname: uds2mf
date: 2019-01-11 14:00:00 +0800
tags: [VSCode]
categories: [其它]
---

- 新建一个.code-workspace 格式文件。然后写入以下代码

```json
{
  "folders": [
    {
      "path": "../wx"
    }
  ],
  "settings": {}
}
```

<!-- more -->

path 指的是地址、指定项目路径
setings 当前 workspace 的配置

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602938194659-bb6239fa-c6ce-403b-92e0-957efbf24c1b.png#align=left&display=inline&height=964&margin=%5Bobject%20Object%5D&originHeight=964&originWidth=2524&size=0&status=done&style=none&width=2524)

双击 service.code-workspace 文件使用 Visual Studio Code 打开项目。
