---
title: "14.3 数据报分包层PMTU发现"
anchor: "14.3_Datagram_Packetization_Layer_PMTU_Discovery"
weight: 1430
---

DPLPMTUD [DPLPMTUD] relies on tracking loss or acknowledgment of QUIC packets that are carried in PMTU probes. PMTU probes for DPLPMTUD that use the PADDING frame implement "Probing using padding data", as defined in Section 4.1 of [DPLPMTUD].

DPLPMTUD（详见《[DPLPMTUD]()》）依赖对PMTU探测包中携带的QUIC数据包的丢失或确认进行追踪。DPLPMTUD中使用了填充帧的PMTU探测包实现了如《[DPLPMTUD]()》的[第4.1章]()所述的“使用填充数据进行探测”。

Endpoints SHOULD set the initial value of BASE_PLPMTU (Section 5.1 of [DPLPMTUD]) to be consistent with QUIC's smallest allowed maximum datagram size. The MIN_PLPMTU is the same as the BASE_PLPMTU.

终端{{< req_level SHOULD >}}将`BASE_PLPMTU`（详见《[DPLPMTUD]()》的[第5.1章]()）的初始值设置为与QUIC允许的最大数据报尺寸中的最小值一致的值。`MIN_PLPMTU`就是`BASE_PLPMTU`。

QUIC endpoints implementing DPLPMTUD maintain a DPLPMTUD Maximum Packet Size (MPS) (Section 4.4 of [DPLPMTUD]) for each combination of local and remote IP addresses. This corresponds to the maximum datagram size.

实现DPLPMTUD的QUIC终端为每一对本地IP地址和远程IP地址的组合维护一个DPLPMTUD最大数据包尺寸（MPS）（详见《[DPLPMTUD]()》的[第4.4章]()）。它对应着最大数据报尺寸。