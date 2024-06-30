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

搜索安装 Continue

### 配置

````json
{
  "models": [
    {
      "title": "Llama 3",
      "provider": "ollama",
      "model": "llama3:8b"
    },
    {
      "title": "deepseek coder api",
      "provider": "openai",
      "model": "deepseek-coder",
      "apiBase": "https://api.deepseek.com",
      "apiType": "openai",
      "apiKey": "",
      "useLegacyCompletionsEndpoint": false,
      "contextLength": 32768
    },
    {
      "title": "deepseek chat api",
      "provider": "openai",
      "model": "deepseek-chat",
      "apiBase": "https://api.deepseek.com",
      "apiType": "openai",
      "apiKey": "",
      "useLegacyCompletionsEndpoint": false,
      "contextLength": 32768
    }
  ],
  "customCommands": [
    {
      "name": "test",
      "prompt": "{{{ input }}}\n\nWrite a comprehensive set of unit tests for the selected code. It should setup, run tests that check for correctness including important edge cases, and teardown. Ensure that the tests are complete and sophisticated. Give the tests just as chat output, don't edit any file.",
      "description": "Write unit tests for highlighted code"
    }
  ],
  "tabAutocompleteOptions": {
    "template": "Please teach me what I should write in the `hole` tag, but without any further explanation and code backticks, i.e., as if you are directly outputting to a code editor. It can be codes or comments or strings. Don't provide existing & repetitive codes. If the provided prefix and suffix contain incomplete code and statement, your response should be able to be directly concatenated to the provided prefix and suffix. Also note that I may tell you what I'd like to write inside comments. \n{{{prefix}}}<hole></hole>{{{suffix}}}\n\nPlease be aware of the environment the hole is placed, e.g., inside strings or comments or code blocks, and please don't wrap your response in ```. You should always provide non-empty output.\n",
    "useCache": true,
    "maxPromptTokens": 2048,
    "debounceDelay": 1000
  },
  "tabAutocompleteModel": {
    "title": "DeepSeek-V2",
    "model": "deepseek-coder",
    "apiKey": "",
    "contextLength": 8192,
    "apiBase": "https://api.deepseek.com",
    "completionOptions": {
      "maxTokens": 4096,
      "temperature": 0,
      "topP": 1,
      "presencePenalty": 0,
      "frequencyPenalty": 0
    },
    "provider": "openai",
    "useLegacyCompletionsEndpoint": false
  },
  "allowAnonymousTelemetry": false,
  "embeddingsProvider": {
    "provider": "transformers.js"
  }
}
````

使用 ollama 实现 tab complete

```json
{
  "tabAutocompleteOptions": {
    "useCache": true,
    "debounceDelay": 1000
  },
  "tabAutocompleteModel": {
    "title": "ollama",
    "model": "codellama:7b",
    "contextLength": 8192,
    "provider": "ollama",
    "useLegacyCompletionsEndpoint": false,
    "useCopyBuffer": false,
    "maxPromptTokens": 400,
    "prefixPercentage": 0.5
  }
}
```
