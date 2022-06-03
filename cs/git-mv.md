---
type: Git
tags: Dev QoK-B
---

# git mv 命令

`git mv [<options>] <source>... <destination>`

在 Git 中修改文件、目录名称的大小写时可以使用 `git mv` 命令，但是要注意直接进行大小写修改会导致命令执行失败，折中方法是先将目录修改为一个临时名称，再执行命令修改为需要的名称。

比如将 Git 中的 `HOME` 目录修改为小写的 `home`，可以这样操作。

```shell
# 先将 HOME 临时改为 homes
git mv HOME homes
# 再将 homes 改为最终的 home
git mv homes home
```

## Usage

```shell
git mv -h
usage: git mv [<options>] <source>... <destination>

    -v, --verbose         be verbose
    -n, --dry-run         dry run
    -f, --force           force move/rename even if target exists
    -k                    skip move/rename errors
```
