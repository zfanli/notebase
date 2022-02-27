---
created: 2022-01-22
type: JavaScript
tags: JavaScript
---

# Document

`Document` 接口代表以下含义：

- 浏览器加载的当前网页
- 页面的入口
- [[javascript-DOM Tree|DOM 树]] 本树

`Document` 接口描述所有类型文档的共通的属性和方法。根据文档所属的类型（比如 HTML、XML、SVG 等），在其接口的 API 上会有更多的扩展：比如 HTML 文档，Content-Type 为 `text/html`，会实现 [[javascript-HTMLDocument|`HTMLDocument`]] 接口，而 XML 和 SVG 类型的文档会实现 [[javascript-XMLDocument|`XMLDocument`]] 接口。

## 浏览器兼容性

`Document` 接口所有浏览器都有，但是不同平台不同浏览器对部分属性存在兼容问题，在使用前需要检查。

## 参考资料

- [Document - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document)
