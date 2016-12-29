# Midonet

----


## 简介

MidoNet 项目由日本 Midokura 公司开源，MidoNet 是一款网络虚拟化软件，
其基于底层物理设备来实现网络虚拟化，具有分布式、分散、多层次的特点。主要作为 OpenStack 系统中的默认网络组件，提供虚拟网络解决方案，为云平台如 OpenStack 服务
。MidoNet 是类似于 OpenContrail, Neutron DVR, DragonFlow, OVN 的 SDN 产品。

Midonet 网络解决方案，实现了很多企业级的特性，比如 BGP 的支持、Tunnel Zone、DoS 抵御隔离的支持等。

Midonet 充分借助了已有成熟的分布式软件降低自己本身的复杂性，而且只使用了
 OpenvSwitch 的 Datapath 模块，使转发和控制更加灵活，不失为一个好的设计。
但是其企业级服务还需要定制，对社区的部分高级功能也支持有限，这也是它的缺点。

Midokura 没有开源 MidoNet GUI 部分的代码。而 GUI 将作为企业版 MidoNet 的一部
分。企业版 MidoNet（MEM，Midokura Enterprise MidoNet）包括了 MidoNet 管理工
具和集成了 VMware 的 vSphere 技术及第三方管理工具，此外还包括 OpenStack 
发行认证等服务和支持等。

更详尽的资料，请参见： [Midonet](
http://docs.midonet.org/)


