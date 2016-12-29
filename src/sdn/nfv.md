# NFV

---

## NFV (Network Function Virtualization)

引用维基百科的定义:

    Network functions virtualization (NFV) is a network architecture 
    concept that uses the technologies of IT virtualization to virtualize 
    entire classes of network node functions into building blocks that 
    may connect, or chain together, to create communication services.

网络功能虚拟化（NFV）是一种网络架构概念，其使用 IT 虚拟化技术将整个类别的网
络节点功能虚拟化为可连接或链接在一起以创建通信服务的构建块。

## NFV MANO (NFV Management and Orchestration)

NFV MANO 是 ETSI(European Telecommunications Standards Institute) 定义的一个 
NFV 架构。

NFV MANO架构主要包括三个部分：

NFV Orchestrator（NFV编排器）
VNF Manager（VNF管理器）
Virtualized Infrastructure Manager （虚拟基础设施管理器）

下面是 NFV MANO 架构图:

 ![mano][1]

NFV MANO  是图上天蓝色框内的部分，其中三个主要功能块 NFVO，VNFM，VIM 互相连接
，因为这是一个设计架构图，把连接称为 reference point，
在具体实现中这些连接就是功能块之间的 API 接口。

图上左边的部分从下往上看，最下面是 NFVI，往上是 VNF 实例，
再往上连接着网络运营商传统的 EMS(Element Management System) 网元管理系统和 
OSS/BSS(Operations support system / business support system) 运营支撑系统 / 
业务支撑系统，可以看到 MANO 架构中的 VNFM 和 NFVO 也分别和这两个系统有接口。

## NFVI(NFV Infrastructure)

NFVI 包括 NFV 底层物理的服务器，存储，网络设备，以及虚拟化后的虚拟机，虚拟存储，虚拟网络资源。


## VNFD(VNF Descriptor)

VNFD 就是一个 VNF 的部署模板，包含了一个 VNF 自身的所有信息。图上的 VNF Catalog，
就是所有 VNFD 的目录。


## VIM(Virtualized Infrastructure Manager)

VIM 通常是一个云平台，管理一个域下的 NFVI，在一个 NFV 架构下可能有多个 NFVI，
每个 NFVI 都有一个对应的 VIM 来管理。


## VNFM(VNF Manager)

VNFM 主要管理 VNF 的生命周期，负责包括调用 VIM，根据 VNFD(VNF Descriptor) 
创建，维护，终结 VNF，VNF 监控，VNF 的 self-healing，扩容缩容。
VNFM 可能有多个。


## NFVO(NFV Orchestrator)

我们知道可能有多个 VIM 管理多个域的 NFVI，也可能有多个 NFVM 分别管理它们的 VNF。
所以在上层还需要一个能协调管理这些资源和服务的东西，就是 NFVO。

对于 NFVI 的资源，NFVO 需要与多个 VIM 的接口交互，协调，认证，分配，
释放这些资源。对于 VNF 的服务，NFVO 需要：

1. 与多个 VNFM 的接口交互，分别创建这些 VNF。
2. 创建完 VNF 后，还要管理 VNF 的拓扑，对这些 VNF 进行编排，
也就是实现 VNFFG(VNF Forwarding Graph)，也就是 SFC(Service Function Chain)。



[1]: ../../../images/sdn/mano.png
