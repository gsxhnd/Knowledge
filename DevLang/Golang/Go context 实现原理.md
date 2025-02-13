---
title: Go context 实现原理
created: 2024-10-14 09:11
tags:
  - Golang
---

<!-- markdownlint-disable MD025 -->

# Context

在 Go 语言中，Context 是一个非常重要的概念，它用于在不同的 goroutine 之间传递请求域的相关数据，并且可以用来控制 `goroutine` 的生命周期和取消操作。除此之外，context 还兼有一定的数据存储能力。

## 核心结构数据

![context.Context](https://pic-1257414393.cos.ap-hongkong.myqcloud.com/Knowledge/7e0c24ff343c53e5dcfe8d9c06974359.jpeg)

在 Go 语言中，Context 被定义为一个接口类型，它包含了三个方法：

```go
# go version 1.18.10
type Context interface {
    Deadline() (deadline time.Time, ok bool)
    Done() <-chan struct{}
    Err() error
   Value(key any) any
}
```

- **Deadline()** 方法用于获取 Context 的截止时间，
- **Done()** 方法用于返回一个只读的 channel，用于通知当前 Context 是否已经被取消，
- **Err()** 方法用于获取 Context 取消的原因，
- **Value()** 方法用于获取 Context 中保存的键值对数据。

### 标准 error

```go
var Canceled = errors.New("context canceled")

var DeadlineExceeded error = deadlineExceededError{}

type deadlineExceededError struct{}

func (deadlineExceededError) Error() string   { return "context deadline exceeded" }
func (deadlineExceededError) Timeout() bool   { return true }
func (deadlineExceededError) Temporary() bool { return true }
```

1. Canceled：context 被 cancel 时会报此错误.
2. DeadlineExceeded：context 超时时会报此错误.

我们日常编写代码时，Context 对象会被被约定作为函数的第一个参数传递，eg：

```go
func users(ctx context.Context, request *Request) {
    // ... code
}
```

在函数中，可以通过 **ctx** 参数来获取相关的 Context 数据，举个超时的 eg：

```go
deadline, ok := ctx.Deadline()
if ok && deadline.Before(time.Now()) {
    // 超时
    return
}
```

## Context 控制 goroutine 的生命周期

在 Go 语言中，goroutine 是一种非常常见的并发编程模型，而 Context 可以被用来控制 goroutine 的生命周期，从而避免出现 goroutine 泄漏或者不必要的等待操作。

eg，看一下下方代码：

```go
func users(ctx context.Context, req *Request) {
    // 启动一个 goroutine 来处理请求
    go func() {
        // 处理请求...
    }()
}
```

上面的代码中，我们启动了一个 goroutine 来处理请求，但是没有任何方式来控制这个 goroutine 的生命周期，如果这个请求被取消了，那么这个 goroutine 就会一直存在，直到它完成为止。为了避免这种情况的发生，我们可以使用 Context 来控制 goroutine 的生命周期，eg：

```go
func users(ctx context.Context, req *Request) {
    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 处理请求...
    }(ctx)
}
```

在上面的代码中，我们将 Context 对象作为参数传递给了 goroutine 函数，这样在请求被取消时，goroutine 就可以及时退出。

## 使用 WithValue() 传递数据

除了用于控制 goroutine 的生命周期，Context 还可以被用来在不同的 goroutine 之间传递请求域的相关数据。为了实现这个目的，我们可以使用 Context 的 **WithValue()** 方法，eg：

```go
type key int

const (
    userKey key = iota
)

func users(ctx context.Context, req *Request) {
    // 从请求中获取用户信息
    user := req.GetUser
    // 将用户信息保存到 Context 中
    ctx = context.WithValue(ctx, userKey, user)
    
    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 从 Context 中获取用户信息
        user := ctx.Value(userKey).(*User)
    
        // 处理请求...
    }(ctx)

}
```

在上面的代码中，我们定义了一个 `key`类型的常量 `userKey`，然后在 `users()`函数中将用户信息保存到了 Context 中，并将 Context 对象传递给了 goroutine 函数。

在 goroutine 函数中，我们使用 `ctx.Value()`方法来获取 Context 中保存的用户信息。

**注：**

Context 中保存的键值对数据应该是线程安全的，因为它们可能会在多个 goroutine 中同时访问。

## 使用 WithCancel() 取消操作

除了控制 goroutine 的生命周期和传递数据之外，Context 还可以被用来执行取消操作。为了实现这个目的，我们可以使用 Context 的 `WithCancel()`方法，eg：

```go
func users(ctx context.Context, req *Request) {
    // 创建一个可以取消的 Context 对象
    ctx, cancel := context.WithCancel(ctx)

    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 等待请求完成或者被取消
        select {
        case <-time.After(time.Second):
            // 请求完成
            fmt.Println("Request finish")
        case <-ctx.Done():
            // 请求被取消
            fmt.Println("Request canceled")
        }
    }(ctx)

    // 等待一段时间后取消请求
    time.Sleep(time.Millisecond * 800)
    cancel()
}
```

在上面的代码中，我们使用 WithCancel() 方法创建了一个可以取消的 Context 对象，并将取消操作封装在了一个 cancel() 函数中。然后我们启动了一个 goroutine 函数，使用 select 语句等待请求完成或者被取消，最后在主函数中等待一段时间后调用 cancel() 函数来取消请求。

## 使用 WithDeadline() 设置截止时间

除了使用 WithCancel() 方法进行取消操作之外，Context 还可以被用来设置截止时间，以便在超时的情况下取消请求。为了实现这个目的，我们可以使用 Context 的 WithDeadline() 方法，eg：

```go
func users(ctx context.Context, req *Request) {
    // 设置请求的截止时间为当前时间加上 1 秒钟
    ctx, cancel := context.WithDeadline(ctx, time.Now().Add(time.Second))

    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 等待请求完成或者超时
        select {
            case <-time.After(time.Millisecond * 500):
            // 请求完成
            fmt.Println("Request finish")
            case <-ctx.Done():
            // 请求超时或者被取消
            fmt.Println("Request canceled or timed out")
        }
    }(ctx)

    // 等待一段时间后取消请求
    time.Sleep(time.Millisecond * 1500)
    cancel()
}
```

在上面的代码中，我们使用 WithDeadline() 方法设置了一个截止时间为当前时间加上 1 秒钟的 Context 对象，并将超时操作封装在了一个 cancel() 函数中。然后我们启动了一个 goroutine 函数，使用 select 语句等待请求完成或者超时，最后在主函数中等待一段时间后调用 cancel() 函数来取消请求。

**注：**

在使用 WithDeadline() 方法设置截止时间的时候，如果截止时间已经过期，则 Context 对象将被立即取消。

## 使用 WithTimeout() 设置超时时间

除了使用 **WithDeadline()** 方法进行截止时间设置之外，Context 还可以被用来设置超时时间。为了实现这个目的，我们可以使用 Context 的 **WithTimeout()** 方法，eg：

```go
func users(ctx context.Context, req *Request) {
    // 设置请求的超时时间为 1 秒钟
    ctx, cancel := context.WithTimeout(ctx, time.Second)

    // 启动一个 goroutine 来处理请求
    go func(ctx context.Context) {
        // 等待请求完成或者超时
        select {
        case <-time.After(time.Millisecond * 500):
            // 请求完成
            fmt.Println("Request completed")
        case <-ctx.Done():
            // 请求超时或者被取消
            fmt.Println("Request canceled or timed out")
        }
    }(ctx)

    // 等待一段时间后取消请求
    time.Sleep(time.Millisecond * 1500)
    cancel()
}
```

在上面的代码中，我们使用 WithTimeout() 方法设置了一个超时时间为 1 秒钟的 Context 对象，并将超时操作封装在了一个 cancel() 函数中。然后我们启动了一个 goroutine 函数，使用 select 语句等待请求完成或者超时，最后在主函数中等待一段时间后调用 cancel() 函数来取消请求。

**注：**

需要注意的是，在使用 WithTimeout() 方法设置超时时间的时候，如果超时时间已经过期，则 Context 对象将被立即取消。

在一个应用程序中，不同的 goroutine 可能需要共享同一个 Context 对象。为了实现这个目的，Context 对象可以通过函数调用或者网络传输等方式进行传递。

例如，我们可以在一个处理 HTTP 请求的函数中创建一个 Context 对象，并将它作为参数传递给一个数据库查询函数，以便在查询过程中使用这个 Context 对象进行取消操作。代码 eg：

```go
func users(ctx context.Context, req *Request) {
    // 在处理 HTTP 请求的函数中创建 Context 对象
    ctx, cancel := context.WithTimeout(ctx, time.Second)
    defer cancel()

    // 调用数据库查询函数并传递 Context 对象
    result, err := findUserByName(ctx, req)
    if err != nil {
        // 处理查询错误...
    }

    // 处理查询结果...
}

func findUserByName(ctx context.Context, req *Request) (*Result, error) {
    // 在数据库查询函数中使用传递的 Context 对象
    rows, err := db.QueryContext(ctx, "SELECT * FROM users WHERE name = ?", req.Name)
    if err != nil {
     // 处理查询错误...
   }
   defer rows.Close()
   // 处理查询结果...
}
```

在上面的代码中，我们在处理 HTTP 请求的函数中创建了一个 Context 对象，并将它作为参数传递给了一个数据库查询函数 `findUserByName()`。在 `findUserByName()` 函数中，我们使用传递的 Context 对象来调用 `db.QueryContext()` 方法进行查询操作。由于传递的 Context 对象可能会在查询过程中被取消，因此我们需要在查询完成后检查查询操作的错误，以便进行相应的处理。

**注：**

在进行 Context 的传递时，我们需要保证传递的 Context 对象是原始 Context 对象的子 Context，以便在需要取消操作时能够同时取消所有相关的 goroutine。如果传递的 Context 对象不是原始 Context 对象的子 Context，则取消操作只会影响到当前 goroutine，而无法取消其他相关的 goroutine。

## 总结

在 Go 语言中，Context 是一个非常重要的特性，包括其基本用法和一些高级用法。Context 可以用于管理 goroutine 的生命周期和取消操作，避免出现资源泄漏和死锁等问题，同时也可以提高应用程序的性能和可维护性。

在使用 Context 的时候，**需要注意以下几点：**

- 在创建 goroutine 时，需要将原始 Context 对象作为参数传递给它。
- 在 goroutine 中，需要使用传递的 Context 对象来进行取消操作，以便能够及时释放相关的资源。
- Context 的传递时，需要保证传递的 Context 对象是原始 Context 对象的子 Context，以便在需要取消操作时能够同时取消所有相关的 goroutine。
- 在使用 WithCancel 和 WithTimeout 方法创建 Context 对象时，需要及时调用 cancel 函数，以便能够及时释放资源。
- 在一些场景下，可以使用 WithValue 方法将数据存储到 Context 中，以便在不同的 goroutine 之间共享数据。

## Refer

- [Golang context 实现原理 - 知乎](https://zhuanlan.zhihu.com/p/597234214)
