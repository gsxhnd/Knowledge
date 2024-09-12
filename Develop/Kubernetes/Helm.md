---
title: Helm
created: 2024-09-12 19:36
tags:
  - Kubernetes
  - Helm
---

<!-- markdownlint-disable MD025 -->

# Helm

Helm^[https://helm.sh/] 是一个 Kubernetes 包管理工具，用于简化 Kubernetes 应用程序的部署和管理。它允许用户定义、安装和升级复杂的 Kubernetes 应用程序。Helm 的核心概念包括：

Chart：一个 Helm 包，包含了定义 Kubernetes 资源所需的所有信息。Chart 可以看作是一个 Kubernetes 应用的模板。

Release：当一个 Chart 被部署到 Kubernetes 集群中时，它就成为了一个 Release。每个 Release 都有唯一的名称，并且可以被升级、回滚或删除。

Repository：存储 Helm Chart 的地方。用户可以从公共或私有的 Repository 中下载和安装 Chart。

Helm 的主要功能包括：

- 简化部署：通过 Helm Chart，用户可以轻松地定义和部署复杂的 Kubernetes 应用。
- 版本管理：Helm 支持对应用的版本进行管理，用户可以轻松地升级或回滚到之前的版本。
- 依赖管理：Helm Chart 可以包含其他 Chart 作为依赖，简化了复杂应用的部署。
- 模板化：Helm 使用 Go 模板语言，允许用户在部署时动态生成 Kubernetes 资源配置。
- Helm 是 Kubernetes 生态系统中非常重要的工具，广泛用于管理和部署 Kubernetes 应用。
