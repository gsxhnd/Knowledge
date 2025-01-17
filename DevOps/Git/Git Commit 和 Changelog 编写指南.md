---
title: Commit 和 Changelog 编写指南
created: 2019/12/08 02:29
tags:
  - Git
---

## Commit message

参考 Angular 规范[^1]

```shell
<type>(<scope>) : <subject>
<空行>
<body>
<空行>
<footer>
```

- type：本次提交的类别，必填
- scope：影响范围，可以不填
- subject：提交的标题，一句话概括提交的内容
- body：详细描述提交的内容，可以不填
- footer：放置写备注啥的，如果是 bug ，可以把 bug id 放入

### Header

Header 部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）

```shell
feat(pencil): add 'graphiteWidth' option
```

#### Type

`type`用于说明 commit 的类别，只允许使用下面标识。

- feat：新功能（feature）
- fix：修补 bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动
- merge：合并分支
- perf：优化相关，比如提升性能、体验
- revert：回滚到上一个版本
- build：构建

如果 `type` 为 `feat` 和 `fix`，则该 commit 将肯定出现在 Change log 之中。

#### scope

`scope` 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

#### subject

`subject` 是 commit 目的的简短描述，不超过 50 个字符。

### Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```shell
More detailed explanatory text, if necessary.  Wrap it to
about 72 characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

### Footer

Footer 部分只用于两种情况。

#### 1. 不兼容变动

#### 2. 关闭 Issue

如果当前 commit 针对某个 issue，那么可以在 Footer 部分关闭这个 issue 。

```shell
fix(graphite): stop graphite breaking when width < 0.1

Closes #234
```

也可以一次关闭多个 issue 。

```shell
Closes #123, #245, #992
```

## Change log

### Header

版本 ==> 版本地址 ==> 发布日期

```shell
## [9.0.0-rc.5](url)(2006-01-02)
```

### Describtion

详细描述分三种

1. Bug Fixes bug 修复
2. Features 新功能
3. Performance Improvements 性能提升

```shell
<scope>: <subject> (issue) (commit)
```

`scpoe`: 更新范围
`subject`: 详细信息
`issue`: 错误编号，如果没有可以不填
`commit`: 提交的 hash

```shell
core: allow css custom variables/properties in the style sanitizer (#33841) (61cc7a3), closes #23485 #23485
```

## 参考文献

[^1]: [AngularJS Git Commit Message Conventions](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)
