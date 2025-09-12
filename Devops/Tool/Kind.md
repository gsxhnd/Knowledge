---
title: Kind 使用指南
created: 2024-08-10 11:34
tags:
  - Kubernetes
---
<!-- markdownlint-disable MD025 -->

# Kind

**Kind** 是一款利用 Docker 容器“节点”运行本地 [Kubernetes](../Kubernetes/Kubernetes%20备忘清单.md) 集群的工具。主要设计用于测试 Kubernetes 自身，但也可用于本地开发或持续集成。

## 安装

假如有安装 go 1.16 版本

```bash
go install sigs.k8s.io/kind@v0.24.0
brew install kind
```

## 命令行

```bash
# 创建集群
kind create cluster --name cluster-1
kind create cluster --name cluster-2
# 查看多集群列表
kind get clusters
# 删除多个集群
kind delete cluster -n [集群名字]
# 删除全部集群
kind delete clusters -A
```

## Refer

- [Kind 官方文档](https://kind.sigs.k8s.io/)
