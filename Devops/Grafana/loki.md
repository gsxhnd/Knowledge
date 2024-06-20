---
title: Loki 介绍
---

<!-- markdownlint-disable MD025 -->

# Loki 介绍

Loki 是用来接受、存储、处理、查询日志的集合体。

Loki 采用读写分离架构，关键组件有：

- Distributor 分发器：日志数据传输的“第一站”，Distributor 分发器接收到日志数据后，根据元数据和 hash 算法，将日志分批并行地发送到多个 Ingester 接收器上
- Ingester 接收器：接收器是一个有状态的组件，在日志进入时对其进行 gzip 压缩操作，并负责构建和刷新 chunck 块，当 chunk 块达到一定的数量或者时间后，就会刷新 chunk 块和对应的 Index 索引存储到数据库中
- Querier 查询器：给定一个时间范围和标签选择器，Querier 查询器可以从数据库中查看 Index 索引以确定哪些 chunck 块匹配，并通过 greps 将结果显示出来，它还会直接从 Ingester 接收器获取尚未刷新的最新数据
- Query frontend 查询前端：查询前端是一个可选的组件，运行在 Querier 查询器之前，起到缓存，均衡调度的功能，用于加速日志查询

![](https://ask.qcloudimg.com/http-save/yehe-3322396/bc8e98128eb38e8d4e00f440a8b86274.jpeg?imageView2/2/w/2560/h/7000)

loki 组件通信

Loki 提供了两种部署方式：

- 单体模式，ALL IN ONE：Loki 支持单一进程模式，可在一个进程中运行所有必需的组件。单进程模式非常适合测试 Loki 或以小规模运行。不过尽管每个组件都以相同的进程运行，但它们仍将通过本地网络相互连接进行组件之间的通信（grpc）。使用 Helm 部署就是采用的该模式。
- 微服务模式：为了实现水平可伸缩性，Loki 支持组件拆分为单独的组件分开部署，从而使它们彼此独立地扩展。每个组件都产生一个用于内部请求的 gRPC 服务器和一个用于外部 API 请求的 HTTP 服务，所有组件都带有 HTTP 服务器，但是大多数只暴露就绪接口、运行状况和指标端点。

![Loki组件架构](https://ask.qcloudimg.com/http-save/yehe-3322396/ec2f17d4cd32ec0e6ca3c8c0e4142112.jpeg?imageView2/2/w/2560/h/7000)

以 Helm 部署 Loki (StatefulSet 方式) 和 Promtail（DaemonSet 方式）采集 k8s pod 应用的日志为例
![](https://ask.qcloudimg.com/http-save/yehe-3322396/e6803b446f0e875f0ae03f5bf1bd9e3f.jpeg?imageView2/2/w/2560/h/7000)
