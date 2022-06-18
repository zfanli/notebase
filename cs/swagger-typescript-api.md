---
type: Tool
tags: Frontend Typescript
---

# swagger-typescript-api

这个工具的作用在于根据输入的 Swagger 文档数据生成对应的 Typescript 类型定义。类型定义包括：

- 接口请求入参的类型定义
- 接口请求返回值的类型定义

工具提供选项，可以同时生成接口请求 Class 和请求方法体。

## 工作原理

读取 Swagger 文档的 OpenAPI Spec 定义，将其转换为标准化格式，然后以此来对准备好的代码模版进行渲染。

代码模版使用 `eta` 渲染语言编写，根据 API 规格数据来生成对应的入参、返回值的接口类型定义，并且根据选项，动态生成接口请求 Class 和对应的请求方法体。

官方提供了一套功能完善的基础模版，分别为：

- 单文件模式，所有类型定义、请求客户端和请求方法都在同一个文件
- 模块化模式，根据 controller 命名空间分割文件，一个 controller 一个文件

接口请求提供两个模式：

- 默认 fetch 模式
- 可选 axios 模式

模版可以方便修改，基本可以满足需求。

## 使用方法

安装。

```shell
yarn add -D swagger-typescript-api
```

这个工具提供 npm 引入和命令行接口，使用 js 脚本自定义更加灵活。

### 脚本用法

用法示例。

```js
const { generateApi } = require('swagger-typescript-api')
const path = require('path')
const fs = require('fs')

/* NOTE: all fields are optional expect one of `output`, `url`, `spec` */
generateApi({
  name: 'MySuperbApi.ts',
  output: path.resolve(process.cwd(), './src/__generated__'),
  url: 'http://api.com/swagger.json',
  input: path.resolve(process.cwd(), './foo/swagger.json'),
  spec: {
    swagger: '2.0',
    info: {
      version: '1.0.0',
      title: 'Swagger Petstore',
    },
    // ...
  },
  templates: path.resolve(process.cwd(), './api-templates'),
  httpClientType: 'axios', // or "fetch"
  defaultResponseAsSuccess: false,
  generateRouteTypes: false,
  generateResponses: true,
  toJS: false,
  extractRequestParams: false,
  extractRequestBody: false,
  prettier: {
    printWidth: 120,
    tabWidth: 2,
    trailingComma: 'all',
    parser: 'typescript',
  },
  defaultResponseType: 'void',
  singleHttpClient: true,
  cleanOutput: false,
  enumNamesAsValues: false,
  moduleNameFirstTag: false,
  generateUnionEnums: false,
  extraTemplates: [],
  hooks: {
    onCreateComponent: (component) => {},
    onCreateRequestParams: (rawType) => {},
    onCreateRoute: (routeData) => {},
    onCreateRouteName: (routeNameInfo, rawRouteInfo) => {},
    onFormatRouteName: (routeInfo, templateRouteName) => {},
    onFormatTypeName: (typeName, rawTypeName) => {},
    onInit: (configuration) => {},
    onParseSchema: (originalSchema, parsedSchema) => {},
    onPrepareConfig: (currentConfiguration) => {},
  },
})
  .then(({ files, configuration }) => {
    files.forEach(({ content, name }) => {
      fs.writeFile(path, content)
    })
  })
  .catch((e) => console.error(e))
```

通过示例代码可以看出工具大致提供哪些定制方法。

现在尝试定制工具行为，来生成符合项目要求的类型定义和请求方法。首先整理项目要求：

- 根据模块来划分不同的文件
- 项目有封装 request 客户端，所以不使用工具提供的默认客户端实现
- 后端 Swagger 文档接口名称定义随意，需要对生成的请求方法名称做一定裁剪
- 后端根据模块化程度会存在多个服务依次生成类型定义和请求方法
- 后端命名空间不同服务风格不同，存在无意义路径成为模块名称，需要裁剪

要实现上述需求，需要修改到代码渲染模版上，所以第一步，先将 `node_modules` 中的模版复制一份到项目目录中。

这里我们需要使用 `modular` 选项告诉工具给每个模块生成一个文件，所以模版只需要复制 `templates/base` 和 `templates/modular` 两个目录。

准备好之后，开始依次实现上述需求。

**根据模块来划分不同的文件**

`generateApi` 入参额外提供一个选项即可开启。

```js
await generateApi({
  // ...
  modular: true,
  // ...
})
```

**不使用工具提供的默认客户端实现**

这里根据项目不同，需要做不同处理。

> 这里存在一个问题，工具源代码中根据执行路径寻找当前目录下的 `template` 目录来设置 `base` 目录的地址。当我们使用 npx 执行脚本的时候，模版存放的目录并非当前执行的上下文目录，这会造成 `templates/base` 目录的模版文件无法被正确加载。
>
> 这个问题可能需要修改到工具的源代码才能解决，好在工具暴露的生命周期钩子存在一个初始化阶段，这里我们可以拿到组装好的配置，根据需要进行修改。

```js
await generateApi({
  // ...
  hooks: {
    onPrepareConfig: (currentConfiguration) => {
      currentConfiguration.config.templatePaths.base = templatePathBase
      // 挂载自定义配置
      currentConfiguration.config.apiBaseUrlKey = item.apiBaseUrlKey
      return currentConfiguration
    },
  },
  // ...
})
```

目前我处理的项目完整封装了一套 request 客户端，所以 `templates/base/http-clients/axios-http-client.eta` 文件内容基本删除，仅留下以下内容。

```ts
export enum ContentType {
  Json = 'application/json',
  FormData = 'multipart/form-data',
  UrlEncoded = 'application/x-www-form-urlencoded',
}
```

在 `templates/modular/api.eta` 模版对 http 客户端导入和初始化逻辑进行修改。删除 http-client 中的多余导入，将客户端正确初始化，示例：

```ts
// 从项目封装模块中导入请求客户端
import { Api } from '@/shared';
import { useAppConfigStore } from '@/services/config';
import type { ApiOptions, RequestConfig, LegacyResponseBody } from '@/shared/request/types';

export class <%= apiClassName %> {
  http: Api

  constructor (baseUrl?: () => string, options?: ApiOptions) {
    // 从配置文件获取正确的后端地址，进行初始化
    const appConfig = useAppConfigStore();
    const defaultUrlGetter = () => appConfig.map.<%~ config.apiBaseUrlKey %> as string;
    this.http = new Api(baseUrl ?? defaultUrlGetter, options);
  }

    <% routes.forEach((route) => { %>
        <%~ includeFile('./procedure-call.eta', { ...it, route }) %>
    <% }) %>
}
```

可以看到该文件引用了 `templates/modular/procedure-call.eta` 文件，这个模版用来生成对应的请求方法，我们对其也进行一定修改。主要修改为原本 http-client 定义了一些参数，最终回填到 url、params、data 和 header 等实际选项上了，这个行为和我们的封装不一致。

> 这里的修改都需要具体问题具体分析。

```ts
<%~ route.routeName.usage %> = (<%~ wrapperArgs %>)<%~ config.toJS ? `: ${describeReturnType()}` : "" %> => {
    const headers = { ...(params?.headers ?? {}) }
    <% if (bodyContentKindTmpl) { %> headers['Content-Type'] = <%~ bodyContentKindTmpl %> <% } %>

    return <%~ config.singleHttpClient ? 'this.http.request' : 'this.request' %><LegacyResponseBody<<%~ type %>>>({
        url: `<%~ path %>`,
        method: '<%~ _.upperCase(method) %>',
        <%~ queryTmpl ? `params: ${queryTmpl},` : '' %>
        <%~ bodyTmpl ? `data: ${bodyTmpl},` : '' %>
        <%~ securityTmpl ? `secure: ${securityTmpl},` : '' %>
        <%~ responseFormatTmpl ? `format: ${responseFormatTmpl},` : '' %>
        ...<%~ _.get(requestConfigParam, "name") %>,
        headers,
    })
}
```

到这里，在做些细微调整，模版的修改就结束了。

**对生成的请求方法名称做一定裁剪**

工具提供了钩子在生命周期中修改数据。我们使用这个机制来修改请求方法名。

```js
await generateApi({
  // ...
  hooks: {
    onFormatRouteName: (_routeInfo, templateRouteName) => {
      // remove auto generated suffix
      return templateRouteName.replace(
        /Using(Post|Get|Put|Delete|Patch|Head|Option)$/,
        ''
      )
    },
  },
  // ...
})
```

**多个服务依次生成类型定义和请求方法**

这个只需要封装 `generateApi` 方法调用，根据配置遍历执行即可。代码示例：

> 这里需要注意要使用 `await`，该工具使用单例模式执行，如果并行处理会造成配置污染。

```js
;(async () => {
  /* NOTE: all fields are optional expect one of `output`, `url`, `spec` */
  for (const item of spec)
    await generateApi({
      //  ...
    })
      .then(({ files, configuration: _configuration }) => {
        files.forEach(({ content, name: _name }) => {
          fs.writeFile(path, content)
        })
      })
      .catch((e) => console.error(e))
})()
```

**无意义路径成为模块名称，需要裁剪**

目前遇到部分服务使用 `api` 作为前缀，这会造成模块名称变成 `api`，但是这是没有意义的，这会造成多个服务被合并在同一个文件中。

这里拿到所有接口配置，将 `api` 前缀的配置逐一修改成路径第二段的名称。

```js
await generateApi({
  // ...
  hooks: {
    onCreateRoute: (routeData) => {
      if (routeData.namespace.toLowerCase() === 'api') {
        routeData.namespace = routeData.raw.route.split('/')[2]
        routeData.raw.moduleName = routeData.namespace
        // console.log(routeData)
      }
      return routeData
    },
  },
  // ...
})
```

定制过程到此，执行结果基本能满足需求。最后将脚本执行命令写入 `package.json`，方便每次执行。

```json
{
  "scripts": {
    "generateTypes": "node ./scripts/generate-types"
  }
}
```

执行：

```shell
yarn generateTypes
```

### 命令行用法

```shell
npx swagger-typescript-api -p ./swagger.json -o ./src -n myApi.ts
```

```shell
Usage: sta [options]
Usage: swagger-typescript-api [options]

Options:
  -v, --version                 output the current version
  -p, --path <path>             path/url to swagger scheme
  -o, --output <output>         output path of typescript api file (default: "./")
  -n, --name <name>             name of output typescript api file (default: "Api.ts")
  -t, --templates <path>        path to folder containing templates
  -d, --default-as-success      use "default" response status code as success response too.
                                some swagger schemas use "default" response status code
                                as success response type by default. (default: false)
  -r, --responses               generate additional information about request responses
                                also add typings for bad responses (default: false)
  --union-enums                 generate all "enum" types as union types (T1 | T2 | TN) (default: false)
  --route-types                 generate type definitions for API routes (default: false)
  --no-client                   do not generate an API class
  --enum-names-as-values        use values in 'x-enumNames' as enum values (not only as keys) (default: false)
  --js                          generate js api module with declaration file (default: false)
  --extract-request-params      extract request params to data contract (default: false)
                                Also combine path params and query params into one object
  --extract-request-body        extract request body type to data contract (default: false)
  --module-name-index <number>  determines which path index should be used for routes separation (default: 0)
                                (example: GET:/fruites/getFruit -> index:0 -> moduleName -> fruites)
  --module-name-first-tag       splits routes based on the first tag
  --modular                     generate separated files for http client, data contracts, and routes (default: false)
  --disableStrictSSL            disabled strict SSL (default: false)
  --disableProxy                disabled proxy (default: false)
  --clean-output                clean output folder before generate api. WARNING: May cause data loss (default: false)
  --axios                       generate axios http client (default: false)
  --single-http-client          Ability to send HttpClient instance to Api constructor (default: false)
  --silent                      Output only errors to console (default: false)
  --default-response <type>     default type for empty response schema (default: "void")
  --type-prefix <string>        data contract name prefix (default: "")
  --type-suffix <string>        data contract name suffix (default: "")
  -h, --help                    display help for command
```

## 参考

- [GitHub - acacode/swagger-typescript-api: TypeScript API generator via Swagger scheme](https://github.com/acacode/swagger-typescript-api)
- [GitHub - eta-dev/eta: Embedded JS template engine for Node, Deno, and the browser. Lighweight, fast, and pluggable. Written in TypeScript](https://github.com/eta-dev/eta)
