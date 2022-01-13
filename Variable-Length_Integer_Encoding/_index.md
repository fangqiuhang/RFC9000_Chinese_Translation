---
title: "16. 可变长度整型编码"
anchor: "16_Variable-Length_Integer_Encoding"
weight: 1600
---

QUIC packets and frames commonly use a variable-length encoding for non-negative integer values. This encoding ensures that smaller integer values need fewer bytes to encode.

QUIC数据包和帧经常对非负整型值使用一种可变长度编码。这种编码确保较小的整型值能被编码到更少的字节中。

The QUIC variable-length integer encoding reserves the two most significant bits of the first byte to encode the base-2 logarithm of the integer encoding length in bytes. The integer value is encoded on the remaining bits, in network byte order.

QUIC可变长度整型编码占用了首个字节最高的两个有效位，用它们来存储正在编码的整型值的字节长度的以2为底的对数值。整型值以网络字节序被编码进剩余比特位中。

This means that integers are encoded on 1, 2, 4, or 8 bytes and can encode 6-, 14-, 30-, or 62-bit values, respectively. Table 4 summarizes the encoding properties.

这意味着被编码进1，2，4或8字节的整型值可以分别编码6，14，30和62位值。[表格4](#Table_4_Summary_of_Integer_Encodings)总结了这种编码的属性。

Table 4: Summary of Integer Encodings
2MSB	Length	Usable Bits	Range
00	1	6	0-63
01	2	14	0-16383
10	4	30	0-1073741823
11	8	62	0-4611686018427387903

{{% block_ref
indx="Table_4_Summary_of_Integer_Encodings"
title="表格4：整型编码概要" %}}

| 最高的2个有效位 | 字节长度 | 可用位数 | 可表示范围                 |
|:---------|:-----|:-----|:----------------------|
| 00       | 1    | 6    | 0-63                  |
| 01       | 2    | 14   | 0-16383               |
| 10       | 4    | 30   | 0-1073741823          |
| 11       | 8    | 62   | 0-4611686018427387903 |

{{% /block_ref %}}

An example of a decoding algorithm and sample encodings are shown in Appendix A.1.

在[附录A.1]()中可以找到一种解码算法的例子和一些样例编码。

Values do not need to be encoded on the minimum number of bytes necessary, with the sole exception of the Frame Type field; see Section 12.4.

值并不一定要被编码进正正好好最少的所需字节数，帧类型字段是主要的例外，详见[第12.4章]()。

Versions (Section 15), packet numbers sent in the header (Section 17.1), and the length of connection IDs in long header packets (Section 17.2) are described using integers but do not use this encoding.

版本（[第15章]()），头部中发送的数据包号（[第17.1章]()）和长包头中的连接ID长度（[第17.2章]()）使用整型值来描述但不使用本编码。
