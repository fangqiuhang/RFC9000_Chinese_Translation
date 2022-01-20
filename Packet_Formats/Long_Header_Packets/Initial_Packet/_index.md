---
title: "17.2.2 初始数据包"
anchor: "17.2.2_Initial_Packet"
weight: 1722
rank: "h2"
---

An Initial packet uses long headers with a type value of 0x00. It carries the first CRYPTO frames sent by the client and server to perform key exchange, and it carries ACK frames in either direction.

初始数据包使用类型值为`0x00`的长包头。它携带着发送自客户端和服务器的最初的加密帧以进行密钥交换，同时它还携带着任意方向的ACK帧。

Initial Packet {
Header Form (1) = 1,
Fixed Bit (1) = 1,
Long Packet Type (2) = 0,
Reserved Bits (2),
Packet Number Length (2),
Version (32),
Destination Connection ID Length (8),
Destination Connection ID (0..160),
Source Connection ID Length (8),
Source Connection ID (0..160),
Token Length (i),
Token (..),
Length (i),
Packet Number (8..32),
Packet Payload (8..),
}
Figure 15: Initial Packet

{{% block_ref
indx="Figure_15_Initial_Packet"
title="图15：初始数据包" %}}

```
初始数据包 {
  包头形式 (1) = 1,
  固定比特位 (1) = 1,
  长数据包类型 (2) = 0,
  保留比特位 (2),
  数据包号长度 (2),
  版本 (32),
  目标连接ID长度 (8),
  目标连接ID (0..160),
  源连接ID长度 (8),
  源连接ID (0..160),
  令牌长度 (i),
  令牌 (..),
  长度 (i),
  数据包号 (8..32),
  数据包载荷 (8..),
}
```

{{% /block_ref %}}

The Initial packet contains a long header as well as the Length and Packet Number fields; see Section 17.2. The first byte contains the Reserved and Packet Number Length bits; see also Section 17.2. Between the Source Connection ID and Length fields, there are two additional fields specific to the Initial packet.

初始数据包包含长包头、长度字段和数据包号字段，详见[第17.2章]()。首个字节包含保留比特位和数据包号长度比特位，详见[第17.2章]()。在源连接ID和长度字段间，是初始数据包特有的两个额外字段。

Token Length:
A variable-length integer specifying the length of the Token field, in bytes. This value is 0 if no token is present. Initial packets sent by the server MUST set the Token Length field to 0; clients that receive an Initial packet with a non-zero Token Length field MUST either discard the packet or generate a connection error of type PROTOCOL_VIOLATION.

令牌长度（Token Length）：

:   一个可变长度整型值，它指定了令牌字段以字节为单位的长度。如果不存在令牌，则该值为0。由服务器发送的初始数据包{{< req_level MUST >}}将令牌长度字段设置为0；收到一个令牌长度字段为非零值的客户端{{< req_level MUST >}}要么丢弃数据包，要么产生一个类型为`PROTOCOL_VIOLATION`的连接错误。

Token:
The value of the token that was previously provided in a Retry packet or NEW_TOKEN frame; see Section 8.1.

令牌（Token）：

:   先前由重试数据包或新令牌帧提供的令牌的值，详见[第8.1章]()。

In order to prevent tampering by version-unaware middleboxes, Initial packets are protected with connection- and version-specific keys (Initial keys) as described in [QUIC-TLS]. This protection does not provide confidentiality or integrity against attackers that can observe packets, but it does prevent attackers that cannot observe packets from spoofing Initial packets.

为了防止不感知版本的中间件的篡改，初始数据包由连接特定密钥和版本特定密钥（初始密钥）所保护，如[QUIC-TLS]()中描述的那样。这种保护在面对有能力观测数据包的攻击者时并不能提供可信度或保证完整性，但是它能防止无法观测到数据包的攻击者伪造初始数据包。

The client and server use the Initial packet type for any packet that contains an initial cryptographic handshake message. This includes all cases where a new packet containing the initial cryptographic message needs to be created, such as the packets sent after receiving a Retry packet; see Section 17.2.5.

客户端和服务器使用初始数据包这一类型，用于任何包含着初始加密握手消息的数据包。这包含所有需要新创建包含初始加密消息的数据包的情况，例如接收到一个重试数据包后被发送的的那个数据包，详见[第17.2.5章]()。

A server sends its first Initial packet in response to a client Initial. A server MAY send multiple Initial packets. The cryptographic key exchange could require multiple round trips or retransmissions of this data.

服务器发送它的首个初始数据包，作为客户端初始包的响应。服务器{{< req_level MAY >}}发送多个初始数据包。加密密钥交换可能需要多轮往返或数据重传。

The payload of an Initial packet includes a CRYPTO frame (or frames) containing a cryptographic handshake message, ACK frames, or both. PING, PADDING, and CONNECTION_CLOSE frames of type 0x1c are also permitted. An endpoint that receives an Initial packet containing other frames can either discard the packet as spurious or treat it as a connection error.

一个初始数据包的载荷包含一个（或多个）加密帧，每个加密帧包含一条加密握手消息，或数个ACK帧，或两者兼具。Ping帧、填充帧和类型为`0x1c`的连接关闭帧也是被允许的。终端收到包含其他帧的初始数据包后可以将该数据包作为虚假数据包丢弃或将其视为连接错误。

The first packet sent by a client always includes a CRYPTO frame that contains the start or all of the first cryptographic handshake message. The first CRYPTO frame sent always begins at an offset of 0; see Section 7.

由客户端发出的首个数据包总是包含一个加密帧，这个加密帧包含首个加密握手消息的起始或所有部分。首个加密帧总是在偏移为0处起始，详见[第7章]()。

Note that if the server sends a TLS HelloRetryRequest (see Section 4.7 of [QUIC-TLS]), the client will send another series of Initial packets. These Initial packets will continue the cryptographic handshake and will contain CRYPTO frames starting at an offset matching the size of the CRYPTO frames sent in the first flight of Initial packets.

注意如果服务器发送了一个TLS HelloRetryRequest（详见《[QUIC-TLS]()》的[第4.7章]()），客户端将另外发送一系列初始数据包。这些初始数据包将继续进行加密握手，并且其包含的加密帧的起始位置将与第一轮初始数据包中的加密帧尺寸相匹配。