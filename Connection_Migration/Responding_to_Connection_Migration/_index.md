---
title: "9.3 响应连接迁移"
anchor: "9.3_Responding_to_Connection_Migration"
weight: 930
rank: "h2"
---

Receiving a packet from a new peer address containing a non-probing frame indicates that the peer has migrated to that address.

从新的对端地址接收到一个包含非探测帧的数据包表明对端已经迁移到那个地址。

If the recipient permits the migration, it MUST send subsequent packets to the new peer address and MUST initiate path validation (Section 8.2) to verify the peer's ownership of the address if validation is not already underway. If the recipient has no unused connection IDs from the peer, it will not be able to send anything on the new path until the peer provides one; see Section 9.5.

如果接收方承认这次迁移，那它{{< req_level MUST >}}改向新的对端地址发送后续数据包，并且，如果还没有发起，则{{< req_level MUST >}}发起路径验证（详见[第8.2章]()）来验证对端对那个地址的所有权。如果接收方没有来自对端的尚未使用的连接ID，那么在对端提供一个之前它将无法在新路径上发送任何东西；详见[第9.5章]()。

An endpoint only changes the address to which it sends packets in response to the highest-numbered non-probing packet. This ensures that an endpoint does not send packets to an old peer address in the case that it receives reordered packets.

终端仅在响应具有最大数据包号的非探测数据包时改变他将数据包发向的地址。这确保终端不会在接收到乱序数据包时将数据包发向旧的对端地址。

An endpoint MAY send data to an unvalidated peer address, but it MUST protect against potential attacks as described in Sections 9.3.1 and 9.3.2. An endpoint MAY skip validation of a peer address if that address has been seen recently. In particular, if an endpoint returns to a previously validated path after detecting some form of spurious migration, skipping address validation and restoring loss detection and congestion state can reduce the performance impact of the attack.

终端{{< req_level MAY >}}向未经验证的对端地址发送数据，但它{{< req_level MUST >}}抵御如[第9.3.1章]()和[第9.3.2章]()所述的潜在攻击。如果对端地址最近被见到过，那么终端{{< req_level MAY >}}跳过验证。特别是，如果终端在检测到某些形式的虚假迁移后返回先前验证过的路径，那么跳过地址验证并恢复丢包检测和拥塞控制状态可以减少由攻击带来的性能影响。

After changing the address to which it sends non-probing packets, an endpoint can abandon any path validation for other addresses.

终端在改变它将非探测数据包发向的地址后，它可以不再为其他地址进行路径验证。

Receiving a packet from a new peer address could be the result of a NAT rebinding at the peer.

接收到一个来自新对端地址的数据包可能是对端遭遇NAT重绑定的结果。

After verifying a new client address, the server SHOULD send new address validation tokens (Section 8) to the client.

在验证新的客户端地址后，服务器{{< req_level SHOULD >}}向客户端发送新的地址验证令牌（详见[第8章]()）。
