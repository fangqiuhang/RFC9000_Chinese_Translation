---
title: "14.4 发送QUIC的PMTU探测包"
anchor: "14.4_Sending_QUIC_PMTU_Probes"
weight: 1440
---

PMTU probes are ack-eliciting packets.

PMTU探测包是ACK触发包。

Endpoints could limit the content of PMTU probes to PING and PADDING frames, since packets that are larger than the current maximum datagram size are more likely to be dropped by the network. Loss of a QUIC packet that is carried in a PMTU probe is therefore not a reliable indication of congestion and SHOULD NOT trigger a congestion control reaction; see Item 7 in Section 3 of [DPLPMTUD]. However, PMTU probes consume congestion window, which could delay subsequent transmission by an application.

终端可以限制PMTU探测包的内容为**Ping帧**和**填充帧**，因为比当前最大数据报尺寸更大的数据包更有可能被网络所丢弃。因此丢失一个在PMTU探测包中携带的QUIC数据包并不是个可靠的拥塞依据且{{< req_level SHOULD_NOT >}}触发拥塞控制的反应；详见《[DPLPMTUD]()》的[第3章]()的第7款。然而PMTU探测包会消耗拥塞窗口，这会延迟应用程序的后续传输。
