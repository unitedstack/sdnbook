# OpenContrail

----


## 简介

OpenContrail 是由瞻博网络公司开源的网络虚拟化和智能化解决方案，包含所有用于创建虚拟覆盖网络的组件：SDN 控制器、vRouter 和分析引擎。当进行网络配置时，Contrail 是连接物理网络与虚拟环境、配置底层服务、减少时间、降低成本和风险的一种简便方法。

和 VMware NSX 相似的是，OpenContrail 对通用路由封装（GRE）或虚拟可扩展局域网（VXLAN）技术提供了技术支撑。另一方面，OpenContrail 也和 NSX 方案一样能够控制网络设备硬件。两个平台方案的根本区别在于两者与编排系统的连接方式。OpenContrail被设计为能够在 OpenStack 云管理平台上工作，而 NSX 是和 Vmware 的云自动化中心（vCAC）相连。

值得注意的是，OpenContrail 通过一种分布式哈希表（DHT）NoSQL 方式来访问数据库以防止单点失败。

OpenContrail 系统由两个主要部件组成：OpenContrail 控制器和 OpenContrail 虚拟路由器

OpenContrail vRouter（虚拟路由器）是一个转发平面（或者是分布部署的路由器），运行在虚拟服务器的 hypervisor，将一个数据中心的物理路由器和交换机扩展成一个虚拟的 overlay 网络，OpenContrail 虚拟路由器从概念上和开源的 OVS 很像，但是他会提供路由以及更高层的服务（使用 vRouter 替代 vSwitch）。

OpenContrail 控制器提供了系统的逻辑集中控制平面和管理平面，并且协调管理 vRouter。该控制器是一个逻辑上集中但是物理上分布的 SDN 控制器，为虚拟网络提供管理、控制和分析功能。

更详尽的OpenContrail资料，请参见这里：[OpenContrail]( 
http://www.sdnap.com/sdnap-post/2886.html)
