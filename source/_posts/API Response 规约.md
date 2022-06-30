---
title: API Response 规约
urlname: inrnx0
date: 2019-11-03 18:00:00 +0800
tags: [开发规范]
categories: [其它]
---

# API Response 规约

## 接口响应报文规约

- 响应类型统一为 `application/json`。
- 报文最外层统一为 `JSONObject-最外层使用result包裹`，如果要返回数组，也需要返回一个对象，在对象中包含一个数组。
- POST、PUT 操作后要返回创建/修改后的数据。
- 响应 4xx、5xx 的报文内统一为一个 `error` 对象，包括 `type`、`reason`、`message`。
  - type: HTTP 状态码类型。
  - reason: 4xx 时为英文的错误提示（国际化扩展），5xx 时为服务器异常信息。
  - message: 友好的错误提示，用于前端展示。
- 如业务逻辑比较复杂 则根据项目定义返回体内的 业务 code 与 message - 作为 result 的子集存在

<!-- more -->

## 报文示例

- 200

```json
- 分页-列表
{
    "result": {
        "total": 5,
        "per_page": 25,
        "current_page": 1,
        "last_page": 1,
        "data": [
            {
                "commodity_uuid": "28389b2104bbd10b527698049eaf68f8",
                "commodity_name": "强力定眩片",
                "commodity_icon": "Fo9nlr-AErDg5m2luKgk9ZPCxX2E",
                "sales_volume": 3,
                "commodity_amount": "1.00",
                "commodity_stock": 97
            },
            {
                "commodity_uuid": "6175afdc9113bf7ee69d82bdcbb839d1",
                "commodity_name": "安神补脑液",
                "commodity_icon": "FmZQYsLpVRNc4iNdEBidIzkbx0El",
                "sales_volume": 10,
                "commodity_amount": "0.01",
                "commodity_stock": 60
            },
            {
                "commodity_uuid": "55f104f1d3ea2479e9f2db7dfd9d81d3",
                "commodity_name": "感冒灵颗粒",
                "commodity_icon": "Flz7JxCxeo3RPAhdED_SL9XFVwZV",
                "sales_volume": 17,
                "commodity_amount": "0.10",
                "commodity_stock": 12
            },
            {
                "commodity_uuid": "4",
                "commodity_name": "双黄连口服液",
                "commodity_icon": "Flt5rdJSjjLHqJuroGKO6z1z0mND",
                "sales_volume": 102,
                "commodity_amount": "1.00",
                "commodity_stock": 407
            },
            {
                "commodity_uuid": "2",
                "commodity_name": "七叶神安片",
                "commodity_icon": "FkNvmy52gapSPOANZTJrTTP6C3CP",
                "sales_volume": 104,
                "commodity_amount": "0.10",
                "commodity_stock": 418
            }
        ]
    }
}

查看详情：
{
    "result": {
        "uuid": "28389b2104bbd10b527698049eaf68f8",
        "commodity_name": "强力定眩片",
        "commodity_icon": "Fo9nlr-AErDg5m2luKgk9ZPCxX2E",
        "sales_volume": 3,
        "commodity_amount": "1.00",
        "commodity_stock": 97,
        "status": "1",
        "is_delete": "1",
        "create_time": "2020-04-18 01:08:52",
        "update_time": "2020-07-04 12:14:41",
        "commodity_carousel": [
            "FvG8hXEuXXoxvUUJnLmAH6GFfAk1",
            "Fmdh1yusxMaC3HpKynIcFf7jNWhM",
            "Fo9nlr-AErDg5m2luKgk9ZPCxX2E"
        ],
        "disease_species_uuid": "5d964f4e7702c85486a0332c5c5274a1",
        "sort": 5,
        "origin_commodity_amount": "2"
    }
}

```

- 400

```json
{
  "error": {
    "type": "Bad Request",
    "reason": "parameter 'xxx' missing.",
    "message": "请求体不完整"
  }
}
```

- 401

```json
{
  "error": {
    "type": "Unauthorized",
    "reason": "parameter 'X-Access-Token' invalid",
    "message": "鉴权失败"
  }
}
```

- 403 - 用作业务异常返回

```json
{
  "error": {
    "type": "Forbidden",
    "reason": "account was ban",
    "message": "账号已被冻结"
  }
}
```

- 404

```json
{
  "error": {
    "type": "Not Found",
    "reason": "data does not exist",
    "message": "数据不存在"
  }
}
```

- 409

```json
{
  "error": {
    "type": "Conflict",
    "reason": "data already exists",
    "message": "xxx已存在"
  }
}
```

- 412

```json
{
  "error": {
    "type": "Precondition Failed",
    "reason": "parameter 'X-Access-Token' missing",
    "message": "鉴权失败"
  }
}
```

- 423

```json
{
  "error": {
    "type": "Locked",
    "reason": "resource was locked",
    "message": "xxx已被锁定"
  }
}
```

- 500

```json
{
  "error": {
    "type": "Internal Server Error",
    "reason": "java.lang.NullPointerException at cn.dankal.xx.xxx() in line xxx",
    "message": "网络繁忙，请稍后再试"
  }
}
```

# HTTP Status Code

接口的增、删、改、查，对应 `POST`、 `DELETE`、 `PUT`、 `GET` 这四种方法，不使用其它方法例如 `PATCH` 等。

接口鉴权，在请求头中添加 `X-Access-Token` 这个 key。

## 全局状态码

- `200` 操作成功。
- `400` 请求体不完整。
- `401` 鉴权失败，headers 有 `X-Access-Token` 这个 key，但 value 失效。
- `403` 请求被拒绝，因为某种原因服务器拒绝了该访问，例如登录的账号被屏蔽、参加某种限时活动但已过期，或者请求达到上限等。
- `412` 鉴权失败，headers 没有 `X-Access-Token` 这个 key。
- `500` 服务器内部错误。

## GET

GET 方法用于数据查询的接口，一般的响应情况有：

- `404` 数据不存在。

## POST

POST 方法用于创建数据的接口，一般的响应情况有：

- `409` 重复创建，导致冲突。

## PUT

PUT 方法用于修改数据的接口，一般的响应情况有：

- `404` 需要更新的数据不存在。

## DELETE

DELETE 方法用于删除数据的接口，一般的响应情况有：

- `404` 需要删除的数据不存在，或者已被删除，包括软删除。
