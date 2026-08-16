# Node.js

## 什么是前端工程化？

前端工程化：开发项目直到上线，过程中集成的所有**工具和技术**

Node.js是前端工程化的基础

工具例如：代码压缩工具、格式化工具、转换工具、脚手架工具(Vite等)、打包工具(Webpack)...

## Node.js初识

### 运行js文件

在项目文件夹中打开终端

在终端中输入

```shell
node xxx.js
```

### fs模块-读写文件

此模块的使用方式和大部分模块相似 所以这里就用FileSystem模块来举例

#### 导入模块

##### CJS(CommonJS) Node.js早期常使用的模块化方案

```javascript
const fs = require('node:fs')
```

##### ESM(ECMAScript Modules) JavaScript官方标准的模块规范

```javascript
import * as fs from 'node:fs'
```

如遇到在Vue项目中的.ts文件中导入该包报出的

**找不到名称“node:fs”。是否需要安装 Node.js 的类型定义? 请尝试运行 `npm i --save-dev @types/node`，然后将 "node" 添加到 tsconfig 的 types 字段。**

node环境只在后端项目中拥有，浏览器环境找不到node对象，如需强制获取这个ts类型，在`tsconfig.app.json`中的"compilerOption"中添加"types":['node']

#### 模块的简单使用示例

```javascript
import * as fs from "node:fs"

fs.writeFile('./text.md', 'NodeFileSystemTest', (err) => {
    console.log(err)
})

fs.readFile('./text.md', (err, data) => {
    if (err) {
        console.log(err)
    } else {
      // fs.readFile获取到的data数据为16进制的buffer数据流
      // 需要调用 .toString() 转换成字符串
        console.log(data.toString())
    }
})
```

### Path模块

在Node环境中，相对路径是根据终端所在的路径为起点，所以在Node环境中编写的代码，使用绝对路径

使用ES Modules导入

```javascript
import * as fs from "node:fs"
import path from "node:path"

// 使用import.meta.dirname获取当前模块的目录路径
// 使用path.join将目录名和相对路径进行拼接处理成绝对路径

fs.readFile(path.join(import.meta.dirname, '../test.md'), (err, data) => {
    if (err) {
        console.log(err)
    } else {
        console.log(data.toString())
    }
})
```

#### 代码压缩Demo

```javascript
const filePath = path.join(import.meta.dirname, '../index.html')

fs.readFile(filePath, (err, data) => {
    if (err) {
        console.log(err)
    } else {
        const htmlStr = data.toString()
        const resultStr = htmlStr.replace(/[\r\n]/gm, '')
        fs.writeFile(filePath, resultStr, (err) => {
            console.log(err)
        })
    }
})
```

### import.meta

包含文件路径等属性的Object对象，仅在ES Modules中可用

#### 常用属性

##### import.meta.dirname

当前模块的目录名`/Users/clearsparkling/Desktop/TsTest/03`

##### import.meta.filename

当前模块的完整绝对路径和文件名`/Users/clearsparkling/Desktop/TsTest/03/index.js`

## Modularization

### CommonJS

在ESModule未出现的时候，Node选择了使用CommonJS作为Node的模块化方案

现如今已是只推荐使用ESModule作为JS的模块化方案，但如遇到维护老旧项目时，了解CommonJS的语法也是非常重要的。

从Node.js 12.17.0/13.2.0开始正式支持ESM，自那以后在.js文件中，仅支持ESM或者CommonJS，两者不可存在同一个js文件中。

*CommonJS语法*

```javascript
// 导入
const import = require('moduleName/modulePath')

// 导出
module.exports = {
	key: value
}

```

### ESModule

在新项目中更加推荐使用ESM，此语法是ECMAScript官方的语法。

*ESM语法*

*默认导入导出*

```javascript
// 默认导入
import moduleName from modulePath

// 默认导出
export default moduleName/{moduleName1,moduleName2}
```

*命名导入导出*

```javascript 
// 命名导入
import {name1,name2...} from modulePath

// 命名导出
// 行内命名导出
export const name1
// 批量命名导出
export {
	name1,
  name2,
  name3
}
```

## package.json

`package.json`作为Node.js项目的描述文件和配置文件，它常见的作用包括

### 管理项目的元信息

`name`:包名/项目名，用于npm registry标识，不能含大写字母，推荐使用字符分隔`-`

`version`:版本号，格式为 主版本.次版本.修订版本

`private`:npm包的公开发布权限，设置为true时无法执行`npm publish`发布到npm registry，默认为false

`description`:项目描述

`author`:作者信息

`license`:开源协议

`repository`:仓库地址

`keywords`:关键词，便于npm检索

### 管理项目的运行环境

`type`:指定模块系统，默认为commonjs，可设定为module使用ESModule

`engines`:限制Node版本

### 管理项目的依赖

`dependencies`:生产环境依赖

`devDependencies`:开发环境依赖

### 脚本命令类

`scripts`:管理命令调用工具

### 模块入口类

`main`:控制模块入口文件

`exports`:更现代的控制模块入口文件，限制使用者只允许导入特定模块，无法触及底层代码。

```javascript
{
  "name":"export-test",
  "exports": {
    ".": "./index.js",
    "./server": "./server/server.js"
  }
}
```

使用`.`控制指定路径在导入模块时，导入对应的包。

```javascript
// 导入index.js
import {} from 'export-test'

// 导入server.js
import {} from 'export-test/server'
```

# WebPack

## 安装Webpack

```shell
npm init -y
npm install webpack webpack-cli --save-dev
```

配置自定义脚本命令

`package.json`

```json
{
  "scripts": {
    "build": "webpack"
  }
}
```

## 修改webpack打包入口和出口

`webpack.config.js`

```javascript
export default {
	entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(import.meta.dirname,'dist'),
    filename:'my-first-webpack.bundle.js'
  }
}
```

## 自动生成html文件

插件 [html-webpack-plugin](https://webpack.docschina.org/plugins/html-webpack-plugin/#root) 在webpack打包时生成html文件

`webpack.config.js`

```javascript
import HtmlWebpackPlugin from 'html-webpack-plugin'

export default {
	entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(import.meta.dirname,'dist'),
    filename:'my-first-webpack.bundle.js'
  },
  plugin: [
    new HtmlWebpackPlugin({
      template: path.resolve(import.meta.dirname,"src/index.html"),
      filename: path.resolve(import.meta.dirname,'dist/index.html')
    })
  ]
}
```

## 加载器/plugin

加载器 [css-loader](https://webpack.docschina.org/loaders/css-loader/#root) :解析css代码

加载器 [style-loader](https://webpack.docschina.org/loaders/style-loader/#root) :解析后的css代码插入到Dom中

加载器 [less-loader](https://webpack.docschina.org/loaders/less-loader/#root) :解析less代码编译为css代码

[mini-css-extract-plugin](https://webpack.docschina.org/plugins/mini-css-extract-plugin/#root) :提取css代码打包成单独的文件

[css-minimizer-webpack-plugin](https://webpack.docschina.org/plugins/css-minimizer-webpack-plugin/#root) :css代码提取后压缩

[webpack-dev-server](https://webpack.docschina.org/guides/development/#using-webpack-dev-server) :开发环境热更新

[source map](https://webpack.docschina.org/configuration/devtool/#root) :source-map 资源地图 定位原始报错位置 不要在production中启用

[alias]() ：解析别名

[CDN使用]()

## 多页面打包

`webpack.config.js`

```javascript
export default (env, argv) => {
    // 使用cross-env可以自定义多种环境 
    // 使用argv.mode仅development和production可选
    // 具体使用哪种 根据需求选择
    const customizableMode = process.env.NODE_ENV
    const mode = argv.mode ?? 'development'
    
    const isProduction = argv.mode === 'production'
    const baseURL = isProduction ? 'https://www.clearsparkling.xyz/api' : '/api'

    const config = {
        mode: mode,
        entry: {
        		'login': path.resolve(import.meta.dirname,'src/login/index.js'),
          	'content': path.resolve(import.meta.dirname,'src/content/index.js')
        },
        output: {
            path: path.resolve(import.meta.dirname, 'dist'),
            filename: './[name]/index.js'
        },
        plugins: [
            // 自动生成html页面
            new HtmlWebpackPlugin({
                // 定义生成的html页面的模版
                template: path.resolve(import.meta.dirname,'public/login/index.html'),
                // 定义生成的html文件名
                filename: path.resolve(import.meta.dirname,'dist/login/index.html'),
              	chunks: ['login']
            }),
          	new HtmlWebpackPlugin({
                // 定义生成的html页面的模版
                template: path.resolve(import.meta.dirname,'public/content/index.html'),
                // 定义生成的html文件名
                filename: path.resolve(import.meta.dirname,'dist/content/index.html'),
              	chunks: ['content']
            }),
            // css代码提取到单独文件
            new MiniCssExtractPlugin({
              	filename: './[name]/index.css'
            })
        ]
    }

    return config
}
```

