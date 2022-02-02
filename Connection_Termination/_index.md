---
title: "10. 连接终止"
anchor: "10_Connection_Termination"
weight: 1000
rank: "h1"
---

An established QUIC connection can be terminated in one of three ways:

共有三种方法可以终止一个已建立的QUIC连接：

* idle timeout (Section 10.1)

* 空闲超时（详见[第10.1章]()）

* immediate close (Section 10.2)

* 立即关闭（详见[第10.2章]()）

* stateless reset (Section 10.3)

* 无状态重置（详见[第10.3章]()）

An endpoint MAY discard connection state if it does not have a validated path on which it can send packets; see Section 8.2.

如果终端没有可以用来发送数据包的网络路径，那么它{{< req_level MAY >}}丢弃连接状态，详见[第8.2章]()。
