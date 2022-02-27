---
type: Foam
# tags: Foam
created: 2022-01-16
---

# Note Template

笔记模版储存在 `.foam/templates` 目录，使用模版可以快速创建指定类型的笔记。

## 笔记模版相关的命令

### 创建笔记模版

- 可以手动在 `.foam/templates` 目录创建 `.md` 文件创建模版
- 通过 `Foam: Create New Template` 创建新模版

### 通过模版创建笔记

- `Foam: Create New Note` 从默认模版创建新的笔记
  - 默认模版 `.foam/templates/new-note.md`
- `Foam: Create New Note From Template` 从相应模版创建新的笔记
- `Foam: Open Daily Note` 从日记类型模版创建新的笔记

## 特殊的笔记模版

Foam 笔记模版保留了几个文件名称，在 Foam 提供的默认命令中会使用。

- `.foam/templates/new-note.md` -> 使用 `Foam: Create New Note` 会使用该模版
- `.foam/templates/daily-note.md` -> 使用 `Foam: Open Daily Note` 会使用该模版

## 变量

Foam 笔记模版可以使用 [VSCode snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_variables) 中提供的变量。

除此之外 Foam 提供以下的变量：

- `FOAM_SELECTED_TEXT` 如果执行创建笔记命令时存在选中的文字，可以通过这个变量访问到，同时选中文字会被替换为一个 wikilink
- `FOAM_TITLE` 笔记的标题，如果使用到这个变量，Foam 会提示你输入笔记到标题
- `FOAM_DATE_*` Foam 提供了一套和日期相关的变量，并且推荐使用这些变量来获取时间值，而不是 VSCode 提供的 `CURRENT_*` 变量，原因在于 Foam 笔记模版可能会用来创建非当前时间的笔记，比如在创建非当日时间的日记笔记时，Foam 会自动计算出准确的时间并通过这个变量前缀将日期暴露给模版

## Metadata

在模版的 Front Matter 中可以定义模版的名称、创建笔记的路径以及笔记模版的描述信息。这些信息储存在模版的 Front Matter 的 `foam_template` 命名空间下。

- `name` 笔记模版的名称
- `filepath` 创建笔记的路径
- `description` 笔记模版的描述信息

```md
---
existing_front_matter: "Existing Front Matter block"
foam_template: # this is a YAML "Block" mapping ("Flow" mappings aren't supported)
  name: My Note Template # Attributes must be on the lines immediately following `foam_template`
  description: This is my note template
  filepath: `journal/$FOAM_TITLE.md`
---

This is the rest of the template
```
