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

- [Grafana](Grafana.md): 监控面板 Dashboard
- Grafana [Loki](Loki.md): 日志
- Grafana Tempo^[https://grafana.com/oss/tempo/]: 链路追踪
- [Prometheus](Prometheus.md): 指标、告警
- [Minio](../../Devops/Database/Minio/minio.md): 数据持久化存储
- [Vector](Vector.md): 收集数据的 pipeline
- [OpenTelemetry](https://opentelemetry.io)
- [Fluent Bit](https://docs.fluentbit.io/manual)
- [Jaeger Tracing](https://www.jaegertracing.io/docs/1.46/)
- [OpenObserve](https://openobserve.ai/docs/)

![otel](https://opentelemetry.io/img/otel-diagram.svg)

```text
App -> otel -> Vector -> Loki       -> Storage
App -> otel -> Vector -> Tempo       -> Storage
App -> otel -> Vector -> Prometheus       -> Storage

Grafan <-> Backend API
```
