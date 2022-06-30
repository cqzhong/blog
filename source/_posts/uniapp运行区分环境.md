---
title: uniapp运行区分环境
urlname: so3h7l
date: 2020-04-24 20:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

#### 1 page.json

```json
{
  "path": "pages/mine/mine",
  "style": {
    "navigationBarTitleText": "我的",
    "mp-weixin": {
      "navigationStyle": "custom"
    },
    "mp-alipay": {
      "transparentTitle": "always",
      "allowsBounceVertical": "NO"
    }
  }
}
```

#### 2、 template 内

```html
<template>
  <view>
    <!-- #ifdef MP-ALIPAY -->
    <view
      :class="{ 'uni-collapse-cell__title-arrow-active': isOpen, 'uni-collapse-cell--animation': showAnimation === true }"
      class="uni-collapse-cell__title-arrow"
    >
      <uni-icons color="#bbb" size="20" type="arrowdown" />
    </view>
    <!-- #endif -->
    <!-- #ifndef MP-ALIPAY -->
    <uni-icons
      :class="{ 'uni-collapse-cell__title-arrow-active': isOpen, 'uni-collapse-cell--animation': showAnimation === true }"
      class="uni-collapse-cell__title-arrow"
      color="#bbb"
      size="20"
      type="arrowdown"
    />
    <!-- #endif -->
  </view>
</template>
```

<!-- more -->

#### script 内区分

```vue
// #ifdef APP-PLUS ...判断内容，当运行环境为 app时候 // #endif // #ifndef
APP-PLUS|| MP-WEIXIN || MP-ALIPAY || H5 ...判断内容, 当运行环境为
非App、微信小程序、支付宝小程序、h5时候 // #endif
```

#### style

```css
.uni-swipe_button-group {
  /* #ifndef APP-VUE|| MP-WEIXIN||H5 */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  z-index: 0;
  /* #endif */
  /* #ifndef APP-NVUE */
  display: flex;
  flex-shrink: 0;
  /* #endif */
  flex-direction: row;
}
```

### 推荐阅读

- [条件编译](https://uniapp.dcloud.io/platform?id=pagesjson-%E7%9A%84%E6%9D%A1%E4%BB%B6%E7%BC%96%E8%AF%91)
