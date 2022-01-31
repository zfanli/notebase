---
type: Foam
tags: Foam
created: 2022-01-31
---

# Graph Visualization

Foam 提供一个可视化图谱，预览工作区所有笔记的联系。

使用 `Foam: Show Graph` 命令可以打开可视化图谱。

## 功能 & 用法

- 光标悬停在指定文档上，可以高亮与该文档关联的文档
- 按住 `shift` 键可以选中多个文档，高亮所有与选中文档相关联的文档
- 按住 `cmd` or `ctrl` 键选择文档，可以跳转到选中的文档进行阅读
- 自动将编辑的文档置于图谱的中心，让你快速看到与之关联的文档

## 自定义图谱样式

目前可以通过 `foam.graph.style` 设置来调整图谱的样式。

下面是一个配置示例

```json
"foam.graph.style": {
    "background": "#202020",
    "fontSize": 12,
    "lineColor": "#277da1",
    "lineWidth": 0.2,
    "particleWidth": 1.0,
    "highlightedForeground": "#f9c74f",
    "node": {
        "note": "#277da1",
        "placeholder": "#545454",
    }
}
```

### 根据笔记类型来设置节点的样式

在 Front Matter 部分添加 `type` 字段来指定当前的笔记类型。这样我们可以在自定义图谱样式的时候针对指定类型的笔记来设置样式。

举例来说，在下面的笔记中我们设置其笔记类型为 `feature`。

```md
---
type: feature
---

# Backlinking

...
```

在 Foam 设置中（`setting.json`），指定这个类型来针对 `feature` 笔记设置节点的样式。

```json
"foam.graph.style": {
    "node": {
        "feature": "red",
    }
}
```

打开可视化图谱，你会看到样式的变化。
