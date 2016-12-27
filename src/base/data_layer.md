# 数据链路层

---

## 简介

数据链路层的工作就是把网络层交下来的 IP 数据报封装为帧发送到链路上，
以及把接收到的帧中的数据取出并上交给网络层。
为达到这一目的，数据链路必须具备一系列相应的功能，主要有：

  * 封装成帧： 将数据封装为帧，帧是数据链路层的传送单位；
  * 透明传输： 控制帧的传输，包括处理传输差错，调节发送速率与接收方相匹配；
  * 差错检测： 在两个网络实体之间提供数据链路通路的建立、维持和释放的管理。

## 数据链路层的两种信道类型
数据链路层属于计算机网络的底层，它使用的信道主要有以下两种类型：

1. 点对点信道：这种信道使用一对一点对点的通信方式。

   对于点对点的链路，PPP（Point-to-Point Protocol点到点协议）是为在同等单元之间传输数据包这样的简单链路设计的链路层协议。
这种链路提供全双工操作，并按照顺序传递数据包。

   PPP 协议通常用在两节点间创建直接的连接，并可以提供连接认证、传输加密([RFC 1968](https://www.ietf.org/rfc/rfc1968.txt)) 以及压缩。
PPP 被用在许多类型的物理网络中，包括串口线、电话线、中继链接、移动电话、特殊无线电链路以及光纤链路(如 [SONET](https://zh.wikipedia.org/zh-hans/同步光网络
))。PPP 还用在互联网接入连接上（现在称作宽带）。
互联网服务提供商（[ISP](https://en.wikipedia.org/wiki/Internet_service_provider)) 使用 PPP 为用户提供到 Internet 的拨号接入，
这是因为 IP 报文无法在没有数据链路协议的情况下通过调制解调器线路自行传输。PPP 的两个派生物 [PPPoE](https://zh.wikipedia.org/zh-hans/PPPoE) 和 [PPPoA](https://en.wikipedia.org/wiki/Point-to-Point_Protocol_over_ATM) 被 
ISP 广泛用来与用户创建数字用户线路（[DSL](https://zh.wikipedia.org/zh-hans/DSL
)）Internet 服务连接。

2. 广播信道：使用一对多的广播通信方式。由于连接的主机很多，因此必须使用专用的 共享信道协议来协调这些主机的数据发送。

   我们经常说的局域网使用的就是广播信道。局域网最主要的特点是：网络为一个单位 所拥有，且地理范围和站点数目均受限。局域网具有如下的一些优点：

  * 具有广播功能，从一个站点可以很方便的访问全网，局域网上的主机可以共享
     连接在局域网上的各种硬件和软件资源。
  * 便于系统的扩展和逐渐的演变，各个设备的位置可灵活调整和改变。
  * 提高了系统的可靠性、可用性和生存性。


## 以太网：使用广播信道的数据链路层

以太网（Ethernet）是一种计算机局域网技术。IEEE 组织的 [IEEE 802.3](https://en.wikipedia.org/wiki/IEEE_802.3)  标准制定了以太网的技术标准，
它规定了包括物理层的连线、电子信号和介质访问层协议的内容。以太网是目前应用最普遍的局域网技术，替换了其他局域网标准如[令牌环](https://zh.wikipedia.org/wiki/令牌环)、[FDDI](https://zh.wikipedia.org/wiki/FDDI) 和 ARCNET。

为了使数据链路层能更好地适应多种局域网标准，802 委员会就将局域网的数据链路层拆成两个子层：

 * 逻辑链路控制 LLC ( Logical Link Control ) 子层

 * 媒体接入控制 MAC ( Medium Access Control ) 子层。

与接入到传输媒体有关的内容都放在 MAC 子层，而 LLC 子层则与传输媒体无关，
不管采用何种协议的局域网对 LLC 子层来说都是透明的，如下图所示：

 ![data_layer_mac_and_llc][1]


## 网络适配器（网卡NIC）的作用

计算机与外键局域网的连接是通过通信适配器（ adapter ）。

网络接口板又称为通信适配器或网络接口卡 NIC (Network Interface Card)，或`网卡`,，在适配器上面装有处理器和存储器（ 包括 RAM 和 ROM ）。

适配器和局域网之间的通信是通过电缆或双绞线以[串行传输方式](https://zh.wikipedia.org/wiki/串行传输方式)进行的，
而适配器和计算机之间的通信则是通过计算机主板上的 I/O 总线以[并行传输方式]((https://zh.wikipedia.org/wiki/并行传输方式)进行的，因此适配器的一个主要功能就是进行数据串行传输和并行传输的转换。

适配器的重要功能：

  * 进行串行 / 并行转换。
  * 对数据进行缓存。
  * 在计算机的操作系统安装设备驱动程序。
  * 实现以太网协议。  

网卡的功能主要有两个:

 * 将电脑的数据封装为帧，并通过网线(对无线网络来说就是电磁波)将数据发送到网络上去;

 * 接收网络上其它设备传过来的帧，并将帧重新组合成数据，发送到所在的电脑中。
网卡能接收所有在网络上传输的信号，但正常情况下只接受发送到该电脑的帧和广播帧，将其余的帧丢弃。

计算机通过适配器和局域网进行通信，如下图所示：


 ![data_layer_nic][2]


## VLAN

虚拟区域（Virtual Local Area Network 或简写 VLAN, V-LAN）是一种建构于局域网交
换技术（LAN Switch）的网络管理的技术，
网管人员可以借此通过控制交换机有效分派出入局域网的数据包到正确的出入端口，
达到对不同实体局域网中的设备进行逻辑分群（Grouping）管理，并降低局域网内大量数据流通时，因无用数据包过多导致拥塞的问题，以及提升局域网的信息安全保障。(注：
来自 [维基百科](https://zh.wikipedia.org/wiki/虚拟局域网))

通俗的说，划分物理以太网产生的每一个广播域等同于一个逻辑上独立的以太网，
由于这些逻辑上独立的以太网存在于同一个物理以太网中，因而被称为虚拟局域网（VirtualLAN，VLAN）。说白了就是自定义广播域。


[IEEE 802.1Q](https://zh.wikipedia.org/wiki/IEEE_802.1Q) 以及 VLAN Tagging 
属于互联网下 IEEE 802.1 的标准规范，允许多个网桥在信息不被外泄的情况下公开的共用同一个实体网络。

传统的以太网数据帧在目的MAC地址和源MAC地址之后封装的是上层协议的类型字段，如下图所示：

 ![data_layer_normal][3]

其中 DA 表示目的 MAC 地址，SA 表示源 MAC 地址， Type 表示报文所属协议类型。
IEEE 802.1Q 协议规定在目的 MAC 地址和源 MAC 地址之后封装4个字节的 VLAN Tag，
用以标识 VLAN 的相关信息。

 ![data_layer_vlan][4]

如上图所示，VLAN Tag 包含四个字段，分别是：TPID（Tag Protocol Identifier，标签协议标识符）、Priority、CFI（Canonical Format Indicator，标准格式指示位）和 VLAN ID。

  * TPID 用来判断本数据帧是否带有VLAN Tag，长度为16bit，缺省取值为0x8100。
  * Priority 表示报文的802.1P优先级，长度为3bit
  * CFI 字段标识 MAC 地址在不同的传输介质中是否以标准格式进行封装，
长度为1bit，取值为 0 表示 MAC 地址以标准格式进行封装，为 1 表示以非标准格式
封装，缺省取值为0。
  * VLAN ID 标识该报文所属 VLAN 的编号，长度为 12bit，取值范围为0 ～ 4095。
由于 0 和 4095 为协议保留取值，所以 VLAN ID 的取值范围为 1 ～ 4094。

网络设备利用 VLAN ID 来识别报文所属的 VLAN，根据报文是否携带 VLAN Tag 以及携带
的 VLAN Tag 值，来对报文进行处理。


## 隧道技术

隧道协议（Tunneling Protocol）是一种网络协议，在其中，
使用一种网络协议（发送协议），将另一个不同的网络协议，封装在负载部分。
使用隧道的原因是在不兼容的网络上传输数据，或在不安全网络上提供一个安全路径。


隧道技术主要体现在 VPN 的使用上，VPN 将企业网的数据封装在隧道中进行传输。
隧道协议可分为第二层隧道协议 PPTP、 L2F、L2TP 和第三层隧道协议 GRE、IPsec。 它们的本质区别在于用户的数据包是被封装在哪种数据包中在隧道中传输的。

二层隧道协议：

 * PPTP ：[PPTP 点到点隧道协议](https://en.wikipedia.org/wiki/Point-to-Point_Tunneling_Protocol)是由 PPTP 论坛开发的点到点的安全隧道协议，
为使用电话上网的用户提供安全 VPN 业务。

 * L2F ：[L2F（Layer 2 Forwarding）](https://en.wikipedia.org/wiki/Layer_2_Forwarding_Protocol)是由 Cisco 公司提出的，
可以在多种介质（如 ATM、帧中继、IP）上建立多协议的安全 VPN 的通信方式。

 * L2TP：[L2TP](https://en.wikipedia.org/wiki/Layer_2_Tunneling_Protocol) 结合了 L2F 和 PPTP 的优点，可以让用户从客户端或接人服务器端发起VPN 连接。
L2TP 定义了利用公共网络设施封装传输链路层 PPP 帧的方法。


三层隧道协议：

 * IPSec：[IPSec](https://en.wikipedia.org/wiki/IPsec) 是一个三层 VPN 协议标准，它支持信息通过 IP 公网的安全传输。

 * GRE：[GRE](https://en.wikipedia.org/wiki/Generic_Routing_Encapsulation) 规定了如何用一种网络协议去封装另一种网络协议的方法。
GRE 的隧道由两端的源 IP 地址和目的 IP 地址来定义，允许用户使用 IP 包封装 IP、 IPX、AppleTalk 包，并支持全部的路由协议（如 RIP2、OSPF 等）。



[1]: ../../images/base/data_layer_mac_llc.png
[2]: ../../images/base/data_layer_nic.png
[3]: ../../images/base/data_layer_normal.png
[4]: ../../images/base/data_layer_vlan.png
