---
title: "10.3 无状态重置"
anchor: "10.3_Stateless_Reset"
weight: 1030
---

A stateless reset is provided as an option of last resort for an endpoint that does not have access to the state of a connection. A crash or outage might result in peers continuing to send data to an endpoint that is unable to properly continue the connection. An endpoint MAY send a Stateless Reset in response to receiving a packet that it cannot associate with an active connection.

如果终端无法访问连接的状态数据，那么无状态重置将是其最后手段。崩溃或中断可能造成对端持续向一个无法正确地维持连接的终端发送数据。终端{{< req_level MAY >}}在接收到一个它无法关联到某个活跃连接的数据包时发送无状态重置作为回应。

A stateless reset is not appropriate for indicating errors in active connections. An endpoint that wishes to communicate a fatal connection error MUST use a CONNECTION_CLOSE frame if it is able.

无状态重置不适合用来表明在活跃连接中出现的错误。想要传达致命的连接错误这一消息的终端在有能力的情况下{{< req_level MUST >}}使用**连接关闭帧**。

To support this process, an endpoint issues a stateless reset token, which is a 16-byte value that is hard to guess. If the peer subsequently receives a Stateless Reset, which is a UDP datagram that ends in that stateless reset token, the peer will immediately end the connection.

为了支持无状态重置，终端会签发一个无状态重置令牌，它是一个难以猜测的16字节长的值。如果对端后续收到了无状态重置，也就是一个以那个无状态重置令牌结尾的UDP数据报，那么对端将立即结束这条连接。

A stateless reset token is specific to a connection ID. An endpoint issues a stateless reset token by including the value in the Stateless Reset Token field of a NEW_CONNECTION_ID frame. Servers can also issue a stateless_reset_token transport parameter during the handshake that applies to the connection ID that it selected during the handshake. These exchanges are protected by encryption, so only client and server know their value. Note that clients cannot use the stateless_reset_token transport parameter because their transport parameters do not have confidentiality protection.

每个无状态重置令牌都是特定于某个连接ID的。终端签发无状态重置令牌时，将它的值包含在**新连接ID帧**的无状态重置令牌字段中。服务器还可以在握手期间签发传输参数`stateless_reset_token`（无状态重置令牌），它会被应用到服务器在握手期间选择的连接ID上。这些信息交换是被加密保护的，所以只有客户端和服务器知道它们的值。注意，客户端不能使用传输参数`stateless_reset_token`，因为它们的传输参数没有可信度保护。

Tokens are invalidated when their associated connection ID is retired via a RETIRE_CONNECTION_ID frame (Section 19.16).

当令牌关联的连接ID被**停用连接ID帧**（详见[第19.16章]()）停用时，令牌将失效。

An endpoint that receives packets that it cannot process sends a packet in the following layout (see Section 1.3):

如果终端接收到了它无法处理的数据包，那么它会以这种形式发送一个数据包：

Stateless Reset {
Fixed Bits (2) = 1,
Unpredictable Bits (38..),
Stateless Reset Token (128),
}
Figure 10: Stateless Reset

{{% block_ref
indx="Figure_10_Stateless_Reset"
title="图10：无状态重置" %}}

```
无状态重置 {
  固定比特位 (2) = 1,
  不可预测的比特位 (38..),
  无状态重置令牌 (128),
}
```

{{% /block_ref %}}

This design ensures that a Stateless Reset is -- to the extent possible -- indistinguishable from a regular packet with a short header.

这种设计确保无状态重置——尽可能地——无法和常规的短包头数据包区分。

A Stateless Reset uses an entire UDP datagram, starting with the first two bits of the packet header. The remainder of the first byte and an arbitrary number of bytes following it are set to values that SHOULD be indistinguishable from random. The last 16 bytes of the datagram contain a stateless reset token.

无状态重置使用从数据包头部的最前面两个比特位开始的一整个UDP数据报。首个字节的剩余部分和在它之后任意数量的字节都{{< req_level SHOULD >}}被设置为无法分辨是不是随机的值。数据报的最后16个字节包含了无状态重置令牌。

To entities other than its intended recipient, a Stateless Reset will appear to be a packet with a short header. For the Stateless Reset to appear as a valid QUIC packet, the Unpredictable Bits field needs to include at least 38 bits of data (or 5 bytes, less the two fixed bits).

对于不是意图的接收方的实体来说，无状态重置看起来就是一个短包头数据包。为了让无状态重置表现得像一个合法QUIC数据包，不可预测的比特位应该包含至少38比特的数据（或5字节，并去掉两个固定比特位）。

The resulting minimum size of 21 bytes does not guarantee that a Stateless Reset is difficult to distinguish from other packets if the recipient requires the use of a connection ID. To achieve that end, the endpoint SHOULD ensure that all packets it sends are at least 22 bytes longer than the minimum connection ID length that it requests the peer to include in its packets, adding PADDING frames as necessary. This ensures that any Stateless Reset sent by the peer is indistinguishable from a valid packet sent to the endpoint. An endpoint that sends a Stateless Reset in response to a packet that is 43 bytes or shorter SHOULD send a Stateless Reset that is one byte shorter than the packet it responds to.

如果接收方要求使用连接ID，那么那作为结果的至少21个字节并不保证无状态重置很难与其他数据包区分。为了做到这一点，终端{{< req_level SHOULD >}}确保所有它发送的数据包都比它要求对端在数据包中包含的最小的连接ID的长度要长至少22字节，不够就加**填充帧**。这么做能确保所有对端发出的无状态重置都无法和发送给当前终端的合法数据包区分。在发送无状态重置来响应一个不超过43字节的数据包时，终端{{< req_level SHOULD >}}发送一个比它要响应的那个数据包要短一个字节的无状态重置。

These values assume that the stateless reset token is the same length as the minimum expansion of the packet protection AEAD. Additional unpredictable bytes are necessary if the endpoint could have negotiated a packet protection scheme with a larger minimum expansion.

以上数值假设了无状态重置令牌和数据包保护AEAD的最小扩充量具有相同长度。如果终端协商了具有更大的最小扩充量的数据包保护方案，那么就需要额外的不可预测字节。

An endpoint MUST NOT send a Stateless Reset that is three times or more larger than the packet it receives to avoid being used for amplification. Section 10.3.3 describes additional limits on Stateless Reset size.

终端发送的无状态重置尺寸{{< req_level MUST_NOT >}}是它接收到的数据包的三倍或更大，以免被用于放大攻击。[第10.3.3章]()描述了无状态重置尺寸的额外限制。

Endpoints MUST discard packets that are too small to be valid QUIC packets. To give an example, with the set of AEAD functions defined in [QUIC-TLS], short header packets that are smaller than 21 bytes are never valid.

终端{{< req_level MUST >}}丢弃因过小而不合法的QUIC数据包。举个例子，由于《[QUIC-TLS]()》中定义的AEAD函数组，小于21字节的短包头数据包永远不可能合法。

Endpoints MUST send Stateless Resets formatted as a packet with a short header. However, endpoints MUST treat any packet ending in a valid stateless reset token as a Stateless Reset, as other QUIC versions might allow the use of a long header.

终端{{< req_level MUST >}}将无状态重置以短包头数据包的格式发送。然而，终端{{< req_level MUST >}}将任何以有效无状态重置令牌结尾的数据包视作无状态重置，因为其他QUIC版本可能允许使用长包头。

An endpoint MAY send a Stateless Reset in response to a packet with a long header. Sending a Stateless Reset is not effective prior to the stateless reset token being available to a peer. In this QUIC version, packets with a long header are only used during connection establishment. Because the stateless reset token is not available until connection establishment is complete or near completion, ignoring an unknown packet with a long header might be as effective as sending a Stateless Reset.

终端{{< req_level MAY >}}在响应长包头数据包时发送无状态重置。在无状态重置令牌在对端处可用前发送无状态重置是没有效果的。在本QUIC版本中，只能在连接建立期间使用长包头数据包。出于无状态重置令牌只有在连接建立完成或临近完成时才可用，忽略长包头数据包的效果和发送无状态重置的效果可能是一样的。

An endpoint cannot determine the Source Connection ID from a packet with a short header; therefore, it cannot set the Destination Connection ID in the Stateless Reset. The Destination Connection ID will therefore differ from the value used in previous packets. A random Destination Connection ID makes the connection ID appear to be the result of moving to a new connection ID that was provided using a NEW_CONNECTION_ID frame; see Section 19.15.

终端不能从短包头数据包中判断源连接ID；因此，它不能在无状态重置中设置目标连接ID。于是目标连接ID会和之前发送的数据包中使用的值不一样。随机的目标连接ID能使连接ID表现得像是迁移至由**新连接ID帧**提供的新连接ID的结果；详见[第19.15章]()。

Using a randomized connection ID results in two problems:

使用随机的连接ID会有两个问题：

* The packet might not reach the peer. If the Destination Connection ID is critical for routing toward the peer, then this packet could be incorrectly routed. This might also trigger another Stateless Reset in response; see Section 10.3.3. A Stateless Reset that is not correctly routed is an ineffective error detection and recovery mechanism. In this case, endpoints will need to rely on other methods -- such as timers -- to detect that the connection has failed. 

* 数据包可能无法抵达对端。如果目标连接ID在路由至对端时起关键作用，那么该数据包可能被错误地路由。作为响应，这可能会触发另一个无状态重置；详见[第10.3.3章]()。没有被正确路由的无状态重置使得错误检测和恢复机制失去效果。在这种情况下，终端需要依靠其他方法——比如计时器——来检测连接已失败。

* The randomly generated connection ID can be used by entities other than the peer to identify this as a potential Stateless Reset. An endpoint that occasionally uses different connection IDs might introduce some uncertainty about this.

* 随机生成的连接ID可以被不是对端的其他实体辨识为可能的无状态重置。偶尔使用不同的连接ID的终端可以对此引入一些不确定性。

This stateless reset design is specific to QUIC version 1. An endpoint that supports multiple versions of QUIC needs to generate a Stateless Reset that will be accepted by peers that support any version that the endpoint might support (or might have supported prior to losing state). Designers of new versions of QUIC need to be aware of this and either (1) reuse this design or (2) use a portion of the packet other than the last 16 bytes for carrying data.

这样的无状态重置设计是特定于QUIC版本1的。支持多个QUIC版本的终端创建的无状态重置应该可以被支持所有当前终端所支持的版本（或所有在当前终端丢失状态前可能支持的版本）的对端接受。新QUIC版本的设计者应当注意上述这一点，并且要么(1)重用当前设计，要么(2)使用数据包除了最后16个字节之外的某个部分来携带数据。
