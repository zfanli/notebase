---
type: HTML
tags: HTML
created: 2022-02-02
---

# Concept: IDL Attributes

HTML 中大部分属性（Attribute）都存在 2 种形式，分别是 [[concept-content-attributes|Content Attributes]] 和 IDL（Interface Definition Language） Attributes。

**IDL Attributes** 就是 JavaScript 属性，与 Content Attributes 通过 `element.setAttribute()` 接口赋值不同，IDL Attributes 可以通过 `element.foo` 的方式直接赋值。

**IDL Attributes** 会基本反映 Content Attributes，但是会做相应的转换。比如 [[element-input|\<input\>]] 元素属性 `maxlength` 要设置数值 `42`，`element.maxlength = 42`，如果没有设置数值，会尝试转换为数值，如果转换失败，会被赋值为 `0`。

```js
// 给 <input> 元素的 maxlength 赋值
> element.maxLength = 44
44
// Content Attribute 拿到字符串
> element.getAttribute('maxlength')
'44'

// 给 <input> 元素的 maxlength 设置无效值
> element.maxLength = 'asd'
'asd'
// 无效值转换失败，赋值为 `0`
> element.maxLength
0
// Content Attribute 依然拿到字符串
> element.getAttribute('maxlength')
'0'
```

## IDL Attribute 的限制

在用 IDL Attributes 的值做结合运算的时候，没有明确规则规定 IDL Attributes 该采取什么行为。大部分时候这些值会遵从规格 [HTML Standard | 2.6.1 Reflecting content attributes in IDL attributes](https://www.whatwg.org/specs/web-apps/current-work/multipage/urls.html#reflecting-content-attributes-in-idl-attributes) 的定义，但有时不会。虽然 HTML 规格试图让规格对开发者友好，但是历史原因导致部分属性的行为很奇怪（比如 `select.size`），你需要阅读规格才能了解其原因。

> 这段直译了，可以参考原文。
>
> [HTML attribute reference - HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes)

## 布尔值 Attribute

部分 Content Attributes （比如，`required`, `readonly`, `disabled` 等）叫做布尔值属性。这些值只要存在即为 `true`，不存在即为 `false`。

HTML5 规格限制这种布尔值属性如果存在，它的值只能为下面几种情况：

- 值可以为空字符串（包括没有值的情况，等价于为空）
- 值可以为属性名称，如 `required="required"`

下面都是有效的布尔值属性定义。

```html
<div itemscope>This is valid HTML but invalid XML.</div>
<div itemscope="itemscope">This is also valid HTML but invalid XML.</div>
<div itemscope="">This is valid HTML and also valid XML.</div>
<div itemscope="itemscope">
  This is also valid HTML and XML, but perhaps a bit verbose.
</div>
```

**但是，`"true"` 和 `"false"` 字符串并不是允许的值类型，要设置 `false` 则需要省略这个值。这个限制明确了 `checked="false"` 这样的写法是错误的，因为只要有值，`checked` 就会变被解释成 `true`。**
