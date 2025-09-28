---
title: Go 测试框架
created: 2025-09-28 14:22
---
<!-- markdownlint-disable MD025 -->

# Go 测试框架

理解 Go 测试框架中的这些核心组件，对于编写有效的测试至关重要。下面这个表格清晰地展示了它们的职责和关系。

| 名称 | 类型/角色 | 核心职责与使用场景 |
| :--- | :--- | :--- |
| **`go test`** | 命令行工具 | 驱动整个测试过程的命令，负责查找、编译并运行测试代码。 |
| **`testing.TB`** | 接口 | 是 **`testing.T`** (功能测试)、**`testing.B`** (性能测试) 和 **`testing.F`** (模糊测试) 的**公共接口**，定义了如 `Error()`, `Log()`, `Fail()` 等通用方法。主要用于编写可重用的测试辅助函数，该函数能同时接受 `*testing.T`、`*testing.B` 或 `*testing.F` 作为参数。 |
| **`testing.T`** | 结构体 (实现 `TB`) | **功能测试**。用于验证代码逻辑是否正确，提供错误报告、日志、跳过测试等功能。测试函数格式为 `func TestXxx(t *testing.T)`。 |
| **`testing.B`** | 结构体 (实现 `TB`) | **性能测试（基准测试）**。用于衡量代码性能，如运行时间和内存分配。测试函数格式为 `func BenchmarkXxx(b *testing.B)`，其核心是 `b.N`（迭代次数）。 |
| **`testing.F`** | 结构体 (实现 `TB`) | **模糊测试**。用于自动生成随机输入来探测代码的边界情况和潜在漏洞。测试函数格式为 `func FuzzXxx(f *testing.F)`。 |
| **`testing.M`** | 结构体 | **测试主入口**。用于在运行所有测试函数之前和之后进行全局的初始化和清理工作（如连接数据库、释放资源）。需定义 `func TestMain(m *testing.M)`。 |
| **`testing.PB`** | 结构体 | **并行循环控制器**。用于在基准测试中管理多个 goroutine 的并发执行，通常与 `b.RunParallel` 方法配合使用。其 `Next()` 方法用于协调并发迭代。 |

## 💡 核心概念详解与使用技巧

### 1. 驾驭 `go test` 命令

`go test` 是你进行测试的主要工具，它支持多种标志来控制测试行为：

- **常用标志**：
  - `-v`: 输出详细日志。
  - `-run`: 通过正则表达式选择运行哪些功能测试函数（如 `go test -run TestAdd$`）。
  - `-bench`: 运行性能测试（如 `go test -bench .` 运行所有）。
  - `-benchmem`: 在性能测试结果中显示内存分配情况。
  - `-count=1`: 禁用测试结果缓存，确保重新运行测试。

### 2. 编写有效的功能测试 (`testing.T`)

功能测试关注代码的正确性。`testing.T` 提供了丰富的方法来控制测试流程：

- **错误报告**：
  - `t.Error() / t.Errorf()`: 标记测试失败，但**继续**执行当前测试函数。
  - `t.Fatal() / t.Fatalf()`: 标记测试失败，并**立即终止**当前测试函数。
- **日志记录**：`t.Log() / t.Logf()` 输出信息，通常在 `-v` 开启或测试失败时显示。
- **组织测试**：使用 `t.Run()` 创建**子测试**，可以更好地组织测试用例并支持并行执行（通过 `t.Parallel()`）。

### 3. 进行精准的性能测试 (`testing.B`)

性能测试（基准测试）关注代码的性能表现。其核心是 `b.N`，测试框架会自动调整该值，直到获得稳定的测量结果。

- **控制计时器**：如果测试包含耗时的初始化代码，可以使用 `b.ResetTimer()`, `b.StopTimer()`, `b.StartTimer()` 来排除这些时间对结果的影响。
- **内存分析**：结合 `-benchmem` 标志，可以分析每次操作的内存分配次数和字节数，帮助优化内存使用。

### 4. 管理测试生命周期 (`testing.M`)

当测试需要全局的初始化和清理时（例如，建立数据库连接池），可以使用 `TestMain` 函数：

```go
func TestMain(m *testing.M) {
    // 测试前初始化
    fmt.Println("Setup...")
    code := m.Run() // 执行所有测试函数
    // 测试后清理
    fmt.Println("Teardown...")
    os.Exit(code)
}
```

### 5. 利用 `testing.TB` 编写通用辅助函数

`testing.TB` 接口的强大之处在于它允许你编写通用的测试辅助函数。例如，一个验证结果的函数可以同时用于功能测试和性能测试：

```go
// verifyResult 是一个通用的验证函数，既能用于 *testing.T，也能用于 *testing.B
func verifyResult(tb testing.TB, got, want int) {
    tb.Helper() // 标记此函数为辅助函数，错误信息将指向调用它的代码行
    if got != want {
        tb.Errorf("验证失败: 期望 %d, 实际得到 %d", want, got)
    }
}

// 在功能测试中使用
func TestAdd(t *testing.T) {
    result := Add(1, 2)
    verifyResult(t, result, 3)
}

// 在性能测试中也验证正确性
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        result := Add(i, i+1)
        verifyResult(b, result, 2*i+1)
    }
}
```
