---
title: Axum
created: 2025-01-10 14:57
tags:
  - Rust
---
<!-- markdownlint-disable MD025 -->

# Axum

axum：构建在 Tokio 和 hyper 之上的现代网络框架，强调安全性、简洁性和高性能。

## Axum 简介

Rust 生态中一个流行的 Web 框架，以其简洁、高性能和对异步处理的良好支持而闻名。充分利用 `tower`[^1]和 `tower-http`[^2] 的中间件、服务和工具生态系统。

在Rust Axum框架中，使用 Router（路由） 来创建接口， 和 Java类比的话，那就是Java Spring Web项目中Controller类中定义的接口。创建 Router 时就要指定这个接口对应的 handler 方法。

- 使用 Route 定义接口
- 使用 handler 定义调用接口要执行的方法
- 使用 Extractors 解析传入的请求参数
- 在 handlers 之间共享 state

## Refer

- [Axum架构分析源码阅读笔记 - 知乎](https://zhuanlan.zhihu.com/p/14233666379)

[^1]: <https://github.com/tower-rs/tower>
[^2]: <https://github.com/tower-rs/tower-http>
