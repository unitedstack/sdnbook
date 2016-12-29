# 网络配置

----

## 简介

如今很多系统管理员依然通过组合使用诸如 ifconfig、route、arp 和 netstat 
等命令行工具（统称为net-tools）来配置网络功能，解决网络故障。
net-tools 起源于 BSD 的 TCP/IP 工具箱，后来成为老版本 Linux 内核中配置网络功
能的工具。但自 2001 年起，Linux 社区已经对其停止维护。
同时，一些 Linux 发行版比如 Arch Linux 和 CentOS/RHEL 7 则已经完全抛弃了 net-tools，只支持 iproute2。

本节详细介绍 iproute2 在 Linux 网络下的使用。

## iproute2 的基本使用

注：以下所有命令的执行环境均为：CentOS 7.2

### 显示所有已连接的网络接口

  需要注意的是，通过 `ip link` 命令查看到的所有网络接口包括未激活的接口。

    [root@packstack ~]# ip link
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT qlen 1000
        link/ether fa:16:3e:08:e6:9c brd ff:ff:ff:ff:ff:ff

### 静态路由的查看、添加和删除

查看当前路由：

    [root@packstack ~]# ip route
    default via 192.168.0.1 dev eth0  proto static  metric 100
    192.168.0.0/24 dev eth0  proto kernel  scope link  src 192.168.0.9  metric 100

添加路由：

  添加静态路由：
  
    [root@packstack ~]# ip route add 192.168.1.0/24 dev eth0
    [root@packstack ~]# ip route add 192.168.2.0/24 via 192.168.0.10 dev eth0

  添加缺省路由：

    [root@packstack ~]# ip route add default via 192.168.0.1


删除路由：


    [root@packstack ~]# ip route delete 192.168.1.0/24
    [root@packstack ~]# ip route delete 192.168.2.0/24
    [root@packstack ~]# ip route delete default


### IP 地址的配置

通过 `ip` 命令可以给网络接口配置一个或多个 IP 地址，如下：

    [root@packstack ~]# ip addr add 192.168.0.100 broadcast 192.168.0.255 dev eth0
    [root@packstack ~]# ip addr add 192.168.0.200 broadcast 192.168.0.255 dev eth0

查看所有的 IP 地址：

    [root@packstack ~]# ip addr
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether fa:16:3e:08:e6:9c brd ff:ff:ff:ff:ff:ff
        inet 192.168.0.9/24 brd 192.168.0.255 scope global dynamic eth0
           valid_lft 57934sec preferred_lft 57934sec
        inet 192.168.0.100/32 brd 192.168.0.255 scope global eth0
           valid_lft forever preferred_lft forever
        inet 192.168.0.200/32 brd 192.168.0.255 scope global eth0
           valid_lft forever preferred_lft forever
        inet6 fe80::f816:3eff:fe08:e69c/64 scope link
           valid_lft forever preferred_lft forever

删除一个 IP 地址：

    [root@packstack ~]# ip addr delete 192.168.0.100/32 dev eth0
    [root@packstack ~]# ip addr delete 192.168.0.200/32 dev eth0


### 基于策略的路由(Linux Policy Routing)

Linux 有传统的基于数据包目的地址的路由算法，和新的基于策略的路由算法。
新算法优点：支持多个路由表，支持按数据报属性（源地址、目的地址、协议、端口、数据包大小、内容等）选择不同路由表。

查看所有的路由规则：

    [root@packstack ~]# ip rule list
    0:	from all lookup local
    32766:	from all lookup main
    32767:	from all lookup default
    
系统默认有3条记录

    0: from all lookup local
    32766: from all lookup main
    32767: from all lookup default

各部分解释
xx: 第一列数字是优先级，小的数字优先级高
lookup [xxx] : 表示搜索 xxx 路由表，1-252 之间的数字或名称
中间部分内容：如 from all， 这是规则

整行的意思就是，如果一个数据包符合规则（源地址、目的地址、协议、端口、数据包大小、内容等），则使用指定路由表。

Linux 缺省有 3 个路由表， local (不能改也不能删), main, 和 default。添加路由的时候如果不指定，缺省是添加到main路由表里的。

系统最多支持255个路由表。

    [root@packstack ~]# cat /etc/iproute2/rt_tables
    #
    # reserved values
    #
    255	local
    254	main
    253	default

 可以看到系统保留的表及对应名称，
  * 253:default 
  * 254:main 
  * 255:local
 可以自由添加自定义名称
 查看路由表命令，参数可用数字或名称


    [root@packstack ~]# ip route list table 254
    default via 192.168.0.1 dev eth0  proto static  metric 100
    192.168.0.0/24 dev eth0  proto kernel  scope link  src 192.168.0.9  metric 100
    192.168.1.0/24 dev eth0  scope link
    192.168.2.0/24 via 192.168.0.10 dev eth0

假如现在我们有两块网卡，网卡一的地址是 10.1.0.80，另一块网卡的地址是 10.1.16.80， 我们现在定义一条新的路由规则，匹配源地址 10.1.16.80 的数据包，
匹配路由表 218，并且路由表 218 的缺省网关为 10.1.16.1。

具体做法如下：

    # 匹配源地址是 10.1.16.80 的数据包，匹配路由表 218
    [root@packstack ~]# ip rule add from 10.1.16.80/32 lookup 218 

    # 在 table 218 中添加路由
    [root@packstack ~]# ip route add 10.1.16.0/24 table 100 dev eth2

    # 在 table 218 中添加缺省网关
    [root@packstack ~]# ip route change default via 10.1.16.1 table 218
    
当服务器中路由信息较为复杂时，可以通过 `ip route get` 命令查看路由匹配情况，
例如：

    [root@packstack ~]# ip route
    default via 192.168.0.1 dev eth0  proto static  metric 100
    192.168.0.0/24 dev eth0  proto kernel  scope link  src 192.168.0.9  metric 100
    192.168.1.0/24 dev eth0  scope link
    192.168.2.0/24 via 192.168.0.10 dev eth0
    [root@packstack ~]# ip route get 192.168.0.100
    192.168.0.100 dev eth0  src 192.168.0.9
        cache
