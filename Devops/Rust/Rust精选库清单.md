---
title: Rust精选库清单
created: 2024-03-12 09:57
---

<!-- markdownlint-disable MD025 -->

# Rust 精选库清单

> “ <https://Lib.rs>是一个 rust 库搜索服务网站，下面整理了其首页的 70 个类库分类和精选 creates，并增加了一句话介绍。”

## Rust patterns

_提供了一系列代码设计模式，帮助 Rust 开发者高效解决常见编程问题。_

- bitflags: 通过宏定义创建能代表一组位标志的安全类型，常用于表示一组布尔值。
- thiserror: 一个便捷的库，用于为 Rust 中的错误类型派生 Error 实现，简化错误处理。
- miette: 强调用户体验的库，提供了美观的错误报告和高级诊断功能。
- itertools: 提供了一系列额外的迭代器适配器、方法和宏，扩展了 Rust 标准库中的迭代器功能。
- once_cell: 提供了全局和本地懒加载变量的支持，允许单次初始化后的不可变访问。
- eyre: 提供了一种高度灵活的错误处理和报告方式，允许开发者定义错误报告的样式和上下文。
- ordered-float: 提供了浮点数封装器，使得不稳定的浮点数可以被完全排序，从而在集合类型等场合中使用。
- bytes: 专注于字节序列操作的库，用于简化字节切片（如网络缓冲）的处理。
- indoc: 一个宏，用于在源代码中嵌入经过适当缩进处理的多行字符串字面量，增强可读性。
- volatile: 提供了对原始指针的封装，允许更安全的对内存进行易失（volatile）读写操作。
- dyn-hash: 提供了可以操作动态（dyn）类型对象的 Hash 特征，用于构建能够接受任何实现了该特征的对象的通用哈希集合。
- glib: Rust 语言中 GLib 库的绑定，使得开发者可以在 Rust 中使用 GLib 及相关库提供的底层操作和结构。
- const-type-layout: 通过派生宏相对于原生 Rust 类型描述（TypeLayout），实现了类型布局信息的常量表示，有利于在编译时检查结构体布局。
- rustler: 一个允许 Rust 代码以外部函数接口的形式集成到 Erlang 虚拟机的框架。
- rustrix: 专注于线性代数，特别是矩阵运算，提供了运算宏和基本函数来操作矩阵。
- cel-interpreter: 实现了通用表达式语言（CEL）的解释器，用于执行 CEL 表达式，常见于策略和规则引擎。

## 网络编程（Network programming）

_例如 FTP、HTTP、SSH 或更低级别的 TCP 或 UDP 等网络协议。_

- socket2：对网络套接字进行高度可配置的处理，使得底层网络编程更加灵活。
- ipnet：提供用于处理网络地址和子网的数据类型，便于实现 IP 网络计算。
- rustls-native-certs：允许 rustls 使用平台本地的证书存储，以支持 TLS 加密通信。
- native-tls：提供一个跨平台的 TLS API，封装各个操作系统的本地 TLS 实现。
- async-graphql：一个强大、类型安全的 GraphQL 服务器实现，支持异步处理。
- tower：提供网络服务构建的抽象层和中间件，旨在简化构造和组合网络服务。
- aws-sdk-s3：亚马逊简单存储服务（Amazon S3）的官方 AWS SDK 客户端，用于 Rust。
- quinn：基于 Rust 的 QUIC(快速 UDP 互联网连接)协议实现，支持高效可靠的传输。
- port_check：用于检查本地端口是否可用或远程端口的连通性。
- rqbit：用 Rust 编写的 bittorrent 客户端和服务器，功能丰富且高效。
- tokio-modbus：一个异步的 Modbus 库，基于 Tokio 运行时，适合构建实时通信应用。
- cidr_calc：方便计算 CIDR（无类别域间路由选择）相关子网和 IP 地址范围的库。
- azure_devops_rust_api：用于访问 Azure DevOps 服务的 Rust API 库，允许集成和自动化工作流。

## 数据结构（Data structures）

_为特定目的实现的 Rust 数据结构。_

- hashbrown：一种高效的哈希表实现，基于 Google 的 SwissTable 设计，提供快速的查找、插入和删除操作。
- bitvec：让你能够以单个比特为单位操作内存，用于创建紧凑的位数组和位字段。
- phf：静态生成的完美哈希函数库，允许在编译时创建高效的查找表。
- indexmap：保证插入顺序的哈希表映射和集合，结合了快速插入和快速迭代的优势。
- half：提供半精度浮点数(f16 和 bf16)的 Rust 实现，适用于精简的浮点计算。
- num：收集了各种数字类型和相关特征（Trait），包括整数、浮点数、有理数和大整数等。
- ropey：为编辑和其他文本操作而设计的 Rust 文本绳索数据结构，提供高性能。
- priority-queue：实现了优先队列数据结构，基于二叉堆算法，支持动态元素优先级变化。
- smallvec：实现小容量优化的向量，在堆栈上存储少量元素以避免堆分配。
- fixedbitset：简单且高效的位集合实现，用于处理固定大小的比特集合。
- yrs：基于 Yjs CRDT 算法的 Rust 实现，支持同步合作编辑等功能需求。
- char-list：提供了类似于字符串但持久（immutable）的特性的字符列表集合。
- kbs-types：定义了可序列化和反序列化的 Rust 类型，主要用于 KBS 系统。
- any-range：支持 Rust 标准库中各种 Range 类型的枚举，赋予更大的灵活性解析和操作范围。
- more_collections：提供了一系列额外的集合类型，补充标准库中的 collections 模块。
- xot：一款功能全面的 Rust XML 处理库，支持构建和操作 XML 树结构。
- bao-tree：结合 BLAKE3 哈希算法，提供可以逐块校验而不是一次性校验整个流的功能。

## 算法

_包括哈希、排序和搜索等核心算法。_

- rand：用于 Rust 语言的综合随机数生成库，包括各种随机数生成器以及随机性功能和分布。
- crc：提供 CRC（循环冗余校验）算法的实现，适用于多种 CRC 标准和位宽。
- fastrand：以性能为主要目标设计的随机数生成器。
- strsim：包含多种算法来计算和比较字符串的相似度和距离。
- twox-hash：高性能的哈希算法库，基于 XXHash 算法，适用于快速数据指纹或哈希表的场景。
- bytecount：高效计算字节流中某个特定字节的出现次数或字符串中 UTF-8 字符的数量。
- xxhash-rust：提供 xxHash 算法的纯 Rust 实现，用于高速哈希计算。
- rustfft：一个采用 Rust 编写的快速傅里叶变换（FFT）库，致力于性能和易用性。
- ndarray-slice：基于 ndarray 库，提供了对数组进行切片和相关操作的功能。
- metaheuristics-nature：聚集了基于自然过程启发的各类元启发式优化算法，如遗传算法、蚁群算法等。
- rand_simple：一个简单且功能有限的随机数生成器，适用于较不复杂的随机数生成需求。
- hasty：提供对系统级 BLAS（基础线性代数子程序）库的接口，以实现高效的线性代数计算。
- dfp-number-sys：为 Intel® 十进制浮点数学库（libdfp）提供绑定，支持十进制浮点数学运算。
- wyrand：是一个简单快速的非加密伪随机数生成器，旨在提供高性能和良好的随机性。

## 开发工具

_测试、调试、代码检查、性能分析、自动完成、格式化等开发工具。_

- git2：提供对 libgit2 的绑定，允许在 Rust 中直接与 Git 仓库进行交互，执行克隆、拉取、推送等操作。
- pretty_assertions：改进 Rust 的 assert_eq!和 assert_ne!宏输出，使比较失败时的结果更加可读和美观。
- assert-json-diff：使比较 JSON 文档更加直观，当断言失败时，显示两个 JSON 值之间的差异。
- kube：用于与 Kubernetes API 交云集的 Rust 客户端，支持构建异步控制器和操作器。
- fake：一个用于生成各种虚假数据的库，如姓名、地址、电话号码等，用于测试和样本生成。
- embed-resource：一个 Cargo 构建工具，帮助将静态资源如文件和目录打包到最终的 Rust 可执行文件中。
- duct：用于构建和管理子进程管道的库，有助于编写简洁的进程间通信代码。
- include_dir：一个宏库，用来嵌入整个目录的内容到你的 Rust 程序中，使得这些内容成为编译时资源。
- irust：一个跨平台的交互式 Rust 解释器，它提供了类似 REPL 的环境来测试和评估 Rust 代码片段。
- gitui：一个高速、终端用户界面，提供了直观且易用的界面来执行 git 操作。
- gostd：尝试在 Rust 中重新实现 Go 语言的标准库，以学习和比较这两门语言。
- qk：一个快速启动新 Rust 项目的命令行工具，帮助设置和创建新的项目结构。
- precious：代码质量分析工具，聚合了多种代码分析工具的结果，以提供统一的视图。
- deno_lint：由 Deno 项目使用的静态代码分析工具，用于检查并提升 Rust 代码的质量。
- soldeer：Solidity 语言的包管理和构建工具，旨在与智能合约开发流程集成。
- streamdal-protos：为 Rust 生成的 protobuf 文件集合，主要与 streamdal 或类似服务端组件进行数据交换。

## 调试

_分类描述：通过日志、追踪或断言了解代码的内部运行情况。_

- log：轻量级的 Rust 日志门面（facade），为 Rust 提供统一的日志抽象层。
- env_logger：为 log 库提供环境变量配置的日志处理实现。
- tracing：提供结构化的日志记录、错误处理、以及性能分析工具的 Rust 库。
- opentelemetry：提供了一套 API、SDK 和相关工具用于收集应用遥测数据如跟踪、度量和日志。
- prometheus：用于 Rust 程序的监控和度量数据收集的库。
- tracing-opentelemetry：为 tracing 库提供与 OpenTelemetry 协议的集成支持。
- backtrace：用于在 Rust 程序中生成和处理调用栈跟踪信息的库。
- iced-x86：提供了高性能且功能齐全的 x86 (16/32/64 位) 指令集反汇编能力。
- flexi_logger：一个功能丰富且可定制的日志记录器，支持文件输出、控制台输出和其他各种日志记录策略。
- cadence：提供高性能的 Statsd 协议支持用于统计和度量数据收集。
- forensic-adb：是一个基于 Tokio 异步 I/O 框架构建的 Android Debug Bridge (adb)客户端库。
- tracing-axiom：集成到 Axiom 云服务的 tracing 层，允许发送和查看 Rust 应用的跟踪数据。
- minidump-writer：为了生成与 Breakpad 兼容的 minidumps 而从 Breakpad minidump_writer 库中重写的 Rust 库。

## 构建工具（Build Utils）

_用于构建脚本和其他构建时步骤的实用工具。_

- pkg-config：提供从 Cargo 构建脚本调用 pkg-config 命令的功能，帮助处理 C 语言库的编译和链接。
- cc：用于在 Cargo 的构建脚本中处理 C 语言族（C/C++/Objective-C）源代码的编译。
- cmake：为构建脚本提供调用 cmake 命令的能力，以构建需要此系统的本地库依赖。
- vergen：当构建 Cargo 项目时，通过 build.rs 脚本动态地生成代码版本信息。
- system-deps：用于自动寻找和使用系统级别的库依赖项，简化构建过程。
- shadow-rs：用于在 Rust 项目编译时嵌入版本信息、构建时间等元数据。
- vcpkg：允许在 Cargo 构建过程中通过 vcpkg 管理系统查找和使用 C/C++库。
- built：收集当前构建的元数据如版本号、构建时间，可以嵌入到 Rust 项目中。
- cargo-platform：提供工具和库用于解析和使用 Cargo 关于目标平台的 specifications。
- mc-sgx-sdk-tools：提供辅助工具用于构建运行在 Intel SGX 安全隔离区的 Rust 应用。
- cargo：Rust 的官方包管理工具，用于项目的构建、运行、测试和依赖管理。
- cxx-build：生成和集成由 cxx crate 指定的 C++绑定的构建工具。
- pargit：用于提供和 Git 工作流相关的一系列操作和工具。
- garden-tools：为维护和管理多个 Git 仓库提供的集合工具。
- aya-rustc-llvm-proxy：一个库，允许将 Rust 编译器的 LLVM 调用代理到 Rust 自身提供的共享库。

## 测试（Testing）

_验证您的代码的正确性。_

- trybuild：用于编写检查编译器错误消息的测试，有助于测试 rust 代码的编译失败场景。
- proptest：提供基于属性的测试框架，自动生成输入数据并根据属性进行测试，支持失败用例的简化。
- insta：用于捕获和对比 Rust 测试用例结果的快照，以便进行回归测试。
- test-case：提供一个宏，为单个测试函数生成多个测试用例，每个用例用不同的参数调用该函数。
- rstest：一个测试框架，允许使用 fixtures 来创建可重用的测试输入和配置，支持参数化测试。
- assert_fs：测试库，提供文件系统的断言和工具，用于验证在测试过程中文件操作的结果。
- arbitrary：一个为 libfuzzer 等工具提供无结构化输入到结构化数据转换的特性的库。
- serial_test：提供一个属性宏，用于确保测试会顺序执行，从而避免并发相关的问题。
- testcontainers-modules：Testcontainers 库社区模块的集合，用于在 Rust 中进行容器化测试。
- mockable：提供了在 Rust 测试中用于替代依赖组件的模拟对象的工具。
- bintest：专门用来测试使用 Cargo 的二进制(crate)项目产生的可执行文件的工具。
- proptest-state-machine：扩展 proptest 库以支持状态机模式的测试，有助于模拟更复杂的场景。
- arrow-integration-test：用于测试 Apache Arrow 实现的库，支持 Arrow JSON 格式的集成测试数据。

## 外部功能接口（FFI）

_与其他语言的接口。包括绑定生成器和有用的语言构造。_

- pyo3：提供绑定，允许 Rust 代码将功能暴露给 Python，或从 Rust 调用 Python 代码。
- napi：用于创建 Rust 与 Node.js N-API 之间的绑定，以编写高性能的 Node.js 插件。
- bindgen：自动化工具，用于生成 Rust 中的 C 或 C++代码的外部函数接口（FFI）绑定。
- numpy：一个 rust 库，提供对 NumPy C-API 的绑定，通过 PyO3 使 Rust 与 Python 的 NumPy 数组交互。
- cbindgen：生成 C 语言绑定的工具，可以通过 Rust 代码生成 C 头文件，用于 C 与 Rust 代码之间的互操作。
- jni：允许 Rust 代码与 Java Native Interface (JNI) 相互操作，可用于编写 Android 或任何 Java 平台应用的原生模块。
- cxx：提供安全的双向 Rust 与 C++互操作的库。
- uniffi：一个生成器，用于创建跨多种语言通用的 FFI 接口层，允许 Rust 代码与其他语言交互。
- emacs：允许创建可在 GNU Emacs 中加载的动态模块。
- zits：用于在 Rust 中为 Holochain zome 代码生成 TypeScript 绑定的工具。
- cxx-gen：一个代码生成器，基于 cxx crate 提供 C++代码绑定，使得在高级别的工具集成中使用变得更容易。
- maturin：一个命令行工具，用于构建和发布使用 Python 绑定的 Rust crate，如通过 pyo3 或 rust-cpython。
- opencv-binding-generator：自动生成绑定，用于将 Rust 代码与 OpenCV 库进行互操作。
- rustler_sys：提供绑定，允许在 Rust 中使用 C NIF API 来创建用于 Erlang 虚拟机的本地实现的插件（NIF 模块）。
- cxxbridge-cmd：作为 cxx crate 的一部分，提供了一个命令行工具来为非 Cargo 构建的环境生成 C++绑定代码。
- flutter_rust_bridge：用于 Flutter/Dart 和 Rust 之间通信的桥接代码生成器，专注于内存安全和易用性。

## Cargo 插件

_扩展 Cargo 功能的子命令。_

- cargo_metadata：提供了程序化访问 cargo metadata 命令产生的 JSON 输出的库。
- cargo-sort：一个 Cargo 子命令，用于检查 Cargo.toml 文件的依赖项是否已经按字母顺序排序。
- cargo-deb：Cargo 子命令，简化了将 Rust 项目打包成 Debian 软件包格式(.deb)的过程。
- cargo-hack：一个 Cargo 子命令，它为 Cargo 工具添加了额外的、有用的命令行选项和功能。
- cargo-make：是一个 Rust 项目的强大任务运行器和构建工具，用于定义和运行复杂的工作流程。
- cargo-outdated：Cargo 子命令，用于检查项目的 Cargo.lock 或 Cargo.toml 文件中列出的依赖项是否有新版本可用。
- flamegraph：Cargo 子命令，用于方便地为 Rust 程序创建性能火焰图。
- cargo-wasi：Cargo 子命令，简化了构建目标是 wasm32-wasi 的 WebAssembly 应用程序的过程。
- cargo-c：一个辅助工具，用于协助生产和安装可以被 C 程序调用的 Rust 库。
- cargo-bundle-licenses：一个 Cargo 插件，用于收集并捆绑项目依赖项中的许可证信息。
- cargo-dist：提供 Rust 应用打包成各种可交付格式的工具。
- cargo-aur：Cargo 子命令，用于帮助将 Rust 项目打包并发布到 Arch Linux 用户存储库（AUR）中。

## 性能分析

_分析代码的性能表现_

- criterion：为 Rust 提供强大的统计性能基准测试的库，用于准确测量代码性能变化。
- pprof：提供性能分析工具，用于分析 Rust 程序的 CPU 使用和内存分配。
- profiling：一个轻量级接口，简化了在 Rust 程序中集成多种性能分析工具的过程。
- divan：专注于提供用户友好和实用统计输出的 Rust 基准测试库。
- inferno：用于生成火焰图的工具集，是 Brendan Gregg 的 FlameGraph 工具的 Rust 语言移植版。
- puffin：专为游戏开发设计的 Rust 性能分析器，集成易用的图形界面。
- tracing-chrome：提供了一个为 tracing 日志框架输出可以在 Chrome 浏览器跟踪视图查看的数据的组件。
- iai-callgrind：提供高精度的基准测试，用于测量 Rust 代码的性能及其在调用图中的行为。
- hdfs-native：一个在 Rust 中实现的原生 HDFS(Hadoop 分布式文件系统)客户端库。
- rd-hashd：为 resctl-demo (资源控制演示项目) 提供的延迟敏感的模拟工作负载。
- datadog-statsd：用于发送统计信息到 Datadog 平台的 Rust 语言 dogstatsd 客户端实现。
- resctl-bench：一个基准测试工具，用于根据真实场景测试整体系统资源控制的效果。
- peekbufread：Rust 库，为 std::io::Read 特性实现了支持检查点和预览的缓冲读取器。
- mq-workload-generator：工具用来生成测试 Apache RocketMQ 和 Apache Kafka 中间件性能的工作负载。

## 过程宏

_使用过程宏扩展 Rust 语言。_

- derive_more：提供额外的派生宏，简化了常见派生特性，如 Clone, Eq, PartialEq 等的实现过程。
- proc-macro-error：提供过程宏中错误报告的辅助工具，使错误处理更加友好和易于定位问题。
- strum：提供一系列宏，用于枚举类型与字符串之间的转换以及其他枚举相关的工具。
- proc-macro-crate：帮助过程宏定位本身所在的 crate，解决宏内部引用宏定义所在的 crate 时的路径问题。
- proc-macro2：代替编译器内建的 proc_macro 库，提供更稳定且全面的 API 用于构建过程宏。
- syn：用于解析 Rust 代码的库，常用于编写自定义过程宏。
- quote：用于引用 Rust 代码并生成过程宏中的代码片段。
- unicode-ident：用于检测字符是否符合 Unicode XID 标准，通常用于确定字符是否可以作为合法的源码标识符。
- r2r：提供了 Rust 异步编程风格的 Robot Operating System (ROS 2) 绑定，无需关心底层实现细节。
- napi-derive-backend：napi 库的一部分，处理过程宏代码生成的后端逻辑。
- simpl_cache：一个简单易用的缓存数据结构的实现库。
- int-enum：为枚举类型派生 trait 来实现与整数类型间的相互转换。
- derive-adhoc：一个允许高效编写自定义 derive 宏的工具库。

## 网络编程

_为 Web 创建应用程序。_

- tonic：提供一个 Rust 的 gRPC 框架，是基于 Tokio 提供异步 I/O 的高性能服务端与客户端实现。
- jsonwebtoken：Rust 中用于编码和解码 JSON Web Tokens（JWT）的库，特色是强类型和易用性。
- h2：Rust 中的 HTTP/2 协议的客户端和服务器实现，完全异步且性能高效。
- web-sys：提供 Rust 绑定所有 Web APIs 的库，这些 API 通过 WebIDL 自动生成。
- http：一个提供 HTTP 请求和响应的类型的 Rust 库，作为基本的 HTTP 元素的抽象。
- mockito：一个用于 Rust，可以模拟 HTTP 请求和设置预期响应的库，常用于测试。
- tower-http：基于 Tower 服务抽象的 HTTP 中间件和实用工具集合，适用于构建客户端和服务器。
- mime：Rust 库，用于表示和解析 MIME 类型，提供强类型接口。
- sourcemap：用于解析和处理 JavaScript 源码映射的 Rust 库。
- sendgrid：用于从 Rust 应用中向 SendGrid 发送邮件的非官方库。
- tame-index：用于访问本地和 Git 上的 Cargo 注册索引的库，支持私有和公共索引。
- quick-js-dtp：一个包装了 QuickJS JavaScript 引擎的 Rust 库，包括日期解析器的改进。
- richard：一个用 Rust 编写的模块化的聊天机器人框架。
- flipkart_scraper：一个 Rust 库，用于爬取 Flipkart 电子商务平台的产品细节和信息。
- qcs-api-client-openapi：根据 QCS（量子计算服务）OpenAPI 规范自动生成的 Rust 客户端库。

## HTTP 服务器

- axum：构建在 Tokio 和 hyper 之上的现代网络框架，强调安全性、简洁性和高性能。
- tiny_http：提供简单且低级别的 HTTP 服务器功能，允许细粒度控制响应处理。
- warp：基于 Filter 组合构建的 Rust Web 服务器框架，通过这种方式提供 API 的声明和组合。
- actix-web：基于 Actix 系统，是一个异步、模块化且功能丰富的 Web 框架，注重速度和简洁性。
- [salvo.rs](https://link.zhihu.com/?target=https%3A//salvo.rs/): Salvo 是一个用 Rust 语言编写的现代化、简单、快速的 web 服务器框架，设计哲学强调最小化，旨在提供核心功能，同时保持足够的灵活性。
- lambda_runtime：提供库和宏来编写可以部署到 AWS Lambda 上的 Rust 函数。
- actix-cors：用于在 Actix-web 框架中处理 CORS 规则的中间件。
- hyper-rustls：将 Rustls（一个纯 Rust 实现的 TLS 工具包）与 Hyper 结合，以支持 HTTPS。
- lambda_http：支持 AWS Lambda 上的 HTTP 事件，如那些来自应用负载均衡器或 API 网关。
- mollysocket：一个库，用于与实现 UnifiedPush 规范的推送服务协商和接收通知。
- deadnews-template-rust：用作新 Rust 项目的起点的模板，提供了标准的项目结构和配置。
- trillium-testing：旨在帮助开发者为 Trillium Web 框架编写测试的库。
- firewall：提供一层抽象，允许 Rust 应用程序轻松地处理例如连接过滤和拒绝的网络策略。

## HTTP 客户端

- ureq：一个简单，安全，并拥有最小运行时的 HTTP 客户端库，它阻塞而不使用异步 Rust 特性。
- reqwest：一个简单易用的，基于异步 Rust 特性的 HTTP 客户端，支持多种 HTTP 请求和自定义中间件。
- hyper：提供低级别 HTTP 功能，支持异步 Rust，可作为 HTTP 客户端和服务器的构建块。
- octocrab：面向 GitHub REST API v3 的客户端库，提供流畅的接口和可扩展性。
- curl：为 libcurl 提供 Rust 语言绑定，libcurl 是一个成熟且功能强大的 HTTP 客户端库。
- reqwest-middleware：为 reqwest 构建的中间件系统，使得在 reqwest 的请求响应流程中添加自定义逻辑变得简单。
- minreq：设计轻量，尽量减少依赖的简单 HTTP 客户端，适用于需要极小体积的应用。
- h3：基于 quinn（Rust 语言的 QUIC 协议实现）的 HTTP/3 协议客户端和服务器侧实现。
- attohttpc：致力于减少不必要复杂性的 HTTP 客户端库，易于上手。
- modio：面向[http://mod.io](https://link.zhihu.com/?target=http%3A//mod.io)服务的 API 客户端，用于交互式游戏内容管理。
- solaredge：提供了 solaredge 光伏设备监控系统 API 的客户端实现。
- dbl-rs：为 top.gg（原[http://discordbots.org](https://link.zhihu.com/?target=http%3A//discordbots.org)）提供的 Rust API 绑定，用于与其平台进行交云。
- rest-json-client：封装了发送 HTTP JSON 请求的复杂性，提供简单的调用方式。
- monoio-http：为支持异步 runtime monoio 提供的 HTTP 客户端和服务器实现。
- malwaredb-client：一个客户端库，用于与在线恶意软件数据库 MalwareDB 交云。

## WebSocket

- tokio-tungstenite：为 Tungstenite WebSocket 库提供的 Tokio 异步 runtime 支持，使其能够在 Tokio 生态中使用 WebSocket。
- async-tungstenite：允许在任何异步 runtime 上运行的 Tungstenite WebSocket 库的异步接口。
- ws_stream_wasm：使 WebAssembly 项目中使用 WebSockets 变得简单，提供适用于浏览器环境的封装。
- tungstenite：一个简单易用的 Rust 实现的 WebSocket 库，不依赖于特定的异步 runtime。
- headless_chrome：一个用于通过程序控制 Chrome 或 Chromium 浏览器进行自动化操作的库。
- rust_socketio：一个为 Rust 提供的[http://socket.io](https://link.zhihu.com/?target=http%3A//socket.io)协议客户端实现，支持与[http://socket.io](https://link.zhihu.com/?target=http%3A//socket.io)服务器进行实时通信。
- fastwebsockets：一个高性能且符合 WebSocket 标准（RFC 6455）的服务器端 WebSocket 实现。
- soketto：是一个低层次的 WebSocket 库，用于处理 WebSocket 连接的握手和帧协议。
- opentalk-janus-client：针对 WebRTC 服务器 Janus 的客户端库，专门用于 OpenTalk 项目中的 WebRTC 通信。
- irelia：一个封装了 League of Legends 游戏 API 的 Rust 库，方便访问和交互游戏数据。

## 编码数据

_将数据从一种格式编码和/或解码为另一种格式。_

- base64：提供用于对字节数据进行 Base64 编码和解码的功能，允许在 Rust 代码中进行 Base64 转换。
- serde_with：提供扩展给 serde 库的自定义 de/serialization 函数，允许更灵活地处理特殊数据类型或复杂的序列化逻辑。
- bincode：一种基于二进制的高效序列化和反序列化库，适用于 Rust 语言。
- prost：一个 Protocol Buffers（被广泛使用的跨语言的结构数据序列化格式）的 Rust 实现。
- encoding_rs：Mozilla 开发的编码库，主要用于 Firefox，实现了在 Web 中广泛使用的字符编码。
- protobuf：Rust 版本的 Google Protocol Buffers，一个灵活的数据序列化工具，广泛用于远程过程调用和数据交换。
- erased-serde：提供类型擦除的序列化特征的库，允许在不知道具体类型的情况下对数据进行序列化和反序列化。
- serde-wasm-bindgen：整合了 serde 和 wasm-bindgen，用以在 WebAssembly 绑定中使用 Serde 序列化和反序列化。
- bs58：实现 Base58 编码和解码的库，常用于比特币和其他加密货币中。
- rkyv：专注性能，为 Rust 提供无需序列化和反序列化即可读取的二进制格式。
- cookie-factory：一个编写序列化代码的库，受到了 Rust 的解析库 nom 的启发。
- serde_json_lenient：支持从格式宽松的 JSON 数据进行反序列化的库。
- cbor-data：CBOR（Concise Binary Object Representation）序列化和反序列化的实现，用于高效的二进制数据交换。
- recoord：提供简化不同坐标系统之间转换的库，比如将地理位置从一种投影转换到另一种。

## WebAssembly

- instant：提供了一个时间测量库，它在 WebAssembly 环境中与 std::time::Instant 兼容，但也在非 WASM 环境下工作。
- wasmi：是一个独立的 WebAssembly（WASM）模块的纯 Rust 解释器，用于执行 WASM 代码。
- rhai：一个小巧快速的嵌入式脚本语言，适用于 Rust 项目中需要脚本支持的地方。
- console_log：一个日志前端，它将 log crate 的日志消息转发到浏览器的控制台中。
- wit-component：处理 WIT(WebAssembly Interface Types)文件和 WebAssembly 组件的 Rust 工具。
- wit-parser：用于解析 WebAssembly Interface Types(WIT)文件的库，允许在 Rust 中操作文件内容。
- js-sys：提供 Rust 绑定到 JavaScript 的全局对象和函数的 WebAssembly 绑定。
- ts-rs：一个库，可以根据 Rust 结构体和枚举类型自动生成对应的 TypeScript 类型定义。
- awsm_web：旨在让与 WebAssembly 工作变得更加简单，提供了一系列便利的封装和工具函数。
- async-timer：为 Rust 异步编程提供的定时器库，提供单次或重复触发的定时器功能。
- js-component-bindgen：将 Rust 编译成 WebAssembly 并生成 JavaScript 组件的工具，简化了 WebAssembly 模块在前端项目中的集成。
- wasmtime-runtime：是 Wasmtime WebAssembly 运行时所使用的核心库，支持 WebAssembly 模块的执行。
- wasmtime-cranelift：整合 Wasmtime 运行时和 Cranelift 代码生成器的工具，用于编译 WebAssembly 字节码为本地机器码。

## Cryptography

_旨在保护数据安全的算法。_

- rustls：一个安全的 TLS 库，完全用 Rust 实现，不依赖于本地 TLS 库，提供 TLS 协议的通信。
- blake3：一个快速的加密哈希函数，提供了并行计算的优势。
- curve25519-dalek：基于 Rust 语言实现的椭圆曲线库，专门用于 Curve25519 算法的群操作。
- openssl：为 OpenSSL 提供 FFI（外部函数接口）绑定，使 Rust 代码可以使用 OpenSSL 的加密功能。
- secp256k1：为 secp256k1 比特币椭圆曲线提供绑定的库，包括数字签名算法。
- sha1：提供 SHA-1 加密哈希算法的 Rust 实现。
- signature：一个定义加密签名应具备特性的库，支持多种加密签名算法如 ECDSA 和 Ed25519。
- rustls-pemfile：一个简单的在 Rust 中解析 PEM 格式文件（如 TLS 证书和密钥）的库。
- rustls-pki-types：定义了 rustls 证书相关的公共类型和特性，用于 PKI（公钥基础设施）。
- rufendec：一个文件加密和解密的工具，用于保护文件内容的安全。
- xrc_cli：一个多线程处理文件加密/解密操作的命令行工具。
- dco3_crypto：DRACOON 的加密库，在 Rust 中实现对称和非对称加密方法。
- cloudproof：由 Cosmian 提供的一个用于数据隐私保护和安全多方计算的高级加密库。
- russh：一个用 Rust 编写的 SSH 协议实现，包含客户端和服务器功能的库。

## Parser

_解析数据格式或语言的工具。_

- nom：Rust 中的解析库，它使用宏来构建出高性能、零拷贝的解析器组合子。
- uuid：Rust 中用于生成和解析通用唯一识别码（UUID）的库。
- quick-xml：快速、灵活的 XML 处理库，支持读取和写入 XML 文档。
- semver：用于解析和比较遵循语义化版本控制规范（SemVer）的版本号。
- url：解析、构造和序列化 URL 的库，遵循 WHATWG 的 URL 标准。
- xml-rs：一个简单易用的 XML 解析器，完全用 Rust 编写。
- sqlparser：一个可扩展的 SQL 解析器，支持解析各种 SQL 方言，包括 ANSI SQL:2011。
- syntect：用于代码和其他文本进行语法高亮显示的库。
- html5ever：一个高性能的 HTML5 解析库，能以接近浏览器的方式解析错误格式的 HTML 文档。
- simd-json：利用 simd 指令集优化的 JSON 解析库，基于 C++simdjson 库。
- markup5ever：作为 xml5ever 和 html5ever 共享的底层库，包含 HTML 和 XML 解析的通用代码。
- xml5ever：一个高性能的推送式 XML 解析库，可以作为 HTML5 解析的底层。
- ada-url：提供快速且 WHATWG 标准兼容的 URL 解析。
- pest_fmt：基于 pest 库的格式化工具，用于美化或格式化 pest 语法规则。
- kalk：数学表达式解析和求值库，支持自定义函数。
- cargo-util-schemas：用于 Cargo 配置文件和其他相关结构的反序列化模式。
- svgrtypes：用于解析和操作 SVG 文档类型的 Rust 库。

## 异步 Asynchronous

_使用 futures、promises、等待或事件化等技术的异步程序流。_

- tokio: 一个 Rust 编程语言的异步运行时，专门设计用于开发高效率的网络服务。它基于 Rust 的异步 IO 功能构建，并实现了事件循环和任务调度。
- tokio-rustls: 一个结合了 Rust 的异步网络库 tokio 和 Rustls TLS 库的项目。它允许开发者在 tokio 程序中使用基于 Rustls 的 TLS 功能。
- rdkafka: 是 Apache Kafka 客户端库 librdkafka 的 Rust 接口封装。提供高性能生产者和消费者用于 Rust 语言的操作 Kafka 集群。
- mio: 是一个轻量级的异步 IO 库，专注于非阻塞的 I/O 实现。它为建立自定义的事件循环提供了底层的构建块。
- mlua: 是一个 Rust 接口的 Lua 绑定库，允许对 Lua 代码进行高级的和安全的操作，支持 Lua 5.4/5.3/5.2 和 5 版本。
- async-process: 一个提供异步接口的 Rust 库，用于启动和管理子进程。
- async-compat: 提供了一个适配器层，允许在 tokio 和使用 futures 库的代码之间轻松进行转换。
- bb8: 是一个基于 tokio 的异步数据库连接池，灵感来源于同步连接池 r2d2。
- futures-rustls: 该库结合了 futures 库和 Rustls，为基于 futures 的异步 Rust 代码提供 TLS/SSL 流的支持。
- async-shutdown: 提供了一套用于异步应用的优雅关闭机制，有助于正确处理清理和资源释放。
- openraft: 是一个高级的 Rust Raft 协议实现，可以用在需要分布式共识的场景中，例如分布式数据库、分布式系统等。
- remoc: 这个库旨在提供 Rust 语言中远程对象和通道的多路复用和透明的序列化/反序列化。
- automerge: 是一种用于构建本地和分布式应用程序的数据结构（CRDTs），它允许多个用户同时对数据进行更改和合并。
- monoio: 是一个基于 Linux io_uring 的纯 Rust 异步运行时，专注于提供更高的 I/O 性能和吞吐量。
- nodecraft: 该项目听起来像是分布式系统构建的工具，但无法提供更准确的描述，因为它不是一个广为人知的项目或库。
- screeps-async: 是为了游戏 Screeps 设计的异步 Rust 运行时环境，该游戏是一个用于编程 AI 来控制游戏单位的 MMO RTS 游戏。这使得玩家可以在游戏中使用 Rust 编程语言。

## 并发编程

_实现并发和并行计算的类库（crate）。_

- parking_lot：提供更紧凑和高效的互斥锁和其他同步原语。
- spin：提供自旋锁等基于忙等待的同步原语。
- rayon：一个数据并行计算库，允许你轻松地将计算转化为并行工作。
- dashmap：提供一个快速且多线程安全的 HashMap 实现。
- flume：高性能的多生产者，单消费者(MPSC)通道实现。
- threadpool：用于创建线程池，可用于并行任务执行的管理。
- thread_local：允许线程私有的变量存储，避免锁的使用。
- crossbeam：包含数据结构和并发工具，用于编写多线程 Rust 代码。
- pueue：一个命令行工具，可以用来排队执行长时间运行的 shell 命令，并管理它们的执行。
- messaging_thread_pool：一个用于创建管理消息传递的类型化线程池的库。
- omango：是一个库，但当前知识库中没有预存信息，保留原描述。
- melodium：是一个专注项目，但当前知识库中没有预存信息，保留原描述。
- parseq：提供并行和顺序迭代器，以优化集合的处理。
- vlock：是一个库，但当前知识库中没有预存信息，保留原描述。
- gix-actor：一个库，用于识别和处理与 git 相关的 actor 信息。
- asyncgit：一个库，它允许你在异步环境中使用 git2-rs 库，以非阻塞方式与 git 仓库交互。

## 解析工具

_底层工具和解析器生成器。_

- pest：一个优雅的解析器生成库，用于构建基于规则的解析器，具有简洁的语法。
- logos：一个用于创建极其快速的 Rust 词法分析器的库。
- combine：功能强大的解析库，可以在任何类型的输入流上应用，并支持无拷贝操作。
- chumsky：一个对开发者友好的解析器构建库，特别关注于错误报告和恢复。
- lalrpop：一个易于使用的 LR(1)解析器生成器，主要用于编译器开发。
- pom：基于 PEG（解析表达式文法）的解析器组合子库，使用 Rust 操作符重载方便地定义解析规则，无需宏。
- pest_meta：处理 pest 定义的语法，并提供解析器和验证器。
- yap：一个轻量级，并且没有依赖的解析库，但当前知识库中没有预存信息，保留原描述。
- yggdrasil-rt：是一个库，但当前知识库中没有预存信息，保留原描述。
- derive-finite-automaton：一个过程宏，帮助生成有限状态自动机。
- parol_runtime：是’parol’解析器生成器所生成的解析器的运行时支持库。
- ruly2：一个可以基于上下文无关的文法规则生成解析器的库。
- binator：一个可以用于构建复杂解析逻辑的解析器组合子库。
- chainchomp：自称为 Rust 中最强固的小型解析器组合子库。
- parcours：一个提供唯一结果的解析器组合子库。
- pest_vm：是 pest 解析器框架的虚拟机部分，执行 pest 语法。

## 文本处理

_处理以文本形式表达的人类语言的复杂性。_

- regex：强大的正则表达式库，提供高性能的模式匹配和相关功能。
- textwrap：用于对文本进行自动换行、缩进，以及删除不必要的空白字符。
- unicode-segmentation：处理 Unicode 文本，按照图形簇、单词和句子边界进行分割。
- fancy-regex：支持回溯等高级正则表达式功能的库。
- similar：一个对文本和二进制文件执行差异对比的库。
- const_format：在编译时对字符串进行格式化的工具。
- unicode-xid：用于检查字符是否符合 XID_Start 或 XID_Continue 属性的库。
- ascii：处理纯 ASCII 字符、字符串的轻量级库，提供一些常用的 ASCII 相关操作。
- zhconv：用于转换繁体字和简体字，以及处理不同地区中文之间的相互转换。
- ncase：库名可能涉及用于大小写转换和样式强制的功能，但没有更多信息。
- quranize：将音译文本编码为古兰经文本格式的库。
- simple-string-patterns：简化字符串中的匹配、分割和提取模式的操作。
- mdbook-numthm：为 mdbook 工具提供的预处理器，自动为定理、引理等元素编号。

## 命令行界面

_参数解析器、行编辑或输出着色与格式化。_

- colored：用于在 Rust 终端输出中轻松添加颜色和风格的库。
- rustyline：为 Rust 提供类似 readline 的行编辑功能和历史功能，基于 Antirez’s Linenoise 实现。
- crossterm：一个提供通用的接口来操作各种终端功能的跨平台库。
- codespan-reporting：生成带有代码高亮和注释的诊断信息，便于在文本编程语言开发中展示错误。
- clap：一个功能丰富的命令行参数解析库，易于使用且性能出色。
- prettytable-rs：一个在 Rust 终端中生成和打印美观表格的库。
- nu-ansi-term：用于在 ANSI 兼容终端上操作颜色和样式（如粗体、下划线）的库。
- owo-colors：一个零分配的库，用于在终端输出中添加颜色，强调简单性和性能。
- bpaf：一个命令行参数解析器，它使用解析器组合子来提供灵活的参数解析策略。
- inquire：一个用于在终端创建交互式用户输入提示的库。
- cling：是一个库，但当前知识库中没有预存信息，保留原描述。
- colog：看起来是一个简单的日志格式化库，当前没有更多信息。
- miniarg：一个为资源受限环境设计的最小化命令行参数解析器，支持 no-std 和 no-alloc 环境。
- mvgfahrinfo：可能是一个用于获取慕尼黑公共交通实时发车信息的库，当前没有更多信息。
- alacritty_config：一个用于操作和管理 Alacritty 终端仿真器配置的 Rust 库。

## 日期和时间

_处理第四维度。_

- chrono：一个功能丰富的日期和时间处理库，支持时区和格式化。
- chrono-tz：为 chrono 库提供时区支持，基于全世界的 IANA 时区数据库。
- httpdate：用于解析和格式化 HTTP 日期标头的 Rust 库。
- iana-time-zone：一个用于获取当前系统 IANA 时区名称的库。
- hifitime：一个用于高精度日期和时间计算的库，保留原描述。
- cron：用于解析 cron 语法的解析器和用于时间表达式的库。
- humantime：用于解析和格式化 std::time::{Duration, SystemTime}的库，具有人类友好的接口。
- coarsetime：一个为速度优化而设计的时间和持续时间操作库。
- speedate：一个用于快速而简单的日期和时间解析的库。
- utcnow：一个可以在没有标准库支持的环境中获取当前 Unix 时间戳的库。
- zone-detect：ZoneDetect C 库的 Rust 绑定，用于地理位置上的时区检测。
- radio_datetime_utils：为无线电时间信号解码设备提供日期和时间结构的实用工具库。
- gostd_time：可能是为带有 time 包功能的 Golang 标准库提供 Rust 实现的库，但当前缺乏详细信息。
- easy_time：易于使用的时间操作库，旨在使在 Rust 中处理时间更加简单。

## 数据库接口

_与数据库管理系统进行接口交云操作。_

- sqlx：一个异步、无需 ORM 的 SQL 查询库，支持静态查询验证和多个数据库后端。
- redis：为 Rust 编程语言提供的 Redis 数据库的客户端驱动程序。
- diesel：一个安全且可扩展的 ORM 和查询构建器，专为 PostgreSQL、MySQL 和 SQLite 设计。
- rusqlite：SQLite 数据库的高级 Rust 封装，提供方便的访问功能。
- webpki-roots：包含 Mozilla 维护的 CA 根证书，用于 webpki，可用于 TLS 认证。
- mongodb：Rust 的官方 MongoDB 驱动程序，提供异步操作数据库功能。
- libsqlite3-sys：为 libsqlite3 数据库引擎提供低级（unsafe）绑定的库。
- sea-query：一个数据库独立的 SQL 查询生成器，支持 MySQL、Postgres 和 SQLite。
- couch_rs：用于访问和操纵 CouchDB 的 Rust 库。
- sqlite-hashes：为 SQLite 提供了支持聚合的哈希函数，如 MD5 等。
- malwaredb：管理恶意软件和良性软件数据集的库，但缺乏详细信息。
- rusty-sidekiq：提供 Rust 中的 sidekiq 兼容服务器和客户端实现，使用 tokio 异步运行时。
- spin-sdk：Spin 的 Rust SDK，简化了使用 Rust 构建和部署 Spin 组件的过程。
- typeql：可能是一个为 Rust 设计的查询语言库，但缺乏详细信息。
- charybdis_parser：Charybdis ORM 使用的解析器库，可能用于解析 SQL 查询目的，但具体细节不详。

## 数据库实现

_用 Rust 实现的数据库管理系统。_

- tantivy：一个快速、全文搜索引擎库，使用 Rust 编写，便于构建自己的搜索引擎。
- redb：为嵌入式使用情况设计的高性能 Rust 数据库。
- sonic-server：一个快速、轻量、无模式的搜索后端，旨在替代 Elasticsearch 等更重的方案。
- indicium：适用于在内存中处理集合和键值存储搜索的 Rust 库。
- dittolive-ditto：Ditto 是一个对等的、能够在不同平台间同步数据的跨平台数据库。
- persy：提供单一文件存储的事务性持久引擎，支持索引和复合事务。
- oxigraph：一个实现了 SPARQL 查询语言的 RDF 数据库和工具集合，用于处理关联数据。
- marble：一个 Rust 写的磁盘上对象存储库，具备自动垃圾回收功能。
- surrealdb-core-beta：surrealdb-core crate 的测试版，是 SurrealDB 数据库的核心组件。
- worterbuch：可能是一个消息代理与数据库功能结合的库，但当前缺乏详细信息。
- rustdb：一个 Rust 编写的 SQL 数据库，适合学习和小项目使用。
- seekstorm：可能是搜索引擎库和多租户搜索服务器的合集，但当前没有更多信息。
- airomem：受 Prevayler 系统和@jarekratajski 工作的启发，提供简单的持久性解决方案的 Rust 库。
- polars-ffi：Polars 数据科学库的跨语言接口（FFI），允许从其他编程语言访问 Polars 功能。

## 缓存

- cached：一个用于 Rust 的缓存库，提供易用的函数记忆化功能，通常用来加速重复计算的场景。
- string_cache：为 Rust 提供的字符串缓存库，池化常用字符串以节省内存和提高性能。
- moka：一个受 Java Caffeine 项目启发的高性能、并发缓存库适用于 Rust。
- lru：一个提供最近最少使用（LRU）缓存算法实现的 Rust 库。
- ustr：一个高效且对外部函数接口（FFI）友好的 Rust 字符串内联库。
- quick_cache：一个轻量级、高性能的并发缓存实现。
- lru_time_cache：使用 LRU 缓存算法的库，有加入了元素的有效期。
- http-cache-semantics：用于实现符合 RFC 7234 的 HTTP 缓存规则，处理 HTTP 头以做出缓存决策。
- concread：面向 Rust 并发读取场景的数据结构库。
- hashlru：一个简单的 LRU 缓存实现，使用哈希表提供快速查找。
- razel：一个用于带有缓存的数据处理和命令执行管道的 Rust 工具。
- s3-fifo：专门为 Amazon S3 服务设计的高效 FIFO 缓存实现。
- build-clean：一个用于清理磁盘上所有构建缓存的工具，有助于清理编译过程中产生的临时文件。
- assets_manager：一个方便加载、缓存和管理外部资源（如游戏资源）的 Rust 库。

## 压缩

- flate2：Rust 中提供 DEFLATE 算法压缩和解压缩功能的库。
- tar：用于在 Rust 中读写 tar 归档文件的库。
- brotli：一个支持高压缩率的压缩和解压缩库，基于 Brotli 算法。
- miniz_oxide：一个纯 Rust 实现的 DEFLATE 压缩和解压缩库。
- lz4_flex：宣称为 Rust 中最快的 LZ4 压缩实现，致力于安全而高效的数据压缩。
- zstd：Rust 绑定到 zstd（Zstandard）压缩库。
- log4rs：一个高度可配置的日志记录库，支持多种日志目的地。
- parquet：Rust 语言中的 Apache Parquet 格式读写实现。
- laz：Laszip 算法的 Rust 移植，专门用于压缩和解压 LAS 点云数据格式。
- crabz：一个专注于提高压缩效率的并行压缩库。
- libz-sys：提供对系统级 zlib 库的绑定的 Rust crate。
- sn-releases：一个库，用于下载并解压 safe_network 仓库发布的二进制文件。
- pco：对数字序列提供良好压缩效果的 Rust 库，但当前没有更多信息。
- thc：一个针对 H3（一种六边形地理索引系统）单元索引定制压缩方案的库。
- fqkit：一个跨平台的 Rust 程序，用于快速操作 fastq 文件，这是一种用于存储生物信息测序数据的格式。
- gdeflate：压缩和解压 GDeflate 格式的 Rust 库。
- sux：在 Rust 中实现的简洁和压缩数据结构的纯 Rust 库。
- fst-native：一个 Rust 实现的 FST（一种音频数据格式）波形格式读取器。
- rc-zip：一个与 I/O 操作无关，纯 Rust 实现的 ZIP 文件格式解析和构建库。
- tsz-compress：一个专门为时间序列数据设计的压缩库，采用 Delta-delta 和 Delta 编码技术。

## 文件系统

_处理文件和文件系统的类库（crate）。_

- tempfile：用于在 Rust 中安全地创建临时文件和目录的库。
- directories：提供平台特定的数据、配置和缓存文件夹的路径。
- camino：Rust 库，提供用于处理文件路径的 UTF-8 API。
- notify：一个实现文件系统修改通知的跨平台库，支持多种操作系统。
- glob：用于基于 Unix shell 样式模式的文件路径匹配的库。
- which：一个用于发现系统中命令位置的库，类似于 Unix 的"which"命令。
- fs_extra：提供标准库 std::fs 和 std::io 之外的额外文件系统操作功能，如递归复制和移动文件。
- mime_guess：通过给定的文件扩展名推断可能的 MIME 类型。
- trash：一个将文件和目录移动到回收站而不是直接删除的库。
- temp-dir：用于管理 Rust 中带有清理功能的临时目录。
- c2patool：一个用于展示和创建 C2PA（一种内容验证标准）清单的工具。
- trasher：一个命令行工具，用垃圾箱系统代替使用’rm’和’del’命令删除文件。
- super_speedy_syslog_searcher：一个用于快速搜索和合并指定日期时间范围内日志信息的工具。
- simple-disk-benchmark：一个简单的磁盘性能基准测试工具。

## 操作系统

_绑定到特定操作系统 API 的库。_

- sysinfo：一个用于获取系统信息、CPU、内存使用情况、网络信息或者列出当前的进程等的库。
- getrandom：为 Rust 提供的一个简单的跨平台 API，用于获取随机数。
- libc：提供 Rust 绑定到本地 C 库（例如 libc）的一个底层（unsafe）接口。
- whoami：一个用于检索当前用户和环境信息（如用户名、主机名）的库。
- signal-hook：用于处理 Unix 信号的 Rust 库。
- ctrlc：提供简单的方法来处理用户输入 Ctrl-C（中断信号）的 Rust 库。
- os_info：一个侦测当前操作系统类型和版本的库。
- errno：访问 errno 变量的跨平台 Rust 库。
- redox_syscall：为 Redox 操作系统提供低级（unsafe）系统调用的 Rust 库。
- mid：一个生成基于系统硬件信息的唯一机器 ID 或哈希的库。
- memprocfs：专注于物理内存分析的 Rust 框架。
- brt：一个在 Rust 中实现类似于顶部（btop）功能的系统监控工具。
- nc：提供对底层系统调用直接访问的 Rust 库。
- riot-sys：为 RIOT 操作系统（轻量级、适用于物联网的操作系统）提供 Rust FFI 绑定的库。
- httm：一个命令行工具，用于查看在 ZFS 和 btrfs 文件系统上的快照文件版本。
- rattler_virtual_packages：处理 Conda 虚拟包和它们之间依赖性检测的库。
- rattler：一个自动化安装 Conda 环境的 Rust 库或工具。

## 硬件支持

_与特定的 CPU 或其他硬件特性进行接口交云操作。_

- num_cpus：一个用于确定运行当前进程的机器上有多少个 CPU 核心的 Rust 库。
- serialport：提供对串行端口的编程访问的跨平台 Rust 库，用于与通过串口连接的设备进行通信。
- crc32fast：一个使用 SIMD 指令集加速的 CRC32（循环冗余校验码）计算库，能够快速处理大量数据。
- blake2b_simd：一个用 Rust 编写的 BLAKE2b 哈希函数实现，利用 SIMD 指令进行优化以提高性能。
- rusb：提供 Rust 应用程序访问 USB 设备的能力，允许执行 USB 通信。
- hidapi：对 hidapi C 库的 Rust 封装，提供对 HID（人机接口设备）的访问。
- raw-cpuid：一个用 Rust 编写的库，它允许解析和提取 x86 CPU 的 CPUID 信息。
- riscv：Rust 库，用于在 RISC-V 架构上进行低级访问和操作。
- mc-sgx-trts：对英特尔软件保护扩展（Intel SGX）的 Trusted Runtime System 库（sgx_trts）的 Rust 封装。
- mc-sgx-capable：为判断系统是否支持 Intel SGX 以及是否有使能 SGX 的 Rust 封装。
- libarc2：ArC TWO™ 算法的低级接口 Rust 库，用于存取和鉴权操作。
- autd3-firmware-emulator：为 AUTD3（声压传输设备）提供固件仿真的 Rust 库。
- mc-sgx-tservice：针对英特尔 SGX 安全服务(enclave)的信任服务库(sgx_tservice)的 Rust 封装。

## 嵌入式开发

_用于嵌入式设备或没有操作系统的设备。_

- portable-atomic：在 Rust 中提供跨多种平台兼容的原子类型，并支持 128 位原子操作。
- postcard：轻量级、zero-allocation 序列化器，支持 no_std 环境，与 serde 兼容。
- embedded-hal：一套为嵌入式设备（如微控制器）定义的硬件抽象层接口标准，用于促进库和驱动程序的跨硬件可重用性。
- brotli-decompressor：一个实现了 Brotli 解压缩算法的库，并特别注意避免使用标准库接口，以便在 no_std 环境中使用。
- embedded-graphics：针对具有有限资源的微控制器环境中小型硬件屏幕的显示库。
- fixed：在 Rust 中提供定点数支持的库。
- critical-section：为抽象系统关键区域（中断禁用代码块）的操作提供跨平台的支持。
- smoltcp：为嵌入式系统设计的独立、可配置、易于使用的 TCP/IP 协议栈。
- uefi：用于编写 UEFI 应用程序的库，专注于类型安全和易用性。
- tinyrlibc：为 no_std 和裸机目标提供的小型、不完整的 C 标准库替代品。
- sbat：一个用于 UEFI 安全启动高级态（Secure Boot Advanced Targeting, SBAT）的 no_std Rust 库。
- gd32e1：为 GD32E1 系列微控制器提供的支持库，这是一系列 ARM Cortex-M 微控制器。
- stm32_i2s_v12x：用于 STM32 微控制器，通过 SPI 外设实现 I2S（Inter-IC Sound）通信的驱动程序。

## 内存管理

_分配、内存映射、垃圾回收、引用计数或接口到外部的内存管理器。_

- bumpalo：一个快速的堆分配器（竞技场分配器），设计用于在单个内存块上快速分配和释放对象。
- slab：预先分配存储空间并管理统一数据类型集合的内存分配器。
- arc-swap：一个用于在 Rust 中原子性地交换 Arc（原子引用计数类型）的库。
- jemallocator：一个使用 jemalloc 内存分配器的 Rust 库，用于提供性能优化的内存分配。
- sharded-slab：一个无锁的并发 slab 内存分配器，允许并发访问而无需互斥锁。
- tikv-jemallocator：特别针对 TiKV 键值数据库定制的 jemalloc 分配器。
- arcstr：提供零成本（无分配）的 Arc（原子引用计数）字符串类型的库，优化了常见字符串操作的内存使用。
- heapless：适用于嵌入式系统的无堆内存分配，其中不使用动态内存分配。
- recycle_vec：一个库，它提供 Vec 回收其后盾分配（内存块）的能力，可以提高内存重用效率。
- lazy-st：一个为单线程环境设计的库，用于创建惰性计算值，避免重复计算。
- slabbin：一个高效的板材分配器，提供对象的稳定地址，减少重分配次数。
- peakmem-alloc：一个分配器包装器，用于测量内存使用的峰值，有助于识别内存高消耗点。
- vm-memory：提供访问虚拟机物理内存的安全抽象，适合虚拟化环境中的内存操作。
- stak-primitive：可能是一个用于执行栈分配原始操作的 Rust 库，但当前缺乏详细信息。

## 编程语言

- rustc-demangle：用于解码和去除 Rust 编译器产生的符号名称的 “修饰” 或 “扭曲” 的库。
- ariadne：为 CLI（命令行界面）提供美观、丰富多彩的诊断和错误报告信息的库。
- tree-sitter：提供对高性能 Tree-sitter 解析库的 Rust 绑定，用于构建语法和代码分析器。
- rustc-hash：Rust 编译器使用的快速非加密哈希算法的 Rust 实现。
- boa_engine：Boa 是一个 JavaScript 引擎，实现了解析器和执行器，完全用 Rust 编写。
- apollo-parser：一个遵循 GraphQL 规范的解析器，用于构建 GraphQL 查询和模式分析工具。
- llvm-sys：Rust FFI 绑定，用于访问 LLVM 编译器工具链的 C API。
- ra_ap_syntax：一个保留了注释和空白的 Rust 语言解析器，常用于代码分析和工具集成。
- annotate-snippets：创建格式化的代码片段注释和错误显示的 Rust 库。
- mers：一个拥有动态类型但支持类型检查的编程语言，但缺乏更多信息。
- cxxbridge-flags：用于支持 cxx 包的编译器标志，通常是一个编译器配置（实现细节）库。
- adana-script：用于配置命令行工具和基本脚本的命名空间别名的库。

## 数值格式化 (Value formatting)

_分类描述：为用户显示的数值进行格式化，可能需要适应不同的语言和地区。_

- arrow：Apache Arrow 项目
- humansize：便于表示大小的可配置库
- bytesize：人性化的字节表示库
- prettyplease：一个最小化的 syn 语法树美化打印者
- itoa：快速的整数基本类型转字符串转换
- hex-literal：将十六进制字符串转换为数组的宏
- faster-hex：快速的十六进制编码
- strfmt：动态字符串格式化
- num2words：将数字如 42 转换为文字如 forty-two
- ryu-js：快速的浮点数转字符串转换，符合 ECMAScript 标准
- crud-pretty-struct：结构体的漂亮显示
- a1_notation：用于从 A1 电子表格记法转换和转入的包
- crud-tidy-viewer：CLI 生成器，API 的数组美化打印者
- lash：lambda 表达式的交互式 shell

## 模板引擎 (Template engine)

_结合模板和数据以产生文档，通常强调文本处理。_

- handlebars：在 Rust 中实现的模板引擎
- minijinja：具有最小依赖的用于 Rust 的强大模板引擎
- tera：基于 Jinja2/Django 模板的模板引擎
- askama：Rust 中的类型安全的、编译时的类似 Jinja 的模板
- liquid：Rust 的模板语言
- tinytemplate：轻量级模板引擎
- tpnote：极简主义的笔记记录：保存和编辑你的…
- hayagriva：处理参考资料：文献数据库管理…
- mrml：MJML 渲染器
- acorns：从跟踪票据生成 AsciiDoc 发布说明文档
- html_compile：用于生成 HTML 字符串的模板引擎…
- aiken-project：Aiken 项目工具
- onefetch-ascii：在终端显示彩色的 ascii 艺术
- minijinja-embed：为 MiniJinja 提供的模板嵌入支持

## 科学 (Science)

_科学类别涉及到数学、物理以及其他科学领域中问题的解决方案。_

- uom：单位测量库（Units of Measurement），用于 Rust 中的类型安全的单位转换和表达。
- git-commitgraph：一个提供对 Git 的 commit-graph 文件的只读访问的库，commit-graph 是一个优化 Git 性能的文件格式。
- aelhometta：需要更多详细信息来提供准确描述。
- splashsurf：一个用于从流体动力学（SPH）仿真的粒子数据进行表面重构的命令行工具。
- rapier2d：一个用 Rust 编写的二维物理仿真引擎，适用于游戏和交互式应用。
- feos：一个状态方程和经典等热力学函数框架，需要更多信息来提供详细描述。
- picovoice：Rust SDK 包装了 Picovoice 平台，支持端到端语音识别与语音激活。
- average：一个为迭代计算提供便捷方法的统计库，能够处理基本统计量如均值、方差等。
- dynast：一个工具，用于识别并处理量子场论中 Feynman 图的拓扑结构。
- gosh-model：可能与化学模拟相关的库，但需要更多信息。
- gosh-adaptor：为化学模型提供适配器，具体细节不明。
- roqoqo_for_braket_devices：为 Amazon Web Services（AWS）的量子计算服务 Braket 提供 roqoqo（一个设备无关的量子计算构建库）界面的库。
- gmt_dos-clients_windloads：可能与 GMT（Giant Magellan Telescope）的 DOS（Data and Operations System）风载荷客户端有关的库，但需要更多信息来确认。

## 数学 (Math)

_数学类别适于解决数学和逻辑问题。_

- rust_decimal：为 Rust 提供十进制数的支持，以便精确的数值计算，避免浮点数的问题。
- bigdecimal：支持任意精度计算的十进制库，非常适合要求高精度的金融应用。
- nalgebra：一个广泛线性代数库，用于 Rust 编程，支持各种数学操作和转换。
- euclid：一个几何图形和变换的库，提供了一组通用的几何类型。
- num-rational：实现了有理数并在 Rust 中提供数值运算。
- matrixmultiply：一个库，用于执行单精度（f32）和双精度（f64）矩阵的通用矩阵乘法。
- ruint：用于 Rust 的自定义、宽位宽的无符号整型的类型。
- statrs：为 Rust 编程语言提供统计函数、分布和其它实用的统计计算方法。
- hexasphere：可用于在球面上生成和布局六边形瓦片（例如行星渲染）。
- plotpy：允许使用 Python 的 Matplotlib 来自 Rust 代码绘制图形的库。
- kalker：支持变量、用户自定义函数和单位的科学计算器。
- temp-converter：一个用于在摄氏度、华氏度等不同温度单位之间转换的终端应用。
- series：提供 Laurent 级数的单变量实现，是一种表示各种数学函数的工具。
- fj-viewer：一个正在开发的 b-rep（boundary representation）CAD 内核的早期版本。
- biconnected-components：一个用于识别图中的双连通分量（biconnected components）的算法实现库。

## 可视化

_分类描述：数据视图方式，例如绘图或图形化。_

- tabled：一个允许 Rust 开发者轻松创建和打印格式化表格的库，支持多种风格和布局。
- prodash：提供丰富的仪表板显示，用于跟踪和展示异步任务的进度。
- plotly：基于 JavaScript 库 Plotly.js，为 Rust 提供绘图功能。
- tokei：一个统计代码行数（包括注释、空行等）的命令行工具，提供快速统计。
- poloto：一个专注于 2D 绘图的库，输出 SVG 格式，并允许使用 CSS 进行样式定制。
- plotters：一个数据可视化库，用于创建各种复杂的数据图表。
- rerun：用于记录和可视化图像、点云等数据的工具库。
- urdf-viz：一个可视化 URDF（统一机器人描述格式）文件的工具，用于机器人模型。
- gnuplot：为 Gnuplot 创建图表的 Rust 控制器，允许在 Rust 中生成和操控 Gnuplot 绘图。
- forceatlas2：一个为 n 维数据提供快速力导向图形布局算法的库。
- krates：一个用于生成货物元数据（cargo metadata）中 crate 关系图的库。
- autd3-link-visualizer：为 AUTD3（声压传输设备）提供的输出可视化工具。
- star-history：用来展示 GitHub 用户或仓库星标历史变化的图表工具。
- genominicus：一个用于创建基因树图的工具库，支持基因组间关系的可视化表示。
- rust_dot：一个为 Graphviz DOT 语言提供的轻量级 Rust 实现，用于生成和处理 DOT 文件。

## 机器学习 (Machine learning)

- mosec：在云中有效地服务模型。
- safetensors：旨在比普通数据格式更安全的函数读取和写入 safetensors。（需要进一步详细信息）
- ort：ONNX Runtime 1.17 的安全 Rust 封装。
- candle-core：极简主义的 ML 框架。
- rust-bert：即用型的 NLP 管道和语言模型。
- rstats：统计、信息度量、数据分析。
- lance：一种列式数据格式，旨在比传统格式快 100 倍。（需要进一步详细信息）
- torch-sys：为 PyTorch C++ api (libtorch) 提供低级 FFI 绑定。
- langchain-rust：用 Rust 实现的 LangChain，有助于简化…编写。（需要进一步详细信息）
- scandir：快速目录扫描器。
- pllm：便携式 LLM（大规模语言模型）。
- rgwml：在使用 Rust 进行 ML 时减少认知负荷。
- llm-weaver：用任何 LLM 管理长对话。
- oaapi：OpenAI API 的非官方 Rust 客户端。
- hdbscan：纯 Rust 实现的聚类算法。
- mlflow_rs：与 MLflow 进行实验跟踪的客户端库。
- web-rwkv：基于 WebGPU 的纯 RWKV 语言模型实现。

## 地理空间 (Geospatial)

_涉及 GIS、地图以及地球上的相关内容。_

- geos：GEOS C API 的 Rust 绑定。
- google_maps：非官方 Google Maps 平台客户端库…
- d3_geo_rs：D3/d3-geo 的端口。
- geodesy：进行大地测量转换和数据流实验的平台。
- flatgeobuf：Rust 的 FlatGeobuf。
- geozero：零拷贝读写 WKT/WKB…中的地理空间数据。（需进一步详细信息）
- kml：Rust 的 KML 支持。
- proj：PROJ 最新稳定版本的高级 Rust 绑定。
- geohash：Rust 的 Geohash 实现。
- versatiles：用于转换、检查和服务…的工具箱。（需进一步详细信息）
- contour：计算等值环和等值多边形（使用…
- tzf-rs：快速将经度、纬度转换为时区名称。
- osm-io：读写 OSM 数据。
- berlin-core：识别位置并用 UN-LOCODE 标记它们…
- japanese-address-parser：解析日本地址。

## 命令行实用程序 (Command line utilities)

_运行于命令行的应用程序。_

- bat：一个有翅膀的 cat(1)克隆。
- zoxide：你的终端中更智能的 cd 命令。
- lsd：带有很多漂亮颜色和其他一些东西的 ls 命令。
- fd-find：一个简单、快速且用户友好的 find 替代方案。
- coreutils：~ GNU coreutils（已更新）；实现为通用的（跨平台）…
- names：拥有适合用于容器的名称的随机名称生成器。
- emplace：命令行工具，用于在多台机器上镜像已安装的软件。
- sarif-fmt：在终端中查看（漂亮打印）SARIF 文件。
- pacman-repo-builder：从收藏品…构建自定义的 pacman 仓库。（需进一步详细信息）
- search-cli：Cli 程序，在浏览器中搜索参数单词。
- stu：使用 ratatui 在 Rust 中编写的 AWS S3 TUI 应用程序。
- chwp：从命令行界面更改壁纸。
- scafalra：scafalra(sca)是一个用于管理模板的命令行接口工具。
- git-mob-tool：CLI 应用程序，可以帮助用户自动添加共同作者…
- nix-your-shell：一个 nix 和 nix-shell 包装器，适用于除了 bash 之外的壳。
- jobcan-cli：操作 Jobcan 的命令行工具。

## 邮件

_发送、接收、格式化和解析邮件。_

- lettre：电子邮件客户端
- email_address：提供一个符合 RFC 的 EmailAddress 新类型的实现
- imap：Rust 的 IMAP 客户端
- mailchecker：跨语言的临时（一次性/丢弃）…
- aws-sdk-ses：Amazon 简单邮件服务的 AWS SDK
- vsmtp-mail-parser：下一代 MTA。安全、更快、更环保
- gix-mailmap：gitoxide 项目用于解析 mailmap 文件
- mail-builder：Rust 的电子邮件构建库
- spamassassin-milter：使用 SpamAssassin 进行垃圾邮件过滤的 Milter
- commit-email：提醒您使用正确的电子邮件地址提交
- tempmail：简化临时电子邮件的管理和交云…
- mailboxvalidator：使用 MailboxValidator API 的 Rust 电子邮件验证包

## No standard 无标准库

_不依赖 Rust 标准库的类库。_

- heck ：大小写转换类库
- libm ：纯 Rust 实现的数学库
- target-lexicon ：用于编译器及相关工具的目标平台实用工具
- assert_matches ：断言一个值匹配某个模式
- petname ：生成人类可读的随机名称，可用性…
- const_panic ：支持格式化的 const panic
- colorous ：从 d3-scale-chromatic 移植的专业色彩方案
- hash32 ：32 位哈希算法
- mc-sgx-util ：被 SGX 类库共享使用的工具集
- constgebra ：常量线性代数
- rawbytes ：将任意大小的值视为&[u8]来查看/访问
- riot-wrappers ：为 RIOT 操作系统提供的 Rust API 包装器
- cranelift-module ：支持使用 Cranelift 链接函数和数据
- cranelift-entity ：使用实体引用作为映射键的数据结构

## 认证

_帮助确认身份的过程。_

- keyring ：跨平台的密码/凭证管理类库
- aws-config ：AWS SDK 配置和凭证提供者实现
- argon2 ：Argon2 密码哈希算法的纯 Rust 实现
- oauth2 ：一个可扩展的、强类型的 OAuth2 实现
- rpassword ：控制台应用程序中读取密码
- casbin ：支持 ACL 等访问控制模型的授权类库
- scrypt ：基于密码的密钥派生函数
- aws-sdk-sts ：用于 AWS 安全令牌服务的 AWSSDK
- vaultrs ：Hashicorp Vault API 的异步 Rust 客户端类库
- tame-oauth ：一个非常简单的 oauth 2.0 类库
- rs-firebase-admin-sdk ：Rust 的 Firebase Admin SDK
- winauth ：Rust 中的 Windows 认证（NTLMv2）
- dco3 ：Rust 中的 DRACOON 异步 API 包装器
- vaultier ：从 Hashicorp Vault 写入和读取秘密
- cargo-credential ：协助编写 Cargo 凭证辅助工具

## 配置管理

_应用程序的配置管理。_

- config ：Rust 应用程序的分层配置系统
- dotenvy ：dotenv 类库的一个维护良好的分支
- figment ：如此无忧无虑的配置库，简直不真实
- envy ：将环境变量反序列化到类型安全的结构体
- rust-ini ：Rust 中用于解析 Ini 配置文件的类库
- cedar-policy ：Cedar 是一个用于定义权限策略的语言…
- cargo-config2 ：加载和解析 Cargo 配置
- etcetera ：不带偏见的获取配置的类库…
- nccl ：最小化配置文件格式和类库
- rmuxinator ：tmux 项目配置实用程序
- gostd_settings ：读写属性配置文件的工具。是一个用于读写属性配置…
- stak-configuration ：Stak Scheme 配置
- aws-sdk-finspace ：AWS SDK，用于 FinSpace 用户环境管理服务

## 图形数据格式 (Gfx data formats)

_加载和解析用于 2D/3D 渲染的数据，如 3D 模型或动画。_

- gltf：glTF 2.0 模型加载器
- fontdb：具有类似 CSS 查询能力的内存中字体数据库
- kcl-lib：KittyCAD 语言实现和工具
- swash：字体内省分析、复杂文本构形和字符渲染
- tween：用于游戏的补间动画库
- ab_glyph_rasterizer：线条、二次及三次贝塞尔曲线的填充光栅化
- obj-rs：Wavefront obj 格式解析器，用于 Rust 编程语言
- poppler-rs：poppler-glib 的高层次（安全）绑定
- nobject-rs：使用 Nom 编写的 wavefront Obj/Mtl 文件解析器
- kittycad-modeling-cmds：KittyCAD 建模 API 中的命令
- notoize：提示你需要哪款 Noto 字体堆栈
- image_dds：将图像转换到压缩的 DDS 格式及其反向转换
- bevy_gaussian_splatting：bevy 高斯模糊渲染管线插件

## 渲染引擎 (Rendering engine)

_在屏幕上进行渲染的高层次解决方案。_

- meshopt：网格优化器的 Rust ffi 绑定和符合惯用性的封装
- vk-mem：AMD 的 ffi 绑定和符合惯用性的封装
- intel_tex_2：Intel ISPC 纹理压缩的 Rust 绑定
- screen-13：采用 QBasic 精神的 Vulkan 渲染引擎
- rs_pbrt：用 Rust 进行的基于物理的渲染(PBR)
- all-is-cubes：递归体素游戏引擎，可用于体素光线追踪
- crystal_ball：用 Rust 编写的路径追踪库
- sugarloaf：旨在多平台使用的 Rio 渲染引擎
- galileo：跨平台通用地图渲染引擎
- simple-pixels：创建窗口并在上面绘制像素
- pax-core：Pax 的核心共享运行时和渲染引擎
- piet-cosmic-text：基于 cosmic-text 的 piet 文本布局引擎
- all-is-cubes-gpu：为 all-is-cubes 库提供的可选 GPU 渲染实现

## 图形用户界面 (GUI)

_创建图形用户界面。_

- winit：跨平台的窗口创建库
- ratatui：关于制作终端用户界面的库
- egui：可以在网页和原生应用上运行的即时模式 GUI
- taffy：灵活的 UI 布局库
- notify-rust：显示桌面通知（支持 linux, bsd, mac）
- raw-window-handle：Rust 窗口应用程序的互操作性库
- softbuffer：跨平台的软件缓冲区
- iced：受 Elm 启发的跨平台 GUI 库
- fltkrs-richdisplay：基于 fltk-rs 的富文本组件，支持增强的样式组合，支持图文混排…
- leftwm-layouts：用于基于列表的动态平铺窗口管理器的可自定义布局
- wry：跨平台的 WebView 渲染库
- applin：为 Applin™ 服务器驱动的 UI 框架后端库
- nwg_ui：在 native-windows-gui 之上构建的 GUI 库
- hyprland-per-window-layout：Hyprland Wayland 合成器的每个窗口的键盘布局（语言）

## 游戏开发

_用于创建游戏的 Crates。_

- evy：一个令人耳目一新的简单数据驱动的游戏引擎和应用程序框架
- lam：用于游戏和图形的快速 3D 数学库
- evy_egui：用于 Bevy 集成 Egui 的插件
- obj：精神上类似于 tinyobjloader 的轻量级 OBJ 加载器
- evy-inspector-egui：bevy 游戏引擎的检视器插件
- eafwing-input-manager：Bevy 游戏引擎的强大直接状态输入管理器
- gez：用于制作 2D 游戏的轻量级游戏框架，摩擦最小……
- ilrs：Rust 的游戏输入库
- irtual_joystick：Bevy 虚拟摇杆，适用于移动游戏
- ardpack：通用纸牌游戏牌组
- ollo：基于 Rust 的多人游戏框架
- amie：经典小游戏的抽象层
- evy_health_bar3d：以广告牌着色器实现的 bevy 血量条
- pin：虚拟弹球生态系统

## Unix API

_分类描述: 绑定到特定 Unix API 的库。_

- rustix：对 POSIX/Unix/Linux/Winsock 类似系统调用的安全 Rust 绑定。
- nix：对 nix API 的 Rust 友好绑定。
- zbus：用于 D-Bus 通信的 API。
- dbus：绑定到 D-Bus，这是常见的总线系统。
- arboard：处理操作系统剪贴板的图像和文本。
- procfs：与 linux procfs 伪文件系统的接口。
- pango：Pango 库的 Rust 绑定。
- shell-words：根据 UNIX shell 的解析规则处理命令行。
- timerfd：与 Linux 内核的 timerfd API 接口。
- waybar-module-pacman-updates：用于 Arch 的 waybar 模块，显示系统可用更新。
- corrator：验证在 docker 容器内部使用的应用程序版本。
- bpf-linker：BPF 静态链接器。
- r2d2-alpm：用于管理 ALPM 连接的 R2D2 资源池。
- clamav-client：带有可选 Tokio 和 async-std 支持的 ClamAV 客户端库。
- ktls-recvmsg：从 nix 库中提取出来的部分内容。

## Windows API

_绑定到特定 Windows API 的库。_

- windows：Windows 的 Rust 实现。
- winreg：对 MS Windows 注册表 API 的 Rust 绑定。
- clipboard-win：与 Windows 剪贴板交互的方法。
- wild：在 Windows 上扩展命令行参数的 Glob（通配符）。
- native-windows-gui：用于 Microsoft Windows 桌面开发原生 GUI 应用的库。
- winapi：Windows API 的所有原始 FFI 绑定。
- windows-sys：Windows。
- wmi：Rust 的 WMI 库。
- uiautomation：Windows 的 UI 自动化框架。
- ioslice：无需 std 的 I/O 分片，仍是可选的。
- windows-ext：windows-rs 的扩展，提供更多功能。
- grob：尤其适用于 Windows API 调用的可增长缓冲区。
- dotnetdll：读写 .NET 元数据文件的框架。
- webview2-com：WebView2 COM API 的 Rust 绑定。
- shawl：为任意命令提供的 Windows 服务包装器。

## macOS 和 iOS API

_绑定到苹果特定 API 的库。_

- objc：为 Rust 提供的 Objective-C 运行时绑定和封装。
- core-foundation：macOS 的 Core Foundation 绑定。
- cocoa：macOS 的 Cocoa 绑定。
- core-text：Core Text 框架的绑定。
- swift-bridge：生成用于 Rust 和 Swift 安全互操作的 FFI 绑定。
- block2：苹果的 C 语言扩展块。
- kb-remap：协助重新映射 macOS 键盘按键。
- objc2：Objective-C 接口和运行时绑定。
- fse_dump：转储 mac 上的 fseventsd 条目。
- system-configuration：macOS 的 SystemConfiguration 框架绑定。
- ceviche：Rust 守护进程/服务包装器。
- ash-molten：使用 Ash 在 Mac 上提供 Vulkan 的 MoltenVK 静态链接。
- check-macos-updates：用于检查 macOS 是否具有 Nagios 兼容插件的更新。

## 多媒体

_用于音频、视频和图像处理或渲染引擎。_

- exr ：无需不安全代码即可读写 OpenEXR 文件
- gstreamer-editing-services ：GStreamer 编辑服务的 Rust 绑定
- spectrum-analyzer ：易于使用且快速的 no_std 库（支持 alloc）
- souvlaki ：跨平台的媒体按键和元数据处理库
- nokhwa ：简单易用的跨平台 Rust 网络摄像头捕获库
- smrec ：极简的多轨音频录音机
- deltae ：在 CIE Lab 色彩空间中计算两种颜色之间的 Delta E
- riff ：读写 RIFF 格式文件
- stream-download ：将流式内容下载到本地文件缓存
- four-cc ：提供便利表示的 Newtype 封装
- gstreamer-editing-services-sys ：libges-1.0 的 FFI 绑定
- glide ：基于 GStreamer 和 GTK 的跨平台媒体播放器

## 图像

_处理或制作图像。_

- palette ：以正确性为重点，转换和管理颜色
- fast_image_resize ：使用 SIMD 指令快速调整图像大小
- jpeg-decoder ：JPEG 解码器
- cairo-rs ：Cairo 库的 Rust 绑定
- png ：纯 Rust 中的 PNG 解码和编码库
- image ：图像处理库。提供基本的图像处理功能
- kamadak-exif ：纯 Rust 编写的 Exif 解析库
- lcms2 ：ICC 颜色配置文件处理。Little CMS 的 Rust 包装器
- imageproc ：图像处理操作
- opencv ：OpenCV 的 Rust 绑定
- image-compare ：基于 image crate 的图像比较库
- pixcil ：像素艺术编辑器
- typst-ts-core ：Typst.ts 的核心功能
- usvgr-text-layout ：SVG 文本布局实现

## 音频

_分类描述：录制、输出或处理音频。_

- cpal ：纯 Rust 的低级跨平台音频 I/O 库
- rodio ：音频播放库
- hound ：wav 编码和解码库
- spotify_player ：具有完整功能的终端 Spotify 播放器
- kira ：游戏用的富有表现力的音频库
- rubato ：面向音频数据的异步重采样库
- id3 ：读写 ID3 元数据
- libpulse-binding ：PulseAudio libpulse 库的语言绑定
- mpd-discord-rpc ：显示您当前播放的歌曲/专辑
- midi_fundsp ：支持实时 MIDI 合成器软件的创建
- mixxc ：极简主义音量混合器
- megra_rs ：使用马尔可夫链的实时编码语言
- mp3lame-encoder ：mp3lame 编码器的高级绑定
- xmrsplayer ：安全的声音追踪器音乐播放器
- libfmod ：用于 Rust 应用程序集成 FMOD 引擎的包装器

## 视频

_分类描述：录制、输出或处理视频。_

- rav1e ：最快且最安全的 AV1 编码器
- webrtc ：WebRTC API 的纯 Rust 实现
- openh264 ：OpenH264 的习惯性绑定
- ffmpeg-next ：安全的 FFmpeg 包装器（FFmpeg 4 兼容的 ffmpeg crate 分支）
- ab-av1 ：使用快速 VMAF 采样进行 AV1 编码
- dash-mpd ：解析、序列化、下载 MPD 清单
- gifski ：基于 pngquant 的 GIF 制作器，用于生成高质量动态 GIF
- m3u8-rs ：解析 m3u8 文件（Apple 的 HTTP 直播流（HLS）协议）
- rusty_ytdl ：Youtube 视频搜索和下载器
- rfc6381-codec ：解析和生成 codec-string 值的解析器和生成器
- mp4ra-rust ：类型和相关常量表示 MP4 注册机构
- avirus ：用于目的如故障艺术等操作 AVI 文件
- video-rs ：基于 ffmpeg 的高级视频工具包
- gst-plugin-togglerecord ：GStreamer 切换录制插件

## 渲染

_实时或离线渲染 2D 或 3D 图形，通常在 GPU 上。_

- tiny-skia ：Rust 移植的小型 Skia 子集
- encase ：数据布局到 GPU 缓冲区的机制
- inlyne ：引入 Inlyne，一个受 GPU 支持但无浏览器的…
- glyph_brush ：使用 ab_glyph 的快速缓存文字渲染库
- vello ：实验性的 GPU 计算中心 2D 渲染器
- three-d ：2D/3D 渲染器 - 简化绘制
- sdl2 ：Rust 的 SDL2 绑定
- flo_curves ：操作贝塞尔曲线
- rustic-zen ：用于创造艺术性渲染的 Photon-Garden 光线追踪器
- rasterize ：小型 2D 渲染库
- bevy-single-variable-function-mesh ：2D 或 3D 网格（bevy::render::mesh::Mesh）…
- smaa ：使用 SMAA 的后处理抗锯齿
- pdf2pwg ：使用 pdfium 将 PDF 渲染成 A4 页面内容并转换为 PWG/URF
- dessin ：为 PDF、SVG 构建复杂绘图

## 图形 API

_直接访问硬件或操作系统的渲染能力。_

- wgpu ：WebGPU API 包装器
- ash ：Vulkan 的 Rust 绑定
- core-graphics ：macOS Core Graphics 的绑定
- naga_oil ：使用 naga IR 组合和操作着色器
- x11rb ：X11 的 Rust 绑定
- alacritty ：快速的跨平台 OpenGL 终端仿真器
- vulkano ：Vulkan 图形 API 的安全包装器
- metal ：Metal 的 Rust 绑定
- font-kit ：跨平台字体加载库
- surf_n_term ：Posix 终端渲染库
- spirq ：图形的轻量级 SPIR-V 查询工具
- plotters-cairo ：Plotters Cairo 后端
- gtk4_glium ：将 Gtk4 和 Glium 一起使用
- fidget ：用于复杂闭合式隐式表面的基础设施
- svgr ：SVG 渲染库
- blade-egui ：Blade 的 egui 集成
- surfman ：跨平台的 GPU 表面管理低级工具包

## 国际化（i18n）

_和本地化（l10n）。为各种语言和地区开发软件。_

- num-format：生成数字的字符串表示…
- language-tags：Rust 中的语言标签
- rust-i18n：使用 Rust 代码生成加载 YAML 文件的 I18n…
- whatlang：Rust 的快速轻量级语言识别库
- icu_collator：按照语言相关惯例比较字符串的 API
- sys-locale：获取活动系统地域设置的小型轻量级库
- rust_icu_uenum：Unicode 的 ICU4C 库的原生绑定
- icu：Unicode 的国际组件
- icu_locid：管理 Unicode 语言和地区标识符的 API
- boa_icu_provider：Boa JavaScript 引擎的 ICU4X 数据提供方
- rust_iso3166：ISO 3166-1（表示国家的代码……
- i18n_provider_sqlite3-rizzen-yazston：Internationalisation 项目的 i18n_provider_sqlite3 crate

## 游戏

_娱乐和休闲。使用 Rust 实现的游戏和模组。_

- oxyromon：ROM 整理器
- ferium：用于管理 Minecraft 模组的快速 CLI 程序…
- steamlocate：定位 Steam 游戏安装目录（以及 Steam 本身！）
- shticker_book_unwritten：Toontown Rewritten MMORPG 的最小 CLI 启动器
- cargo-screeps：用于将 Rust WASM 代码部署到 Screeps 游戏服务器的构建工具
- rosu-pp：所有模式的 osu!难度和 pp 计算
- retro：游戏目录管理
- riven：Riot Games API 库
- piston_mix_economy：在 MMO 世界中混合调整经济的研究项目
- tmaze：跨平台迷宫求解游戏，完全用 Rust 编写，适用于终端
- discord-rpc-helper：根据运行的 Proton 游戏自动设置 Discord 活动
- wooting-rgb：Wooting RGB SDK Rust 库

## 仿真器

_运行在宿主计算机上原生不可用的软件或游戏。_

- kvm-ioctls：对 KVM ioctls 的安全封装
- qemu：QEMU 二进制安装器
- jailer：在生产中启动 Firecracker 的进程…
- mizu：精确的 gameboy(DMG)和 gameboy color 仿真器…
- polkavm：基于 RISC-V 的快速安全虚拟机
- agb：Game Boy Advance 开发
- tetanes：Rust 编写且支持 SDL2 和 WebAssembly 的 NES 仿真器
- miden-vm：Miden 虚拟机
- peppi：Slippi 重放文件的解析器
- ncvm：脚本虚拟机。开发中！！！
- netsblox-vm：运行 NetsBlox 代码，带有可选的原生扩展
- femtos：基于飞秒的时间表示…
- qemu-plugin：QEMU 插件 API 的高级绑定
- risc0-tools：RISC Zero 开发工具
- librashader-presets：各种情况的 RetroArch 着色器
- risc0-binfmt：RISC Zero 二进制格式 crate

## 仿真

_为某些活动建模或构建模型，例如模拟网络协议。_

- bender：硬件项目的依赖管理工具
- asynchronix：用于系统仿真的高性能异步计算框架
- particular：用 Rust 编写的 N-body 仿真库……
- physx：Nvidia PhysX 的高级 Rust 接口
- qoqo-qryd：qoqo 量子计算工具包的 QRyd 后端
- gmt_dos-actors：Giant Magellan Telescope 动态光学仿真 Actor Model
- rems：时域有限差分（FDTD）电磁仿真器
- quantr：轻松创建、模拟和打印量子电路
- dubins_paths：计算 Dubin’s 路径的 Rust 代码
- rs-event-emitter：为 rust 模拟 promise 实现
- spacerocks：太阳系计算软件
- carla：Carla 仿真器的 Rust 客户端库
- madsim-rdkafka：madsim 上的 rdkafka 仿真器
- autd3-link-simulator：autd-simulator 的链接

## 文本编辑器

_文本编辑应用程序。_

- lsp-types：与语言服务器互动的类型…
- lsp-server：通用 LSP 服务器脚手架
- tui-textarea：为 ratatui 和 tui-rs 设计的强大文本编辑器小部件。多行。
- tower-lsp：基于 Tower 的语言服务器协议实现
- tree-sitter-rust：tree-sitter 的 Rust 语法
- neocmakelsp：cmake 的 Lsp
- tree-sitter-swift：tree-sitter 解析库的 swift 语法
- git-interactive-rebase-tool：用于 git 交互式变基的全功能终端序列编辑器
- neophyte：WebGPU 渲染的 Neovim GUI
- tree-sitter-md：tree-sitter 的 Markdown 语法

## 辅助技术

_辅助技术_

- atk：ATK 库的 Rust 绑定
- kayle_cli：用于网页辅助功能审核的 kayle CLI
- atspi：基于 zbus 的纯粹的 Rust，AT-SPI2 协议实现
- a11ywatch_cli：A11yWatch 网页辅助功能 CLI
- mathcat：数学能力辅助技术（‘从 MathML 到语音和盲文’）
- bevy_a11y：Bevy 引擎的辅助功能支持
- serialize_with_bson：使用 bson DateTime 序列化
- msedge-tts：MSEdge 阅读功能 API 的包装…
- afrim：afrim 输入法核心库
- accesskit_macos：AccessKit UI 辅助功能基础设施：macOS 适配器
- accessibility-rs：Rust 的网页辅助功能引擎
- sem-reg：语义处理某些 Windows 注册表二进制值…
- accesskit_consumer：AccessKit 消费者库（内部）

## 金融

_使用实际货币的支付、会计、交易_

- RustQuant：量化金融
- investments：管理您的投资
- iso_currency：ISO 4217 货币代码
- mpesa：Rust 的 M-PESA API 包装
- lfest：为……的杠杆化永续期货交易所
- databento：官方 Databento 客户端库
- trade_aggregation：将交易聚合到用户定义的蜡烛…
- abacus-rs：用于简化的纯文本 cli 会计工具
- twelvedata：Twelve Data API 客户端
- tindi：股市技术图表指标
- trading212：与 Trading212 API 互动
- quickfix：快速修复 C++库的高级绑定
- debot-position-manager：管理交易位置的函数

## 生物学

_生物信息学_

- needletail：FASTX 解析和 k-mer 方法
- simpleaf：使使用 alevin-fry 更加简单的框架
- rasusa：随机进行读取降低到指定覆盖度
- finch：min-wise 独立排列局部…
- sgcount：快速灵活的 sgRNA 计数器
- noodles：生物信息学 I/O 库
- rust-htslib：HTSlib 绑定和高级 Rust API…
- light_phylogeny：系统发育的方法和功能
- minimap2：libminimap2 的绑定
- fakit：fasta 文件操作程序
- sprocket：Workflow Description Language 文件的包管理器

## 机器人学

_机器人学与车辆工程_

- optimization_engine：纯 Rust 框架，用于嵌入式非凸优化问题。该框架特别适用于低功耗设备和嵌入式系统，允许开发者执行高效的数值优化。
- zenoh-plugin-dds：Zenoh 插件，用于 ROS2 和通用的 DDS（数据分布服务）。此插件能够将 Zenoh 系统与 DDS 生态系统集成，让开发者能在 ROS2 环境中使用 Zenoh 的高性能网络通信。
- rsruckig：Ruckig 运动规划库的 Rust 实现。该库提供实时时间最优轨迹规划功能，适用于工业机器人和自动化系统。
- opencv-ros-camera：用于摄影测量的 OpenCV/ROS 相机的几何模型。通过这个库，开发者可以方便地在 OpenCV 和 ROS 中处理相机模型，以进行图像处理和分析。
- dimas：用于分布式多智能体系统的框架。DIMAS 旨在提供一个简单的接口，用于构建和模拟分布式算法，适用于多个领域，包括机器人协调和智能交通系统。
- zenoh-plugin-ros2dds：Zenoh 插件，用于 ROS 2 和通用的 DDS。类似于 zenoh-plugin-dds，这个插件也是为在 ROS2 和 DDS 中使用 Zenoh 提供支持的工具。
- roslibrust：用于与 ROS 的 rosbridge_server 进行通信的接口。这个库使得 Rust 程序可以轻松接入 ROS 生态系统，实现 ROS 之间的信息交换和通信。
- rosrust：ROS 客户端库的纯 Rust 实现。rosrust 旨在为 Rust 开发者提供与 ROS 系统交互的高效、类型安全的方法，无需依赖原生 ROS 库。
