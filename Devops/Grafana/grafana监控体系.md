---
title: Grafana 监控体系
created: 2024-04-22 09:54
---

<!-- markdownlint-disable MD025 -->

# Grafana 监控体系

观测平台

1. Log
2. Trace
3. Metrics

Grafana: 监控面板 Dashboard
Grafane Loki: 日志收集程序
Grafana Tempo: Trace 收集
Prometheus: 指标收集程序
Vector: 收集数据的 pipeline <https://github.com/vectordotdev/vector>
OpenTelemetry <https://opentelemetry.io/>

![otel](https://opentelemetry.io/img/otel-diagram.svg)

```text
App -> otel -> Vector -> Loki       -> Storage
App -> otel -> Vector -> Tempo       -> Storage
App -> otel -> Vector -> Prometheus       -> Storage

Grafan <-> Backend API
```
