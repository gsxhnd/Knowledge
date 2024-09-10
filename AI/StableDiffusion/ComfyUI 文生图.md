---
title: ComfyUI 文生图
created: 2024-09-10 15:59
---

<!-- markdownlint-disable MD025 -->

# ComfyUI 文生图

## 默认 Workflow

打开 ComfyUI 后，你会看到一个这样的界面（图片可以点击放大 👇）：

> [!info]
> 如果你打开 ComfyUI 后没有看到这个界面，可以点击右侧面板里的 Load Default 按钮。

![](https://www.comflowy.com/comfyui-foundation/001.png)

你可以点击右侧面板上的 Queue Prompt 按钮，就可以生成图片了。

尝试完文生图后，你可能会很疑惑，这些方块和线都代表什么？

为了方便后续的介绍，我先简单介绍下里面的一个个方块。这个方块我在后续会称其为 Node（节点），其左侧端点是 Input（输入）端，右侧是 Output（输出）端，节点里还会有一些配置项，这些配置项我会称其为 Parameter（参数）：

![](https://www.comflowy.com/comfyui-foundation/005.png)

然后我们再看回这些节点，其实里面的节点我们在上一章中都有提到过。我稍微调整下里面的顺序，你应该就很熟悉了（图片可以点击放大 👇）：
![](https://www.comflowy.com/comfyui-foundation/002.png)

现在再看看，这些方块是不是变得似曾相识了？初始 Workflow 上的这些块块（后续我会用「节点」来称呼这些块块），其实就是前面一章中提到的 Stable Diffusion 包含的三大组件，只是这些组件在 ComfyUI 里名字不太一样而已：
