---
title: Suphx：掌握麻将与深度强化学习
created: 2023-09-27 09:38
---

<!-- markdownlint-disable MD025 -->

# Suphx：掌握麻将与深度强化学习

## Suphx AI 算法摘要

### 方法概述

### 决策流程

#### 模型

---

| 模型          | 模型     | 功能               |
| :------------ | :------- | :----------------- |
| discard model | 舍牌模型 | 通常情况下舍张     |
| riichi model  | 立直模型 | 决定是否立直       |
| chow model    | 吃模型   | 决定是否吃和怎么吃 |
| pong model    | 碰模型   | 决定是否碰         |
| kong model    | 杠模型   | 决定是否杠         |

#### 模型结构

日本麻将中总共有 34 张不同的牌，所以 Suphx 中使用多个 `34 × 1` 的通道来表示场上的状态，比如手牌可以用四个通道来编码，如下图所示：

![fig3]

类似的，副露、宝牌以及舍牌顺序也通过这种方式进行编码。分类特征（categorical feature）用全为`0`或`1`的多个通道来编码。整数特征（integer feature）则会被区间化，然后每个区间用全为`0`或`1`的通道来编码。

除了这些直接可见的状态之外，作者还设计了 look-ahead 特征用来表示打出某一张牌后赢牌向听数和分数，例如使用一个特征来表示打出某张牌后能否在进 3 张牌后和出 12000 点。由于日麻中总共有 89 种面子和 34 种对子，能和的牌型数量非常多，所以作者没有考虑对手的行为（例如通过舍牌判断是否在做混一色/清一色），只是用深度优先搜索来计算自己和出各种牌型的概率。在这种简化之下，总共构建出 100 多种 look-ahead 特征，每种特征都用一个 34 维的向量来表示。

除了输入和输出维度之外，所有模型的网络结构都差不多，具体结构和维度如下图下表所示。在吃、碰、杠模型中，除了状态特征和 look-ahead 特征以外，还有对哪些牌吃、碰、杠的信息。另外，这些模型都是没有池化层的，因为每个通道中的每一列都有自己的含义，所以池化之后会导致信息损失。

- 牌型特征：自己的手牌、其他玩家的可见牌、宝牌、所有打出牌的序列。例如，N 的前 4 维用来编码自己的手牌，如图 3 所示，每一行表示 34 种牌，每种牌最多有 4 张，所以用 4 行来编码手牌。
- 整数特征：各个玩家的累积分数、剩余可摸牌数。离散到 buckets 中，再用 0 和 1 表示激活对应的 bucket
- 类别特征：当前轮数（一局游戏 8-12 轮）、庄家、counters of repeat dealer、Riichi bets。直接在一行的 34 维向量中使用 0 或 1 来表示。
- Look ahead features：通过搜索的方式，计算打出某张牌后获得一定分数的概率。例如，用一行特征来表示，打出一个特定牌，并经过 3 次换牌后，可以赢取 12000 分数的概率；再用一行表示打出一张特定牌，并经过 2 次换牌后，可以赢取 6000 分的概率。Suphx 中这种特征有 100 多个，并在搜索时进行了一些简化：深度优先和忽略对手行为。相当于只看自己的手牌，打出某张牌后，距离获胜的概率。

<table style="text-align:center;">
  <tr>
    <td></td>
    <td>discard</td>
    <td>riichi</td>
    <td>chow</td>
    <td>pong</td>
    <td>kong</td>
  </tr>
  <tr>
    <td>Input</td>
    <td colspan="2">35 x 838</td>
    <td colspan="3">34 x 958</td>
 </tr>
 <tr>
    <td>Output</td>
    <td >34</td>
    <td colspan=4>2</td>
 </tr>
</table>

各个模型的网络结构大体相同，输入经过卷积后，连接 50 层类似 resnet 的结构，最后连接输出层到指定的维度。因为 Chow、Pong 这种只需要输出是否，导致输出维度不全相同，模型最后几层有些区别。具体参考表 2 和图 4。

### 算法

Suphx 学习过程分三个阶段

- 通过监督学习训练 5 个模型。训练用的 (state, action) 对来自天凤平台。
- 用训练过的模型作为策略进行 _self-play_， 并通过*熵正则化分布式强化学习（Distributed Reinforcement Learning with Entropy Regularization）*来更新策略。在训练过程中使用了*全局奖励预测（global reward predictction）*和*先知教练（oracle guiding）*来处理麻将中特有的困难之处。
- 在 online playing 过程中使用了*运行时策略适应（run-time policy adaption）*来针对本局游戏的初始状态进行调整以获得更好的表现

#### 强化学习

---

self-play

#### 熵正则化分布式强化学习 (Distributed Reinforcement Learning with Entropy Regularization)

---

#### 全局预测奖励(Global Reward Predictction)

在一场游戏中有许多局组成，每一局结束后，和牌的选手会获得正分，其他选手则为 0 分或者负分。

每一场游戏结束后，根据每一局的得分计算最终得分并计算排名  。

然而以每一局的得分或者游戏的最终得分来作为强化学习的训练信号都不太合适，因为：

- 如果用一场游戏的最终得分来训练，则每一局都会有一样的训练信号，那么就无法区分打得好跟打得不好的对局；
- 如果用每一局的得分来训练，则不一定能真实反映选手水平。比如在一场游戏的最后几局，有很大优势的一位选手会打得比较保守，可能会让三位或四位的选手的获得胜利来确保自己可以在这一场游戏结束时处于第一位。那么这几局里面的获得的负的分数并不能说明该选手的策略不好，相反这是一个好策略的体现。

#### 先知教练(Oracle Guiding)

在麻将中有非常多的隐藏信息，在这种情况下用强化学习来学习策略会非常慢。为了加速训练，作者使用了一个可以获取完全信息的先知智能体（oracle agent）。完全信息包括：

1. 当前选手的手牌
2. 所有选手的副露及弃牌
3. 其他公开的信息比如累计积分数、立直棒
4. 另外三名选手的手牌
5. 牌山上的牌

对于一个通常智能体（normal agent，指没有完全信息的 agent）来说，只有前三项是可见的。

由于拥有完全信息，oracle agent 可以通过强化学习很快精通麻将，问题是怎么让 oracle agent 来加速 normal agent 的训练。在这里普通的知识蒸馏（knowledge distillation）的效果并不好，因为一个没有完全信息的 normal agent 很难去模仿 oracle agent 的行为。在 Suphx 中采用的方式是先用完全信息进行训练 oracle agent，然后通过 drop out 的方式逐渐减少完全信息中的特征，慢慢地让 oracle agent 转变为 normal agent。在 oracle agent 完全转变为 normal agent 后，还会进行一定轮次的训练。此时学习率会降到之前的 1 / 10，并且会拒绝重要性大于某个阈值的 state-action 对。如果不添加这两个限制，后续的训练就会不稳定，性能也不会获得提升。

#### 参数化蒙特卡洛策略适应（Parametric Monte-Carlo Policy Adaption）

在麻将中，人类会在拿到不同的手牌的时候使用不同的策略，比如会在拿到好的起手的时候打得更激进来赢得更多分数，在拿到不太好的起手的时候打得保守一点来避免更大的损失。所以如果可以把离线训练的策略针对起手做一定的适应，那么就很可能可以获得更好的表现。跟围棋或星际争霸不同，蒙特卡洛树搜索（Monte-Carlo tree search，MCTS）在麻将上的表现并不好，因此作者提出了一种新的方法，名为参数化蒙特卡洛策略适应（parametric Monte-Carlo policy adaption，pMCPA）。

当一局麻将开始，`agent`摸了初始手牌之后，对离线训练的策略按以下方式进行调整：

1. 模拟：固定自己的手牌，对另外三个选手的手牌及牌山的牌进行随机采样，然后用离线训练的策略来试运行游戏并记录出牌顺序。总共记录 K 个出牌顺序。
2. 适应：根据这 K 个出牌顺序用策略梯度来微调离线训练的策略。
3. 推理：用微调过的策略来进行本局游戏。

## 尾

论文地址： <https://arxiv.org/pdf/2003.13590.pdf>

[fig3]: https://pic-1257414393.cos.ap-hongkong.myqcloud.com/tenpai_project/suphx_figure_3.png

- <https://blog.csdn.net/u013169673/article/details/105486469/>
- <https://blog.csdn.net/qq_42914528/article/details/105383642>
- <https://zhuanlan.zhihu.com/p/139764115>
- <http://wiki.lingshangkaihua.com/mediawiki/index.php/%E9%A6%96%E9%A1%B5>