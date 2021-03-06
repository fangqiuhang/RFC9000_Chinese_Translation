---
title: "9.6 服务器的首选地址"
anchor: "9.6_Server's_Preferred_Address"
weight: 960
rank: "h2"
---

QUIC允许服务器在一个IP地址上接受连接，然后在握手之后立刻尝试将这些连接转移到一个更优先使用的地址。这在客户端起初连接到由多个服务器共享的地址但想优先使用一个单播地址来确保连接稳定性时尤其有用。本节描述了这种将连接迁移到首选服务器地址的协议。

在这份文档制定的QUIC规范版本中，在连接途中迁移到新服务器地址是不受支持的。如果客户端在还没有发起一次前往新服务器地址的迁移的情况下就从那个地址接收到了数据包，那么客户端{{< req_level SHOULD >}}丢弃这些数据包。
