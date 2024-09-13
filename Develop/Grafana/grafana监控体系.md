---
title: Grafana 监控体系
created: 2024-04-22 09:54
tags:
  - Grafana
---

<!-- markdownlint-disable MD025 -->

# Grafana 监控体系

观测平台

1. Log
2. Trace
3. Metrics

- [Grafana](./grafana.md): 监控面板 Dashboard
- Grafane [Loki](./loki.md): 日志
- Grafana Tempo^[https://grafana.com/oss/tempo/]: 链路追踪
- [Prometheus](./prometheus.md)^[https://prometheus.io/]: 指标、告警
- [Minio](../Database/minio.md): 数据持久化存储
- [Vector](./vector.md): 收集数据的 pipeline
- OpenTelemetry^[https://opentelemetry.io]

![otel](https://opentelemetry.io/img/otel-diagram.svg)

```text
App -> otel -> Vector -> Loki       -> Storage
App -> otel -> Vector -> Tempo       -> Storage
App -> otel -> Vector -> Prometheus       -> Storage

Grafan <-> Backend API
```
