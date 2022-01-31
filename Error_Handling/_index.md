---
title: "11. 错误处理"
anchor: "11_Error_Handling"
weight: 1100
---

An endpoint that detects an error SHOULD signal the existence of that error to its peer. Both transport-level and application-level errors can affect an entire connection; see Section 11.1. Only application-level errors can be isolated to a single stream; see Section 11.2.

检测到错误的终端{{< req_level SHOULD >}}向对端发送出现了这个错误的信号。传输层和应用层的错误都会影响整条连接，详见[第11.1章]()。只有应用层的错误可以被隔离到单条流上，详见[第11.2章]()。

The most appropriate error code (Section 20) SHOULD be included in the frame that signals the error. Where this specification identifies error conditions, it also identifies the error code that is used; though these are worded as requirements, different implementation strategies might lead to different errors being reported. In particular, an endpoint MAY use any applicable error code when it detects an error condition; a generic error code (such as PROTOCOL_VIOLATION or INTERNAL_ERROR) can always be used in place of specific error codes.

在发送信号的帧中{{< req_level SHOULD >}}使用最合适的错误码（详见[第10章]()）。这份规范中在描述错误情形的地方会定义这时候使用的错误码；尽管这些定义被称作”要求“，但是不同的实现策略可能导致最终报告不同的错误。特别是，终端在检测到错误情形时，{{< req_level MAY >}}使用任何适用的错误码；通用的错误码（比如`PROTOCOL_VIOLATION`（协议违背）或`INTERNAL_ERROR`（内部错误））总是可以用在其他错误码可以用的地方。

A stateless reset (Section 10.3) is not suitable for any error that can be signaled with a CONNECTION_CLOSE or RESET_STREAM frame. A stateless reset MUST NOT be used by an endpoint that has the state necessary to send a frame on the connection.

对于可以使用**连接关闭帧**或**流重置帧**来发送信号的错误，不适合使用无状态重置（[第10.3章]()）。具有足够的状态数据来在连接上发送帧的终端{{< req_level MUST_NOT >}}使用无状态重置。
