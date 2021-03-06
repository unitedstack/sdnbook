## 保护内网的安全

----

### 有状态的访问控制
在网络发展初始，并没有专业的安全设备，那么如果希望能够将企业的内网与外网隔离开来并且仅仅放行有限的内外网访问权限，
最简单的实现方式就是在出口路由器上配置相应的访问控制列表，
由于访问控制列表同时可以记录会话的状态，就实现了简单的基于状态的访问控制。

基本上常见的路由器都支持相应的功能，通过访问控制列表的配置可以实现最简单的针对内网的保护。

### 防火墙
随着时间的推移，出口路由器的负担开始加重，逐渐演进出了专业的设备来实现相应的功能，那就是防火墙。

最初的防火墙的实现方式可以理解为一台专门维护各种访问策略的设备，同时为客户提供了较为友好的管理配置方式，随着企业网络拓扑的逐渐复杂，逐渐演进出了基于 zone 的防火墙，各厂商为客户预置了丰富的场景，企业在购买了防火墙之后只需要针对自己的网络连线做出相应的简单配置即可使用。
后来又逐渐发展出了下一代防火墙，让访问控制策略可以从网络层逐渐提高到应用层，同时提供了各种应用协议的管理与识别等功能，但其工作原理基本类似。

防火墙从形态上来说有两种主要形态，分别为软件和硬件，软件以 CheckPoint 为代表，硬件则以 Palo Auto 为代表，分别为客户提供软件/硬件的安全解决方案。

### 入侵检测、入侵防御与统一风险管理
仅仅能够配置策略是不足够的，很多貌似正常的流量经过组合后其实可以实现攻击者的某些功能，那么就需要专业的设备来对很多流量进行特征分析，并且识别其中的恶意流量。

最初的实现方式常见为在网络中旁路一台相应的检测设备，将所有的网络流量镜像到该设备上，此设备则会通过其内部逻辑去判断网络中是否存在恶意流量，这样的设备则被称为入侵检测系统。

入侵检测的特点是旁路流量，可以不影响在网的网络性能和吞吐，但是也带来了相应的问题：只能检测到恶意流量，无法对其相应的行为进行阻止，随着网络设备的性能的提升，厂商逐渐开始将相应的设备从旁路改为在线，这样的设备我们称之为入侵防御系统。

随着业务场景的逐渐丰富，客户主要对网络设备也提出了更多的要求，比如防病毒、数据泄露保护等内容，厂商也逐渐开始将满足这一类功能的特性集中在统一的安全设备上，
这样的设备被称之为 UTM。
