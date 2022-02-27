---
type: HTML
tags: JavaScript
created: 2022-02-02
---

# Concept: Content Attributes

HTML 中大部分属性（Attribute）都存在 2 种形式，分别是 Content Attribute 和 [[html-concept-idl-attributes|IDL（Interface Definition Language） Attribute]]。

**Content Attributes** 指的是直接写在 HTML 标签上的属性，Content 可以理解为 HTML 内容。这种属性可以通过 HTML 代码或 JavaScript 的 `element.setAttribute()` 和 `element.getAttribute()` API 对属性进行读写，并且对属性操作的结果可以通过 F12 在对应的 HTML 标签上直接观察到。

但也因为 **Content Attributes** 是 HTML 标签语言直接书写的属性，无论属性接受的值类型如何，其值都会被解析为字符串。

比如 [[html-element-input|\<input\>]] 元素属性 `maxlength` 实际接受数值类型的值，假设要对其设置数值 `42`，需要传递字符串 `setAttribute('maxlength', '42')`，如果不传递字符串，也将发生隐式转换。再通过 `getAttribute('maxlength')` 尝试获取属性修改结果，会得到字符串形式的 `'42'`。

通过 `setAttribute()` API 设置的任何值，无论是否有效，都会被写入到属性中，通过 `getAttribute` 可以获取到。本质上 `setAttribute()` 等于将值写入了 HTML 元素上，通过 F12 可以查看这个 API 的结果。

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
