# OpenvSwitch OVN

----

## 简介

OVS 团队于 2015 年 1 月推出了 C 语言倾向的 OVN 项目，由 VMWARE 公司主导，旨在提高基于 OpenvSwitch 的 OpenStack 网络方案的扩展性和易用性。

OVN 体系架构，如下图所示：

 ![ovs][1]

该项目用于给 OVS 这款在 OpenStack 项目中广泛使用的虚拟交换机引入一个轻量级的控制平面，即类似于 SDN 控制器，致力于提高基于 OVS 的 OpenStack 网络方案的扩展性和易用性，同时也为分布式路由等走捷径似的转发提供了可能性。

OpenvSwitch OVN 项目将租户的概念引入了 OVS，正式向 Neutron 方向发展。提供对 L2/L3 网络虚拟化的支持。从目前的趋势来看，Neutron 及 OpenStack 最好的归宿就是将精力集中于微内核，提供北向 REST API 建立生态圈，扩展的 Service 实现可以由第三方公司自己来做。

更加详尽的PDF资料，请参见：[OVN](http://openvswitch.org/support/slides/OVN-Vancouver.pdf)


[1]: ../../../images/sdn/ovn.png
