---
type: Foam
tags: Foam
created: 2022-01-31
---

# Wikilinks

使用 `[[file-name]]` 形式来引用笔记。

- Foam 提供补完功能，在输入 `[[` 之后会提示补完
- 按住 `cmd` or `ctrl` 点击 wikilink 可以跳转到指定文件
  - 光标选中文件，按 `F12` 键可以快速跳转到文件
  - 修改键盘快捷键 `editor.action.revealDefinition` 项目可以自定义按键
- 按住 `cmd` or `ctrl` 点击一个不存在的 wikilink 可以在工作区下创建对应的笔记
  - 这种情况下创建的笔记会使用 `new-note.md` [[note-template#特殊的笔记模版]]

## Sections

通过 `[[wikilinks#sectionName]]` 的形式可以跳转到对应笔记到指定位置。`#` 后跟随指定位置的小标题名称。

## Markdown 兼容性

Foam 插件会在文件底部自动生成 [[Link Reference Definitions]]，让 wikilinks 可以和 Markdown 工具和解析器兼容。
