---
title: Prometheus 架构原理
created: 2024-07-24 13:08
---

<!-- markdownlint-disable MD025 -->

# Prometheus 架构原理

<https://zhuanlan.zhihu.com/p/710612579>

在本指南中，我们将详细了解 Prometheus 架构，以有效地理解、配置和利用 Prometheus。

Prometheus 是一个用 Golang 编写的流行开源监控和警报系统，能够收集和处理来自各种目标的指标。您还可以查询、查看、分析指标并根据阈值收到警报。

此外，在当今世界，可观察性对于每个组织都变得至关重要，而 Prometheus 是开源领域的关键观测工具之一。

![动图](https://pic3.zhimg.com/v2-f6bcbd02b0293afab1601f7fef839992_b.webp)

Prometheus 主要由以下部分组成：

- Prometheus Server
- Service Discovery
- Time-Series Database (TSDB)
- Targets
- Exporters
- Push Gateway
- Alert Manager
- Client Libraries
- PromQL

让我们详细看看每个组件。

## Prometheus Server

Prometheus 服务器是基于指标的监控系统的大脑。服务器的主要工作是使用拉模型从各个目标收集指标。目标只不过是服务器、pod、端点等，我们将在下一个主题中详细介绍它们。使用 Prometheus 从目标收集指标的通用术语称为抓取。

![](https://pic3.zhimg.com/v2-0c57290da7110db7f7ddeef6e8a97352_b.jpg)

Prometheus 根据我们在 Prometheus 配置文件中给出的抓取间隔定期抓取指标。这是一个配置示例。

```text
global:
  scrape_interval: 15s
  evaluation_interval: 15s
  scrape_timeout: 10s

rule_files:
  - "rules/*.rules"

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']
```

## Time-Series Database (TSDB)

prometheus 接收到的指标数据随着时间的推移而变化（CPU、内存、网络 IO 等）。它被称为时间序列数据。因此 Prometheus 使用时间序列数据库（TSDB）来存储其所有数据。默认情况下，Prometheus 以高效的格式（块）将其所有数据存储在本地磁盘中。随着时间的推移，它会压缩所有旧数据以节省空间。它还具有删除旧数据的保留策略。TSDB 具有内置的机制来管理长期保存的数据。您可以选择以下任意数据保留策略。

- 基于时间的保留：数据将保留指定的天数。默认保留期为 15 天。
- 基于大小的保留：您可以指定 TSDB 可以容纳的最大数据量。一旦达到这个限制，普罗米修斯将释放空间来容纳新数据。

Prometheus 还提供远程存储选项。这主要是存储可扩展性、长期存储、备份和灾难恢复等所需要的。

## Prometheus Targets

Target 是 Prometheus 抓取指标的来源。目标可以是服务器、服务、Kubernetes Pod、应用程序端点等。

![](https://pic3.zhimg.com/v2-9070796b2f033447a019aa60788968be_b.jpg)

默认情况下，prometheus 会在目标的 /metrics 路径下查找指标。可以在目标配置中更改默认路径。这意味着，如果您不指定自定义指标路径，Prometheus 会在 /metrics 下查找指标。

目标配置位于 Prometheus 配置文件中的 scrape_configs 下。这是一个配置示例。

```text
scrape_configs:

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter1:9100', 'node-exporter2:9100']

  - job_name: 'my_custom_job'
    static_configs:
      - targets: ['my_service_address:port']
    metrics_path: '/custom_metrics'

  - job_name: 'blackbox-exporter'
    static_configs:
      - targets: ['blackbox-exporter1:9115', 'blackbox-exporter2:9115']
    metrics_path: /probe

  - job_name: 'snmp-exporter'
    static_configs:
      - targets: ['snmp-exporter1:9116', 'snmp-exporter2:9116']
    metrics_path: /snmp
```

Prometheus 需要来自目标端点的特定文本格式的数据。每个指标都必须换行。通常，这些指标使用 Prometheus exporters 来暴露。Prometheus exporter 通常和 target 伴生在一起。

## Prometheus Exporters

Exporter 就像在目标上运行的代理。它将指标从特定系统转换为普罗米修斯可以理解的格式。它可以是系统指标，如 CPU、内存等，也可以是 Java JMX 指标、MySQL 指标等。

![](https://pic1.zhimg.com/v2-ccc2af663f7640c5356968e69314c5ec_b.jpg)

默认情况下，这些转换后的指标由 Exporter 在目标的 /metrics 路径（HTTP 端点）上公开。例如，如果要监控服务器的 CPU 和内存，则需要在该服务器上安装 Node Exporter，并且 Node Exporter 以 prometheus 指标格式在 /metrics 上公开 CPU 和内存指标。一旦 Prometheus 提取指标，它将结合指标名称、标签、值和时间戳生成结构化数据。

社区有很多 Exporters 可用，但只有其中一些获得 Prometheus 官方认可。如果您需要更多自定义采集，则需要创建自己的导出器。Prometheus 将 Exporter 分为各个部分，例如数据库、硬件、问题跟踪器和持续集成、消息系统、存储、公开 Prometheus 指标的软件、其他第三方实用程序等。您可以从[官方文档](https://link.zhihu.com/?target=https%3A//prometheus.io/docs/instrumenting/exporters/)中查看每个类别的 Exporter 列表。

在 Prometheus 配置文件中，所有 Exporter 的详细信息将在 scrape_configs 下给出。

```text
scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter1:9100', 'node-exporter2:9100']

  - job_name: 'blackbox-exporter'
    static_configs:
      - targets: ['blackbox-exporter1:9115', 'blackbox-exporter2:9115']
    metrics_path: /probe

  - job_name: 'snmp-exporter'
    static_configs:
      - targets: ['snmp-exporter1:9116', 'snmp-exporter2:9116']
    metrics_path: /snmp
```

## Prometheus Service Discovery

Prometheus 使用两种方法从目标中获取指标。

- 静态配置：当目标具有静态 IP 或 DNS 端点时，我们可以使用这些端点作为目标。
- 服务发现：在大多数自动伸缩系统和 Kubernetes 等分布式系统中，目标不会有静态端点。在这种情况下，使用 prometheus 服务发现来发现目标端点，并且目标会自动添加到 prometheus 配置中。

![](https://pic4.zhimg.com/v2-cf2e17dbe770acfc97d8bee03b8561a3_b.jpg)

在进一步讨论之前，让我展示一个使用 kubernetes_sd_configs 的 Prometheus 配置文件的 Kubernetes 服务发现块的小示例。

```text
scrape_configs:
  - job_name: 'kubernetes-apiservers'
    kubernetes_sd_configs:
    - role: endpoints
    scheme: https
    tls_config:
      ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
    bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
    relabel_configs:
    - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
      action: keep
      regex: default;kubernetes;https
```

Kubernetes 是动态目标的完美示例。在这里，您不能使用静态目标方法，因为 Kubernetes 集群中的目标（pod）可能是短暂存活的。

Kubernetes 中还有基于文件的服务发现 file_sd_configs 。它适用于静态目标，但经典静态配置 static_configs 和 file_sd_configs 之间的主要区别在于，在这种情况下，我们创建单独的 JSON 或 YAML 文件并将目标信息保存在文件中。Prometheus 将读取文件来识别目标。

不仅这两种，还可以使用各种服务发现方法，例如 consul_sd_configs（prometheus 从 consul 获取目标详细信息）、ec2_sd_configs 等。如需了解更多配置细节，请访问[官方文档](https://link.zhihu.com/?target=https%3A//prometheus.io/docs/prometheus/1.8/configuration/configuration/)。

## Prometheus Pushgateway

Prometheus 默认使用 pull 方式来抓取指标。然而，有些场景需要将指标推送到 prometheus。让我们举一个在 Kubernetes cronjob 上运行的批处理作业的示例，该作业每天根据某些事件运行 5 分钟。在这种情况下，Prometheus 将无法使用拉机制正确抓取服务级别指标。因此，我们需要将指标推送到 prometheus，而不是等待 prometheus 拉取指标。为了推送指标，prometheus 提供了一个名为 Pushgateway 的解决方案。它是一种中间网关。

Pushgateway 需要作为独立组件运行。批处理作业可以使用 HTTP API 将指标推送到 Pushgateway。然后 Pushgateway 在 /metrics 端点上公开这些指标。然后 Prometheus 从 Pushgateway 中抓取这些指标。

![](https://pic1.zhimg.com/v2-8cf01bcace1a8daa47c05ba81c8afbdc_b.jpg)

Pushgateway 将指标数据临时存储在内存中。它更像是一个临时缓存。Pushgateway 配置也将在 Prometheus 配置中的 scrape_configs 部分下进行配置。

```text
scrape_configs:
  - job_name: "pushgateway"
    honor_labels: true
    static_configs:
    - targets: [pushgateway.monitoring.svc:9091]
```

要将指标发送到 Pushgateway，您需要使用 prometheus 客户端库对应用程序插桩，或使用脚本暴露指标。

## Prometheus Client Libraries

Prometheus 客户端库是可用于检测应用程序代码的软件库，以 Prometheus 理解的方式公开指标。如果您需要自行埋点插桩或想要创建自己的 Exporter，则可以使用客户端库。

一个非常好的用例是需要将指标推送到 Pushgateway 的批处理作业。批处理作业使用客户端库来埋点，以 prometheus 格式暴露指标。下面是一个 Python Client Library 的示例，它公开了名为 batch_job_records_processed_total 的自定义指标。

```python
from prometheus_client import start_http_server, Counter
import time
import random

RECORDS_PROCESSED = Counter('batch_job_records_processed_total', 'Total number of records processed by the batch job')

def process_record():
    time.sleep(random.uniform(0.01, 0.1))
    RECORDS_PROCESSED.inc()

def batch_job():

    for _ in range(100):
        process_record()

if __name__ == '__main__':

    start_http_server(8000)
    print("Metrics server started on port 8000")

    batch_job()
    print("Batch job completed")

    while True:
        time.sleep(1)
```

在使用客户端库时，prometheus_client 会在 /metrics 端点上公开指标。Prometheus 拥有几乎所有编程语言的客户端库，如果您想创建客户端库，也 OK。要了解更多创建指南和查看客户端库列表，您可以参考[官方文档](https://link.zhihu.com/?target=https%3A//prometheus.io/docs/instrumenting/clientlibs/%23client-libraries)。

## Prometheus Alert Manager

Alertmanager 是 Prometheus 监控系统的关键部分。它的主要工作是根据 Prometheus 警报配置中设置的指标阈值发送警报。警报由 Prometheus 触发（注意，是由 Prometheus 进程触发原始告警）并发送到 Alertmanager。Alertmanager 对告警去重、抑制、静默、分组，最后使用各类通知媒介（电子邮件、slack 等）发出告警事件。其具体功能：

- Alert Deduplicating：消除重复警报
- Grouping：将相关警报分组在一起
- Silencing：静默维护
- Routing：路由，根据严重性将警报路由到适当的接收者
- Inhibition：抑制，当存在中高严重性警报时停止低严重性警报的过程

![](https://pic1.zhimg.com/v2-62d0fcd6d124c662588a5048847ad7b4_b.jpg)

以下是警报规则的配置示例：

```text
groups:
- name: microservices_alerts
  rules:
  - record: http_latency:average_latency_seconds
    expr: sum(http_request_duration_seconds_sum) / sum(http_request_duration_seconds_count)
  - alert: HighLatencyAlert
    expr: http_latency:average_latency_seconds > 0.5
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High latency detected in microservices"
      description: "The average HTTP latency is high ({{ $value }} seconds) in the microservices cluster."
```

这是 Alertmanager 配置文件的路由配置示例：

```text
routes:
- match:
    severity: 'critical'
  receiver: 'pagerduty-notifications'

- match:
    severity: 'warning'
  receiver: 'slack-notifications'
```

Alertmanager 支持大多数消息和通知系统，例如 Discord、电子邮件、Slack 等，以将警报作为通知发送给接收者。

## PromQL

PromQL 是一种灵活的查询语言，可用于从 prometheus 查询时间序列指标。

> 译者注：这是 Prometheus 生态的重中之重，我之前写过一篇《[PromQL 从入门到精通](https://link.zhihu.com/?target=https%3A//download.flashcat.cloud/promql-learning.pdf)》，尽量融入了生产使用场景，读者可以下载学习。

我们可以直接从 Prometheus 用户界面使用查询，也可以使用 curl 命令通过命令行界面进行查询。

**Prometheus UI**

![](https://pic1.zhimg.com/v2-5ad58988f0c11501b2a5b68cabd8087c_b.jpg)

另外，当您将 prometheus 作为数据源添加到 Grafana 时，您可以使用 PromQL 来查询和创建 Grafana 仪表板，如下所示。

![](https://pic2.zhimg.com/v2-583b5bb566a76f62d13b5ca41efa51ed_b.jpg)

## 总结

这解释了 Prometheus 架构的主要组件，并将给出 Prometheus 配置的基本概述，您还可以使用配置做很多事情。每个组织的需求会有所不同，并且 Prometheus 在不同环境（例如 VM 和 Kubernetes）中的实现也有所不同。如果您了解基础知识和关键配置，您就可以轻松地在任何平台上落地它。

本文翻译自[这里](https://link.zhihu.com/?target=https%3A//devopscube.com/prometheus-architecture/)。读者既然读到这里了，说明对 IT 监控这块很感兴趣，如下内容可能也会对您有所帮助：

- [运维监控实战笔记](https://link.zhihu.com/?target=https%3A//time.geekbang.org/column/intro/100522501) 是我在极客时间写的一个专栏，对监控体系的方方面面都有涉及，已经有近万人学习了，您也可以试读一下。
- [github.com/ccfos/nightingale](https://link.zhihu.com/?target=https%3A//github.com/ccfos/nightingale) 是我们开源的运维监控系统，解决 Prometheus 告警规则管理的问题，可以 star 一下收藏备用。
- [FlashDuty 事件 OnCall 中心](https://link.zhihu.com/?target=https%3A//flashcat.cloud/product/flashduty/) 是统一的告警分发平台，一般公司都有多套监控系统（Zabbix、Prometheus、Nightingale、蓝鲸、云监控、Graylog、Skywalking、监控宝等），FlashDuty 可以对接各类监控系统，做统一的告警收敛降噪、排班、认领升级、报表统计、团队协同等功能。
