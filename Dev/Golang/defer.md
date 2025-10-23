---
title: defer
created: 2025-10-23 13:44
---
<!-- markdownlint-disable MD025 -->

# defer

Go语言中的`defer`是一种延迟执行机制，主要用于资源释放、错误处理和流程控制，遵循*后进先出（LIFO）*原则，其核心特性包括参数预解析、与`panic/recover`的协同工作等。

Go defer的核心要点

- **延迟执行**: 函数返回前或发生panic时执行，确保资源释放（如文件关闭、锁释放）。‌‌
- **‌执行顺序‌**: 多个defer按后进先出（LIFO）顺序执行。‌‌
- **‌参数求值‌**: defer参数在声明时立即求值，后续变量修改不影响已注册的defer参数值。‌‌
- **典型场景‌**: 文件操作（打开后立即defer关闭）、锁管理、错误恢复（与recover配合）。‌‌

## 执行顺序

## Refer

- [Go基础系列：defer、panic和recover](https://www.cnblogs.com/f-ck-need-u/p/9879198.html)
- [深入浅出 Go 语言的 defer 机制](https://zhuanlan.zhihu.com/p/689615742)
- [Go 语言设计与实现 - defer](https://draven.co/golang/docs/part2-foundation/ch05-keyword/golang-defer/)
