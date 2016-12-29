# 数据链路层的两种信道类型

---

数据链路层属于计算机网络的底层，它使用的信道主要有以下两种类型：


###  点对点信道

   这种信道使用一对一点对点的通信方式。

   对于点对点的链路，PPP（Point-to-Point Protocol点到点协议）是为在同等单元之
间传输数据包这样的简单链
路设计的链路层协议。
这种链路提供全双工操作，并按照顺序传递数据包。

   PPP 协议通常用在两节点间创建直接的连接，并可以提供连接认证、传输加密([RFC 1968](https://www.ietf.org/rfc/rfc1968.txt)) 以及压缩。
PPP 被用在许多类型的物理网络中，包括串口线、电话线、中继链接、移动电话、特殊无线电链路以及光纤链路(如 [SONET](https://zh.wikipedia.org/zh-hans/同步光网络
))。PPP 还用在互联网接入连接上（现在称作宽带）。
互联网服务提供商（[ISP](https://en.wikipedia.org/wiki/Internet_service_provider)) 使用 PPP 为用户提供
到 Internet 的拨号接入，
这是因为 IP 报文无法在没有数据链路协议的情况下通过调制解调器线路自行传输。PPP 的两个派生物 [PPPoE](https://zh.wikipedia.org/zh-hans/PPPoE) 和 [PPPoA](https://en.wikipedia.org/wiki/Point-to-Point_Protocol_over_ATM) 被
ISP 广泛用来与用户创建数字用户线路（[DSL](https://zh.wikipedia.org/zh-hans/DSL
)）Internet 服务连接。

### 广播信道
    使用一对多的广播通信方式。由于连接的主机很多，因此必须使用专用的共享信道协议来协调这些主机的数据发送。

   我们经常说的局域网使用的就是广播信道。局域网最主要的特点是：网络为一个单位 所拥有，且地理范围和站点数目均受限。局域网具有如下的一些优点：

  * 具有广播功能，从一个站点可以很方便的访问全网，局域网上的主机可以共享
     连接在局域网上的各种硬件和软件资源。
  * 便于系统的扩展和逐渐的演变，各个设备的位置可灵活调整和改变。
  * 提高了系统的可靠性、可用性和生存性。

