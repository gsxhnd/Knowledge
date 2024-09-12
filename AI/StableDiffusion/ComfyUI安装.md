---
title: ComfyUI安装
created: 2024-09-12 20:15
tags:
  - StableDiffusion
  - AI
---

<!-- markdownlint-disable MD025 -->

# ComfyUI安装

## 提前准备

- python3.10
- [pipenv](../../Develop/Python/Pipenv.md)
- [安装对应的 cuda 版本](../Nvida%20Cuda.md)

## 安装依赖和运行

```bash
# Clone comfyui repo
git clone https://github.com/comfyanonymous/ComfyUI.git
# create pipenv
pipenv --python 3.10
pipenv shell --pypi-mirror https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
# intall torch
pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu124
# pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu124
pip install -r requirements.txt -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
```

启动服务

```bash
# Run server
python main.py

# 访问网页 http://localhost:8188
```
