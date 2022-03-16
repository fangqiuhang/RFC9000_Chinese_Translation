---
title: "21.5. 请求伪造攻击"
anchor: "21.5_Request_Forgery_Attacks"
weight: 210500
rank: "h2"
---

A request forgery attack occurs where an endpoint causes its peer to issue a request towards a victim, with the request controlled by the endpoint. Request forgery attacks aim to provide an attacker with access to capabilities of its peer that might otherwise be unavailable to the attacker. For a networking protocol, a request forgery attack is often used to exploit any implicit authorization conferred on the peer by the victim due to the peer's location in the network.

在请求伪造攻击中，终端使得对端向受害者发起一次由终端控制的请求。请求伪造攻击旨在使得攻击者能够操作对端所具有的功能，而一般情况下攻击者无法访问到这些功能。对于网络协议来说，请求伪造攻击经常被用于破解在受害者与对端间非公开的鉴权认证，利用的是对端在网络中所处的特殊位置。

For request forgery to be effective, an attacker needs to be able to influence what packets the peer sends and where these packets are sent. If an attacker can target a vulnerable service with a controlled payload, that service might perform actions that are attributed to the attacker's peer but are decided by the attacker.

要使请求伪造生效，攻击者需要具有影响对端所发送的数据包以及这些数据包的目的地的能力。如果攻击者可以使用它所控制的载荷来针对某个易受攻击的服务进行攻击，那么该服务就可能进行一些看上去是由攻击者的对端发起实际却是由攻击者控制的操作。

For example, cross-site request forgery [CSRF] exploits on the Web cause a client to issue requests that include authorization cookies [COOKIE], allowing one site access to information and actions that are intended to be restricted to a different site.

举例来说，互联网上的跨站请求伪造（详见《[CSRF]()》）漏洞会使得客户端发起携带着认证Cookie（详见《[COOKIE]()》）的请求，使得某个网站能够访问到本应只有另一个网站才能访问的信息和操作。

As QUIC runs over UDP, the primary attack modality of concern is one where an attacker can select the address to which its peer sends UDP datagrams and can control some of the unprotected content of those packets. As much of the data sent by QUIC endpoints is protected, this includes control over ciphertext. An attack is successful if an attacker can cause a peer to send a UDP datagram to a host that will perform some action based on content in the datagram.

由于QUIC运行在UDP之上，主要关心的攻击形式是攻击者能够操纵其对端将UDP数据报发向的地址并且能够控制数据包载荷中的部分原始内容。由于QUIC终端发送的许多数据都是受保护的，这种攻击包含这对密文的控制。如果攻击者能够使得对端向某个会基于数据报内容进行相应操作的主机发送UDP数据报，那么攻击就会成功。

This section discusses ways in which QUIC might be used for request forgery attacks.

本节讨论了可能使用QUIC来进行伪造请求攻击的各种方式。

This section also describes limited countermeasures that can be implemented by QUIC endpoints. These mitigations can be employed unilaterally by a QUIC implementation or deployment, without potential targets for request forgery attacks taking action. However, these countermeasures could be insufficient if UDP-based services do not properly authorize requests.

本节还讨论了QUIC终端可以实现但是效果有限的对抗措施。这些抵御方式可以被某个QUIC实现或部署方式单方面实施，而不需要请求伪造攻击的潜在目标采取行动。然而，如果基于UDP的服务没有恰当地对请求进行鉴权认证，那么单靠这些对抗措施会是不够充分的。

Because the migration attack described in Section 21.5.4 is quite powerful and does not have adequate countermeasures, QUIC server implementations should assume that attackers can cause them to generate arbitrary UDP payloads to arbitrary destinations. QUIC servers SHOULD NOT be deployed in networks that do not deploy ingress filtering [BCP38] and also have inadequately secured UDP endpoints.

因为在[第21.5.4章]()中描述的迁移攻击相当强力并且针对它没有适当的对抗措施，所以QUIC实现应该假定攻击者可以利用它们来生成任意UDP载荷并将其发送至任意目标。QUIC服务器{{< req_level SHOULD_NOT >}}被部署到没有启用传入流量过滤（详见《[BCP38]()》）和存在安全性未经充分加固的UDP终端的网络中。

Although it is not generally possible to ensure that clients are not co-located with vulnerable endpoints, this version of QUIC does not allow servers to migrate, thus preventing spoofed migration attacks on clients. Any future extension that allows server migration MUST also define countermeasures for forgery attacks.

虽然通常不太可能确保客户端没有和易受攻击的终端处在一起，但是本QUIC版本并不允许服务器发起迁移，从而阻止了对客户端发起虚假迁移攻击。任何将来的允许服务器迁移的扩展{{< req_level MUST >}}为伪造攻击定义对抗措施。
