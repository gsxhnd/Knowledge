---
title: Git 查找大文件、删除大文件
created: 2020/05/26 13:29
tags:
  - Git
---

<!-- markdownlint-disable MD025 -->

# 查找大文件 删除

```shell
# "tail -20"中的20表示条数
git rev-list --objects --all | grep "$(git verify-pack -v .git/objects/pack/*.idx | sort -k 3 -n | tail -20 | awk '{print$1}')"

# 查看项目仓库大小
git count-objects -v

## 删除大文件 xxx.framework“是上一步中列出的大文件路径，遍历所有的 commit，删除指定的文件，重写历史 commit

git filter-branch --force --index-filter 'git rm -rf --cached --ignore-unmatch xxx.framework' --prune-empty --tag-name-filter cat -- --all

## 强行远程推送
git push origin --force --all

# 清除缓存
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now

```
