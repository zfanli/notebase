---
type: NPM
tags: NPM QoK-B
---

# NPM Registry

NPM Registry 是一个 JavaScript 软件和 metadata 信息组成的包的数据库。

## Packages 和 modules

NPM Registry 包含 package，每个 package 同时是 module，或者包含 module。

### Package

在 Npm 的世界，一个 `package.json` 文件描述的目录或文件被视作一个 package。因此一个 package 必然存在一个 `package.json` 文件用来发布到 NPM Registry。

Package 允许以下格式：

1. 一个文件夹里放了一个程序，有一个 `package.json` 文件描述这个程序；
2. 一个压缩文件包含（1）的内容；
3. 一个地址可以被解析为（2）的内容；
4. 一个 `<name>@<version>` 表示一个发布到 registry 上的（3）；
5. 一个 `<name>@<tag>` 指向某一个（4）；
6. 一个 `<name>` 有一个 `latest` tag 满足（5）；
7. 一个 git 地址拉下来代码后可被视作（1）。

### Module

一个 module 可以是 `node_modules` 目录下的任何一个文件或目录，可以被 Node.js 的 `require()` 方法加载。

要能够被 Node.js 的 `require()` 方法加载，一个模块需要满足以下条件之一：

1. 一个目录下存在一个 `package.json` 文件并包含一个 `"main"` 字段；
2. 一个 JavaScript 文件。

注意，由于一个模块不一定需要一个 `package.json` 文件，所以不是所有模块都属于 package。

## 作用域

包名如果使用 `@<scope>/package_name` 格式，`@` 到 `/` 之间被视作作用域名称。

在 NPM Registry 中：

1. 未使用作用域的包肯定是公开的；
2. 私有的包肯定是使用作用域的；
3. 使用作用域的包默认是私有的；你需要在发布时传递参数指定公开发布。
