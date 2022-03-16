---
title: "A.4. ECN验证算法样例"
anchor: "A.4_Sample_ECN_Validation_Algorithm"
weight: 230400
rank: "h2"
---

Each time an endpoint commences sending on a new network path, it determines whether the path supports ECN; see Section 13.4. If the path supports ECN, the goal is to use ECN. Endpoints might also periodically reassess a path that was determined to not support ECN.

每次终端开始在新的网络路径上发送时，它都要判断该路径是否支持ECN；详见[第13.4章]()。如果该路径支持ECN，那么就要将ECN利用起来。终端还可以每隔一段时间重新判断一次被认为不支持ECN的路径。

This section describes one method for testing new paths. This algorithm is intended to show how a path might be tested for ECN support. Endpoints can implement different methods.

本节描述了一种用于测试新路径的方法。本算法的意图是展示如何测试一条路径是否支持ECN。终端可以用其他方法实现。

The path is assigned an ECN state that is one of "testing", "unknown", "failed", or "capable". On paths with a "testing" or "capable" state, the endpoint sends packets with an ECT marking -- ECT(0) by default; otherwise, the endpoint sends unmarked packets.

受测试的路径会被指定一种ECN状态，它会是“测试中”、“未知”、“失败”和“支持”中的一种。在状态为“测试中”或“支持”的路径上，终端发送的数据包会带有`ECT`标记——默认是`ECT(0)`；否则，终端发送的数据包上不带标记。

To start testing a path, the ECN state is set to "testing", and existing ECN counts are remembered as a baseline.

要启动对一条路径的测试，其ECN状态会被置为“测试中”，并且当前的ECN计数被记录为基准值。

The testing period runs for a number of packets or a limited time, as determined by the endpoint. The goal is not to limit the duration of the testing period but to ensure that enough marked packets are sent for received ECN counts to provide a clear indication of how the path treats marked packets. Section 13.4.2 suggests limiting this to ten packets or three times the PTO.

测试期间会持续发送数个数据包，或者持续一段指定时间，这由终端决定。此处的目标是在不限制测试时长的情况下确保发送了足够的经标记的数据包以使得接收到的ECN计数能够清晰地表明该路径会怎样对待经标记的数据包。[第13.4.2章]()建议将此时长限制为持续10个数据包的发送或PTO的三倍大小。

After the testing period ends, the ECN state for the path becomes "unknown". From the "unknown" state, successful validation of the ECN counts in an ACK frame (see Section 13.4.2.1) causes the ECN state for the path to become "capable", unless no marked packet has been acknowledged.

在测试期结束时，该路径的ECN状态会变为“未知”。对**ACK帧**中ECN计数的成功验证会使得该路径的状态从“未知”变为“支持”，除非没有经标记的数据包得到确认。

If validation of ECN counts fails at any time, the ECN state for the affected path becomes "failed". An endpoint can also mark the ECN state for a path as "failed" if marked packets are all declared lost or if they are all ECN-CE marked.

一旦对ECN计数的验证失败，那么相关路径的ECN状态就会变为“失败”。终端还可以在经标记的数据包全部被认定为丢包或全部被标记上`ECN-CE`时将ECN状态置为“失败”。

Following this algorithm ensures that ECN is rarely disabled for paths that properly support ECN. Any path that incorrectly modifies markings will cause ECN to be disabled. For those rare cases where marked packets are discarded by the path, the short duration of the testing period limits the number of losses incurred.

按照本算法，能够确保ECN几乎不会在正确支持ECN的路径上被禁用。任何错误地修改了标记的路径都会使得ECN被禁用。对于那些经标记数据包被路径丢弃的罕见情况，短暂的测试期能够限制被丢弃数据包的数量。
