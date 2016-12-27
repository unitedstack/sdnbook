## 以太网


以太网（Ethernet）是一种计算机局域网技术，它通过使用广播信道实现数据链路层。IEEE 组织的 [IEEE 802.3](https://en.wikipedia.org/wiki/IEEE_802.3)  标准制定了以太网的技术标准，
它规定了包括物理层的连线、电子信号和介质访问层协议的内容。
以太网是目前应用最普遍的局域网技术，替换了其他局域网标准如[令牌环](https://zh.wikipedia.org/wiki/令牌环)、[FDDI](https://zh.wikipedia.org/wiki/FDDI) 和 ARCNET。

为了使数据链路层能更好地适应多种局域网标准，802 委员会就将局域网的数据链路层拆成两个子层：

 * 逻辑链路控制 LLC ( Logical Link Control ) 子层

 * 媒体接入控制 MAC ( Medium Access Control ) 子层。

与接入到传输媒体有关的内容都放在 MAC 子层，而 LLC 子层则与传输媒体无关，
不管采用何种协议的局域网对 LLC 子层来说都是透明的，如下图所示：

 ![data_layer_mac_and_llc][1]


[1]: ../../../images/base/data_layer_mac_llc.png
