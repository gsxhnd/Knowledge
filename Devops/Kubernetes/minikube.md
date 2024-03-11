---
title: Minikube
---

<!-- markdownlint-disable MD025 -->

# Minikube

## 创建 Minikube 集群

```bash
minikube start --image-mirror-country='cn' --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers' --container-runtime=containerd
```

## 创建多集群实例

```bash
minikube start --profile cluster1
minikube start --profile cluster2
```
