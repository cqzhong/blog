---
title: 微信小程序request封装
urlname: gft1vo
date: 2019-10-21 21:33:00 +0800
tags: [微信小程序]
categories: [前端]
---

在微信小程序开发中，通过接口和后台进行交互，我们需要在每个页面的 js 文件中写：wx.request。 当接口很多时候，会在很多页面使用 request，显然我们需要对它进行一下封装。

<!-- more -->

#### 首先新建一个 request.js 文件

```javascript
const request = ({ url = "", param = {}, ...other } = {}) => {
  return new Promise((resolve, reject) => {
    wx.request({
      url: getUrl(url),
      data: param,
      ...other,
      header: {
        "content-type": "application/x-www-form-urlencoded",
      },
      success: (res) => {
        // console.log(JSON.stringify(res, null, ' '))
        if (res.data.code == 200) {
          resolve(res.data.data);
        } else {
          reject(res.data);
        }
      },
      fail: (err) => {
        reject(err);
      },
    });
  });
};

const getUrl = (url) => {
  if (url.indexOf("://") == -1) {
    url = "这里是你的域名" + url;
  }
  return url;
};

//RequestMethod

const get = (url, param = {}) => {
  return request({
    url,
    param,
    method: "get",
  });
};

const post = (url, param = {}) => {
  return request({
    url,
    param,
    method: "post",
  });
};

module.exports = {
  get,
  post,
};
```

#### 使用方法

- eg：搜索接口，新建一个 search.js 文件。

```javascript
const request = require("./request.js");
export const hotWord = (params) =>
  request.get("https://api09ecx7.zhuishushenqi.com/book/hot-word", params);
```

- 在需要调用搜索接口页面 js 文件中

```javascript
import { hotWord } from "../api/search";

let paraM = {};
hotWord(paraM)
  .then((data) => {})
  .catch((res) => {});
```
