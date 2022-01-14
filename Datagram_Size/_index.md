---
title: "14. 数据报尺寸"
anchor: "14_Datagram_Size"
weight: 1400
---

A UDP datagram can include one or more QUIC packets. The datagram size refers to the total UDP payload size of a single UDP datagram carrying QUIC packets. The datagram size includes one or more QUIC packet headers and protected payloads, but not the UDP or IP headers.

一个UDP数据报可以包含一个或多个QUIC数据包。数据报尺寸指的是携带QUIC数据包的单个UDP数据报的UDP载荷总大小。数据报尺寸包含一个或多个QUIC数据包头部和受保护的载荷，但不包含UDP或IP头部。

The maximum datagram size is defined as the largest size of UDP payload that can be sent across a network path using a single UDP datagram. QUIC MUST NOT be used if the network path cannot support a maximum datagram size of at least 1200 bytes.

最大数据报尺寸被定义为能够经由一条网络路径使用单个UDP数据报发送的最大的UDP载荷大小。如果某条网络路径支持的最大数据报尺寸不能达到1200字节，则{{< req_level MUST_NOT >}}使用QUIC。

QUIC assumes a minimum IP packet size of at least 1280 bytes. This is the IPv6 minimum size [IPv6] and is also supported by most modern IPv4 networks. Assuming the minimum IP header size of 40 bytes for IPv6 and 20 bytes for IPv4 and a UDP header size of 8 bytes, this results in a maximum datagram size of 1232 bytes for IPv6 and 1252 bytes for IPv4. Thus, modern IPv4 and all IPv6 network paths are expected to be able to support QUIC.

QUIC假设一个最小的IP数据包尺寸为至少1280字节。这是IPv6数据包的最小尺寸（见《[IPv6]()》），同时被大多数现代IPv4网络所支持。假设使用最小的IP头部，IPv6时为40字节而IPv4时为20字节，以及大小为8字节的UDP头部，则可得到IPv6中的最大数据报尺寸为1232字节，而IPv4中的为1252字节。因此，现代的IPv4网络路径和所有的IPv6网络路径都应该有能力支持QUIC。

Note: This requirement to support a UDP payload of 1200 bytes limits the space available for IPv6 extension headers to 32 bytes or IPv4 options to 52 bytes if the path only supports the IPv6 minimum MTU of 1280 bytes. This affects Initial packets and path validation.

> 注意：如果某条路径仅支持1280字节，即IPv6的最小MTU，那么要求支持1200字节的UDP载荷会限制IPv6扩展头部和IPv4选项的可用空间，前者被限制至32字节，后者被限制至52字节。

Any maximum datagram size larger than 1200 bytes can be discovered using Path Maximum Transmission Unit Discovery (PMTUD) (see Section 14.2.1) or Datagram Packetization Layer PMTU Discovery (DPLPMTUD) (see Section 14.3).

任何超过1200字节的最大数据报尺寸都可以使用路径最大传输单元发现（PMTUD）（见[第14.2.1章]()）或数据报分包层PMTU发现（DPLPMTUD）（见[第14.3章]()）。

Enforcement of the max_udp_payload_size transport parameter (Section 18.2) might act as an additional limit on the maximum datagram size. A sender can avoid exceeding this limit, once the value is known. However, prior to learning the value of the transport parameter, endpoints risk datagrams being lost if they send datagrams larger than the smallest allowed maximum datagram size of 1200 bytes.

强制执行传输参数`max_udp_payload_size`（[第18.2章]()）可能成为一个额外的对最大数据报尺寸的限制。一旦这个值已知，发送者就可以避免超过这个限制。然而在获知这个传输参数的值之前，如果终端发送了比1200字节，这个在允许的最大数据报尺寸中的最小值，更大的数据报，那么终端就要承担数据报丢失的风险。

UDP datagrams MUST NOT be fragmented at the IP layer. In IPv4 [IPv4], the Don't Fragment (DF) bit MUST be set if possible, to prevent fragmentation on the path.

UDP数据报{{< req_level MUST_NOT >}}在IP层被分段。在IPv4（《[IPv4]()》）中{{< req_level MUST >}}尽可能设置不要分段（DF）比特位来防止路径上的分段。

QUIC sometimes requires datagrams to be no smaller than a certain size; see Section 8.1 as an example. However, the size of a datagram is not authenticated. That is, if an endpoint receives a datagram of a certain size, it cannot know that the sender sent the datagram at the same size. Therefore, an endpoint MUST NOT close a connection when it receives a datagram that does not meet size constraints; the endpoint MAY discard such datagrams.

QUIC有时需要数据报不小于一个特定尺寸，比如[第8.1章]()中的例子。然而，数据报的尺寸不会被认证。也就是说，如果终端接收到了某个尺寸的数据报，它不能确定发送者发送的是不是就是那个尺寸的数据报。因此，当终端接收到一个没有满足尺寸限制的数据报时，它{{< req_level MUST_NOT >}}关闭那条连接；终端{{< req_level MAY >}}丢弃这样的数据报。
