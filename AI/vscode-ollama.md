---
title: ollama+vscode实现本地github copilot
created: 2024-05-15 13:32
---

<!-- markdownlint-disable MD025 -->

# 本地实现 github copilot: ollama + vscode

## 安装 ollama

<https://ollama.com/>

```shell
brew install ollama

ollama pull <Model>
ollama run server
ollama run <Model>
```

## 安装 VSCode 插件

搜索安装 Cody AI

### 配置

```json
  "cody.telemetry.level": "off",
  "cody.commandCodeLenses": false,
  "cody.autocomplete.advanced.provider": "experimental-ollama",
  "cody.autocomplete.experimental.ollamaOptions": {
    "url": "http://127.0.0.1:11434",
    "model": "deepseek-coder:1.3b"
  }
```
