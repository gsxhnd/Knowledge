---
title: Helm 最佳实践
created: 2024-09-14 09:33
tags:
  - Kubernetes
  - Monitor
  - Helm
---

<!-- markdownlint-disable MD025 -->

# Helm 最佳实践

## 预先准备

- 安装helm
- k8s 环境，没有可以本地安装 [Kind](Kind.md) 创建一个本地 k8s 环境

## 模板

```bash
helm create monitor
```

会得到以下结构

```bash
monitor
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml
```

`templates/` 目录包括了模板文件。当Helm评估chart时，会通过模板渲染引擎将所有文件发送到`templates/`目录中。 然后收集模板的结果并发送给Kubernetes。

`values.yaml` 文件也导入到了模板。这个文件包含了chart的 _默认值_ 。这些值会在用户执行`helm install` 或 `helm upgrade`时被覆盖。

`Chart.yaml` 文件包含了该chart的描述。你可以从模板中访问它。`charts/`目录 _可以_ 包含其他的chart(称之为 _子chart_)。 指南稍后我们会看到当涉及模板渲染时这些是如何工作的。

### 快速查看 `monitor/templates/`

如果你看看 `monitor/templates/` 目录，会注意到一些文件已经存在了：

- `NOTES.txt`: chart的"帮助文本"。这会在你的用户执行`helm install`时展示给他们。
- `deployment.yaml`: 创建Kubernetes [工作负载](https://kubernetes.io/docs/user-guide/deployments/)的基本清单
- `service.yaml`: 为你的工作负载创建一个 [service终端](https://kubernetes.io/docs/user-guide/services/)基本清单。
- `_helpers.tpl`: 放置可以通过chart复用的模板辅助对象

然后我们要做的是... _把它们全部删掉！_ 这样我们就可以从头开始学习我们的教程。我们在开始时会创造自己的`NOTES.txt`和`_helpers.tpl`。

```console
rm -rf monitor/templates/*
```
