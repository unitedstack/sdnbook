# ARP

---

由于 IP 数据包是放在以太网数据包里发送的，所以必须同时知道两个地址，
一个是对方的 MAC 地址，另一个是对方的 IP 地址。通常情况下，对方的IP地址是已知
的，但是不知道它的 MAC 地址。

所以，需要一种机制，能够从 IP 地址得到 MAC 地址。

这里分为两种情况：

一种是两个主机不在同一个网络内，那么就无法得到对方主机的 MAC 地址，因此只能
将这个数据包发送给 *网关*，交由网关去处理。

另一种是两个主机在一个网络内，那么通过 [ARP 协议](https://en.wikipedia.org/wiki/Address_Resolution_Protocol)，得到对方
的 MAC 地址。ARP 报文头部格式如下：

 ![arp][3]

可以通过Wireshark 抓包详细查看 ARP 报文中的各个字段是如何填充的。

 ![arp_packet][4]

依次说明各个部分的意义：

以太网报头：

 1. 目的 MAC 地址：0xFF-FF-FF-FF-FF-FF ，这是一个广播地址，目标是网络上除了自
己之外的所有主机。
 2. 源 MAC 地址：88:25:93:2c:31:58，本机的 MAC 地址。
 3. 协议类型：表明以太网帧的类型，这里是 0x0806，代表这是 ARP 协议帧。

ARP 报文：

 1. 硬件类型：1 代表是以太网。
 2. 协议类型：表明上层协议的类型，这里是 0x0800，表示上层协议是 IP 协议。
 3. 硬件地址长度：毫无疑问是 6 个字节。
 4. 协议长度：这里 IP 协议的长度是 4 个字节。
 5. 操作类型：在报文中占 2 个字节， 1 表示 ARP 请求， 2 表示 ARP 应答，3 表示 RARP 请求，4 表示 RARP 应答。
 6. 发送方 MAC 地址
 7. 发送方 IP 地址
 8. 目的 MAC 地址：由于是 ARP 请求报文，因此这里的目的 MAC 地址随意填充即可。
 9. 目的 IP 地址


因此，有了 ARP 协议之后，就可以得到同一个子网络内的主机 MAC 地址，
可以把数据包发送到任意一台主机之上了。

### 参考文档

 ARP 协议作为网络中最基本的协议之一，一定要深刻理解 ARP 的意义，在此推荐几篇不错的文档以供参考：
* [Networking Basics: How ARP Works](https://www.tummy.com/articles/networking-basics-how-arp-works/)
* [Understanding How ARP Works](http://www.juniper.net/documentation/en_US/junose16.1/topics/concept/ip-arp-understanding.html)
* [Understanding ARP (Address Resolution Protocol)](https://www.youtube.com/watch?v=bOQ4BoiqTeE)


[3]: ../../../images/base/arp.png
[4]: ../../../images/base/arp_packet.png
