# 故障排查

----

## 网络故障排查思路

一般地，网络故障排查可以遵循以下几步：

    1. Find out at what level of TCP/IP stack(or some other stack) occurs 
       the problem.
    2. Understand what is the correct system behavior, and what is 
       deviation from normal system state.
    3. Try to express the problem in one sentence or in several words
    4. Using obtained information from buggy system, your own experience 
       and experience of other people(google, various forum, etc.), 
       try to solve the problem until success(or failure).
    5. If you fails, ask other people about help or some advice.
    

 * 首先，确定发生的网络故障属于 TCP / IP 的那一层；
 * 其次，理解在正常情况下，系统的状态应该是怎么样的；
 * 然后，使用最 `准确` 的语言描述问题；
 * 然后，在故障系统中尽可能多的获取有用信息，凭借自己的力量去解决；
 * 最后，求助别人。

多数情况下，当我们遇到了网络问题，第一反应就是求助别人，这时往往我们并没有
深入研究该问题，甚至连故障现象都描述不清，就直接求助别人。这时往往被求助的人
由于没有获取到完整的故障描述、故障现象和重现方法，导致被求助人要从零开始分析
问题，导致使用更多的人力资源。正确的网络排障过程应该是，发现问题者首先要
自己确定发生故障的原因，对于无法定位的问题，网上查询是否有相似问题和解决方案。
对于自己实在无法解决的问题，在求助他人之前，一定可以用最准确最精确的语言描述出
来故障现象以及重现方法。在解决问题后，一定要有总结经验的过程。


解决网络问题可以如下表示：

    SolveNetworkProblem(information_about_system_state,
                        my_experience,
                        people_experience)


 * information_about_system_state: 网络故障的信息、重现方法和系统状态
 * my_experience: 发现故障者自身的排障过程
 * people_experience: 包含两方面，网上搜索的过程和求助他人的过程


以下介绍在网络排障时经常使用到的命令：

 * ifconfig (or ip link, ip addr) - for obtaining information about network interfaces
 * ping - for validating, if target host is accessible from my machine. ping is also could be used for basic DNS diagnostics - we could ping host by IP-address or by its hostname and then decide if DNS works at all. And then traceroute or tracepath or mtr to look what's going on on the way to there.
 * dig - diagnose everything DNS
 * dmesg | less or dmesg | tail or dmesg | grep -i error - for understanding what the Linux kernel thinks about some trouble.
 * netstat -antp + | grep smth - my most popular usage of netstat command, which shows information about TCP connections. Often I perform some filtering using grep. See also the new ss command (from iproute2 the new standard suite of Linux networking tools) and lsof as in lsof -ai tcp -c some-cmd.
 * telnet <host> <port> - is very useful for communicating with various TCP-services(e.g. on SMTP, HTTP protocols), also we could check general opportunity to connect to some TCP port.
 * iptables-save (on Linux) - to dump the full iptables tables
 * ethtool - get all the network interface card parameters (status of the link, speed, offload parameters...)
 * socat - the swiss army tool to test all network protocols (UDP, multicast, SCTP...). Especially useful (more so than telnet) with a few -d options.
 * iperf - to test bandwidth availability
 * openssl (s_client, ocsp, x509...) to debug all SSL/TLS/PKI issues.
 * wireshark - the powerful tool for capturing and analyzing network traffic, which allows to analyze and catch many network bugs.
 * iftop - show big users on the network/router.
 * iptstate (on Linux) - current view of the firewall's connection tracking.
 * arp (or the new (Linux) ip neigh) - show the ARP-table status.
 * route or the newer (on Linux) ip route - show the routing table status.
 * strace (or truss, dtrace or tusc depending on the system) - is useful tool 
which shows what system calls does the problem process, it also shows error 
codes(errno) when system calls fails. This information often says enough 
for understanding the system behavior and solving a problem. 
Alternatively, using breakpoints on some networking functions in gdb can 
let you find out when they are made and with which arguments.
 * to investigate firewall issues on Linux: iptables -nvL shows how many 
packets are matched by each rule (iptables -Z to zero the counters). 
The LOG target inserted in the firewall chains is useful to see which 
packets reach them and how they have already been transformed when they 
get there. 
To get further NFLOG (associated with ulogd) will log the full packet.



注：以上部分内容来自 [Linux network troubleshooting and debugging](http://unix.stackexchange.com/questions/50098/linux-network-troubleshooting-and-debugging)
