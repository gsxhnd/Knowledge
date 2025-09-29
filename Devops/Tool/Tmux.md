---
title: tmux
created: 2024-09-06 15:41
---

<!-- markdownlint-disable MD025 -->

# Tmux

Tmux（Terminal Multiplexer）是一款强大的终端复用软件，它允许你在一个终端窗口中创建和管理多个会话、窗口及面板，并能保持会话在后台持久运行，特别适合远程工作和多任务处理。

## 🎯 **Tmux的核心价值**

* **会话持久化**：即使网络连接断开或终端窗口关闭，在Tmux中运行的会话和进程也会继续在后台执行。重新连接后，你可以无缝恢复到之前的工作状态，不会丢失工作进度。
* **多任务管理**：通过分割窗口为多个面板（Pane）或创建多个窗口（Window），你可以同时进行多项操作，例如一边编写代码，一边查看日志输出，一边监控系统资源。
* **会话共享**：多个用户可以同时连接到同一个Tmux会话，实现实时协作，非常适合进行结对编程或远程协助。

## ⌨️ **基本概念与操作**

Tmux的结构分为**会话（Session）**、**窗口（Window）** 和**面板（Pane）**。所有操作通常以一个**前缀键（Prefix Key）** 开始，默认是 `Ctrl+b`。

下面的表格汇总了最常用的快捷键，帮助你快速上手。

| 功能分类 | 快捷键 | 说明 |
| :--- | :--- | :--- |
| **会话管理** | `tmux new -s <会话名>` | 创建新会话 |
| | `Ctrl+b, d` | 分离当前会话（会话在后台继续运行） |
| | `tmux attach -t <会话名>` | 重新连接到指定会话 |
| | `Ctrl+b, s` | 交互式切换会话 |
| | `Ctrl+b, $` | 重命名当前会话 |
| **窗口管理** | `Ctrl+b, c` | 创建新窗口 |
| | `Ctrl+b, n` 或 `p` | 切换到下一个（Next）或上一个（Previous）窗口 |
| | `Ctrl+b, 0-9` | 切换到指定编号的窗口 |
| | `Ctrl+b, ,` | 重命名当前窗口 |
| | `Ctrl+b, &` | 关闭当前窗口 |
| **面板管理** | `Ctrl+b, %` | 垂直分割面板（左右分屏） |
| | `Ctrl+b, "` | 水平分割面板（上下分屏） |
| | `Ctrl+b, 方向键` | 在面板间移动焦点 |
| | `Ctrl+b, x` | 关闭当前面板 |
| | `Ctrl+b, z` | 最大化/恢复当前面板 |
| | `Ctrl+b, Ctrl+方向键` | 调整面板大小 |

## ⚙️ **安装/个性化配置**

通过编辑 `~/.tmux.conf` 文件，你可以深度定制Tmux，使其更符合个人习惯。以下是一些常用配置示例：

```bash
brew install tmux
# install tmux plugin manager 安装插件管理
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

```bash
# 将前缀键从 Ctrl+b 改为更容易按的 Ctrl+a
unbind C-b
set -g prefix C-a
bind C-a send-prefix

# 启用鼠标支持，可以用鼠标选择面板、调整大小、点击切换
set -g mouse on

# 设置终端支持256色，让颜色显示更准确
set -g default-terminal "screen-256color"

# 设置窗口和面板的索引从1开始（默认从0开始）
set -g base-index 1
setw -g pane-base-index 1
```

修改配置文件后，可以重启Tmux会话，或按 `Prefix Key + :` 进入命令模式，输入 `source-file ~/.tmux.conf` 重新加载配置。

## 🚀 **实用技巧**

* **窗格同步**：当需要向多个窗格发送相同命令时（例如在多台服务器上执行相同操作），可以先按 `Prefix Key + :`，然后输入 `setw synchronize-panes on` 开启同步模式。完成操作后，使用 `off` 关闭同步。
* **复制模式**：按 `Prefix Key + [` 进入复制模式，可以使用方向键或搜索（按 `?` 或 `/`）滚动屏幕历史。使用空格键开始选择文本，回车键复制。按 `Prefix Key + ]` 可以粘贴内容。
* **快速搭建工作环境**：你可以通过脚本自动化创建复杂的开发环境。例如，下面的命令可以创建一个名为 "dev" 的会话，并自动打开编辑器并分割窗口：

    ```bash
    tmux new-session -d -s dev -n editor
    tmux send-keys -t dev:1 'vim' C-m
    tmux split-window -v -t dev:1
    tmux attach -t dev
    ```
