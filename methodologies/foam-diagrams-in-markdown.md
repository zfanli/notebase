---
type: Foam
# tags: Foam
created: 2022-01-31
---

# Diagrams In Markdown

在 Markdown 画流程图有 2 种方案。

## 1. Mermaid

通过 [Markdown Preview Mermaid Support](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-mermaid) 插件在 VSCode 制作流程图。但是注意在 Github Pages 上无法展示 Mermaid 流程图，你需要通过静态站点生成器方法来支持 Mermaid 流程图。

这种方法使用文本创建流程图。

## 2. Draw.io

[Draw.io Integration](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio) 插件可以在 VSCode 中编辑 Draw.io，在发布时 Foam 可以自动生成 `.drawio.svg`、`.drawio.png` 文件，内嵌到页面上。

这种方式使用插件生成流程图，在 Markdown 中以图片形式引用。
