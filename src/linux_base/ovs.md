# OpenvSwitch

----

## 简介

Open vSwitch（下面简称为 OVS）是由 Nicira Networks 主导的，
运行在虚拟化平台（例如 KVM，Xen）上的虚拟交换机。在虚拟化平台上，
OVS 可以为动态变化的端点提供 2 层交换功能，很好的控制虚拟网络中的访问策略、网络隔离、流量监控等等。

OVS 遵循 Apache 2.0 许可证，能同时支持多种标准的管理接口和协议。
OVS 也提供了对 OpenFlow 协议的支持，用户可以使用任何支持 OpenFlow  
协议的控制器对 OVS 进行远程管理控制。

在 OVS 中，有几个非常重要的概念：

 * Bridge: Bridge 代表一个以太网交换机（Switch），一个主机中可以创建一个或者多个 Bridge 设备。
 * Port: 端口与物理交换机的端口概念类似，每个 Port 都隶属于一个 Bridge。
 * Interface: 连接到Port 的网络接口设备。在通常情况下，Port 和 Interface 
是一对一的关系, 只有在配置 Port 为 bond 模式后，Port 和 Interface 是一对多的关系。
 * Controller: OpenFlow 控制器。OVS 可以同时接受一个或者多个 OpenFlow 控制器的管理。
 * Datapath: 在 OVS 中，datapath 负责执行数据交换，
也就是把从接收端口收到的数据包在流表中进行匹配，并执行匹配到的动作。
 * Flow table: 每个 datapath 都和一个 `flow table` 关联，当 datapath  接收到
数据之后，OVS 会在 flow table 中查找可以匹配的 flow，执行对应的操作，
例如转发数据到另外的端口。

## OpenvSwitch 组件

OVS 的核心组件包括 ovsdb-server，ovs-vswitchd，ovs kernel module。如下图所示：

 ![ovs][1]

### ovs-vswitchd

OVS 守护进程，是 OVS 的核心部件，实现交换功能，和 Linux 内核兼容模块一起，
实现基于流的交换（flow-based switching）。

### ovsdb-server

OVS 轻量级的数据库服务器，用于整个 OVS 的配置信息，包括接口，交换内容，
VLAN 等等。ovs-vswitchd根据数据库中的配置信息工作。

### ovs-controller

一个简单的OpenFlow控制器。

### ovs kernel module

 * brcompat.ko : Linux网桥兼容模块。
 * openvswitch.ko : Open vSwitch核心数据转发平面模块。

### OpenvSwitch管理命令

 * ovs-dpctl：一个工具，用来配置交换机内核模块，可以控制转发规则。
 * ovs-vsctl：主要是获取或者更改 ovs-vswitchd 的配置信息，
此工具操作的时候会更新 ovsdb-server 中的数据库。
 * ovs-appctl：主要是向 OVS 守护进程发送命令的。
 * ovsdbmonitor：GUI 工具来显示 ovsdb-server 中数据信息，
可以远程获取 OVS 数据库和 OpenFlow 的流表。
 * ovs-ofctl：用来控制 OVS 作为 OpenFlow 交换机工作时候的流表内容。
 * ovs-pki：OpenFlow 交换机创建和管理公钥框架。
 * ovs-tcpundump：tcpdump 的补丁，解析 OpenFlow 的消息。

## OpenvSwitch 组件间通信方式

OVS 组件间通信如下图所示：

  ![work][2]

### 组件间的通信协议

ovs-vswitchd 与 controller：openflow协议。

ovs-vswitchd 与 ovsdb-server：OVSDB协议。

ovs-vswitchd 与内核：netlink。

OVS 中最重要的组件是 ovs-vswitchd，它实现了 OpenFlow 交换机的核心功能，
并且通过 netlink 协议直接和 OVS 的内核模块进行通信。
交换机运行过程中，ovs-vswitchd 还会将交换机的配置、数据流信息及其变化
保存到数据库 ovsdb 中，因为这个数据库由 ovsdb-server 直接管理，
所以 ovs-vswitchd 需要和 ovsdb-server 通过 UNIX socket 机制进行通信以
获得或者保存配置信息。数据库 ovsdb 的存在，使得 OVS 交换机的配置能够被持久化
存储，即便设备被重启后相关的 OVS 配置仍旧能够存在。

ovs-vsctl 组件是一个用于交换机管理的基本工具，它需要直接和 ovs-vswitchd 通信，
能够支持很多管理操作，用户可以登录到交换机部署的服务器上通过 ovs-vsctl 管理
OVS 交换机。同时，ovs-appctl 组件也是一个管理工具，通过发送一些内部命令给 
ovs-vswitchd 组件以改变其配置。另外，在一些情况下，用户可能会需要自行管理
运行在内核中的数据通路，那么也可以通过调用 ovs-dpctl 驱使 ovs-vswitchd 
在不依赖于数据库的情况下去管理内核空间中的数据通路。
当用户需要和 ovs-server 通信以进行一些数据库操作时，可以通过运行 ovsdb-client 组件访问 ovsdb-server，或者直接使用 ovsdb-tool 而不经 ovsdb-server 就对 ovsdb
b 数据库进行操作。
对于支持 OpenFlow 的 OVS 而言，支持集中部署的控制器对其进行远程管理和监控，
才是它所体现出的软件定义网络的核心。在 OVS 实现中，OpenFlow 是用于管理交换机
流表的协议。通过使用 ovs-ofctl，用户可以使用 OpenFlow 去连接交换机并在远程开
展监控和管理。另外，OVS 还提供了 sFlow 协议用于数据包的采样和监控。




[1]: ../../images/linux_base/ovs.png
[2]: ../../images/linux_base/ovs_work.png
