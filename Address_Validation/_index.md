---
title: "8. 地址验证"
anchor: "8_Address_Validation"
weight: 800
rank: "h1"
---

Address validation ensures that an endpoint cannot be used for a traffic amplification attack. In such an attack, a packet is sent to a server with spoofed source address information that identifies a victim. If a server generates more or larger packets in response to that packet, the attacker can use the server to send more data toward the victim than it would be able to send on its own.

地址验证确保终端不会被用于流量放大攻击。在这种攻击中，一个填写了指向受害者的假源地址信息的数据包被发送给服务器。如果服务器在响应那个数据包时生成了更多或更大的数据包，攻击者就能使用这台服务器向受害者发送比攻击者自己能发送的还要多的数据。

The primary defense against amplification attacks is verifying that a peer is able to receive packets at the transport address that it claims. Therefore, after receiving packets from an address that is not yet validated, an endpoint MUST limit the amount of data it sends to the unvalidated address to three times the amount of data received from that address. This limit on the size of responses is known as the anti-amplification limit.

针对放大攻击的主要防御手段是验证对端有能力在它声称的传输地址接收数据包。因此，在从一个未经验证的地址接收到数据包后，终端{{< req_level MUST >}}限制它向未经验证的地址发送的数据量为它从那个地址接收到的数据量的三倍。这个对于响应尺寸的上限被称为抗放大上限。

Address validation is performed both during connection establishment (see Section 8.1) and during connection migration (see Section 8.2).

在连接建立期间（详见[第8.1章]()）和连接迁移期间（详见[第8.2章]()）都会进行地址验证。
