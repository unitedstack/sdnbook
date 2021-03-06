# DNS

---

## 简介

域名 (domain name) 是 IP 地址的代号。域名通常是由字符构成的。对于人类来说，字符构成的域名，比如 www.yahoo.com，要比纯粹数字构成的 IP 地址(106.10.170.118) 容易记忆。域名解析系统(DNS, domain name system) 就负责将域名翻译为对应的 IP 地址。在 DNS 的帮助下，我们可以在浏览器的地址栏输入域名，而不是 IP 地址。这大大减轻了互联网用户的记忆负担。另一方面，处于维护和运营的原因，一些网站可能会变更 IP 地址。这些网站可以更改 DNS 中的对应关系，从而保持域名不变，而 IP 地址更新。由于大部分用户记录的都是域名，这样就可以降低 IP 变更带来的影响。

从机器和技术的角度上来说，域名并不是必须的。但 Internet 是由机器和用户共同构成的。鉴于 DNS 对用户的巨大帮助，DNS 已经被当作 TCP/IP 不可或缺的一个组成部分。

## DNS 服务器

域名和 IP 地址的对应关系存储在 DNS 服务器 (DNS server)中。所谓的 DNS 服务器，是指在网络中进行域名解析的一些服务器(计算机)。这些服务器都有自己的 IP 地址，并使用 DNS 协议(DNS protocol) 进行通信。DNS 协议主要基于 UDP，是应用层协议。

 ![dns][1]

DNS 服务器构成一个分级(hierarchical) 的树状体系。上图中，每个节点(node) 为一个 DNS 服务器，每个节点都有自己的 IP 地址。树的顶端为用户电脑出口处的 DNS 服务器。在 Linux 下，可以使用 `cat /etc/resolv.conf`，在 Windows 下，可以使用 `ipconfig /all`，来查询出口 DNS 服务器。树的末端是真正的域名/ IP 对应关系记录。一次 DNS 查询就是从树的顶端节点出发，最终找到相应末端记录的过程。

## DNS 缓存

用户计算机的操作系统中的域名解析模块 (DNS Resolver) 负责域名解析的相关工作。任何一个应用程序(邮件，浏览器)都可以通过调用该模块来进行域名解析。

并不是每次域名解析都要完整的经历解析过程。DNS Resolver 通常有 DNS 缓存(cache)，用来记录最近使用和查询的域名/ IP 关系。在进行 DNS 查询之前，计算机会先查询 cache 中是否有相关记录。这样，重复使用的域名就不用总要经过整个递归查询过程。


## 反向DNS

上面的 DNS 查询均为正向 DNS 查询：已经知道域名，想要查询对应IP。而反向 DNS(reverse DNS) 是已经知道 IP 的前提下，想要查询域名。反向 DNS 也是采用分层查询方式，对于一个 IP 地址(比如106.10.170.118)，依次查询 in-addr.arpa 节点(如果是IPv6，则为ip6.arpa节点)，106节点，10节点，170节点，并在该节点获得 106.10.170.118 对应的域名。



[1]: ../../../images/base/dns.png
