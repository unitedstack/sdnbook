# VLAN

---

虚拟区域（Virtual Local Area Network 或简写 VLAN, V-LAN）是一种建构于局域网交
换技术（LAN Switch）的网络管理的技术，
网管人员可以借此通过控制交换机有效分派出入局域网的数据包到正确的出入端口，
达到对不同实体局域网中的设备进行逻辑分群（Grouping）管理，并降低局域网内大量数据流通时，因无用数据包>过多导致拥塞的问题，以及提升局域网的信息安全保障。(注：
来自 [维基百科](https://zh.wikipedia.org/wiki/虚拟局域网))

通俗的说，划分物理以太网产生的每一个广播域等同于一个逻辑上独立的以太网，
由于这些逻辑上独立的以太网存在于同一个物理以太网中，因而被称为虚拟局域网（VirtualLAN，VLAN）。说白了>就是自定义广播域。


[IEEE 802.1Q](https://zh.wikipedia.org/wiki/IEEE_802.1Q) 以及 VLAN Tagging
属于互联网下 IEEE 802.1 的标准规范，允许多个网桥在信息不被外泄的情况下公开的共用同一个实体网络。

传统的以太网数据帧在目的MAC地址和源MAC地址之后封装的是上层协议的类型字段，如下图所示：

 ![data_layer_normal][3]

其中 DA 表示目的 MAC 地址，SA 表示源 MAC 地址， Type 表示报文所属协议类型。
IEEE 802.1Q 协议规定在目的 MAC 地址和源 MAC 地址之后封装4个字节的 VLAN Tag，
用以标识 VLAN 的相关信息。

 ![data_layer_vlan][4]

如上图所示，VLAN Tag 包含四个字段，分别是：TPID（Tag Protocol Identifier，标签协议标识符）、Priority>、CFI（Canonical Format Indicator，标准格式指示位）和 VLAN ID。

  * TPID 用来判断本数据帧是否带有VLAN Tag，长度为16bit，缺省取值为0x8100。
  * Priority 表示报文的802.1P优先级，长度为3bit
  * CFI 字段标识 MAC 地址在不同的传输介质中是否以标准格式进行封装，
长度为1bit，取值为 0 表示 MAC 地址以标准格式进行封装，为 1 表示以非标准格式
封装，缺省取值为0。
  * VLAN ID 标识该报文所属 VLAN 的编号，长度为 12bit，取值范围为0 ～ 4095。
由于 0 和 4095 为协议保留取值，所以 VLAN ID 的取值范围为 1 ～ 4094。

网络设备利用 VLAN ID 来识别报文所属的 VLAN，根据报文是否携带 VLAN Tag 以及携带
的 VLAN Tag 值，来对报文进行处理。



[3]: ../../../images/base/data_layer_normal.png
[4]: ../../../images/base/data_layer_vlan.png
