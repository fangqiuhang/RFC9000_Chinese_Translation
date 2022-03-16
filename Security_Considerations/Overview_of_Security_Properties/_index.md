---
title: "21.1. 安全性概述"
anchor: "21.1_Overview_of_Security_Properties"
weight: 210100
rank: "h2"
---

A complete security analysis of QUIC is outside the scope of this document. This section provides an informal description of the desired security properties as an aid to implementers and to help guide protocol analysis.

完整的对于QUIC的安全性分析超出了本文档的范畴。本节对QUIC被期望的安全性给出了一份非正规的描述，将它作为QUIC实现方的辅助手段，并且用它帮助对协议进行分析。

QUIC assumes the threat model described in [SEC-CONS] and provides protections against many of the attacks that arise from that model.

QUIC使用了描述于《[SEC-CONS]()》中的威胁模型假设，并针对来自模型中的许多攻击提供保护手段。

For this purpose, attacks are divided into passive and active attacks. Passive attackers have the ability to read packets from the network, while active attackers also have the ability to write packets into the network. However, a passive attack could involve an attacker with the ability to cause a routing change or other modification in the path taken by packets that comprise a connection.

为此，攻击被分类为被动攻击和主动攻击。被动的攻击者具有从网络中读取数据包的能力，而主动的攻击者还具有向网络写入数据包的能力。然而，被动攻击中的攻击者可以具有引发路由变化的能力或对传递数据包的路径作出其他修改的能力。

Attackers are additionally categorized as either on-path attackers or off-path attackers. An on-path attacker can read, modify, or remove any packet it observes such that the packet no longer reaches its destination, while an off-path attacker observes the packets but cannot prevent the original packet from reaching its intended destination. Both types of attackers can also transmit arbitrary packets. This definition differs from that of Section 3.5 of [SEC-CONS] in that an off-path attacker is able to observe packets.

攻击者还会被归类到在路径上的攻击者或不在路径上的攻击者。在路径上的攻击者可以读取或修改它所观测到的任意数据包，还可以移除任意数据包从而使得数据包无法到达它的目的地，而不在路径上的攻击者能观测数据包但无法阻止原始数据包抵达它的目的地。此外，这两种类型的攻击者都能发送任意数据包。这样的定义与《[SEC-CONS]()》的[第3.5章]()中的不同，区别是不在路径上的攻击者可以观测到数据包。

Properties of the handshake, protected packets, and connection migration are considered separately.

握手的安全性、受保护数据包的安全性和连接迁移的安全性会被分别考量。
