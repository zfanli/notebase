---
type: HTML
tags: JavaScript HTML
created: 2022-02-02
---

# Concept: Content Attributes

HTML 中大部分属性（Attribute）都存在 2 种形式，分别是 Content Attribute 和 [[concept-idl-attributes|IDL（Interface Definition Language） Attribute]]。

**Content Attributes** 指的是通过 HTML 代码或 JavaScript 的 `element.setAttribute()` 和 `element.getAttribute()` 访问的属性。

**Content Attributes** 的值一定是字符串，就算属性要求的是数值，比如 [[element-input|\<input\>]] 元素属性 `maxlength` 要设置数值 `42`，需要这样设值 `setAttribute('maxlength', '42')`。

Content Attributes 只能通过字符串赋值，但是属性能接受的值可以是其他类型，这时存在值的转换。对 Content Attributes 来说，通过 `setAttribute()` 设置的任何值都会保留，通过 `getAttribute` 可以获取到，本质上 `setAttribute()` 等于将值写入了 HTML 元素上。

```js
// 给 <input> 元素的 maxlength 赋值
> element.setAttribute('maxlength', '44')
undefined
// 通过 IDL Attribute 查看赋值的结果
> element.maxLength
44

// 给 <input> 元素的 maxlength 设置一个无效值
> element.setAttribute('maxlength', 'asd')
undefined
// 通过 IDL Attribute 查看赋值的结果，值无效，所以是默认值
> element.maxLength
-1
```

这个赋值结果会反映到 HTML 代码上，即使赋值无效。

```html
<input type="submit" id="test" maxlength="asd" />
```
