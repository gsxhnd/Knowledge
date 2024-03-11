---
title: Kind
---

<!-- markdownlint-disable MD025 -->

# Kind

## 启动多集群实例

```bash
# 创建集群
kind create cluster -n cluster-1
kind create cluster -n cluster-2
```

```bash
# 查看多集群列表
kind get clusters
```

## 删除集群

```bash
# 删除多个集群
kind delete cluster -n [集群名字]
# 删除全部集群
kind delete clusters -A
```