---
title: uni-app 导航栏问题
urlname: tougul
date: 2020-04-16 10:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

#### page.json 文件内的配置

```json
{
  "path": "pages/repair/myOrder/newRepair",
  "style": {
    "navigationBarTitleText": "新增维修项",
    "app-plus": {
      "titleNView": {
        "buttons": [
          {
            "float": "left",
            "fontSize": "30upx",
            "text": "上一项",
            "width": "auto"
          },
          {
            "float": "right",
            "fontSize": "30upx",
            "text": "返回主页面",
            "width": "auto"
          }
        ]
      },
      "scrollIndicator": "none"
    }
  }
}
```

<!-- more -->

#### 页面内修改导航栏

```javascript
var webView = this.$mp.page.$getAppWebview();
const canedit = this.$utils.permissionHander("WB_VEHICE_EDITINFO");
/*
 * 1、安卓设置text 为空子串时候无效果。
 * 2、隐藏按钮 width 一定要设置为0
 */
if (canedit) {
  webView.setTitleNViewButtonStyle(0, {
    text: "编辑",
  });
} else {
  webView.setTitleNViewButtonStyle(0, {
    text: "",
    width: 0,
  });
}
```

#### 导航栏设置左右按钮

```javascript
/*
 * 1、一定要设置text length不能为0
 * 2、如果text的length为0一定设置width为0
 */
var webView = this.$mp.page.$getAppWebview();
const canedit = this.$utils.permissionHander("WB_VEHICE_EDITINFO");
if (canedit) {
  webView.setTitleNViewButtonStyle(0, {
    text: "编辑",
  });
  // this.loadModels()
  this.loadTeams();
} else {
  webView.setTitleNViewButtonStyle(0, {
    text: "",
    width: 0,
  });
}
```
