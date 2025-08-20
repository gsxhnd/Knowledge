---
title: Rust 标准库 Arc
created: 2025-01-10 14:53
tags:
  - Rust
---


<!-- markdownlint-disable MD025 -->

# Rust 标准库 Arc

线程安全的引用计数指针(reference-counting pointer)。`Arc`代表`原子引用计数(Atomically Reference Counted)`。

当线程之间所有权需要共享时，可以使用Arc（共享引用计数，Atomic Reference Counted）。这个结构通过 Clone 实现可以为内存堆中的值的位置创建一个引用指针，同时增加引用计数器。由于它在线程之间共享所有权，因此当指向某个值的最后一个引用指针退出作用域时，该变量将被删除。

`Arc<T>` 类型提供了在堆中分配的 `T` 类型值的共享所有权。 在 `Arc` 上调用 `clone` 会产生一个新的 `Arc` 实例，它指向堆上与源 `Arc` 相同的分配，同时增加引用计数。 当指向给定分配的最后一个 `Arc` 指针被销毁时，存储在该分配中的值（通常称为 "内值(inner value)"）也会被丢弃/销毁。

Rust 中的共享引用默认情况下不允许改变，`Arc` 也不例外：一般情况下，你无法在 `Arc` 中获得可变引用。 如果需要通过 `Arc` 进行改变，使用 `Mutex`,`RwLock` 或`原子类型`之一。

```rust
use std::sync::Arc;
use std::thread;

fn main() {
    // 这个变量声明用来指定其值的地方。
    let apple = Arc::new("the same apple");

    for _ in 0..10 {
        // 这里没有数值说明，因为它是一个指向内存堆中引用的指针。
        let apple = Arc::clone(&apple);

        thread::spawn(move || {
            // 由于使用了Arc，线程可以使用分配在 `Arc` 变量指针位置的值来生成。
            println!("{:?}", apple);
        });
    }
}
```

## 线程安全(Thread Safety)

## 使用 Weak 打破循环引用

循环引用问题：和`Rc`类似，`Arc`也可能会出现循环引用的问题，导致内存泄漏。例如，如果有两个`Arc`实例相互引用对方，它们的引用计数器永远不会变为0，从而无法释放内存。解决循环引用问题通常需要使用`Weak`指针。`Weak`指针是一种不增加引用计数的指针，可以用来打破循环引用。

## 克隆引用

## Refer

- <https://rustwiki.org/zh-CN/rust-by-example/std/arc.html>
- <https://doc.rust-lang.org/std/sync/struct.Arc.html>
