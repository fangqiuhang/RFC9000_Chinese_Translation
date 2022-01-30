---
title: "13. 分包与可靠性"
anchor: "13_Packetization_and_Reliability"
weight: 1300
---

A sender sends one or more frames in a QUIC packet; see Section 12.4.

在一个QUIC数据包中，发送方发送一个或多个帧，详见[第12.4章]()。

A sender can minimize per-packet bandwidth and computational costs by including as many frames as possible in each QUIC packet. A sender MAY wait for a short period of time to collect multiple frames before sending a packet that is not maximally packed, to avoid sending out large numbers of small packets. An implementation MAY use knowledge about application sending behavior or heuristics to determine whether and for how long to wait. This waiting period is an implementation decision, and an implementation should be careful to delay conservatively, since any delay is likely to increase application-visible latency.

发送方可以通过在每个QUIC数据包中包含尽可能多的帧来最小化每个数据包消耗的带宽和计算资源。在发送一个未完全利用的数据包前，发送方{{< req_level MAY >}}短暂等待一会，多收集几个帧，以避免发送出大量的小数据包。QUIC实现{{< req_level MAY >}}使用有关应用的发送习惯的知识或启发式的方法来决定要不要等和等多久。这一等待时间的长短是一个由QUIC实现做的决策，同时实现应该小心地保守地进行等待，因为任何等待都有可能增加被应用观察到的延迟量。

Stream multiplexing is achieved by interleaving STREAM frames from multiple streams into one or more QUIC packets. A single QUIC packet can include multiple STREAM frames from one or more streams.

流的多路复用是通过将来自多个流的**流帧**交错至一个或多个QUIC数据包中来实现的。单个QUIC数据包可以包含来自一个或多个流的数个**流帧**。

One of the benefits of QUIC is avoidance of head-of-line blocking across multiple streams. When a packet loss occurs, only streams with data in that packet are blocked waiting for a retransmission to be received, while other streams can continue making progress. Note that when data from multiple streams is included in a single QUIC packet, loss of that packet blocks all those streams from making progress. Implementations are advised to include as few streams as necessary in outgoing packets without losing transmission efficiency to underfilled packets.

使用QUIC的益处之一是能避免复数流间的队头阻塞问题。当出现丢包时，只有那个数据包中的数据所属的流会被阻塞住，等待收到重传的数据，而其他流能够继续传输。注意，当单个QUIC数据包中包含来自多个流的数据时，丢失那个数据包将阻塞所有有关的流的传输。建议QUIC实现在发出去的数据包中包含尽可能少的流，来避免降低这些未完全利用的数据包的传输效率。
