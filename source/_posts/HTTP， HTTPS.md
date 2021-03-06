---
title: HTTP， HTTPS
urlname: swd5kh
date: 2019-04-26 11:40:00 +0800
tags: [面试]
categories: [移动端]
---

### 一、HTTP 主要有这些不足，例举如下

- 通信使用明文（不加密），内容可能会被窃听
- 不验证通信方的身份，因此有可能遭遇伪装
- 无法验证报文的完整性，所以有可能已遭篡改

因此不论在任何时候，都应该将服务置于 HTTPS 上，因为它可以避免中间人攻击的问题，还采用了共享密钥加密和公开密钥加密两者并用的混合加密机制，在交换密钥环节使用公开密钥加密方式，之后的建立通信交换报文阶段则使用共享密钥加密的方式。

<!-- more -->

### 二、HTTPS 是身披 SSL 外壳的 HTTP

- HTTPS 并非是应用层的一种新协议。只是 HTTP 通信接口部分用 SSL（Secure Socket Layer）和 TLS（Transport Layer Security）协议代替而已。在采用 SSL 后，HTTP 就拥有了 HTTPS 的加密，证书，和完整性保护这些功能。

#### HTTPS 交互原理

简答说，HTTPS 就是 HTTP 协议加了一层 SSL 协议的加密处理，SSL 证书就是遵守 SSL 协议，由受信任的数字证书颁发机构 CA（如 GlobalSign，wosign），在验证服务器身份后颁发，这是需要花钱滴，签发后的证书作为公钥一般放在服务器的根目录下，便于客户端请求返回给客户端，私钥在服务器的内部中心保存，用于解密公钥。

HTTPS 客户端与服务器交互过程：

1、客户端发送请求，服务器返回公钥给客户端。
2、客户端生成对称加密秘钥，用公钥对其进行加密后，返回给服务器。
3、服务器收到后，利用私钥解开得到对称加密秘钥，保存。
4、之后的交互都使用对称加密后的数据进行交互。

### 三、HTTP/1 对比 HTTP/2

参考文章 [解读 HTTP/2 与 HTTP/3 的新特性](https://mp.weixin.qq.com/s/n8HBG9LuzQjOT__M4pxKwA)

HTTP/2 相比于 HTTP/1.1，可以说是大幅度提高了网页的性能，只需要升级到该协议就可以减少很多之前需要做的性能优化工作，当然兼容问题以及如何优雅降级应该是国内还不普遍使用的原因之一。

#### 一、HTTP/1.1 的缺陷

1.高延迟--带来页面加载速度的降低 2.无状态特性--带来的巨大 HTTP 头部 3.明文传输--带来的不安全性 4.不支持服务器推送消息

两个主要的缺点：安全不足和性能不高

#### 二、HTTP/2 新特性

1.二进制传输
2.Header 压缩 3.多路复用
4.Server Push 5.提高安全性
