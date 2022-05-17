---
type: Git
---

# Git: 批量修改提交信息

首先设置 git 提交信息。

```sh
$ git config --global user.name "Robert Lyall"
$ git config --global user.email "rob@deployhq.com"
```

更新指定提交后所有提交的提交信息。

```sh
$ git rebase -i 956951bf -x "git commit --amend --reset-author -CHEAD"
```

执行这个命令后，会提示这个操作影响多少个提交，退出保存即可。
