### ICMP

网络控制消息协定（Internet Control Message Protocol，ICMP）是网路协议族的核心
协议之一。它用于 TCP/IP 网络中发送控制消息，提供可能发生在通信环境中的各种问题反馈，
通过这些信息，令管理者可以对所发生的问题作出诊断，然后采取适当的措施解决。

ICMP 报文是在 IP 数据报内部被传输的。如下图所示：

 ![icmp][5]

ICMP 报文的格式如图所示：

 ![icmp][6]

下面通过 Wireshark 抓包讲解 IP 和 ICMP Request 报文的格式：

 ![icmp_request][7]

IP 报文：
  * 4 位 IP 版本号 + 4 位首部长度
  * Differentiated Services Fields ：[DSCP 相关](https://en.wikipedia.org/wiki/Differentiated_services)
  * total length：IP 报文总长度（包括 IP 头）
  * TTL：8 位 IP 包生存时间 TTL
  * protocol： 8 位协议 (TCP, UDP 或其他)，这里是 ICMP
  * checksum： 16 位 IP 首部校验和，最初置零，等所有包头都填写正确后，
    计算并替换。
  * sourceIP： 32 位源 IP 地址
  * destIP： 32 位目的 IP 地址


ICMP 报文：
  * Type：8，表示 ICMP Request
  * Code：0，表示 Reserved ( 注：有关所有的 Type 和 Code 可参考[ICMP Types and Codes](http://www.nthelp.com/icmp.html) )


### 参考文档

* [Understanding the ICMP Protocol](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwjh7fOZ6ZbRAhWDj5QKHQJjBz8QFggjMAE&url=http%3A%2F%2Fwww.windowsnetworking.com%2Farticles-tutorials%2Fnetwork-protocols%2FUnderstanding-ICMP-Protocol-Part1.html&usg=AFQjCNEUsB0RvcW6k05IiMhRsJpU1B1r6A)


[5]: ../../../images/base/icmp.png
[6]: ../../../images/base/icmp_format.png
[7]: ../../../images/base/ip_ip_header.png
