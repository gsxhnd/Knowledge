---
title: Minikube 使用指南
created: 2024-08-10 11:34
---

<!-- markdownlint-disable MD025 -->

# Minikube

## 命令行

```bash
# 启动
minikube start --image-mirror-country='cn' --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers' --container-runtime=containerd
# 创建多集群实例
minikube start --profile cluster1
minikube start --profile cluster2
```
