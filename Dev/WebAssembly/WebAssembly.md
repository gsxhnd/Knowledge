---
title: WebAssembly
created: 2024-09-13 13:05
tags:
  - WebAssembly
---

<!-- markdownlint-disable MD025 -->

# WebAssembly

## 🚀 什么是 WebAssembly？

WebAssembly（通常缩写为 Wasm）是一种二进制指令格式，它被设计为一种可移植的编译目标，允许使用 C、C++、Rust 等高级语言编写的代码在现代 Web 浏览器中以接近原生的速度运行。它并不是一门让开发者直接手写的语言，而是其他语言编译后产生的低级、紧凑的字节码。

WebAssembly 的核心目标是解决 JavaScript 在处理高性能应用时的性能瓶颈，例如复杂的游戏、图形渲染、科学计算等。它并非旨在取代 JavaScript，而是作为其强大的补充，两者可以协同工作，让开发者能够根据需求选择最合适的工具。

🎯 WebAssembly 的核心目标与设计理念

根据 W3C 标准，WebAssembly 的设计围绕以下几个关键目标展开：

•   高性能与高效率：其紧凑的二进制格式可以实现快速加载和解析，并且执行效率接近原生机器码。

•   可移植性：Wasm 模块是平台无关的，可以在支持 Wasm 的任何环境（如不同的操作系统和浏览器）中运行，实现了“一次编译，随处运行”。

•   安全性：代码在一个安全的沙箱化执行环境中运行，严格遵守 Web 的安全策略（如同源策略），无法直接访问系统资源，确保了用户的安全。

•   不破坏 Web 既有架构：它被设计为与 JavaScript 和其他 Web 技术（如 HTML 和 CSS）良好共存和交互。

## ⚙️ WebAssembly 如何工作？

理解 WebAssembly 的工作原理需要掌握几个核心概念：

1. 模块（Module）：这是编译后的 WebAssembly 二进制代码（.wasm 文件）。它类似于一个无状态的 ES 模块，声明了输入和输出。
2. 内存（Memory）：一个可调整大小的 ArrayBuffer，代表一个线性的内存空间，供 WebAssembly 的低级内存访问指令进行读写操作。
3. 表（Table）：一个存储函数引用等类型的安全数组，用于实现间接函数调用。
4. 实例（Instance）：一个已加载到内存中的模块及其运行时的所有状态（包括内存、表和导入值）的组合。实例化后，JavaScript 可以调用其导出的函数。

基本工作流程如下：
•   编译：开发者使用 C/C++、Rust 等语言编写代码，然后通过编译器工具链（如 Emscripten 或 Rust 的 wasm-pack）将其编译成 .wasm 模块。

•   加载与实例化：在 JavaScript 中，使用 WebAssembly.instantiate() 等 API 来获取 .wasm 文件，并将其编译和实例化。

•   交互：JavaScript 可以调用从 WebAssembly 实例导出的函数，反之，WebAssembly 也可以调用导入的 JavaScript 函数。由于 WebAssembly 不能直接操作 DOM，任何 DOM 交互都必须通过 JavaScript “胶水代码”来完成。

## ✨ WebAssembly 的优势与挑战

优势

•   卓越的性能：相比 JavaScript，WebAssembly 在启动速度和持续运行性能（吞吐量）上具有显著优势，特别适合计算密集型任务。

•   代码体积小：二进制格式非常紧凑，有助于减少加载时间。

•   多语言支持：为 Web 开发打开了新的大门，允许将现有的用 C/C++、Rust 编写的强大库和应用程序（如游戏引擎、音视频处理工具）移植到 Web 平台。

•   跨平台潜力：虽然最初为 Web 设计，但 WebAssembly 也可在服务器端（如 Node.js）或其他运行时环境中运行，展现出“通用运行时”的潜力。

挑战与现状

•   无法直接访问 DOM：这是目前的主要限制之一。所有 DOM 操作都必须通过 JavaScript 进行，这可能会引入一些性能开销。

•   内存管理：WebAssembly 自身不提供垃圾回收（GC）机制，使用 C/C++ 等语言时需要手动管理内存。不过，支持 GC 的语言（如 Kotlin、C#）的 Wasm 编译目标正在发展中。

•   调试体验：调试二进制格式的代码比调试 JavaScript 或文本格式的代码更具挑战性。尽管存在人类可读的文本格式（.wat），但调试工具链仍在不断完善中。

•   生态系统成熟度：虽然发展迅速，但相关的工具、库和社区支持相比成熟的 JavaScript 生态系统仍有差距。

## 💡 WebAssembly 的实际应用场景

WebAssembly 已经在许多高性能 Web 应用中证明了其价值：

•   游戏开发：Unity 和 Unreal 等大型游戏引擎支持将游戏编译为 WebAssembly，使得在浏览器中运行高质量的 3D 游戏成为可能。

•   音视频处理：例如，Bilibili 等网站使用 WebAssembly 版本的 FFmpeg 在浏览器内进行高效的视频编解码和处理。

•   图形设计与 CAD：Photoshop Web 版、Figma 和 AutoCAD Web 等复杂应用利用 WebAssembly 来实现在线版与桌面版相近的性能。

•   科学计算与机器学习：在浏览器中执行复杂的数学模拟、数据分析甚至机器学习模型的推理任务。

- [RustWasm](https://rustwasm.github.io/)
- [RustWasm Doc](https://rustwasm.github.io/book/introduction.html)
- [Wasm MDN](https://developer.mozilla.org/zh-CN/docs/WebAssembly/Rust_to_Wasm)
- Wasm spec <https://webassembly.github.io/spec/core/intro/index.html>
- Wasm org spec <https://webassembly.org/specs/>
