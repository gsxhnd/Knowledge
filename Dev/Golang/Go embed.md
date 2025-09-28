---
title: Go Embed 内嵌资源
created: 2023-04-04 12:37
tags:
  - Golang
---
<!-- markdownlint-disable MD025 -->

# Embed

Go 语言从 1.16 版本开始，通过引入 `embed` 包和 `//go:embed` 编译器指令，原生支持将静态资源文件（如配置文件、HTML、CSS、JS、图片等）直接嵌入到最终编译生成的二进制程序中。这极大地简化了应用程序的分发和部署。

## 💡 什么是 Go Embed？

`//go:embed` 是一个**编译器指令**。它在编译阶段将指定的文件或目录内容读取出来，并初始化到与之关联的 Go 变量中。这样，在程序运行时，这些资源就“住”在了二进制文件内部，无需再依赖外部的物理文件。

## 🛠️ 基本用法与语法

使用 `//go:embed` 主要涉及三种变量类型，其基本语法规则如下：

| 变量类型 | 说明 | 适用场景 |
| :--- | :--- | :--- |
| `string` | 将整个文件内容嵌入为一个字符串 | 适用于嵌入单个**文本文件**，如配置文件、模板 |
| `[]byte` | 将整个文件内容嵌入为一个字节切片 | 适用于嵌入单个**二进制文件**或文本文件，如图片、字体 |
| `embed.FS` | 嵌入一个或多个文件、甚至整个目录结构，形成一个**只读的文件系统** | 最灵活和强大的方式，适用于嵌入大量静态资源，如整个网站的前端文件 |

**核心语法规则：**

1. **导入包**：必须导入 `embed` 包。如果只使用 `string` 或 `[]byte`，可以使用匿名导入 `import _ "embed"`。
2. **指令位置**：`//go:embed` 指令必须**紧挨着**出现在包级变量的声明之上，中间只允许有空行或行注释。
3. **路径规则**：指令中使用的路径是相对于当前 `.go` 源文件所在目录的。可以使用通配符，如 `*` 匹配任意非`.`开头的文件，`image/*` 匹配 `image` 目录下的所有文件。
4. **变量作用域**：变量必须在**包级别**声明，不能是局部变量，且不能有初始化值。

## 📝 代码示例

### 1. 嵌入为字符串或字节切片

适用于处理单个文件。

```go
package main

import (
    _ "embed" // 匿名导入
    "fmt"
)

//go:embed version.txt
var version string // 嵌入为字符串

//go:embed logo.png
var logoBytes []byte // 嵌入为字节切片，适用于图片等二进制文件

func main() {
    fmt.Println("Version:", version)
    // 处理 logoBytes...
}
```

### 2. 使用 embed.FS 嵌入文件系统

这是最常用的方式，可以灵活处理多个文件和目录。

```go
package main

import (
    "embed"
    "fmt"
    "io/fs"
    "net/http"
)

//go:embed static/*.html static/css/*.js static/images/*
var staticFS embed.FS // 嵌入 static 目录下的所有相关文件

func main() {
    // 1. 读取单个文件
    data, err := staticFS.ReadFile("static/index.html")
    if err != nil {
        panic(err)
    }
    fmt.Println(string(data))

    // 2. 列出目录内容
    dirEntries, _ := staticFS.ReadDir("static/images")
    for _, entry := range dirEntries {
        fmt.Println(entry.Name())
    }

    // 3. 作为静态文件服务器（关键步骤）
    // 使用 fs.Sub 获取 "static" 子目录的 FS，这样访问路径就不需要带 "static" 前缀了
    subFS, _ := fs.Sub(staticFS, "static")
    http.Handle("/", http.FileServer(http.FS(subFS)))
    http.ListenAndServe(":8080", nil)
}
```

## 💻 实际应用场景

1. **静态 Web 服务器**：将前端应用的 HTML、CSS、JS、图片等资源全部嵌入，只需部署一个二进制文件。
2. **模板嵌入**：与 `text/template` 或 `html/template` 结合，使用 `template.ParseFS` 函数直接从嵌入的文件系统解析模板，无需担心模板文件路径问题。
3. **内嵌配置文件**：将默认配置嵌入程序中，简化配置管理。
4. **命令行工具**：将必要的脚本、模板或数据文件嵌入，使工具更加自包含。

## ⚠️ 重要注意事项与限制

- **只读性**：通过 `embed.FS` 访问的文件是**只读**的，无法在运行时修改。
- **文件排除**：默认情况下，不会嵌入文件名以 `.` 或 `_` 开头的文件。如需包含，可在模式前加上 `all:` 前缀，如 `//go:embed all:config/.env`。
- **符号链接**：`//go:embed` **不支持**嵌入符号链接（软链接），但支持硬链接。
- **编译期决定**：嵌入的内容在编译时确定，运行时无法更改。这会增加二进制文件的大小。
- **路径访问**：当使用 `embed.FS` 时，访问文件需要包含指令中指定的完整路径。例如，`//go:embed static/index.html` 后，需要用 `f.ReadFile("static/index.html")` 来读取。使用 `fs.Sub` 可以解决路径前缀问题。
