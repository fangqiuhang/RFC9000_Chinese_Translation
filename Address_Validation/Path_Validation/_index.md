---
title: "8.2 路径验证"
anchor: "8.2_Path_Validation"
weight: 820
rank: "h2"
---

Path validation is used by both peers during connection migration (see Section 9) to verify reachability after a change of address. In path validation, endpoints test reachability between a specific local address and a specific peer address, where an address is the 2-tuple of IP address and port.

两侧终端在连接迁移期间（详见[第9章]()）都会使用路径验证以在地址发生变化后验证可达性。在路径验证中，终端会测试特定的本地地址和特定的对端地址间的可达性，这里的地址指的是IP地址和端口的二元组。

Path validation tests that packets sent on a path to a peer are received by that peer. Path validation is used to ensure that packets received from a migrating peer do not carry a spoofed source address.

路径验证测试的是在一条路径上发送给对端的数据包有没有被对端接收到。使用地址验证是为了确保从正在迁移的对端接收到的数据包携带着的源地址不是伪造的。

Path validation does not validate that a peer can send in the return direction. Acknowledgments cannot be used for return path validation because they contain insufficient entropy and might be spoofed. Endpoints independently determine reachability on each direction of a path, and therefore return reachability can only be established by the peer.

地址验证并不验证对端能够在返回的方向上发送数据。确认不能被用于返回路径的验证，因为它们携带的熵不够并且可能是伪造的。不同终端独立判断一条路径在各自方向上的可达性，因此返回方向的可达性只能由对端建立。

Path validation can be used at any time by either endpoint. For instance, an endpoint might check that a peer is still in possession of its address after a period of quiescence.

任一终端都可以在任意时间使用路径验证。例如，某个终端可能想在对端沉默了一段时间后检查对端是否还位于原来的地址上。

Path validation is not designed as a NAT traversal mechanism. Though the mechanism described here might be effective for the creation of NAT bindings that support NAT traversal, the expectation is that one endpoint is able to receive packets without first having sent a packet on that path. Effective NAT traversal needs additional synchronization mechanisms that are not provided here.

路径验证不是被作为NAT遍历机制设计出来的。尽管这里描述的机制可能对创建支持NAT遍历的NAT绑定也是有效的，但我们期望的是某个终端在还没有在一条路径上发送过数据包时就能够接收到数据包。有效的NAT遍历需要额外的同步机制，但这里没有提供。

An endpoint MAY include other frames with the PATH_CHALLENGE and PATH_RESPONSE frames used for path validation. In particular, an endpoint can include PADDING frames with a PATH_CHALLENGE frame for Path Maximum Transmission Unit Discovery (PMTUD); see Section 14.2.1. An endpoint can also include its own PATH_CHALLENGE frame when sending a PATH_RESPONSE frame.

终端{{< req_level MAY >}}在地址验证时将**通道挑战帧**和**回复通道帧**与其他类型的帧一起使用。特别是，终端可以在路径最大传输单元发现（PMTUD）中一起使用**填充帧**和**通道挑战帧**；详见[第14.2.1章]()。终端还可以在发送**回复通道帧**时带上自己的**通道挑战帧**。

An endpoint uses a new connection ID for probes sent from a new local address; see Section 9.5. When probing a new path, an endpoint can ensure that its peer has an unused connection ID available for responses. Sending NEW_CONNECTION_ID and PATH_CHALLENGE frames in the same packet, if the peer's active_connection_id_limit permits, ensures that an unused connection ID will be available to the peer when sending a response.

终端在从新的本地地址发送探测包时使用新的连接ID；详见[第9.5章]()。当探测新路径时，终端得确保对端有一个可以用来响应的未使用的连接ID。只要对端的`active_connection_id_limit`（活跃连接ID上限）允许，在同一个数据包中同时发送**新连接ID帧**和**通道挑战帧**可以确保对端在发送响应时有一个未使用的连接ID。

An endpoint can choose to simultaneously probe multiple paths. The number of simultaneous paths used for probes is limited by the number of extra connection IDs its peer has previously supplied, since each new local address used for a probe requires a previously unused connection ID.

终端可以选择同时探测数条路径。同时探测的路径数量受到对端目前已提供的额外连接ID数量的限制，因为每次为探测包使用新的本地地址时都需要一个未曾使用的连接ID。