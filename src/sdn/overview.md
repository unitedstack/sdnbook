# SDN 网络

----

## 什么是 SDN

 * 为什么需要一个全新的网络架构，比如SDN ?

    在传统的架构中，交换机和路由器不得不在操作上千种分布式协议的控制下实施整个网络的智能。
这就意味着，即使只有一个网元增加了一种新的协议，也需要所有其他网元做出相应的结构变更。
事实上，在网络中增加一种新的协议往往需要数年时间，才能最终完成标准化到实际部署的过程。

    SDN 使得网络可编程化，这就使得网络在满足用户的需求方面更加灵活。

 * SDN 的架构是怎么样的？

    SDN 将控制功能从网络交换设备中分离出来，将其移入到逻辑上独立的控制环境---网络控制系统中。
该系统可以在通用的服务器上运行，任何用户可以随时，直接进行控制功能编程。
控制功能不再局限于路由器中。控制系统提供一组 API，用户可以通过 API 对控制系统
进行监控、管理、维护。

## 控制平面和数据平面

  * 控制平面：是数据网络中做出转发决定的元素，如路由协议，
选路策略以及网络设备上运行这些协议的软硬件资源等。
其决定包括往那条路径上转发，是否要启用多条路径转发同一个数据流等。

  * 数据平面是指定控制平面转发决定的部分，包括数据封装解封装技术，
网络协议的告诉转发芯片等。

## OpenFlow 在 SDN 中扮演的角色是什么？

    OpenFlow 是 SDN 的三大关键要素之一。

    SDN 的第一关键要素是转发和控制分离，这使得网络交换机转发变得更加简单，高效；同时，控制变成可网络操作系统中一个相对集中的逻辑功能。

    第二大关键要是 OpenFlow 协议，它向交换机传送转发表，交换机依此转发报文。
这种做法与传统网络完全不同。在传统网络中，交换机和路由器需要自己决定报文的转发路径，照成成本增加，性能降低。

     第三个关键要素是具有一致性的，全系统范围的网络操作系统可编程接口，
他能让网络实现真正意义上的可编程或者软件定义网络。

    注：OpenFlow 协议不是必须的，可以通过其他途径，只需要将流量表信息传递给交换机。

## SDN 的好处

  * SDN 加快了新业务引入的速度。网络运营商可以通过可控的软件部署相关的功能，而不必像以前那样等待某个设备提供商在专有设备上添加相应的方案；

  * SDN 降低了网络的运营费用。消除了应用和特定网络的细节，比如，端口和 IP 关联，
使得无需花费时间和金钱配置网络设备；

  * SDN 有助于实现网络虚拟化。长期以来通过命令行接口进行人工配置，一直在阻碍网络向虚拟化迈进。

  * SDN 让网络乃至所有 IT 系统更好地以业务目标位导向。增加软件模块来增加 SDN 功能。

  * 简化网络部署。


#### 参考文献

 * [SDN 基础理解](http://blog.csdn.net/freezgw1985/article/details/16873677)