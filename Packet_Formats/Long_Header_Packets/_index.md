---
title: "17.2 长包头数据包"
anchor: "17.2_Long_Header_Packets"
weight: 1720
rank: "h2"
---

Long Header Packet {
Header Form (1) = 1,
Fixed Bit (1) = 1,
Long Packet Type (2),
Type-Specific Bits (4),
Version (32),
Destination Connection ID Length (8),
Destination Connection ID (0..160),
Source Connection ID Length (8),
Source Connection ID (0..160),
Type-Specific Payload (..),
}
Figure 13: Long Header Packet Format

{{% block_ref
indx="Figure_13_Long_Header_Packet_Format"
title="图13：长包头数据包格式" %}}

```
长包头数据包 {
  包头形式 (1) = 1,
  固定比特位 (1) = 1,
  长数据包类型 (2),
  类型特定比特位 (4),
  版本 (32),
  目标连接ID长度 (8),
  目标连接ID (0..160),
  源连接ID长度 (8),
  源连接ID (0..160),
  类型特定载荷 (..),
}
```

{{% /block_ref %}}

Long headers are used for packets that are sent prior to the establishment of 1-RTT keys. Once 1-RTT keys are available, a sender switches to sending packets using the short header (Section 17.3). The long form allows for special packets -- such as the Version Negotiation packet -- to be represented in this uniform fixed-length packet format. Packets that use the long header contain the following fields:

长包头被用于在1-RTT密钥建立前发送的数据包。一旦1-RTT密钥可用，发送方就会改用短包头发送数据包（[第17.3章]()）。长类型包头允许特殊数据包——例如版本协商数据包——以这种统一的固定长度的数据包格式呈现。使用长包头的数据包包含以下字段：

Header Form:
The most significant bit (0x80) of byte 0 (the first byte) is set to 1 for long headers.

包头形式（Header Form）：

:   对于长包头，字节0（第一个字节）的最高有效位（`0x80`）被设置为1。

Fixed Bit:
The next bit (0x40) of byte 0 is set to 1, unless the packet is a Version Negotiation packet. Packets containing a zero value for this bit are not valid packets in this version and MUST be discarded. A value of 1 for this bit allows QUIC to coexist with other protocols; see [RFC7983].

固定比特位（Fixed Bit）：

:   字节0中的下一个比特位（`0x40`）被设置为1，除非该包是一个版本协商数据包。此比特位为0的数据包表示它不是当前版本的合法数据包且{{< req_level MUST >}}被丢弃。此比特位为1允许QUIC与其他协议共存，详见《[RFC7983]()》。

Long Packet Type:
The next two bits (those with a mask of 0x30) of byte 0 contain a packet type. Packet types are listed in Table 5.

长数据包类型（Long Packet Type）：

:   字节0中的后两个比特位（掩码为`0x30`的那两个）包含了数据包类型。[表格5](#Table_5_Long_Header_Packet_Types)罗列了数据包类型。

Type-Specific Bits:
The semantics of the lower four bits (those with a mask of 0x0f) of byte 0 are determined by the packet type.

类型特定比特位（Type-Specific Bits）：

:   字节0中最低的四个比特位（掩码为`0x0f`的那四个）的语义由数据包类型决定。

Version:
The QUIC Version is a 32-bit field that follows the first byte. This field indicates the version of QUIC that is in use and determines how the rest of the protocol fields are interpreted.

版本（Version）：

:   QUIC的版本是一个跟在第一个字节后的32比特长的字段。该字段表明了正在使用的QUIC版本并且决定了如何解释剩下的协议字段。

Destination Connection ID Length:
The byte following the version contains the length in bytes of the Destination Connection ID field that follows it. This length is encoded as an 8-bit unsigned integer. In QUIC version 1, this value MUST NOT exceed 20 bytes. Endpoints that receive a version 1 long header with a value larger than 20 MUST drop the packet. In order to properly form a Version Negotiation packet, servers SHOULD be able to read longer connection IDs from other QUIC versions.

目标连接ID长度（Destination Connection ID Length）：

:   跟在版本后面的那个字节包含了这个字节后方的目标连接ID字段的长度。这个长度以字节为单位，且被编码为一个8位无符号整型值。在QUIC版本1中，该值{{< req_level MUST_NOT >}}超过20字节。收到版本为1的长包头数据包且本字段的值超过20的终端必须丢弃它。为了正确构造一个版本协商数据包，服务器{{< req_level SHOULD >}}有能力从其他QUIC版本中读取更长的连接ID。

Destination Connection ID:
The Destination Connection ID field follows the Destination Connection ID Length field, which indicates the length of this field. Section 7.2 describes the use of this field in more detail.

目标连接ID（Destination Connection ID）：

:   目标连接ID字段跟在目标连接ID长度字段后面，后者指出了本字段的长度。[第7.2章]()更详细地描述了本字段的用法。

Source Connection ID Length:
The byte following the Destination Connection ID contains the length in bytes of the Source Connection ID field that follows it. This length is encoded as an 8-bit unsigned integer. In QUIC version 1, this value MUST NOT exceed 20 bytes. Endpoints that receive a version 1 long header with a value larger than 20 MUST drop the packet. In order to properly form a Version Negotiation packet, servers SHOULD be able to read longer connection IDs from other QUIC versions.

源连接ID长度（Source Connection ID Length）：

:   跟在目标连接ID后面的那个字节包含了这个字节后方的源连接ID字段的长度。这个长度以字节为单位，且被编码为一个8位无符号整型值。在QUIC版本1中，该值{{< req_level MUST_NOT >}}超过20字节。收到版本为1的长包头数据包且本字段的值超过20的终端必须丢弃它。为了正确构造一个版本协商数据包，服务器{{< req_level SHOULD >}}有能力从其他QUIC版本中读取更长的连接ID。

Source Connection ID:
The Source Connection ID field follows the Source Connection ID Length field, which indicates the length of this field. Section 7.2 describes the use of this field in more detail.

源连接ID（Source Connection ID）：

:   源连接ID字段跟在源连接ID长度字段后面，后者指出了本字段的长度。[第7.2章]()更详细地描述了本字段的用法。

Type-Specific Payload:
The remainder of the packet, if any, is type specific.

类型特定载荷（Type-Specific Payload）：

:   数据包的剩余部分是类型特定的，如果有的话。

In this version of QUIC, the following packet types with the long header are defined:

在此版本的QUIC中，定义了以下长包头数据包的类型：

Table 5: Long Header Packet Types
Type	Name	Section
0x00	Initial	Section 17.2.2
0x01	0-RTT	Section 17.2.3
0x02	Handshake	Section 17.2.4
0x03	Retry	Section 17.2.5

{{% block_ref
indx="Table_5_Long_Header_Packet_Types"
title="表格5：长包头数据包类型" %}}

| 类型   | 名称    | 章节           |
|:-----|:------|:-------------|
| 0x00 | 初始    | [第17.2.2章]() |
| 0x01 | 0-RTT | [第17.2.3章]() |
| 0x02 | 握手    | [第17.2.4章]() |
| 0x03 | 重试    | [第17.2.5章]() |

{{% /block_ref %}}

The header form bit, Destination and Source Connection ID lengths, Destination and Source Connection ID fields, and Version fields of a long header packet are version independent. The other fields in the first byte are version specific. See [QUIC-INVARIANTS] for details on how packets from different versions of QUIC are interpreted.

一个长包头数据包的包头形式比特位、目标和源连接ID长度、目标和源连接ID，以及版本字段是版本无关的。第一个字节的其他字段是版本特定的。有关怎样解释不同QUIC版本的数据包的细节，见《[QUIC不变量]()》。

The interpretation of the fields and the payload are specific to a version and packet type. While type-specific semantics for this version are described in the following sections, several long header packets in this version of QUIC contain these additional fields:

第一个字节的其他字段和载荷的解释方法取决于版本和数据包类型。接下来的章节会描述当前版本下，各种数据包类型对它们的特定语义。在这个版本的QUIC中，一些长包头数据包均包含了这些额外字段：

Reserved Bits:
Two bits (those with a mask of 0x0c) of byte 0 are reserved across multiple packet types. These bits are protected using header protection; see Section 5.4 of [QUIC-TLS]. The value included prior to protection MUST be set to 0. An endpoint MUST treat receipt of a packet that has a non-zero value for these bits after removing both packet and header protection as a connection error of type PROTOCOL_VIOLATION. Discarding such a packet after only removing header protection can expose the endpoint to attacks; see Section 9.5 of [QUIC-TLS].

保留比特位（Reserved Bits）：

:   字节0的某两个比特位（掩码为`0x0c`的那两个）在多种数据包类型中都被保留使用。这些比特位被头部保护所保护，详见《[QUIC-TLS]()》的[第5.4章]()。在进行保护前，这两个比特位的值{{< req_level MUST >}}被设置为0。若在移除数据包保护和头部保护之后发现这些位被设置为非零值，则接收到该数据包的终端{{< req_level MUST >}}将该情况视作一个类型为`PROTOCOL_VIOLATION`的连接错误。仅在移除头部保护后就丢弃这样的数据包会使终端暴露于攻击之下，详见《[QUIC-TLS]()》的[第9.5章]()。

Packet Number Length:
In packet types that contain a Packet Number field, the least significant two bits (those with a mask of 0x03) of byte 0 contain the length of the Packet Number field, encoded as an unsigned two-bit integer that is one less than the length of the Packet Number field in bytes. That is, the length of the Packet Number field is the value of this field plus one. These bits are protected using header protection; see Section 5.4 of [QUIC-TLS].

数据包号长度（Packet Number Length）：

:   在包含数据包号字段的数据包类型中，字节0最低的两个有效位（掩码为`0x03`的那两个）包含数据包号字段的长度。该长度被编码为一个2位无符号整型值，这个值比数据包号字段的字节长度小1。也就是说，数据包号字段的长度等于这个字段的值加1。这些比特位被头部保护所保护，详见《[QUIC-TLS]()》的[第5.4章]()。

Length:
This is the length of the remainder of the packet (that is, the Packet Number and Payload fields) in bytes, encoded as a variable-length integer (Section 16).

长度（Length）：

:   这是数据包剩余部分（也就是数据包号字段和载荷字段）的字节长度，被编码为一个可变长度整型值。

Packet Number:
This field is 1 to 4 bytes long. The packet number is protected using header protection; see Section 5.4 of [QUIC-TLS]. The length of the Packet Number field is encoded in the Packet Number Length bits of byte 0; see above.

数据包号（Packet Number）：

:   这个字段的长度是1至4字节。数据包号被头部保护所保护，详见《[QUIC-TLS]()》的[第5.4章]()。数据包号字段的长度被编码进字节0的数据包号长度比特位，见上文。

Packet Payload:
This is the payload of the packet -- containing a sequence of frames -- that is protected using packet protection.

数据包载荷（Packet Payload）：

:   这是数据包的载荷——包含一系列帧——它们被数据包保护所保护。
