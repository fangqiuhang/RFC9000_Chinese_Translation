---
title: "13.4.2 ECN验证"
anchor: "13.4.2_ECN_Validation"
weight: 1342
rank: "h3"
---

It is possible for faulty network devices to corrupt or erroneously drop packets that carry a non-zero ECN codepoint. To ensure connectivity in the presence of such devices, an endpoint validates the ECN counts for each network path and disables the use of ECN on that path if errors are detected.

故障的网络设备可能损坏或错误地丢弃携带非零ECN码点的数据包。为了在有这类设备存在的前提下确保连接，终端为每条网络路径验证ECN计数，并当检测到错误时在那条路径上禁用ECN。

To perform ECN validation for a new path:

要为一条新路径进行ECN验证：

* The endpoint sets an ECT(0) codepoint in the IP header of early outgoing packets sent on a new path to the peer [RFC8311].

* 对于在一条新路径上发向对端的出站数据包，终端在早期的几个数据包的IP头部设置一个`ECT(0)`码点。

* The endpoint monitors whether all packets sent with an ECT codepoint are eventually deemed lost (Section 6 of [QUIC-RECOVERY]), indicating that ECN validation has failed.

* 终端观察是否所有具有`ECT`码点的数据包最终都被视为丢包（详见《[QUIC恢复]()》的[第6章]()），若是则表明ECN验证失败了。

If an endpoint has cause to expect that IP packets with an ECT codepoint might be dropped by a faulty network element, the endpoint could set an ECT codepoint for only the first ten outgoing packets on a path, or for a period of three PTOs (see Section 6.2 of [QUIC-RECOVERY]). If all packets marked with non-zero ECN codepoints are subsequently lost, it can disable marking on the assumption that the marking caused the loss.

如果终端怀疑一个故障的网络设备正在丢弃具有`ECT`码点的IP数据包，那么它可以为路径上仅前十个出站数据包设置`ECT`码点，或以每三个PTO（详见《[QUIC恢复]()》的[第6.2章]()）一次的间隔设置。如果所有标记为非零ECN码点的数据包都接连遭遇丢包，那么它就可以基于标记行为会引发丢包的假设来禁用标记。

An endpoint thus attempts to use ECN and validates this for each new connection, when switching to a server's preferred address, and on active connection migration to a new path. Appendix A.4 describes one possible algorithm.

就这样，终端尝试为每一条新连接使用ECN并做验证，这包括切换到服务器的首选地址时和从活跃连接迁移到新路径时。[附录A.4]()描述了一种可行的算法。

Other methods of probing paths for ECN support are possible, as are different marking strategies. Implementations MAY use other methods defined in RFCs; see [RFC8311]. Implementations that use the ECT(1) codepoint need to perform ECN validation using the reported ECT(1) counts.

可能存在其他探测路径是否支持ECN的方法以及不同的标记策略。QUIC实现{{< req_level MAY >}}使用定义在RFC中的其他方法，详见《[RFC8311]()》。使用了`ECT(1)`码点的实现需要用报告的`ECT(1)`计数进行ECN验证。
