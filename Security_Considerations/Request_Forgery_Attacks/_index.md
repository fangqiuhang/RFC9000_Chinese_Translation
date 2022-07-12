---
title: "21.5. 请求伪造攻击"
anchor: "21.5_Request_Forgery_Attacks"
weight: 210500
rank: "h2"
---

在请求伪造攻击中，终端使得对端向受害者发起一次由终端控制的请求。请求伪造攻击旨在使得攻击者能够操作对端所具有的功能，而一般情况下攻击者无法访问到这些功能。对于一份网络协议来说，正因为对端在网络中所处的特殊位置，请求伪造攻击经常被用于利用在受害者与对端间非公开的鉴权认证。

要使请求伪造生效，攻击者需要具有影响对端所发送的数据包以及这些数据包的目的地的能力。如果攻击者可以使用它所控制的载荷来针对某个易受攻击的服务进行攻击，那么该服务就可能进行一些看上去是由攻击者的对端发起实际却是由攻击者控制的操作。

举例来说，互联网上的跨站请求伪造（详见《[CSRF]()》）漏洞会使得客户端发起携带着认证Cookie（详见《[COOKIE]()》）的请求，使得某个网站能够访问到本应只有另一个网站才能访问的信息和操作。

由于QUIC运行在UDP之上，主要关心的攻击形式是攻击者能够操纵其对端将UDP数据报发向的地址并且能够控制数据包载荷中的部分原始内容。由于QUIC终端发送的许多数据都是受保护的，所以这种攻击包含着对密文的控制。如果攻击者能够使得对端向某个会基于数据报内容进行相应操作的主机发送UDP数据报，那么攻击就会成功。

本节讨论了可能使用QUIC来进行伪造请求攻击的各种方式。

本节还讨论了QUIC终端可以实现但是效果有限的对抗措施。这些抵御方式可以被某个QUIC实现或部署方式单方面实施，而不需要请求伪造攻击的潜在目标采取行动。然而，如果基于UDP的服务没有恰当地对请求进行鉴权认证，那么单靠这些对抗措施会是不够充分的。

因为在[第21.5.4章]()中描述的迁移攻击相当强力并且针对它没有适当的对抗措施，所以QUIC实现应该假定攻击者可以利用它们来生成任意UDP载荷并将其发送至任意目标。QUIC服务器{{< req_level SHOULD_NOT >}}被部署到没有启用传入流量过滤（详见《[BCP38]()》）和存在安全性未经充分加固的UDP终端的网络中。

虽然通常不太可能确保客户端没有和易受攻击的终端处在一起，但是本QUIC版本并不允许服务器发起迁移，从而阻止了对客户端发起虚假迁移攻击。任何将来的允许服务器迁移的扩展{{< req_level MUST >}}为伪造攻击定义对抗措施。