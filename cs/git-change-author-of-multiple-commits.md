---
type: Git
---

# Git: 批量修改提交信息

> 使用场景：疫情影响在家办公之后，公司代码仓库提交的用户有时候会忘记修改，导致使用自己的 github 用户产生了提交记录。这个命令可以将 github 账号产生的记录统一修改为公司账户提交。

首先设置 git 提交信息。

```sh
$ git config --global user.name "Robert Lyall"
$ git config --global user.email "rob@deployhq.com"
```

更新指定提交后所有提交的提交信息。

> 注意这个命令影响指定版本之后的提交，需要找到想要修改的提交的前一个 hash 才行，不然可能会漏改。

```sh
$ git rebase -i 956951bf -x "git commit --amend --reset-author -CHEAD"
```

执行这个命令后，会提示这个操作影响多少个提交，退出保存即可。
