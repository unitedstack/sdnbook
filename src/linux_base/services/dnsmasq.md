# Dnsmasq

---

## 简介

Dnsmasq 提供 DNS 缓存和 DHCP 服务功能。作为域名解析服务器 (DNS)，dnsmasq 可以通过缓存 DNS 请求来提高对访问过的网址的连接速度。作为 DHCP 服务器，dnsmasq 可以用于为局域网电脑分配内网 IP 地址和提供路由。DNS 和 DHCP 两个功能可以同时或分别单独实现。dnsmasq 轻量且易配置，适用于个人用户或少于 50 台主机的网络。

## DHCP 服务

其中一些关键的配置如下，配置文件 `/etc/dnsmasq.conf` 中的注释已经给出了非常详细的解释。


```
# 服务监听的网络接口地址
#interface=
# Or you can specify which interface _not_ to listen on
#except-interface=
# Or which to listen on by address (remember to include 127.0.0.1 if
# you use this.)
listen-address=192.168.1.132,127.0.0.1
 
# dhcp动态分配的地址范围
dhcp-range=192.168.1.50,192.168.1.150,48h
 
# dhcp服务的静态绑定
dhcp-host=00:0C:29:5E:F2:6F,192.168.1.201
dhcp-host=00:0C:29:15:63:CF,192.168.1.202
 
# 设置默认租期
# Set the limit on DHCP leases, the default is 150
#dhcp-lease-max=150
 
# 租期保存在下面文件
#dhcp-leasefile=/var/lib/dnsmasq/dnsmasq.leases
 
# 通过 /etc/hosts 来分配对应的hostname
#dhcp-host=judge
 
# 忽略下面 MAC 地址的 DHCP 请求
#dhcp-host=11:22:33:44:55:66,ignore
 
# dhcp所在的domain
domain=debugo.com
 
# 设置默认路由出口
# dhcp-option 遵循 RFC 2132(http://tools.ietf.org/html/rfc2132)
# （Options and BOOTP Vendor Extensions) 可以通过 dnsmasq --help dhcp 
# 来查看具体的配置
dhcp-option=3,192.168.0.1
```


## DNS 服务

dnsmasq 能够缓存外部 DNS 记录，同时提供本地 DNS 解析或者作为外部 DNS 的代理，即 dnsmasq 会首先查找 `/etc/hosts` 等本地解析文件，然后再查找 `/etc/resolv.conf` 等外部 nameserver 配置文件中定义的外部 DNS。所以说 dnsmasq 是一个很不错的 DNS 中继。DNS 配置同样写入 dnsmasq.conf 配置文件里。


```
# 本地解析文件
#addn-hosts=/etc/banner_add_hosts
 
# Set this (and domain: see below) if you want to have a domain
# automatically added to simple names in a hosts-file.
# 例如，/etc/hosts中的os01将扩展成os01.debugo.com
expand-hosts
# Add local-only domains here, queries in these domains are answered
# from /etc/hosts or DHCP only.
local=/debugo.com/
 
# 强制使用完整的解析名
domain-needed
 
# 添加额外的上级 DNS 主机（nameserver）配置文件
#resolv-file=
 
# 不使用上级 DNS 主机配置文件(/etc/resolv.conf和resolv-file）
no-resolv
# 相应的，可以为特定的域名指定解析它的 nameserver。
# 一般是其他的内部 DNS name server
# 设置DNS缓存大小（单位：DNS解析条数）
# Set the size of dnsmasq's cache. The default is 150 names. 
# Setting the cache size to zero disables caching.
cache-size=500
 
# 关于log的几个选项
log-queries
 
# Log lots of extra information about DHCP transactions.
#log-dhcp
 
# Log to this syslog facility or file. (defaults to DAEMON)
log-facility=/var/log/dnsmasq.log
```
