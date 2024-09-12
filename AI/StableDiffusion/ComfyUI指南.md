---
title: ComfyUI指南
created: 2024-04-11 10:36
tags:
  - StableDiffusion
---

<!-- markdownlint-disable MD025 -->

# ComfyUI 指南

## ComfyUI

ComfyUI^[https://www.comfy.org/]^[https://github.com/comfyanonymous/ComfyUI] 是基于节点的 Stable Diffusion 图形用户界面。您可以通过将不同的区块（称为节点）连锁在一起来构建图像生成工作流程。
一些常用的区块包括加载检查点模型、输入提示、指定采样器等。ComfyUI 将工作流程分解为可重新排列的元素，因此您可以轻松创建自己的工作流程。

## Install ComfyUI

## ComfyUI vs AUTOMATIC1111

The benefits of using ComfyUI are:

- Lightweight: it runs fast.
- Flexible: very configurable.
- Transparent: The data flow is in front of you.
- Easy to share: Each file is a reproducible workflow.
- Good for prototyping: Prototyping with a graphic interface instead of coding.

The drawbacks of using ComfyUI are:

- Inconsistent interface: Each workflow may place the nodes differently. You need to figure out what to change.
- Too much detail: Average users don’t need to know how things are wired under the hood. (Isn’t it the whole point of using a GUI?)
