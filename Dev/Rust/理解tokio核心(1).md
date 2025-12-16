---
title: 理解tokio核心(1) runtime
created: 2025-03-03 13:56
---
<!-- markdownlint-disable MD025 -->

# 理解tokio核心(1): runtime

在使用tokio之前，应当先理解tokio的核心概念：runtime和task。只有理解了这两个核心概念，才能正确地、合理地使用tokio。

## 创建tokio Runtime

要使用tokio，需要先创建它提供的异步运行时环境(Runtime)，然后在这个Runtime中执行异步任务。

使用`tokio::runtime`创建Runtime：

```rust
use tokio;

fn main() {
  // 创建runtime
  let rt = tokio::runtime::Runtime::new().unwrap();
}
```

也可以使用Runtime Builder来配置并创建runtime：

```rust
use tokio;

fn main() {
  // 创建带有线程池的runtime
  let rt = tokio::runtime::Builder::new_multi_thread()
    .worker_threads(8)  // 8个工作线程
    .enable_io()        // 可在runtime中使用异步IO
    .enable_time()      // 可在runtime中使用异步计时器(timer)
    .build()            // 创建runtime
    .unwrap();
}
```

tokio提供了两种工作模式的runtime：

- 1.单一线程的runtime(single thread runtime，也称为current thread runtime)
- 2.多线程(线程池)的runtime(multi thread runtime)

> [!note]
> 这里的所说的线程是Rust线程，而每一个Rust线程都是一个OS线程。

IO并发类任务较多时，单一线程的runtime性能不如多线程的runtime，但因为多线程runtime使用了多线程，使得线程间的通信变得更为复杂，也加重了线程间切换的开销，使得有些情况下的性能不如使用单线程runtime。因此，在要求极限性能的时候，建议测试两种工作模式的性能差距来选择更优解。在后面深入了一些tokio后，我会再花一个小节来解释单一线程的runtime和多线程的runtime的调度区别以及如何选择合适的runtime。

默认情况下(比如以上两种方式)，创建出来的runtime都是多线程runtime，且没有指定工作线程数量时，默认的工作线程数量将和CPU核数(虚拟核，即CPU线程数)相同。

只有明确指定，才能创建出单一线程的runtime。例如：

```rust
// 创建单一线程的runtime
let rt = tokio::runtime::Builder::new_current_thread().build().unwrap();
```

例如，创建一个多线程的runtime，查看其线程数：

```rust
use tokio;

fn main(){
  let rt = tokio::runtime::Runtime::new().unwrap();
  std::thread::sleep(std::time::Duration::from_secs(10));
}
```

在另一个终端查看线程数：

```shell
$ ps -eLf | grep 'targe[t]'
15759    62 15759  6  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15761  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15762  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15763  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15764  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15765  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15766  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15767  0  9 20:42 pts/0  00:00:00 target/debug/async main
15759    62 15768  0  9 20:42 pts/0  00:00:00 target/debug/async main
```

总共9个OS线程，其中8个worker thread(我的电脑是4核8线程的)，外加一个main thread。

## async main

对于main函数，tokio提供了简化的异步运行时创建方式：

```rust
use tokio;

#[tokio::main]
async fn main() {}
```

通过 `#[tokio::main]` 注解(annotation)，使得 `async main` 自身成为一个async runtime。

`#[tokio::main]` 创建的是多线程runtime，还有以下几种方式创建多线程runtime：

```rust
#![allow(unused)]
fn main() {
#[tokio::main(flavor = "multi_thread"] // 等价于#[tokio::main]
#[tokio::main(flavor = "multi_thread", worker_threads = 10))]
#[tokio::main(worker_threads = 10))]
}
```

它们等价于如下没有使用 `#[tokio::main]` 的代码：

```rust
fn main(){
  tokio::runtime::Builder::new_multi_thread()
        .worker_threads(N)
        .enable_all()
        .build()
        .unwrap()
        .block_on(async { ... });
}
```

`#[tokio::main]` 也可以创建单一线程的main runtime：

```rust
#![allow(unused)]
fn main() {
#[tokio::main(flavor = "current_thread")]
}
```

等价于：

```rust
fn main() {
    tokio::runtime::Builder::new_current_thread()
        .enable_all()
        .build()
        .unwrap()
        .block_on(async { ... })
}
```

## Refer

- [理解tokio的核心(1): runtime - Rust入门秘籍](https://rust-book.junmajinlong.com/ch100/01_understand_tokio_runtime.html)
