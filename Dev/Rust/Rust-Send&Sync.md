---
title: Rust-Send&Sync
created: 2025-02-27 13:49
---
<!-- markdownlint-disable MD025 -->

# Rust-Send&Sync

首先看RustBook中对于这两个trait的说明：

- Send：如果一个类型实现了Send，那么暗示着这个类型可以在线程间转移所有权，
- Sync：如果一个类型实现了Sync，那么类型的值可以被多个线程进行不可变引用 。

换句话说，如果 &T是Send，那么T就是 Sync

这里需要注意的是，上面所提到是 **实现 了 Send 或 Sync的类型能够干啥**，但是没有提到满足什么样条件的类型才 **应该** 实现 Send 或 Sync。其实条件很简单，就是不会出现线程安全问题。那么什么样的类型可以实现Send或Sync就具有以下描述：

- Send：如果一个类型 跨线程转移 所有权 不会发生 **线程安全**问题，那么就可以实现 Send
- Sync：如果一个类型 跨线程 进行不可变引用 不会发生 **线程安全**问题，那么就可以实现 Sync

基于此，就容易理解标准库里 为啥有些类型实现了以上俩trait，有些没有实现了。

> 线程安全：没有数据竞争以及其它未定义行为。

### Synd + Sync

> 跨线程移动✔️，共享不可变引用✔️

```text
i8, f32, bool, char, &str, String...
,...
```

rust几乎所有的原始类型都是Send+Sync的。

### Send + !Sync

> 跨线程移动✔️，共享不可变引用❌

主要是一些具有内部可变性的类型，因为即使共享的是不可变引用，由于其内部可变性，多线程同时操作其内部的话也会导致[DataRace](https://zhida.zhihu.com/search?content_id=238537906&content_type=Article&match_order=1&q=DataRace&zhida_source=entity)

### !Send + Sync

> 跨线程移动❌，共享不可变引用✔️

这个类型是 Mutex<T>::lock().unwrap() 返回值类型. 这个是被故意实现为 !Send 的，要保证在获取锁的线程释放锁。

[MutexGuard and others should be Send · Issue #23465 · rust-lang/rust](https://link.zhihu.com/?target=https%3A//github.com/rust-lang/rust/issues/23465)

### !Send + !Sync

> 跨线程移动❌，共享不可变引用❌

Rc<T>:

- 不是Send是因为其引用计数不是原子操作，如果转移到了其它线程，多个线程同时 .clone() 的时候会导致引用计数DataRace。
- 不是Sync的原因和上面一样，因为 &Rc<T> 调用 clone() 会出现同样的问题

\*const T, \*mut T

- raw 指针也不行是因为太灵活了，很容易导致 DataRace。

## 关于跨线程进行不可变引用的拓展说明

第一次看到Sync定义的时候，尝试过复现一下跨线程进行不可变引用的场景，代码如下：

```rust
use std::thread;

fn main() {

    let value = 10;
    let ref_value = &value;
    thread::spawn(move || {println!("{}", 1 + ref_value)});
}
```

但是上述代码编译时会出现问题，错误信息如下

```text
  --> src/main.rs:9:21
   |
8  |     let value = 10;
   |         ----- binding \`value\` declared here
9  |     let ref_value = &value;
   |                     ^^^^^^ borrowed value does not live long enough
10 |     thread::spawn(move || {println!("{}", 1 + ref_value)});
   |     ------------------------------------------------------ argument requires that \`value\` is borrowed for \`'static\`
...
13 | }
   | - \`value\` dropped here while still borrowed
```

value的所有权的在主线程，会由主线程负责释放，但是编译器无法判断 主线程和子线程 线程退出的先后顺序，所以会报 lifetime 的错误。

看到上面报错信息，我就心想，难道如果想要跨线程 进行不可变引用的话，只能把值放到 全局变量/常量了吗？实际上不是的，我们可以在rayon中正常的使用（thread::scope中也可以。

```rust
// rayon = "1.8.0"
use rayon;

fn main() {

    let value = "hello".to_string();
    rayon::join(|| println!("{}", value), || println!("{}", value));

}
```

造成rayon::join 和 thread::spawn 的差异是 其形参的 trait bound。

```rust
pub fn thread::spawn<F, T>(f: F) -> JoinHandle<T>
where
    F: FnOnce() -> T,
    F: Send + 'static,
    T: Send + 'static,
{

pub fn rayon::join<A, B, RA, RB>(oper_a: A, oper_b: B) -> (RA, RB)
where
    A: FnOnce() -> RA + Send,
    B: FnOnce() -> RB + Send,
    RA: Send,
    RB: Send,
{
```

'static trait bound 导致 thread::spawn的时候需要将局部变量的所有权移到闭包中。

## Rust 自动triat推导

- 如果一个结构体的所有字段都是 Send，那么该结构体是 Send
- 如果一个结构体的所有字段都是 Sync， 那么该结构体是 Sync

## 参考资料

1. [Extensible Concurrency with the Sync and Send Traits](hhttps://doc.rust-lang.org/book/ch16-04-extensible-concurrency-sync-and-send.html)
2. [Comprehensive Rust](https://google.github.io/comprehensive-rust/concurrency/send-sync.html)
3. [https://www.ralfj.de/blog/2017/06/09/mutexguard-sync.html](https://www.ralfj.de/blog/2017/06/09/mutexguard-sync.html)
