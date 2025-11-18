---
title: GRPC
created: 2025-11-17 10:17
---
<!-- markdownlint-disable MD025 -->

# GRPC

- [GRPC Gateway](https://grpc-ecosystem.github.io/grpc-gateway/docs/tutorials/introduction/)
- [Buf - Install](https://buf.build/docs/cli/installation/)
- [Buf 插件](https://buf.build/explore)

## 安装

```shell
# brew 安装 buf 可能存在问题
# 可以在 github 中下载二进制包 https://github.com/bufbuild/buf/releases
brew install bufbuild/buf/buf
```

```yaml
# buf.yaml
version: v2
name: buf.build/gsxhnd/grpc
lint:
  use:
    - STANDARD
deps:
  - buf.build/googleapis/googleapis
  - buf.build/grpc-ecosystem/gateway:v2.27.3

```

```yaml
# buf.gen.yaml
version: v2
managed:
  enabled: true
plugins:
  - remote: buf.build/protocolbuffers/go:v1.36.10
    out: .gen
    opt: paths=source_relative
  - remote: buf.build/grpc/go:v1.5.1
    out: .gen
    opt: paths=source_relative
  - remote: buf.build/grpc-ecosystem/gateway:v2.27.3
    out: .gen
    opt: paths=source_relative
```
