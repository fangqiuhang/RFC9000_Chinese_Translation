---
title: "A.2. 数据包号编码算法样例"
anchor: "A.2_Sample_Packet_Number_Encoding_Algorithm"
weight: 230200
rank: "h2"
---

The pseudocode in Figure 46 shows how an implementation can select an appropriate size for packet number encodings.

[图46]()中的伪代码展示了QUIC实现怎样选择合适长度的数据包号编码。

The EncodePacketNumber function takes two arguments:

`EncodePacketNumber`函数接收两个参数：

* full_pn is the full packet number of the packet being sent.

* `full_pn`是正在发送的数据包的完整数据包号。

* largest_acked is the largest packet number that has been acknowledged by the peer in the current packet number space, if any.

* `largest_acked`是当前数据包号空间中已被对端确认的最大数据包号，如果有的话。

{{% block_ref
indx="Figure_46_Sample_Packet_Number_Encoding_Algorithm"
title="图46：数据包号编码算法样例" %}}

```
EncodePacketNumber(full_pn, largest_acked):

  // 比特位的数量必须至少比连续未被确认的数据包号的数量（包括此数据包本身）的
  // 以`2`为底的对数值大`1`
  if largest_acked is None:
    num_unacked = full_pn + 1
  else:
    num_unacked = full_pn - largest_acked

  min_bits = log(num_unacked, 2) + 1
  num_bytes = ceil(min_bits / 8)

  // 将整型值编码，并截断为仅剩最低`num_bytes`个字节
  return encode(full_pn, num_bytes)
```

{{% /block_ref %}}

For example, if an endpoint has received an acknowledgment for packet 0xabe8b3 and is sending a packet with a number of 0xac5c02, there are 29,519 (0x734f) outstanding packet numbers. In order to represent at least twice this range (59,038 packets, or 0xe69e), 16 bits are required.

举例来说，如果终端接收到了对于数据包`0xabe8b3`的确认，并且正在发送数据包号为`0xac5c02`的数据包，那么存在`29,519`（`0x734f`）个未确认的数据包号。为了能够至少表示这个数量的两倍大小（`59,038`个，或者说`0xe69e`个数据包），就需要16个比特位。

In the same state, sending a packet with a number of 0xace8fe uses the 24-bit encoding, because at least 18 bits are required to represent twice the range (131,222 packets, or 0x020096).

在相同的状态下，发送数据包号为`0xace8fe`的数据包会使用长度为24比特位的编码方式，因为至少需要18个比特位才能表示缺口数量的两倍（`131,222`个，或者说`0x020096`个数据包）。
