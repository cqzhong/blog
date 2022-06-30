---
title: TCP状态
urlname: sgxd99
date: 2019-07-30 12:46:21 +0800
tags: [tcp]
categories: [其它]
---

#### 建立连接协议（三次握手）

1. 客户端发送一个带 SYN 标志的 TCP 报文到服务器。这是三次握手过程中的报文 1。

<!-- more -->

2. 服务器端回应客户端的，这是三次握手中的第 2 个报文，这个报文同时带 ACK 标志和 SYN 标志。因此它表示对刚才客户端 SYN 报文的回应；同时又标志 SYN 给客户端，询问客户端是否准备好进行数据通讯。
3. 客户必须再次回应服务段一个 ACK 报文，这是报文段 3。
   ![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602938087427-0fce91a5-6270-4d2d-9387-614474815508.jpeg#align=left&display=inline&height=576&margin=%5Bobject%20Object%5D&originHeight=576&originWidth=839&size=0&status=done&style=none&width=839)

#### 连接终止协议（四次释放）

由于 TCP 连接是全双工的，因此每个方向都必须单独进行关闭。这原则是当一方完成它的数据发送任务后就能发送一个 FIN 来终止这个方向的连接。收到一个 FIN 只意味着这一方向上没有数据流动，一个 TCP 连接在收到一个 FIN 后仍能发送数据。首先进行关闭的一方将执行主动关闭，而另一方执行被动关闭。

1. TCP 客户端发送一个 FIN，用来关闭客户到服务器的数据传送（报文段 4）
1. 服务器收到这个 FIN，它发回一个 ACK，确认序号为收到的序号加 1（报文段 5）。和 SYN 一样，一个 FIN 将占用一个序号。
1. 服务器关闭客户端的连接，发送一个 FIN 给客户端（报文段 6）。
1. 客户段发回 ACK 报文确认，并将确认序号设置为收到序号加 1（报文段 7）。

---

**TIME_WAIT**：通信双方建立 TCP 连接后，主动关闭连接的一方就会进入 TIME_WAIT 状态。客户端主动关闭连接时，会发送最后一个 ack 后，然后会进入 TIME_WAIT 状态，再停留 2 个 MSL 时间(后有 MSL 的解释)，进入 CLOSED 状态。

下图是以客户端主动关闭连接为例，说明这一过程的。

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602938087461-4c025dc4-5da4-4226-bca3-4ca8c1cd9878.jpeg#align=left&display=inline&height=594&margin=%5Bobject%20Object%5D&originHeight=594&originWidth=883&size=0&status=done&style=none&width=883)

**CLOSE_WAIT**: 这种状态的含义其实是表示在等待关闭。怎么理解呢？当对方 close 一个 SOCKET 后发送 FIN 报文给自己，你系统毫无疑问地会回应一个 ACK 报文给对方，此时则进入到 CLOSE_WAIT 状态。接下来呢，实际上你真正需要考虑的事情是察看你是否还有数据发送给对方，如果没有的话，那么你也就可以 close 这个 SOCKET，发送 FIN 报文给对方，也即关闭连接。所以你在 CLOSE_WAIT 状态下，需要完成的事情是等待你去关闭连接。

**MSL**：就是 maximum segment lifetime(最大分节生命期），这是一个 IP 数据包能在互联网上生存的最长时间，超过这个时间 IP 数据包将在网络中消失 。

**TIME_WAIT**状态维持时间：TIME_WAIT 状态维持时间是两个 MSL 时间长度，也就是在 1-4 分钟。Windows 操作系统就是 4 分钟。

A 与 B 建立了正常连接后，从未相互发过数据，这个时候 B 突然机器重启，问 A 此时处于 TCP 什么状态？如何消除服务器程序中的这个状态？
答：A 处于 ESTABLISHED 状态，B 突然重启，A 感知不到 B 的状态，可以通过心跳来探测对方状态。
