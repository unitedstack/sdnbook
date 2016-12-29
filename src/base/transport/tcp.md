# TCP

---


## 简介

传输控制协议（Transmission Control Protocol，缩写为 TCP）是一种面向连接的、
可靠的、基于字节流的传输层通信协议，由 IETF 的[ RFC 793 ](https://tools.ietf.org/html/rfc793) 定义。

TCP 协议是一个可靠的面向连接的传输层协议，它将某结点的数据以字节流的形式无差
错地传送到互联网的任何一台机器上。发送方的 TCP 将用户递交的字节流划分成独立的
报文进行发送，而接收方的 TCP 将接收的报文重新装配上交给接收用户。
TCP 同时处理有关流量控制的问题，以防止快速的发送方"淹没"慢速的接收方。
一旦数据报被破坏或丢失，通常是 TCP 将其重新传输，而不是应用程序或 IP 协议。

## TCP 数据报的传输

发送和接收方 TCP 实体以数据报的形式交换数据。一个数据报包含一个固定的 20 
字节的头、一个可选部分以及 0 或多字节的数据。

TCP 数据报头的格式：

 ![tcp][1]


 * 源端口、目的端口：16 位长。标识出远端和本地的端口号。
 * 顺序号：32 位长。表明了发送的数据报的顺序。
 * 确认号：32 位长。希望收到的下一个数据报的序列号。
 * TCP 头长：4 位长。表明TCP头中包含多少个32位字。
 * 接下来的 6 位未用。
 * URG：报文携带了紧急数据，urgent offset有效。
 * ACK：ACK位置1表明确认号是合法的。如果ACK为0，那么数据报不包含确认信息，确认字段被省略。
 * PSH：表示是带有PUSH标志的数据。接收方因此请求数据报一到便可送往应用程序而不必等到缓冲区装满时才传送。
 * RST：用于复位由于主机崩溃或其它原因而出现的错误的连接。还可以用于拒绝非法的数据报或拒绝连接请求。
 * SYN：用于建立连接。
 * FIN：用于释放连接。
 * 窗口大小：16 位长。窗口大小字段表示在确认了字节之后还可以发送多少个字节。
 * 校验和：16 位长。是为了确保高可靠性而设置的。
它校验头部、数据和伪 TCP 头部之和。
 * 可选项：0个或多个32位字。包括最大TCP载荷，窗口比例、选择重发数据报等选项。


## TCP 三次握手和四次挥手

 ![all][7]


## TCP 三次握手

所谓三次握手(Three-way Handshake)，是指建立一个 TCP 连接时，
需要客户端和服务器总共发送3个包。

三次握手的目的是连接服务器指定端口，建立 TCP 连接，
并同步连接双方的序列号和确认号并交换 TCP 窗口大小信息.


 ![tcp_shake][2]

* 第一次握手:
客户端发送一个 TCP 的 SYN 标志位置 1 的包指明客户打算连接的服务器的端口，
以及初始序号 X ，保存在包头的序列号(Sequence Number) 字段里。

 ![hand1][3]

* 第二次握手:
服务器发回确认包(ACK) 应答。即 SYN 标志位和 ACK 标志位均为 1 同时，将确认序号(Acknowledgement Number) 设置为客户的 SYN 加1 .即 X+1。

 ![hand2][4]


* 第三次握手.
客户端再次发送确认包 (ACK) SYN 标志位为 0，ACK 标志位为 1 。并且把服务器发来 ACK 的序号字段 +1，
放在确定字段中发送给对方。并且在数据段放写 SYN 的 +1。

 ![hand2][5]


## TCP 四次挥手

TCP 的连接的拆除需要发送四个包，因此称为四次挥手(four-way handshake)。
客户端或服务器均可主动发起挥手动作。

 ![bye][6]

由于 TCP 是全双工的，因此关闭连接必须在两个方向上分别进行。
首先发起关闭的一方为主动关闭方，另一方为被动关闭方。四次挥手简单地分为三个过程：

 * 过程一. 主动关闭方发送 FIN，被动关闭方收到后发送该 FIN 的 ACK；
 * 过程二. 被动关闭方发送 FIN，主动关闭方收到后发送该 FIN 的 ACK；
 * 过程三. 被动关闭方收到 ACK。

在过程三后，被动关闭方就可以 100% 确认连接已经关闭，
因此它便可以直接进入 CLOSE 状态了，然而主动关闭的一方，它无法确定最后的那个
发给被动关闭方的 ACK 是否已经被收到。据 TCP 协议规范，不对 ACK 进行 ACK，
因此它不可能再收到被动关闭方的任何数据了，因此在这里就陷入了僵局，
TCP 连接的主动关闭方如何来保证圆圈的闭合？这里，协议外的东西起作用了，
IP 有一个超时值，即[ MSL ](https://en.wikipedia.org/wiki/Maximum_segment_lifetime )，MSL 表明这是 IP 报文在地球上存活的最长时间，[RFC793](https://tools.ietf.org/html/rfc793) 定义 MSL 为 2 分钟。

在 Linux 系统上，这个数值可以用这两个命令中的一种确认：

    sysctl net.ipv4.tcp_fin_timeout
    cat /proc/sys/net/ipv4/tcp_fin_timeout

### MSL 和 TTL 的区别

MSL 是 Maximum Segment Lifetime 的缩写，可译为"最长报文段寿命"，
它是任何报文在网络上存在的最长的最长时间，超过这个时间报文将被丢弃。
我们都知道 IP 头部中有个 TTL 字段，TTL 是 time to live 的缩写，
可译为"生存时间"，这个生存时间是由源主机设置设置初始值但不是存在的具体时间，
而是一个 IP 数据报可以经过的最大路由数，每经过一个路由器，它的值就减1，
当此值为 0 则数据报被丢弃，同时发送 ICMP 报文通知源主机。
RFC793 中规定 MSL 为 2 分钟，但这完全是从工程上来考虑，
对于现在的网络，MSL = 2 分钟可能太长了一些。因此 TCP 允许不同的实现可根据
具体情况使用更小的 MSL 值。TTL 与 MSL 是有关系的但不是简单的相等关系，
MSL 要大于 TTL。



参考文档：

* [TCP 的 TIME_WAIT 快速回收与重用](http://blog.csdn.net/dog250/article/details/13760985)
* [TCP 传输工作原理](http://www.cnblogs.com/mrsandstorm/p/5701691.html)
* [TCP 协议中的三次握手和四次挥手](http://blog.csdn.net/whuslei/article/details/6667471/)
* [TCP/IP 中 MSL 详解](http://www.voidcn.com/blog/10706198/article/p-5980195.html)
* [TCP 的那些事儿（上）](http://coolshell.cn/articles/11564.html)
* [TCP 的那些事儿（下）](http://coolshell.cn/articles/11609.html)


[1]: ../../../images/base/tcp.png
[2]: ../../../images/base/tcp_handshake.png
[3]: ../../../images/base/hand1.png
[4]: ../../../images/base/hand2.png
[5]: ../../../images/base/hand3.png
[6]: ../../../images/base/bye.png
[7]: ../../../images/base/tcp_all.png
