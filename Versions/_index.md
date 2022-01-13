---
title: "15. 版本"
anchor: "15_Versions"
weight: 1500
---

QUIC versions are identified using a 32-bit unsigned number.

QUIC版本使用一个32位无符号整型值来标识。

The version 0x00000000 is reserved to represent version negotiation. This version of the specification is identified by the number 0x00000001.

版本`0x00000000`被保留使用，以表示版本协商。本规范的版本使用数值`0x00000001`来标识。

Other versions of QUIC might have different properties from this version. The properties of QUIC that are guaranteed to be consistent across all versions of the protocol are described in [QUIC-INVARIANTS].

其他QUIC版本的各种属性可能与本版本不同。《[QUIC不变量]()》中描述了保证在协议不同版本间保持一致的一些QUIC属性。

Version 0x00000001 of QUIC uses TLS as a cryptographic handshake protocol, as described in [QUIC-TLS].

如《[QUIC-TLS]()》所述，版本为`0x00000001`的QUIC使用TLS作为加密握手协议。

Versions with the most significant 16 bits of the version number cleared are reserved for use in future IETF consensus documents.

版本号最高的16个有效位被清除的版本被保留使用，用于将来的IETF共识文档。

Versions that follow the pattern 0x?a?a?a?a are reserved for use in forcing version negotiation to be exercised -- that is, any version number where the low four bits of all bytes is 1010 (in binary). A client or server MAY advertise support for any of these reserved versions.

遵循`0x?a?a?a?a`模式的版本——也就是所有字节的四个低位都是`1010`（二进制）的那些版本号——被保留使用，用于强制执行版本协商。客户端或服务器{{< req_level MAY >}}宣称自己支持这些保留版本中的任意版本。

Reserved version numbers will never represent a real protocol; a client MAY use one of these version numbers with the expectation that the server will initiate version negotiation; a server MAY advertise support for one of these versions and can expect that clients ignore the value.

保留版本号绝不会表示一个真实的协议；客户端{{< req_level MAY >}}使用这些版本号中的一个并期望服务器会发起版本协商；服务器{{< req_level MAY >}}宣称自己支持这些版本中的一个，并期望客户端忽略这个值。