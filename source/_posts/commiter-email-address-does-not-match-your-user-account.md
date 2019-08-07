---
title: commiter email address does not match your user account
date: 2019-04-05 00:22:39
tags: git
---

![](screenshot.png)

记一个几个月前出现的问题，git push 失败，出现这个问题的原因是因为提交者使用的 email 和 git 服务器上的所注册的 email 不匹配，即本地提交所使用的 email 并没有 push 的权限。但是我本地已经做了一些 commit，想保留这些 commit 记录，可以使用 `git filter-branch` 命令来批量修改 commit 的 email 信息。（p.s. 这个命令的权限很大，可以修改用户名、邮箱、提交时间、伪造提交记录，甚至删除文件等，慎用）

附上脚本：

```sh
#!/bin/sh

# -f 忽略备份，强制执行
git filter-branch -f --env-filter '

OLD_EMAIL="old@email.com"
CORRECT_NAME="your user name"
CORRECT_EMAIL="correct@email.com"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags

```