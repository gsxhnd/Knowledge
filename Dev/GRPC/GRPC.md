---
title: GRPC
created: 2025-11-17 10:17
---
<!-- markdownlint-disable MD025 -->

# GRPC

- [GRPC Gateway](https://grpc-ecosystem.github.io/grpc-gateway/docs/tutorials/introduction/)
- [Buf - Install](https://buf.build/docs/cli/installation/)

## 安装

```shell
# brew 安装 buf 可能存在问题
# 可以在 github 中下载二进制包 https://github.com/bufbuild/buf/releases
brew install bufbuild/buf/buf

go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway@latest
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```
