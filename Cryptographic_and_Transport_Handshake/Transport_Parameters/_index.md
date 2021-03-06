---
title: "7.4. 加密与传输握手"
anchor: "7.4_Transport_Parameters"
weight: 740
rank: "h2"
---

连接建立期间，双端对各自的传输参数作出验证声明。
终端必须遵守每个参数定义的约束规范，每个参数的描述包括其处理规则。

传输参数由每个终端单方面声明。
每个终端可以独立选择传输参数的值而不依赖于对端的选择。

传输参数的编码详见[第18章](#18_Transport_Parameter_Encoding).

QUIC在加密握手阶段包含编码的传输参数。
一旦握手完成，由对端声明的传输参数就生效了。
每个终端都需要验证对端提供的参数值。

每个传输参数的具体定义详见[第18.2章](#18.2_Transport_Parameter_Definitions)。

终端{{< req_level MUST >}}将收到无效的传输参数值的情况视为一个`TRANSPORT_PARAMETER_ERROR`类型连接错误。

终端{{< req_level MUST_NOT >}}能在一个特定的传输参数扩展中传输同一个参数超过1次。
终端{{< req_level SHOULD >}}将收到重复的传输参数的情况视为一个`TRANSPORT_PARAMETER_ERROR`类型连接错误。

终端使用传输参数认证握手期间的连接ID协商，详见[第7.3章](#7.3_Authenticating_Connection_IDs)。

ALPN（详见[ALPN](https://www.rfc-editor.org/info/rfc7301)）允许客户端在连接建立期间提供多个应用协议。
客户端包含的传输参数对客户端提供的所有应用协议生效。
应用协议可以指定传输参数值，例如初始流控上限。
然而，应用协议对传输参数值设置限制，可能导致因为限制冲突使得客户端不能提供多种应用协议支持。
