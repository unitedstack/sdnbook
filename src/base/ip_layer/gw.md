# 网关协议

---


## 自治系统

在互联网中，一个自治系统（Autonomous system, AS）是指
在一个（有时是多个）实体管辖下的所有 IP 网络和路由器的全体，它们对互联网执行共同的路由策略。
参看 [RFC 1930](http://tools.ietf.org/html/rfc1930) 中更新的定义。

## 路由选择协议

路由选择通过根据路由选择算法来更新路由表信息。

路由选择协议划分为两大类：
 1. 内部网关协议 IGP（Interior Gateway Protocol），使用最多的是 RIP 和 OSPF。
 2. 外部网关协议 EGP（External Gateway Protocol），使用最多的是 BGP。


## 内部网关协议

 * [OSPF](https://en.wikipedia.org/wiki/Open_Shortest_Path_First) 用于在单一自治
系统内决策路由。是对链路状态路由协议的一种实现。

 * [RIP](https://en.wikipedia.org/wiki/Routing_Information_Protocol) 是一种基于距离矢量的路由协议，
以路由跳数作为计数单位的路由协议。适合用于比较小型的网络环境。

## 外部网关协议

 * [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol
)边界网关协议（BGP）是运行于 TCP 上的一种自治系统的路由协议。
BGP 是唯一一个用来处理像因特网大小的网络的协议，也是唯一能够妥善处理好不相关路由域间的多路连接的协议。

