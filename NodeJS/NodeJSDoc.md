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

