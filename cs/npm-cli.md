---
type: NPM
tags: NPM QoK-B
---

# NPM CLI

## Registry

npm 默认设置从 NPM Registry 拉取依赖，但是可以设置为从任何符合 CommonJS Package Registry 规格的来源拉取依赖。

关于 npm registry 需要注意一点，如果你已经使用上一个 registry 安装了依赖，这个依赖安装的路径会被固化在 lock 文件中，就算你后来修改了 registry 地址，如果不做处理的话，以前安装的依赖依然会从当时安装的 registry 拉取。

小 tips，如果在 `package.json` 设置 `"private": true`，npm cli 会阻止这个 package 发布到 NPM Registry 上，以避免将内部的代码发布到网络上。或者你也可以使用 `"publishConfig"` 选项将代码发布到内部的 registry 上。

## Package 规格

Package 名称标识符用来指定 package 的具体信息，标识符允许以下格式：

- `[<@scope>/]<pkg>`
- `[<@scope>/]<pkg>@<tag>`
- `[<@scope>/]<pkg>@<version>`
- `[<@scope>/]<pkg>@<version range>`

还可以指定别名：

- `<alias>@npm:<name>`

Package 规格还可以通过以下方式指定：

- 文件夹路径
- 压缩包地址
- git 地址

## 配置

- `_auth` 认证信息，通常不在 cli 使用，会使用 `~/.npmrc` 文件指定
- `access` 发布使用作用域的包时设定可访问基本，默认为 `restricted` 标记为私有，可选 `public` 公开发布，只有第一次发布指定这个参数有效
- `all` 用在 `outdated` 或者 `ls` 命令上可以列出所有符合条件的包
- `allow-same-version` 允许设置当前版本为新的版本号，避免报错
- `audit` 如果设为 true 将提交审计报告到所有 Registry
- `audit-level` 审计最低报错等级
- `auth-type` npm 登录时到认证策略，可选 legacy\web\sso\saml\oauth\webauthn
- `before` 那之前到版本的依赖？_迷惑选项_
- `bin-links` 是否创建可执行文件的软链
- `browser` npm 该用什么命令打开浏览器
- `ca` 指定证书
- `cache` 缓存路径
- `cafile` 证书文件
- `call` 额外命令
- `cert` 访问 registry 时使用的认证文件
- `ci-name` 当前 CI 系统的名称
- `cidr` CIDR？
- `color` 来点颜色
- `commit-hooks` 使用 `npm version` 时执行 git hooks
- `depth` 指定 `npm ls` 时的深度
- `description` 使用 `npm search` 时展示描述
- `diff` 指定 `npm diff` 时比较的参数
- `diff-dst-prefix` 在 `npm diff` 输出中使用的目标前缀
- `diff-ignore-all-space` 在 `npm diff` 中忽略空格
- `diff-name-only` 在 `npm diff` 中只输出文件名

## 脚本

`package.json` 文件可以指定 `scripts` 属性，定义可执行的脚本。如果存在 `pre-<命令>` 或
`post-<命令>` 的话，这些脚本会在指定的脚本前后执行。

```json
{
  "scripts": {
    "precompress": "{{ executes BEFORE the `compress` script }}",
    "compress": "{{ run command to compress files }}",
    "postcompress": "{{ executes AFTER `compress` script }}"
  }
}
```

依赖中的脚本可以通过 `npm explore <pkg> -- npm run <stage>` 执行。

### 生命周期脚本

npm 还预留了部分命令名称作为特殊脚本。

- `prepare`
  - 打包的时候执行，比如 `npm publish` 或 `npm pack` 命令执行之前会执行的脚本
  - 本地安装依赖的时候执行
  - 本地安装依赖时，`prepare` 脚本会在依赖安装完成后执行
  - `prepare` 脚本会在后台执行
- `prepublish`
  - 废弃了
- `prepublishOnly`
  - 只会在 `npm publish` 之前执行
- `prepack`
  - 在 `npm pack` 或 `npm publish` 或安装一个 git 依赖之前执行
- `postpack`
  - 同上但是在之后执行
- `dependencies`
  - 有任何修改了 `node_modules` 的操作发生之后执行
  - `package.json` 和 `package-lock.json` 更新之后执行
