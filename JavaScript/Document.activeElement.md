---
created: 2022-01-16
tags: JavaScript
---

# Document.activeElement

[[Document]] 接口的只读属性 `activeElement` 会返回当前文档中焦点激活（focus 状态）的元素。

这个属性在输入框被选中的时候通常会指向一个 [[HTMLInputElement]] 或是一个 [[HTMLTextAreaElement]] 对象，可以通过对象的 `selectionStart` 和 `selectionEnd` 属性来获得当前光标选中范围的起始和结束点。

除了输入框，其他时候这个属性返回的对象还可能是 `<select>` 元素，或者其他类型的 `<input>` 元素，比如按钮、checkbox 和 radio。

用户可以使用 `tab` 键让焦点在不同元素之间切换，使用空格键来激活焦点选中的元素。元素是否可以被焦点选中取决于平台和当前的浏览器配置。

## 应用场景

### 输入表单分组触发动作（如保存）

设想业务场景为一个表格，其中每一行为一个用户数据，保存的时机为这一行数据编辑结束。我们可以使用 `tab` 键在表单字段中间切换，但是要求在焦点离开这一行时出发保存动作。这时可以使用 `Document.activeElement` 配合 `dataSet` 属性来完成这个判断。

```html
<!-- ... -->
<tr>
  <td><input name="userName" data-user-id="01" onblur="saveUser" /></td>
  <td><input name="email" data-user-id="01" onblur="saveUser" /></td>
  <td><input name="mobile" data-user-id="01" onblur="saveUser" /></td>
</tr>
<!-- ... -->

<script>
  function saveUser() {
    const currentUserId = "01";
    // 当前焦点元素的用户 id 与当前编辑的用户 id 不一致则表示当前用户输入字段失去焦点
    if (document.activeElement.dataSet["userId"] !== currentUserId) {
      // ...执行具体的保存逻辑
    }
  }
</script>
```

## 检索关键词

- check if focused
- 检查是否激活
