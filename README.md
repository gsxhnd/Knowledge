---
title:  Knowledge 目录
---

<!-- markdownlint-disable MD025 -->

# Knowledge 目录

## AI

```dataview
table without ID
Title, file.tags as Tags,
file.folder as Folder,
file.frontmatter.created as Created1,
dateformat(file.ctime, "yyyy-MM-dd hh:mm") as Created
from "AI" sort file.folder
FLATTEN link(file.link, title) as Title
```

## Devops

```dataview
table without ID
Title, file.tags as Tags,
file.folder as Folder,
file.frontmatter.created as Created1,
dateformat(file.ctime, "yyyy-MM-dd hh:mm") as Created
from "Devops" sort file.folder
FLATTEN link(file.link, title) as Title
```

## Math

```dataview
table without ID
Title, file.tags as Tags,
file.folder as Folder,
file.frontmatter.created as Created1,
dateformat(file.ctime, "yyyy-MM-dd hh:mm") as Created
from "Math" sort file.folder
FLATTEN link(file.link, title) as Title
```

## Tool

```dataview
table without ID
Title, file.tags as Tags,
file.folder as Folder,
file.frontmatter.created as Created
from "Tool" sort file.folder
FLATTEN link(file.link, title) as Title
```

## Blog

- [Home - colah's blog](https://colah.github.io) [关联lstm](./AI/lstm.md)
- [云原生开发 博客](https://ewhisper.cn)
