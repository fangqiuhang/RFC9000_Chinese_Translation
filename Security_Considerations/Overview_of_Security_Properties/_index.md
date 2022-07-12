---
title: "21.1. 安全性概述"
anchor: "21.1_Overview_of_Security_Properties"
weight: 210100
rank: "h2"
---

完整的对于QUIC安全性的分析超出了本文档的范畴。本节对QUIC应有的安全性给出了一份非正规的描述，用它来辅助QUIC实现方对协议进行分析。

QUIC使用了在《[SEC-CONS]()》中描述的威胁模型假设，并针对此模型中的多种攻击方式提供了保护手段。

为此，我们将攻击分类为被动攻击和主动攻击。被动的攻击者具有从网络中读取数据包的能力，而主动的攻击者还具有向网络写入数据包的能力。然而，被动攻击中的攻击者可以具有引发路由变化的能力或对传递数据包的路径作出其他修改的能力。

攻击者还被归类为在路径上的攻击者或不在路径上的攻击者。在路径上的攻击者可以读取或修改它所观测到的任意数据包，还可以移除任意数据包从而使得数据包无法到达它的目的地，而不在路径上的攻击者能观测数据包但无法阻止原始数据包抵达它的目的地。此外，这两种类型的攻击者都能发送任意数据包。这样的定义与《[SEC-CONS]()》的[第3.5章]()中的不同，区别是不在路径上的攻击者可以观测到数据包。

握手的安全性、受保护数据包的安全性和连接迁移的安全性会被分别考量。