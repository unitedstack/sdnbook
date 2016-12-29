## 隧道技术

---

隧道协议（Tunneling Protocol）是一种网络协议，在其中，
使用一种网络协议（发送协议），将另一个不同的网络协议，封装在负载部分。
使用隧道的原因是在不兼容的网络上传输数据，或在不安全网络上提供一个安全路径。


隧道技术主要体现在 VPN 的使用上，VPN 将企业网的数据封装在隧道中进行传输。
隧道协议可分为第二层隧道协议 PPTP、 L2F、L2TP 和第三层隧道协议 GRE、IPsec。 它们的本质区别在于用户的>数据包是被封装在哪种数据包中在隧道中传输的。

二层隧道协议：

 * PPTP ：[PPTP 点到点隧道协议](https://en.wikipedia.org/wiki/Point-to-Point_Tunneling_Protocol)是由 PPTP 论坛开发的点到点的安全隧道协议，
为使用电话上网的用户提供安全 VPN 业务。

 * L2F ：[L2F（Layer 2 Forwarding）](https://en.wikipedia.org/wiki/Layer_2_Forwarding_Protocol)是由 Cisco 公司提出的，
可以在多种介质（如 ATM、帧中继、IP）上建立多协议的安全 VPN 的通信方式。

 * L2TP：[L2TP](https://en.wikipedia.org/wiki/Layer_2_Tunneling_Protocol) 结合了 L2F 和 PPTP 的优点，
可以让用户从客户端或接人服务器端发起VPN 连接。
L2TP 定义了利用公共网络设施封装传输链路层 PPP 帧的方法。


三层隧道协议：

 * IPSec：[IPSec](https://en.wikipedia.org/wiki/IPsec) 是一个三层 VPN 协议标准，它支持信息通过 IP 公>网的安全传输。

 * GRE：[GRE](https://en.wikipedia.org/wiki/Generic_Routing_Encapsulation) 规定了如何用一种网络协议去
封装另一种网络协议的方法。
GRE 的隧道由两端的源 IP 地址和目的 IP 地址来定义，允许用户使用 IP 包封装 IP、 IPX、AppleTalk 包，并支
持全部的路由协议（如 RIP2、OSPF 等）。
