# 虚拟网络设备

----

## 简介

和磁盘设备类似，Linux 用户想要使用网络功能，不能通过直接操作硬件完成，
而需要直接或间接的操作一个 Linux 为我们抽象出来的设备，
既通用的 Linux 网络设备来完成。一个常见的情况是，系统里装有一个硬件网卡，
Linux 会在系统里为其生成一个网络设备实例，如 eth0，
用户需要对 eth0 发出命令以配置或使用它了。更多的硬件会带来更多的设备实例，
虚拟的硬件也会带来更多的设备实例。随着网络技术，虚拟化技术的发展，
更多的高级网络设备被加入了到了 Linux 中，使得情况变得更加复杂。

## Bridge

Bridge（桥）是 Linux 上用来做 TCP/IP 二层协议交换的设备，
与现实世界中的交换机功能相似。Bridge 设备实例可以和 Linux 上其他网络设备实例连接，
既 attach 一个从设备，类似于在现实世界中的交换机和一个用户终端之间连接一根网线。
当有数据到达时，Bridge 会根据报文中的 MAC 信息进行广播、转发、丢弃处理。


## TUN/TAP

TUN/TAP 设备是 Linux 内核中实现的虚拟网卡。物理网卡是从物理线路上收发数据包，
而 TUN/TAP 设备是从用户态应用程序上收发以太网帧或 IP 包。
用户态进程对 `/dev/net/tun` 文件调用 `open()` 获取一个文件描述符，
并调用 `ioctl()` 挂接到该设备上，接着通过读写该文件描述符从 TUN/TAP 设备的
收发数据包。收发的数据包由用户态进程构造好。TUN 和 TAP 设备的区别在于 TUN 
设备收发的是 IP 包，而 TAP 设备收发的是以太网帧。


## Network Namespace

一般情况下，Linux 的网络接口，路由表、协议栈、iptables 规则等资源由操作系统的
全部进程共享。通过使用 netowrk namespace, 可以将这些网络资源隔离开，只由 namespace 内的进程共享。


## Veth Pair

Virtual Ethernet Pair 简称veth pair, 是一对逻辑相连的端口或网络接口。
从其中一个端口进入的数据包将从另一端口流出。可以使用 veth pair 设备来连接
 Linux bridge 或者 OVS 的 bridge，也可以通过 veth pair 将两个 
network namespace 连接起来。

创建 Veth Pair：

```
ip link add dev veth0 type veth peer name veth1
```

## MACVLAN

一般情况下，网卡只有一个MAC地址。然而，有些场景下需要给一个网卡设置多个 MAC 地址。
Linux 通过 MACVLAN 技术在一个物理网卡上创建多个 MACVLAN 虚拟设备，
每个设备有着不同的 MAC 地址。当物理网卡收到数据包时，MACVLAN driver 
根据数据包 MAC 地址将数据包交由匹配的虚拟网卡处理。
使用 MACVLAN 可以替代使用 bridge 来连接物理网卡和虚拟网络设备。

创建 MACVLAN 设备并自动分配MAC地址：

```
ip link add link ens160 name mac0 type macvlan
```

也可以指定MAC地址创建设备：

```
ip link add link ens160 name mac1 address 52:22:33:44:55:66 type macvlan
```


## MACVTAP

虚拟化中一般使用 TAP 和 bridge 来组建虚拟网络，但这样组网结构会稍显复杂。
Linux 上的 MACTAP 设备可以简化这种结构。MACVTAP 设备集成了 MACVLAN 和 TAP 
设备二者的特性。它也可以基于一个物理网卡创建多个 MAC 地址不同的虚拟网卡，
同时虚拟网卡收到的包不再交给内核协议栈，而是通过 TAP 设备的文件描述符传递
到用户态进程。

创建 MACVTAP 设备：

```
ip link add link ens160 macvtap0 type macvtap mode bridge
```

参考文档

* [虚拟网络设备](http://www.just4coding.com/blog/2016/12/04/virtualnetworkdevice/)
