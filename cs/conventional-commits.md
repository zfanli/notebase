---
type: Git
tags: Dev QoK-B
---

# 约定式提交 Conventional Commits

给提交信息添加人机可读的语意的规范。

## 约定式提交 1.0.0

约定式提交规范是一个针对提交信息的轻量约定，目的为：

- 提供简单的规则创建明确的提交历史
- 同时简化自动化工具处理提交信息的复杂度

这个约定与 [[semantic-versioning|SemVer]] 相吻合，在提交信息中会体现功能、修复和破坏性变更。

提交信息格式按照一下结构编写：

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

提交信息包含以下结构化的元素表明提交的意图：

- `fix:` 修复 bug，与语义化版本的 `PATCH` 相当
- `feat:` 添加功能，与语义化版本的 `MINOR` 相当
- `BREAKING CHANGE:` 用作提交信息的 footer，或者在 type 或 scope 后添加 `!` 表示一个破坏性变更，与语义化版本的 `MAJOR` 相当，可以用在任何类型的提交信息上
- 除了 `fix:` 和 `feat:` 类型，也允许其他类型，比如 angular 约定推荐的 `build:`、`chore:`、`ci:`、`docs:`、`style:`、`refactor:`、`perf:`、`test:` 等

额外的类型没有语义化版本对应的定义（除非包含 BREAKING CHANGE）。类型后可以接括号定义作用域，比如 `feat(parser): add ability to parse arrays`。

## 示例

提交信息附带 BREAKING CHANGE footer。

```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

提交信息用 `!` 提示破坏性变更。

```
feat!: send an email to the customer when a product is shipped
```

带作用域的提交信息使用 `!` 提示破坏性变更。

```
feat(api)!: send an email to customer when a product is shipped
```

提交信息同时使用 `!` 和 BREAKING CHANGE footer。

```
chore!: drop support for node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

## 规格

> 程度术语参考 RFC 2119。

1. 提交信息必须在前缀提供类型，类型是一个名词如 `feat` 和 `fix` 等，类型后可选指定作用域，可选添加 `!`，要求添加冒号和空格
2. `feat` 必须用于添加新功能
3. `fix` 必须用于 bug 修复
4. 作用域可以在类型后指定，作用域必须使用括号包围，比如 `feat(parser):`
5. 描述必须跟随在冒号空格之后，描述简短总结变更
6. 提交信息 body 可以提供在简短描述之后，提供额外的信息说明变更内容，body 前必须跟随一个空行
7. body 形式自由，可以有任意段落
8. footer 可以在 body 后提供，每个 footer 必须由一个词接 `: ` 或 ` #` 组成，后面接字符串值
9. footer 的 token 必须使用 `-` 取代空格，只有 `BREAKING CHANGE` 是特殊的
10. footer 的值可以包含空格和空行
11. 破坏性变更必须在类型后用 `!` 表示，或者以 footer 项目的形式表示
12. 在 footer 使用时 `BREAKING CHANGE` 必须使用大写并跟随 `:`
13. 如果使用 `!` 表示包含破坏性变更，`!` 必须在冒号前，使用 `!` 时 footer 的 `BREAKING CHANGE` 可以省略
14. 可以 `feat` 和 `fix` 以外的类型
15. 工具实现必须不能区分大消息，除了例外：`BREAKING CHANGE`
16. `BREAKING-CHANGE` 必须是 `BREAKING CHANGE` 的同义词

## 使用约定式提交的目的

- 自动生成 CHANGELOG
- 自动决定语义化版本变化（基于提交信息的类型）
- 与团队、公众或其他负责人描述修改的性质
- 触发构建和发布流程
- 通过结构化的提交信息降低向项目贡献代码的成本
