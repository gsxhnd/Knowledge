---
title: Kind 使用指南
created: 2024-08-10 11:34
tags:
  - Kubernetes
---

<!-- markdownlint-disable MD025 -->

# Kind

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
