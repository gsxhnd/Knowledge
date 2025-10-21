---
title: Raft 共识/一致性算法
created: 2024-09-13 10:40
tags:
  - 算法
---
<!-- markdownlint-disable MD025 -->

# Raft 共识/一致性算法

Raft是一种设计上易于理解的共识算法。它在容错性和性能方面与[`Paxos`](https://en.wikipedia.org/wiki/Paxos_(computer_science))相当，区别在于其被分解为相对独立的子问题，并清晰地解决了实际系统所需的所有关键组件。我们希望Raft能让共识机制惠及更广泛的用户群体，并使这些用户能够开发出比现有系统更高质量的各类共识型系统。

## Refer

- [Official Website](https://raft.github.io/)
