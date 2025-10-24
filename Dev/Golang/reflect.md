---
title: reflect 反射
created: 2025-10-24 14:00
---
<!-- markdownlint-disable MD025 -->

# 反射 reflect

[`reflect`](https://golang.org/pkg/reflect/) 实现了运行时的反射能力，能够让程序操作不同类型的对象。反射包中有两对非常重要的函数和类型，两个函数分别是：

- [`reflect.TypeOf`](https://draven.co/golang/tree/reflect.TypeOf) 能获取类型信息；
- [`reflect.ValueOf`](https://draven.co/golang/tree/reflect.ValueOf) 能获取数据的运行时表示；

## Refer

- [Reflect - Go 语言设计与实现](https://draven.co/golang/docs/part2-foundation/ch04-basic/golang-reflect/)
