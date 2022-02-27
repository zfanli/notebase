---
type: Foam
# tags: Foam
created: 2022-01-31
---

# Custom Markdown Preview Styles

VSCode 允许自定义样式文件来风格化 Markdown 预览界面。

通过 `settings.json` 的 `"markdown.styles": []` 属性可以设置自定义样式，设置的值可以是本地相对路径文件，也可以是网络路径的文件。

```json
{
  "markdown.styles": ["Style.css"]
}
```

## Foam 元素

Foam 元素会设置特定 `class` 名称，来方便自定义样式。

### `foam-note-link`

笔记链接 `<a>` 标签。

### `foam-placeholder-link`

笔记链接 placeholder。

### `foam-cyclic-link-warning`

Foam 可以发现循环引用的笔记链接，并作出提示。这个提示元素可以自定义样式。

```html
<div class="foam-cyclic-link-warning">
  Cyclic link detected for wikilink: ${wikilink}
</div>
```
