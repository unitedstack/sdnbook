## 私密网络通信

----

在实际场景中，经常会出现企业拥有多个 site 需要通过 internet 互联或者远程办
公的人员需要通过 internet 连接到公司的网络中，通常相应的网络通信内容还需要进行加密，那么就出现了相应的解决方案：VPN。

原则上 VPN 并不强调加密，只要保证了虚拟的网络通道即可，但是在实际应用中通常都
是加密的流量传输。

### Site-to-Site VPN

当在业务场景中，我们需要将两张或者多张网络进行互联的时候，通常我们需要在两台网络设备之间搭建 VPN，网络
设备可以是路由器、防火墙或者专业的 VPN 设备，常用的隧道方式则包括 IPsec、GRE
 或 BGP-MPLS 等，这类 VPN 通常搭建在公司的分支机构之间，对于普通用户来说并不需要进行相应的操作，
但需要网络管理员对各个站点的网络边缘设备进行相应的配置。

### End-to-End VPN

在实际业务场景中，经常出现外出办公的人员需要连入公司内部网络，那么就需要用到第二类的 vpn 即 End-to-End，
这类 vpn 又存在两种形态，可以是一台终端直接连接另一台终端，比如远程工作人员的
PC 直接连接到某台服务器上，
也可以是一台终端连接到某台网络设备，比如远程工作人员的 PC 拨号至公司的 VPN 
设备上，二者的区别则是从网络形态上，前者建立了一条两台终端之间的通路，
而后者则是将远程工作人员的终端纳入到了公司内部的网络中。End-to-End VPN 常见的
方式包括 IPsec、SSL 以及 PPTP 等。