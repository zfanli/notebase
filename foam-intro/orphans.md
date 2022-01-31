---
type: Foam
tags: Foam
created: 2022-01-31
---

# Orphans

孤立元素。没有和任何笔记关联的笔记为孤立状态，Foam 会帮助你找到所有孤立的元素。

Foam 提供一个 Orphans 面板（Explorer）可以查看所有孤立的笔记。

有 2 个配置项目可以控制孤立元素面板的行为：

- `foam.orphans.exclude` 可以用 glob pattern 指定排除指定目录和文件，不将其计算在孤立元素中，比如 `["journal/**/*"]`
- `foam.orphans.groupBy` 设置默认预览模式，可选根据文件夹预览，或者文件列表
