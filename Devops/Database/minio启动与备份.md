---
title: Minio启动与备份
created: 2024-09-30 09:53
tags:
  - ObjectStorage
  - Minio
---

<!-- markdownlint-disable MD025 -->

# Minio 启动与备份

## 启动服务

```shell
docker compose -f docker.compose.single.node.yml up -d
```

## 备份

```bash
# 设置别名和认证
mc alias set demo http://127.0.0.1:9000 minio minio123
# 备份 远程bucket => 本地目录
mc mirror --overwrite demo/12312321 bk
# 恢复备份 本地 => 远程bucket
mc mirror --overwrite bk demo/aaa
```
