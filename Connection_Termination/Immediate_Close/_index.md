---
title: "10.2 立即关闭"
anchor: "10.2_Immediate_Close"
weight: 1020
---

An endpoint sends a CONNECTION_CLOSE frame (Section 19.19) to terminate the connection immediately. A CONNECTION_CLOSE frame causes all streams to immediately become closed; open streams can be assumed to be implicitly reset.

终端发送**连接关闭帧**（详见[第19.19章]()）来立即终止连接。**连接关闭帧**使得所有流都被立即关闭；开放的流可以被假定为被隐式重置。

After sending a CONNECTION_CLOSE frame, an endpoint immediately enters the closing state; see Section 10.2.1. After receiving a CONNECTION_CLOSE frame, endpoints enter the draining state; see Section 10.2.2.

在发送**连接关闭帧**之后，终端立即进入关闭状态，详见[第10.2.1章]()。在收到**连接关闭帧**之后，终端进入排空状态，详见[第10.2.2章]()。

Violations of the protocol lead to an immediate close.

对协议的违背将引发立即关闭。

An immediate close can be used after an application protocol has arranged to close a connection. This might be after the application protocol negotiates a graceful shutdown. The application protocol can exchange messages that are needed for both application endpoints to agree that the connection can be closed, after which the application requests that QUIC close the connection. When QUIC consequently closes the connection, a CONNECTION_CLOSE frame with an application-supplied error code will be used to signal closure to the peer.

在应用协议计划关闭一条连接后，可以使用立即关闭。这时可能是应用协议协商了一次正常关闭。应用协议可以交换用于两侧应用终端协商关闭连接的消息，在那之后请求QUIC关闭连接。当QUIC因此关闭连接后，带着由应用提供的错误码的**连接关闭帧**会被用于向对端发送关闭的信号。

The closing and draining connection states exist to ensure that connections close cleanly and that delayed or reordered packets are properly discarded. These states SHOULD persist for at least three times the current PTO interval as defined in [QUIC-RECOVERY].

关闭和排空状态用于确保连接被干净地关闭，并且延误的和乱序的数据包被正确地丢弃。这些状态的持续时间{{< req_level SHOULD >}}至少为当前PTO间隔的三倍，详见《[QUIC恢复]()》。

Disposing of connection state prior to exiting the closing or draining state could result in an endpoint generating a Stateless Reset unnecessarily when it receives a late-arriving packet. Endpoints that have some alternative means to ensure that late-arriving packets do not induce a response, such as those that are able to close the UDP socket, MAY end these states earlier to allow for faster resource recovery. Servers that retain an open socket for accepting new connections SHOULD NOT end the closing or draining state early.

在退出关闭或排空状态前就处理连接的状态数据，可能导致终端在收到被延误的数据包时不必要地发起无状态重置。有一些备用手段来确保被延误的数据包不会引起响应的终端，比如那些有能力关闭UDP套接字的终端，{{< req_level MAY >}}更早结束连接的状态数据以更快恢复可用资源。始终保持一个套接字开放以接受新连接的服务器{{< req_level SHOULD_NOT >}}过早结束关闭或排空状态。

Once its closing or draining state ends, an endpoint SHOULD discard all connection state. The endpoint MAY send a Stateless Reset in response to any further incoming packets belonging to this connection.

一旦终端的关闭或排空状态结束，它就{{< req_level SHOULD >}}丢弃所有连接状态数据。终端在响应任何将来的属于这条连接的传入数据包时，{{< req_level MAY >}}发送无状态重置。
