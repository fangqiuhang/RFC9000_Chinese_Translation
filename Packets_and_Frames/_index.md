---
title: "12. 数据包与帧"
anchor: "12_Packets_and_Frames"
weight: 1200
rank: "h1"
---

QUIC endpoints communicate by exchanging packets. Packets have confidentiality and integrity protection; see Section 12.1. Packets are carried in UDP datagrams; see Section 12.2.

QUIC终端用交换数据包的方式交流。数据包具有可信度和完整性保护，详见[第12.1章]()。数据包是由UDP数据报携带的，详见[第12.2章]()。

This version of QUIC uses the long packet header during connection establishment; see Section 17.2. Packets with the long header are Initial (Section 17.2.2), 0-RTT (Section 17.2.3), Handshake (Section 17.2.4), and Retry (Section 17.2.5). Version negotiation uses a version-independent packet with a long header; see Section 17.2.1.

本QUIC版本在连接建立期间使用长包头，详见[第17.2章]()。具有长包头的数据包分别是初始数据包（详见[第17.2.2章]()）、0-RTT数据包（[第17.2.3章]()）、握手数据包（[第17.2.4章]()）和重试数据包（[第17.2.5章]()）。版本协商过程使用与版本无关的具有长包头的数据包，详见[第17.2.1章]()。

Packets with the short header are designed for minimal overhead and are used after a connection is established and 1-RTT keys are available; see Section 17.3.

具有短包头的数据包是为了最小化开销而设计的，它们被用于连接建立之后且1-RTT密钥可用时，详见[第17.3章]()。
