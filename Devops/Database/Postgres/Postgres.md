---
title: Postgres
created: 2025-08-19 10:29
tags:
  - Database
  - Postgres
---
<!-- markdownlint-disable MD025 -->

# Postgres

## 启动一个简单实例

```shell
docker run --name pgdb \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_USER=myuser \
  -p 5432:5432 \
  -d postgres:17
```

- [中文文档](http://www.postgres.cn/docs)
