---
title: "21.1.3. 连接迁移"
anchor: "21.1.3_Connection_Migration"
weight: 210130
rank: "h3"
---

Connection migration (Section 9) provides endpoints with the ability to transition between IP addresses and ports on multiple paths, using one path at a time for transmission and receipt of non-probing frames. Path validation (Section 8.2) establishes that a peer is both willing and able to receive packets sent on a particular path. This helps reduce the effects of address spoofing by limiting the number of packets sent to a spoofed address.

连接迁移（详见[第9章]()）为终端提供了在数条路径的IP地址与端口间转移的能力，在同一时间终端只能在一条路径上发送和接收非探测帧。路径验证（详见[第8.2章]()）能证实对端有意愿且有能力接收在某条路径上发送的数据包。通过限制发送至假地址的数据包数量，这有助于减少地址伪造的影响。

This section describes the intended security properties of connection migration under various types of DoS attacks.

本节描述了在受到不同类型的拒绝服务攻击的情况下，连接迁移应该具有的安全性。
