# CubeFS分布式存储和T4C归档集成

## CubeFS分布式存储介绍
CubeFS 是国内首个云原生开源分布式存储产品，2019 年开源并捐赠托管至云原生计算基金会(CNCF)。CubeFS产品自诞生以来，广泛的应用在各个行业、互联网大厂，软件管理着数十PB的数据，有效的支撑企业5G、用户相册等业务的数据存储，尤其是加速了当前人工智能大模型的数据访问效率。

<div align="center"> <img src=./pic/cubefs.png width=70% /> </div>

CubeFS分布式文件系统支持对象、文件和HDFS协议接口，整个系统有Master节点集群、Metadata元数据节点集群、Data数据节点集群、和对象网关组成。Master节点管理整个存储的物理资源和逻辑资源。 元数据集群管理分布式存储的扁平命名空间、层次命名空间、元数据切片等，整个元数据支持线性扩展，提供整个存储的容量和能力。数据节点管理存储的数据，可以多副本数据保护，也可以通过纠删来满足高可用的要求。 CubeFS的产品介绍，可以参考OPPO公司的分享：https://zhuanlan.zhihu.com/p/537839730。

## CubeFS分布式存储磁带归档

为了满足CubeFS分布式存储海量低成本低功耗存储的要求，CubeFS可以结合磁带技术，实现冷数据磁带存储。 整个归档架构由Master节点、Metadata元数据节点、Data数据节点和带库设备组成。 带库设备的带机可以直接连接CFS的数据节点，也可以通过SAN交换机来连接。CubeFS的文件系统客户端、或者客户S3对象存储可以直接访问存储在磁带上的数据。 

### 归档存储系统的硬件架构如下：

<div align="center"> <img src=./pic/cubefs_t4c.png width=70% /> </div>

通过上面硬件架构可以看出，磁带库设备只跟CubeFS的数据节点连接。数据节点上需要安装磁带归档软件和带库驱动，来支持CubeFS和带库集成，实现磁带归档存储的能力。

### 归档存储系统的软件架构如下：

<div align="center"> <img src=./pic/cubefs_t4c_mod.png width=70% /> </div>

## CubeFS分布式磁带归档系统清单

| 机器名字 | Master角色 | Metadata角色 | Data角色| 归档角色|
|:------:|:------:   |:------:     |:------:|:------:|
| node1 | Y | Y | Y | Y |
| node2 | Y | Y | Y | Y |
| node3 | Y | Y | Y | Y |