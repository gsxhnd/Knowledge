---
title: Neovim
created: 2024-03-12 09:57
---

<!-- markdownlint-disable MD025 -->

# NeoVim

## 学习教程

Vim 学习教程: <https://github.com/iggredible/Learn-Vim>

Vim 从入门到精通: <https://github.com/wsdjeg/vim-galore-zh_cn>

Neovim 全 lua 配置: <https://github.com/nshen/learn-neovim-lua>

Vim Cheat Sheet: <https://vim.rtorr.com/lang/zh_cn>

A Great Vim Cheat Sheet: <https://vimsheet.com/>

## 安装

### 安装 NeoVim & NeoVide

在开始之前我们需要配置相应的环境环境

Neovim 官方的安装教程：<https://github.com/neovim/neovim/wiki/Installing-Neovim>
NeoVide 官网： <https://github.com/neovide/neovide>
支持的字体下载地址： <https://www.nerdfonts.com/>

> Windows 安装

如果是`Windows`环境,推荐安装`WSL2`，在`WSL2`的 Linux 环境中使用。

首先确定你的 Windows 系统是否为 WSL 2，如果不是请先 找教程 升级到 WSL 2。 查看方法为在 cmd 中运行 `wls -l -v`

`WSL2 安装教程`： <https://docs.microsoft.com/zh-cn/windows/wsl/install>

> Windows 环境下可以使用微软的`Windows Terminal`或者第三方工具如：`Fluent Terminal`启动终端。

Homebrew 安装 Neovim `brew install neovim`

> Linux & macOS

安装 neovim `brew install neovim neovide`

Linux 可以使用发行版对应的包管理工具安装

### 安装 Astronvim

网站 <https://astronvim.com/>

```shell
# 清除缓存
rm ~/.config/nvim

rm ~/.local/share/nvim
rm ~/.local/state/nvim
rm ~/.cache/nvim
```

```shell
# install
git clone --depth 1 https://github.com/AstroNvim/template ~/.config/nvim
```

## Neovim 使用

### 光标移动基本操作

#### 光标位移（正常/可视模式）

- `h`: 向左移动一个字符

- `j`: 向下移动一个字符

- `k`: 向上移动一个字符

- `l`: 向右移动一个字符

- `w`/`b`: 下一个/上一个单词

- `W`/`B`: 下一个/上一个单词（空格分隔）

- `e`/`ge`: 下一个/上一个单词的结尾

- `0`/`$`: 光标移动到行首/行尾

- `^`: 行的第一个非空白字符（与 `0w` 相同）

#### 文本编辑

- `i`/`a`: 在光标处/之后进入插入模式

- `I`/`A`: 在行开头/结尾处进入插入模式

- `o`/`O`: 在当前行上方/下方添加空白行

- `ESC`/`Ctrl+[`: 退出插入模式

- `dd`: 删除/剪切光标所在行

- `cc`: 删除/剪切光标所在行,然后进入插入模式
