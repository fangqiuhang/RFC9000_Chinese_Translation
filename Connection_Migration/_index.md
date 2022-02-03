---
title: "9. 连接迁移"
anchor: "9_Connection_Migration"
weight: 900
rank: "h1"
---

The use of a connection ID allows connections to survive changes to endpoint addresses (IP address and port), such as those caused by an endpoint migrating to a new network. This section describes the process by which an endpoint migrates to a new address.

连接ID的使用使得连接能经受住终端地址（IP地址和端口）的变化，例如由终端迁移至新的网络环境而引起的变化。本章描述了终端迁移至新地址的过程。

The design of QUIC relies on endpoints retaining a stable address for the duration of the handshake. An endpoint MUST NOT initiate connection migration before the handshake is confirmed, as defined in Section 4.1.2 of [QUIC-TLS].

QUIC的设计要求终端在握手过程中保持稳定的地址。在握手确认前，终端{{< req_level MUST_NOT >}}发起连接迁移，详见《[QUIC-TLS]()》的[第4.1.2章]()。

If the peer sent the disable_active_migration transport parameter, an endpoint also MUST NOT send packets (including probing packets; see Section 9.1) from a different local address to the address the peer used during the handshake, unless the endpoint has acted on a preferred_address transport parameter from the peer. If the peer violates this requirement, the endpoint MUST either drop the incoming packets on that path without generating a Stateless Reset or proceed with path validation and allow the peer to migrate. Generating a Stateless Reset or closing the connection would allow third parties in the network to cause connections to close by spoofing or otherwise manipulating observed traffic.

如果对端发送了传输参数`disable_active_migration`（禁止活跃迁移），终端在握手过程中还{{< req_level MUST_NOT >}}从不同于当前地址的本地地址发送数据包（包括探测数据包；详见[第9.1章]()），除非终端已经对来自对端的传输参数`preferred_address`（首选地址）作出反应。如果对端违反了这项要求，终端{{< req_level MUST >}}要么丢弃来自那条路径的传入数据包而不创建无状态重置，要么用路径验证来应对并且允许对端迁移。创建无状态重置或关闭连接将允许网络中的第三方用伪造或操作被观测的流量的方式使得连接被关闭。

Not all changes of peer address are intentional, or active, migrations. The peer could experience NAT rebinding: a change of address due to a middlebox, usually a NAT, allocating a new outgoing port or even a new outgoing IP address for a flow. An endpoint MUST perform path validation (Section 8.2) if it detects any change to a peer's address, unless it has previously validated that address.

不是所有的对端地址变化都是有意的，或活跃的，迁移。对端可能经历NAT重绑定：一种因中间设备，通常是NAT，为数据流分配新的传出端口或甚至新的出口IP地址而引起的地址变化。如果终端检测到任何与对端地址相关的变化，那么它{{< req_level MUST >}}进行地址验证（详见[第8.2章]()），除非它先前已经验证过那个地址。

When an endpoint has no validated path on which to send packets, it MAY discard connection state. An endpoint capable of connection migration MAY wait for a new path to become available before discarding connection state.

当终端没有经过验证的地址来发送数据包时，它{{< req_level MAY >}}丢弃连接的状态数据。在丢弃连接的状态数据前，有能力进行连接迁移的终端{{< req_level MAY >}}等待新的可用地址。

This document limits migration of connections to new client addresses, except as described in Section 9.6. Clients are responsible for initiating all migrations. Servers do not send non-probing packets (see Section 9.1) toward a client address until they see a non-probing packet from that address. If a client receives packets from an unknown server address, the client MUST discard these packets.

本文档限制连接迁移为仅客户端可以发起，除非是在[第9.6章]()中描述的情况。客户端负责发起所有迁移。服务器在它们从某客户端地址接收到非探测数据包（详见[第9.1章]()）前，不会向那个地址发送非探测数据包。如果客户端从一个未知的服务器地址接收到了数据包，客户端{{< req_level MUST >}}丢弃这些数据包。
