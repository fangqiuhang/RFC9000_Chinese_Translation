---
title: "10.1 空闲超时"
anchor: "10.1_Idle_Timeout"
weight: 1010
---

If a max_idle_timeout is specified by either endpoint in its transport parameters (Section 18.2), the connection is silently closed and its state is discarded when it remains idle for longer than the minimum of the max_idle_timeout value advertised by both endpoints.

如果任一终端在其传输参数（详见[第18.2章]()）中指定了`max_idle_timeout`（最大空闲超时时间），且某个连接持续空闲的时间超过了两个终端宣告的`max_idle_timeout`中的较小值，那么这个连接就会被静默关闭，并且它的状态会被丢弃。

Each endpoint advertises a max_idle_timeout, but the effective value at an endpoint is computed as the minimum of the two advertised values (or the sole advertised value, if only one endpoint advertises a non-zero value). By announcing a max_idle_timeout, an endpoint commits to initiating an immediate close (Section 10.2) if it abandons the connection prior to the effective value.

每个终端都宣告`max_idle_timeout`，但是只取两个值中较小的那一个（或只取有效的那个值，如果只有一个终端宣告了非零值）作为终端使用的有效值。通过宣告`max_idle_timeout`，终端承诺，如果它要在空闲时间还未达到上述有效值时就放弃一条连接，那么它会发起立即关闭（详见[第10.2章]()）。

An endpoint restarts its idle timer when a packet from its peer is received and processed successfully. An endpoint also restarts its idle timer when sending an ack-eliciting packet if no other ack-eliciting packets have been sent since last receiving and processing a packet. Restarting this timer when sending a packet ensures that connections are not closed after new activity is initiated.

当成功接收且处理了一个来自对端的数据包时，终端重启它的空闲计时器。当正要发送ACK触发包时，如果自上一次接收并处理数据包后还没有发送过任何ACK触发包，那么终端也会重启它的空闲计时器。在发送数据包时重启这个计时器确保新的活动被发起后连接不会被马上关闭。

To avoid excessively small idle timeout periods, endpoints MUST increase the idle timeout period to be at least three times the current Probe Timeout (PTO). This allows for multiple PTOs to expire, and therefore multiple probes to be sent and lost, prior to idle timeout.

为了避免过小的空闲超时时间，终端{{< req_level MUST >}}提高空闲超时时间至少到当前探测包超时时间（PTO）的三倍。这使得在空闲超时前有充足的时间供数个探测包被发送和丢失。
