---
title: "Git 账户管理"
date: 2021-10-29T21:16:29+08:00
draft: false
tags:
  - Git
---

## 清除 Git 的局部账户：

1. `git config --local --unset user.name`
2. `git config --local --unset user.email`
3. `git config --local -l`

可以看到已经没有 user 信息了，提交代码时使用的就是全局账户
