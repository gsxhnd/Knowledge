---
title: minio
created: 2024-09-12 09:47
tags:
  - Minio
  - 对象存储
---

<!-- markdownlint-disable MD025 -->

# Minio ^[https://min.io]

MinIO 是一个高性能的开源对象存储服务，它兼容亚马逊 S3 云存储服务接口，非常适合于存储大容量非结构化的数据，例如图片、视频、日志文件、备份数据和容器/虚拟机镜像等。MinIO 支持多种部署方式，包括单节点、多节点分布式部署，以及与容器化技术的集成。它提供了丰富的 API 接口，可以轻松地与其他应用程序集成。

## 特点

**简单、可靠**：Minio 采用简单可靠的集群方案，摒弃复杂的大规模的集群调度管理，减少风险与性能瓶颈，聚焦产品的核心功能，打造高可用的集群、灵活的扩展能力以及超过的性能。建立众多的中小规模、易管理的集群，支持跨数据中心将多个集群聚合成超大资源池，而非直接采用大规模、统一管理的分布式集群。

**功能完善**：Minio 支持云原生，能与 Kubernetes、Docker、Swarm 编排系统良好对接，实现灵活部署。且部署简单，只有一个可执行文件，参数极少，一条命令即可启动一个 Minio 系统。Minio 为了高性能采取无元数据数据库设计，避免元数据库成为整个系统的性能瓶颈，并将故障限制在单个集群之内，从而不会涉及其他集群。Minio 同时完全兼容 S3 接口，因此也可以作为网关使用，对外提供 S3 访问。同时使用 Minio Erasure code 和 checksum 来防止硬件故障。即使损失一半以上的硬盘，但是仍然可以从中恢复。分布式中也允许（N/2）-1 个节点故障。

## 架构

### **去中心化架构**

Minio 采用去中心化的无共享架构，对象数据被打散存放在不同节点的多块硬盘，对外提供统一命名空间访问，并通过负载均衡或者 DNS 轮询在各个服务器之间实现负载均衡

[![architecture_diagram](https://pic-cdn.ewhisper.cn/img/2021/08/10/df3cc09bd99bb57d6e1d1e1a06355cd8-architecture_diagram.svg+xml)](https://pic-cdn.ewhisper.cn/img/2021/08/10/df3cc09bd99bb57d6e1d1e1a06355cd8-architecture_diagram.svg+xml "architecture_diagram")

### **统一的命名空间**

Minio 有两种集群部署方式，一种是常见的本地分布式集群部署，一种是联盟模式部署。本地分布式集群部署即在多个本地服务器节点部署 Minio 服务，并将其组成单套分布式存储集群，并提供统一命名空间和标注的 S3 访问接口。联盟部署则是将多个本地 Minio 集群在逻辑上组成了统一命名空间，实现近乎无线的扩展与海量的数据规模管理，这些集群都可以在本地或者分布在不同地域的数据中心。

[![Architecture-diagram_distributed_32](https://pic-cdn.ewhisper.cn/img/2021/08/10/2c4485654bea1397abb152d5072c30f9-Architecture-diagram_distributed_32.png)](https://pic-cdn.ewhisper.cn/img/2021/08/10/2c4485654bea1397abb152d5072c30f9-Architecture-diagram_distributed_32.png "Architecture-diagram_distributed_32")

### **分布式锁管理**

与分布式数据库类似，Minio 也会存在面临数据一致性的问题: 一个客户端在读取一个对象的同时，另一个客户端可能正在修改或者删除这个对象。为了避免出现不一致的情况。Minio 专门设计并实现了 dsync 分布式锁管理器，来控制数据一致性。

- 任何一个节点的锁请求都会广播给集群内的所有在线节点
- 如果收到 N/2+1 个节点的同意，则获取所成功
- 没有主节点，每个节点互相对等，节点间通过 stale lock 检测机制，判断节点的状态及持有锁情况
- 由于设计简单，比较粗糙。有一定的缺陷性，最多支持 32 个节点。无法避免锁丢失的场景。不过基本满足可用需求。

| EC2 Instance Type | Nodes | Locks/server/sec | Total Locks/sec | CPU Usage |
| --- | --- | --- | --- | --- |
| c3.8xlarge(32 vCPU) | 8 | (min=2601, max=2898) | 21996 | 10% |
| c3.8xlarge(32 vCPU) | 8 | (min=4756, max=5227) | 39932 | 20% |
| c3.8xlarge(32 vCPU) | 8 | (min=7979, max=8517) | 65984 | 40% |
| c3.8xlarge(32 vCPU) | 8 | (min=9267, max=9469) | 74944 | 50% |

### **数据结构**

Minio 对象存储系统把存储资源组织为租户 - 桶 - 对象的形式

[![租户 - 桶 - 对象](https://pic-cdn.ewhisper.cn/img/2021/08/10/44b1bd0bc89ae228b407d11b4487115d-20210810154821.png)](https://pic-cdn.ewhisper.cn/img/2021/08/10/44b1bd0bc89ae228b407d11b4487115d-20210810154821.png "租户 - 桶 - 对象")

- **对象**：类似于 hash 表中的表 xiang 表项，名字是关键字，内容相当于值
- **桶**：是若干个对象的逻辑抽象，是盛装对象的容器
- **租户**：用于隔离存储资源。在租户下可以建立桶、存储对象
- **用户**：在租户下面创建的用于访问不同桶的账号。可以使用 minio 提供的 mc 命令设置不同用户访问各个桶的权限

### **统一域名访问**

Minio 集群扩展加入了新的集群或者桶后，对象存储的客户端程序需要通过统一的域名 /url 来访问数据对象，这个过程涉及了 etcd 与 CoreDns

[![img](https://pic-cdn.ewhisper.cn/img/2021/08/10/cb5c6dd6b127697acb4dc64917bc7af4-minio-global-federation.svg)](https://pic-cdn.ewhisper.cn/img/2021/08/10/cb5c6dd6b127697acb4dc64917bc7af4-minio-global-federation.svg "img")

### **存储机制**

Minio 使用纠删码 erasure code 和 checksum 来保护数据免受硬件故障和无声数据损坏。即使丢失一半数量 (N/2) 的硬盘，仍然可以恢复数据。

纠删码是一种恢复丢失和损坏数据的数学算法，目前纠删码技术在分布式存储系统中的应用分为三类，阵列纠删码 (Array code:RAID5、RAID6 等)、RS(Reed-solomon) 里德 - 所罗门类纠删码和 LDPC(LowDensity Parity Check Code)低密度奇偶检验纠删码。ErasureCode 是一种编码技术，它可以将份原始数据，增加 M 份数据，并能通过 N+M 份中的任意 N 分数据，还原原始数据。即如果有任意小于等于 M 份的数据丢失，仍然能通过剩下的数据还原。

Minio 采用 Reed-solomon code 将对象拆分成 N/2 数据和 N/2 奇偶检验快，这就意味着如果是 12 块盘，一个对象将会被分成 6 个数据块、6 个奇偶检验快，可以丢失任意 6 块盘(不管存放的数据快还是奇偶检验快)，让然可以从剩下的盘中的数据恢复。

在一个 N 节点的分布式 Minio 中，只要有 N/2 个节点在线，你的数据就是安全的。不过至少需要 N/2+1 个节点才能进行写操作。

将一个文件上传至 Minio 后, 对应磁盘上的信息如下：

[![img](https://pic-cdn.ewhisper.cn/img/2021/08/10/8e7eda59e60deeca22712506cb7efb3e-erasure-code.svg+xml)](https://pic-cdn.ewhisper.cn/img/2021/08/10/8e7eda59e60deeca22712506cb7efb3e-erasure-code.svg+xml "img")

其中 xl.json 为此对象的元数据文件。part.1 为此对象的第一个数据分片。(分布式中每一个节点都会存在这两个文件分别是数据块和奇偶检验快)在读取数据时 Minio 会对编码快进行 HighwayHash 编码，然后进行校验，以确保每个编码的正确性。基于 Erasure Code 和 Bit Rot Protection 的 HighwayHash 这两个特性，所以 Minio 的数据可靠性很高。

### **lambda 计算与持续备份**

Minio 支持 lambda 计算通知机制，即桶中的对象支持事件通知机制。当前支持的事件类型有：对象上传、对象下载、对象删除、对象复制等。当前支持事件接受系统有：redis、NATS、AMQP、Kafka、mysql、elasticsearch 等。

对象通知机制增强了 Minio 的扩展性，可以让用户通过自行开发来实现某些 Minio 未实现的功能。比如基于元数据的检索、与用户业务相关的计算等。同时也可以通过这个机制进行快速有效的增量备份。

### **对象存储网关**

Minio 除了可以作为存储系统服务外，还可以作为网关，后端可以与 NAS 系统、HDFS 系统等分布式文件系统或者 S3、OSS 这样的第三方存储系统。有了 Minio 网关，就可以为这些后端系统添加 S3 兼容的 API，便于管理和移植，因为 S3API 已经是对象存储界事实的标注。

[![multi-cloud-gateway](https://pic-cdn.ewhisper.cn/img/2021/08/10/bc63dbe4d410360a83b65479d8e7c137-multi-cloud-gateway.svg+xml)](https://pic-cdn.ewhisper.cn/img/2021/08/10/bc63dbe4d410360a83b65479d8e7c137-multi-cloud-gateway.svg+xml "multi-cloud-gateway")

用户通过统一的 S3 API 请求存储资源，通过 S3 API Router 将各个请求路由到对应的 ObjectLayer，每个 ObjectLayer 对应实现了各个存储系统的对象操作的所有 API。例如 GCS(Google cloud storage)实现了 ObjectLayer 接口后，它对于后端存储的操作就是通过 GCS 的 SDK 实现。当终端通过 S3 API 获取存储桶列表，那么最终的实现会通过 GCS 的 SDK 访问 GCS 服务获取存储桶列表，然后包装成 S3 标准的结构返回给终端。

## Refer

- [Minio 官网](https://min.io)
- [Minio 架构简介](https://ewhisper.cn/posts/9462/)
