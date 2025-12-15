---
title: Tokio 指南
created: 2025-02-19 15:31
tags:
  - Rust
  - Tokio
category: Rust Tokio 指南
---
<!-- markdownlint-disable MD025 -->

# Tokio 指南

Tokio 是 Rust 编程语言的异步运行时。 它提供了编写网络应用程序所需的构建模块。 它提供了针对各种系统的灵活性，从拥有数多个内核的大型服务器到小型嵌入式设备。

在高层次上，Tokio 提供了几个主要组成部分：

- 用于执行异步代码的多线程运行时。
- 标准库的异步版本。
- 庞大的依赖库生态系统。

## Hello Tokio

### 生成一个新的项目

```shell
cargo new hell-tokio
cd  hello-tokio
```

### 添加依赖

打开 `Cargo.toml`，在 `[dependencies]` 下方添加以下内容：

```toml
tokio = { version = "1", features = ["full"] }
```

## Refer

- [深入浅出Rust异步编程之Tokio - 知乎](https://zhuanlan.zhihu.com/p/107820568)
