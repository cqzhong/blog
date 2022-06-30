---
title: uni-app获取当前位置
urlname: ghmqfg
date: 2020-04-28 20:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

#### uniapp 获取当前城市：

官方 api：uni.getLocation()
获取当前的地理位置、速度。 在微信小程序中，当用户离开应用后，此接口无法调用，除非申请后台持续定位权限；当用户点击“显示在聊天顶部”时，此接口可继续调用。
例

<!-- more -->

```javascript
uni.getLocation({
  type: "gcj02", //wgs84 type使用高德时候要用 gcj02
  success: function (res) {
    console.log("当前位置的经度：" + res.longitude);
    console.log("当前位置的纬度：" + res.latitude);
  },
});
```

成功回调函数中会返回当前经纬度等信息
如果想获取当前省市区信息，可以设置参数   geocode  为 true，该属性仅 APP 端支持
例：

```javascript
uni.getLocation({
  type: "wgs84",
  geocode: true,
  success: function (res) {
    console.log(res.address);
  },
});

/*
"address": {
    "country": "中国",
    "province": "上海市",
    "city": "上海市",
    "district": "浦东新区",
    "street": "晨晖路",
    "streetNum": "88号",
    "poiName": "金蝶软件园(晨晖路)",
    "cityCode": "021"
  }
*/
```

APP 端还可使用  plus.geolocation 获取中文地址
​

```javascript
plus.geolocation.getCurrentPosition(
  function (position) {
    console.log(position.addresses);
  },
  function (e) {
    console.log(e.message);
  },
  { geocode: true }
);
```

其他端可使用地图开放平台获取 SDK：
[https://ask.dcloud.net.cn/article/35070](https://ask.dcloud.net.cn/article/35070)

#### 使用地图 API 获取中文地理位置代码

```javascript
/**
 * 获取用户的定位信息
 * @return: Promise
 */
export const getLocationInfo = () => {
  return new Promise((resolve, reject) => {
    uni.getLocation({
      type: "gcj02", // 'wgs84'
      geocode: true,
      success: (res) => {
        getAddress(res.longitude, res.latitude)
          .then((address) => {
            resolve(address);
          })
          .catch(() => {
            reject("");
          });
      },
      fail: () => {
        reject("");
      },
    });
  });
};
```

```javascript
/**
 * 根据经纬度，逆地理编码出用户的中文地址
 * @param {number} longitude 经度
 * @param {number} latitude  纬度
 * @return: Promise
 */
export const getAddress = (longitude, latitude) => {
  return new Promise((resolve, reject) => {
    uni.request({
      url: `https://restapi.amap.com/v3/geocode/regeo`,
      method: "GET",
      data: {
        key: "f7ddefce1ff0128529450ada5ce1a64c",
        location: longitude + "," + latitude,
        radius: "100",
        extensions: "base",
        batch: false,
        roadlevel: 0,
      },
      header: {
        "content-type": "application/x-www-form-urlencoded",
      },
      success: (res) => {
        if (res.data.status !== "1") {
          resolve("");
          return;
        }
        const regeocode = res.data.regeocode;
        if (
          regeocode.formatted_address &&
          regeocode.formatted_address.length < 25
        ) {
          resolve(regeocode.formatted_address);
          return;
        }

        let info = "";
        const addressComponent = regeocode.addressComponent;
        info = `${info}${addressComponent.province}${addressComponent.district}${addressComponent.township}`;
        if (addressComponent.streetNumber) {
          const streetNumber = addressComponent.streetNumber;
          info = `${info}${streetNumber.street}${streetNumber.number}`;
        }
        resolve(info);
      },
      fail: () => {
        reject("");
      },
    });
  });
};
```
