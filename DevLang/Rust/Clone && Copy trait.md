---
title: Clone && Copy trait
created: 2025-03-10 11:16
---


<!-- markdownlint-disable MD025 -->

# Clone && Copy trait

在 Rust 中，`Copy` 和 `Clone` trait 用于控制类型的复制行为。它们允许你定义如何复制类型的值，以及在什么情况下可以复制。

## `Copy` trait

`Copy` trait用于表示一个类型可以按位复制。当一个类型实现了 `Copy` trait 时，它的值可以在赋值、传参和返回值时自动复制

`Copy` trait 是一个标记 trait，它没有任何方法。它只是用来标记一个类型可以按位复制。

```rust
#[derive(Copy)]
struct Point {
    x: i32,
    y: i32,
}
```

要实现 `Copy` trait，你需要在类型定义上添加 `#[derive(Copy)]` 属性。此外，你还需要为该类型实现 `Clone` trait，因为所有实现了 `Copy` 的类型都必须实现 `Clone`。

```rust
#[derive(Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}
```

如果你试图为一个类型实现 `Copy` trait，但没有同时实现 `Clone` trait，那么编译器会报错。例如：

```rust
#[derive(Copy)]
struct Point {
    x: i32,
    y: i32,
}

// error[E0277]: the trait bound `Point: std::clone::Clone` is not satisfied
```

错误信息表明，`Point` 类型没有实现 `Clone` trait，因此不能实现 `Copy` trait。

这是因为所有实现了 `Copy` 的类型都必须实现 `Clone`。当你显式地调用 `clone` 方法时，Rust 会假定你知道自己在做什么，并且希望按位复制该值。因此，如果你想要为一个类型实现 `Copy` trait，你必须同时为它实现 `Clone` trait。

并不是所有类型都可以实现 `Copy` trait。只有满足以下条件的类型才能实现 `Copy`：

- 类型本身是 POD（Plain Old Data）类型，即不包含任何指针或引用。
- 类型的所有字段都实现了 `Copy`。

例如，下面这个类型就不能实现 `Copy`，因为它包含一个引用字段：

```rust
struct Foo<'a> {
    x: &'a i32,
}

// error[E0204]: the trait \`Copy\` may not be implemented for this type
impl Copy for Foo<'_> {}
```

`Copy` trait 允许你控制类型的复制行为。当一个类型实现了 `Copy` trait 时，它的值可以在赋值、传参和返回值时自动复制。这样，你就可以避免显式调用 `clone` 方法来复制值。

此外，由于 `Copy` 类型的值总是按位复制，所以它们的复制开销很小。这对于提高程序性能非常有帮助。

## `Clone` trait

与 `Copy` 不同，`Clone` 是一个普通的 trait，它包含一个方法：`clone`。这个方法用于创建一个新的副本。

```rust
#[derive(Clone)]
struct Point {
    x: i32,
    y: i32,
}
```

要实现 `Clone` trait，你需要在类型定义上添加 `#[derive(Clone)]` 属性或手动实现 `clone` 方法。

```rust
#[derive(Clone)]
struct Point {
    x: i32,
    y: i32,
}

// 或者手动实现 clone 方法
impl Clone for Point {
    fn clone(&self) -> Self {
        Self { x: self.x, y: self.y }
    }
}
```

几乎所有类型都可以实现 `Clone` trait。只要你能够定义如何创建一个新的副本，你就可以实现 `Clone` trait。

`Clone` trait 允许你显式地复制类型的值。这对于那些不能按位复制的类型非常有用，例如包含指针或引用的类型。

此外，`Clone` trait 还允许你自定义复制行为。你可以在 `clone` 方法中添加任何逻辑，以便在复制时执行特定的操作。

##  `Copy` 和 `Clone` 的区别和联系

`Copy` 和 `Clone` trait 都用于控制类型的复制行为，但它们之间还是有一些区别的。

- `Copy` 是一个标记 trait，它表示一个类型可以按位复制。当一个类型实现了 `Copy` trait 时，它的值可以在赋值、传参和返回值时自动复制。
- `Clone` 是一个普通的 trait，它包含一个方法：`clone`。当一个类型实现了 `Clone` trait 时，你可以调用它的 `clone` 方法来显式地创建一个新的副本。

此外，所有实现了 `Copy` 的类型都必须实现 `Clone`。这是因为当你显式地调用 `clone` 方法时，Rust 会假定你知道自己在做什么，并且希望按位复制该值。

## 实例分析

下面是一些使用 `Copy` 和 `Clone` 的代码示例：

```rust
#[derive(Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p1 = Point { x: 1, y: 2 };
    let p2 = p1; // 自动复制
    let p3 = p1.clone(); // 显式复制
}
```

在这个示例中，我们定义了一个 `Point` 类型，并为它实现了 `Copy` 和 `Clone` trait。然后，在 main 函数中，我们创建了一个 `Point` 值，并将它赋值给另一个变量。由于 `Point` 实现了 `Copy` trait，所以这个赋值操作会自动复制该值。此外，我们还调用了 `clone` 方法来显式地复制该值。

## Refer

- [Rust：详解 Copy 和 Clone 在 Rust 中，Copy 和 Clone trait 用于控制类型的复制行为 - 掘金](https://juejin.cn/post/7232665460713373755)
- [Clone in std::clone - Rust](https://doc.rust-lang.org/std/clone/trait.Clone.html)
