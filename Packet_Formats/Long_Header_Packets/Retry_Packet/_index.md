---
title: "17.2.5 重试数据包"
anchor: "17.2.5_Retry_Packet"
weight: 1725
rank: "h2"
---

As shown in Figure 18, a Retry packet uses a long packet header with a type value of 0x03. It carries an address validation token created by the server. It is used by a server that wishes to perform a retry; see Section 8.1.

如[图18](#Figure_18_Retry_Packet)所示，重试数据包使用类型值为`0x03`的长包头。它携带者由服务器创建的一个地址验证令牌。想要进行重试的服务器会使用它，详见[第8.1章]()。

Retry Packet {
Header Form (1) = 1,
Fixed Bit (1) = 1,
Long Packet Type (2) = 3,
Unused (4),
Version (32),
Destination Connection ID Length (8),
Destination Connection ID (0..160),
Source Connection ID Length (8),
Source Connection ID (0..160),
Retry Token (..),
Retry Integrity Tag (128),
}
Figure 18: Retry Packet

{{% block_ref
indx="Figure_18_Retry_Packet"
title="图18：重试数据包" %}}

```
重试数据包 {
  包头形式 (1) = 1,
  固定比特位 (1) = 1,
  长数据包类型 (2) = 3,
  未使用 (4),
  版本 (32),
  目标连接ID长度 (8),
  目标连接ID (0..160),
  源连接ID长度 (8),
  源连接ID (0..160),
  重试令牌 (..),
  重试完整性标签 (128),
}
```

{{% /block_ref %}}

A Retry packet does not contain any protected fields. The value in the Unused field is set to an arbitrary value by the server; a client MUST ignore these bits. In addition to the fields from the long header, it contains these additional fields:

重试数据包不包含任何受保护的字段。未使用字段中的值被服务器设置为任意值；客户端{{< req_level MUST >}}忽略这些比特位。除了来自长包头的字段之外，它还包含以下额外字段：

Retry Token:
An opaque token that the server can use to validate the client's address.

重试令牌（Retry Token）：

:   服务器用来验证客户端的地址的不透明令牌。

Retry Integrity Tag:
Defined in Section 5.8 ("Retry Packet Integrity") of [QUIC-TLS].

重试完整性标签（Retry Integrity Tag）：

:   在《[QUIC-TLS]()》的[第5.8章]()（《重试完整性标签》）中定义。
