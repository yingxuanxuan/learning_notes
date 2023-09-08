# node.js

## 基础

* Node.js是一个JavaScript运行环境（runtime）
* 基于Chrome V8引擎
* 官网 <https://nodejs.org/zh-cn>
* 文档 <https://nodejs.org/zh-cn/docs>
* 无法使用浏览器提供的Web API，如BOM、DOM、AJAX
* 内置提供了一些基础API
  * fs，文件系统
  * path，路径
  * http
* 流行的第三方API
  * express，web框架
  * Electron，客户端开发
  * rest，restful api构建

### 查看当前版本

```bash
node -v
```

### 使用node执行js代码

```javascript
// hello.js

console.log('Hello World!');
```

```bash
node hello.js
# Hello World!
```

### 使用模块

* 传统开发模式的问题
  * 命名冲突
  * 文件依赖

* 使用Node内置模块和第三方模块，仅需要引用名称
* 使用用户自定义模块，需要加上相对路径，可以省略后缀名.js
* 加载模块时会执行模块中的代码

```javascript
const os = require('os');

console.log('Operating system:', os.platform());
console.log('Release:', os.release());
console.log('Version:', os.version());

/* 运行结果
Operating system: win32
Release: 10.0.19045
Version: Windows 10 Home China
*/
```

## 模块化

### 模块化历史

* 阶段一：划分文件
  * 特点
    * 在html中导入多个js文件
    * 按js划分模块
    * 多文件间协调依靠约定
  * 缺点
    * 污染全局作用域，命名冲突
    * 无法管理模块间依赖关系
* 阶段二：类命名空间
  * 特点
    * 每个js封装在一个对象中
  * 优点
    * 没有命名冲突
  * 缺点
    * 没有私有空间，私有空间暴露在外
    * 无法管理模块间依赖关系

```javascript
module = {
    method1,
    method2
}
```

* 阶段三：IIFE，立即执行函数

  * 特点
    * 每个js封装在一个闭包中
    * 手动挂载公共部分到全局空间window
    * 使用立即执行函数的参数，表示依赖关系
  * 优点
    * 没有命名冲突
    * 有私有空间，私有空间在闭包中外部无法访问
    * 可以管理模块间依赖关系
  * 缺点
    * 没有规范
    * 人为约定，不受代码控制

```javascript
;(function($){
	$('body').animate();
    window.moduleA = {
        method1,
        method2
    }
})(jQuery)
```

*  阶段四：CommonJS规范
  * 特点
    * 一个文件就是一个模块
    * 每个模块有单独的作用域
    * 通过module.exports导出
    * 通过requre函数导入
  * 优点
    * 解决了前面阶段的缺点
  * 缺点
    * 只支持node.js
    * 全部同步加载
* 阶段五：AMD规范，Asynchronous Module Definition
  * 特点
    * 通过Require.js实现了这个规范
    * 通过define函数定义模块
  * 优点
    * 支持前端
    * 异步加载
    * 普及程度高
  * 缺点
    * 复杂度高
    * 模块多时，请求次数多，效率低

  ```javascript
  // 定义一个模块
  define('module1', ['jquery', './module2'], function($, module2) {
      return {
          start: function () {
              $('body').animate({});
              module2();
          }
      }
  });
  
  // 加载一个模块
  require(['module1'], function(module1){
      module1.start();
  })
  
  ```

* 阶段六：现状
  * （主流）在node.js环境中使用CommonJS规范
  * （正在普及）node.js也支持ES6 Module规范
  * （正在普及）在浏览器中使用ES6 Module规范
  * （主流）后端打包
    * 通过打包工具兼容不支持ES6 Module的浏览器
    * 通过打包为单文件，解决多模块多次请求的问题

### 模块化规范

* 浏览器端的模块化规范：AMD
  * 典型代表：Require.js
* 浏览器端的模块化规范：CMD
  * 典型代表：Sea.js
  * 已经被Require.js支持
* 服务器端的模块化规范：Node的CommonJS规范
  * 模块分为单文件模块和包
  * 导出：module.exports和exports
  * 导入：require('模块化标识符')
* ES6模块化规范，浏览器端、服务器端通用的模块化规范
  * 每个js文件都是一个独立的模块
  * 导入：import
  * 导出：export
  * 在Node.js中需要使用`babel`语法转换工具体验ES规范

### ES6模块化规范

#### ES6规范

* 默认导出和按需导出可以共存
* 不使用花括号接收的是默认导出的属性
* 默认导出只能导出一次
* 按需导出可以导出多次
* 与CommonJS规范不同，ES6规范导入时不能省略`.js`后缀名
* 与CommonJS规范不同，ES6规范导入时不能只导入目录，不会自动搜索，不能省略文件名index.js
* 与CommonJS非凡不同，ES6非凡导入时不能省略路径`./`，不会自动搜索

#### 默认导出示例

```javascript
// 当前文件m1.js
let a = 10
let c = 20
let d = 30
function show() {}

// 这里的花括号是对象
export default {
    a,
    c,
    show
}
```

#### 默认导入示例

```javascript
import m1 from './m1.js'
// m1为接收名称，default导出必须重命名
// 这里不能省略.js后缀名

console.log(m1)
// {a: 10, c: 20, show: [Function: show]}
```

#### 按需导出示例

```javascript
// 分别导出
export let s1 = 'aaa'
export let s2 = 'ccc'
export function say = function() {}

// 尾部集中导出
let s1 = 'aaa'
let s2 = 'ccc'
function say = function() {}

export {s1, s2, say} // 这里的花括号不是对象，是一个固定语法

// 导出时重命名
export {
    s1 as s1_a,
    s2 as s2_a,
    say as say_a
}
```

#### 按需导入示例

```javascript
import {s1} from 'module'  // 这里的花括号不是对象拆包，是一个固定语法
// s1 必须与导出名称相对应
```

#### 同时导入默认和按需导出

```javascript
// m1为默认
// s1为按需
import m1, {s1} from './m1'

// 使用default关键字拿到默认
import {default as m1, s1} from ./m1
```

#### 使用as给按需导入重命名

```javascript
import {s1 as s1_alias} from './m1'
```

#### 导入的同时导出

* 一般用于index.js编写，汇聚所有模块

```javascript
export {foo, bar} from './module.js'

// 汇聚示例
export { Button } from './button.js'
export { Avatar } from './avatar.js'
```

#### 浏览器中使用ES6模块

* 通过给script添加type=module执行ES6模块标准
* ES Module中自动采用严格模块，不需要`use strict`，this指向undefined
* 运行在私有作用域中

```html
<!-- 在html中定义一个ES Module -->
<script type="module">
</script>
```

* 通过CORS的方式请求外部JS模块，非本地模块会报跨域错误

```html
<script type="module" src="https://libs.baidu.com/jquery/2.0.0/jquery.min.js">
```

* ES Module中的script会延迟执行，不会阻碍网页显示（原生script会根据代码位置按顺序执行）

```html
<!-- 延迟加载 -->
<script type="module"></script> 
<script defer></script>
```

#### 使用babel使node支持ES6规范（早期node版本）

```bash
# 安装babel
npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
npm install --save @babel/polyfill

# 在项目根目录创建babel.config.js配置文件

# 编辑babel.config.js添加
const preset = [
	[
		"@babel/env", 
        {
            targets: {
                edge: "17",
                firefox: "60",
                chrome: "67",
                safari: "11.1"
            }
        }
	]
]
module.export = { presets };

# 通过npx执行代码
npx babel-node index.js
```

#### node原生支持ES6module（node 10）

* 所有文件必须使用mjs后缀名
* 导入时使用文件全名`import test from 'test.mjs'`

```bash
node index.mjs

# 低版本
node --experimental-modules index.mjs
```

#### node默认使用ES6module（node 12）

* 在package.json中配置type字段为module（node 12）
* mjs无需修改为`.msj`后缀名
* 但是CommonJS需要命名为`.csj`

#### 只执行模块不导入模块

```javascript
// 只执行
import {} from './module.js'

// 简写
import './module.js'
```

#### 导入所有按需导出

```javascript
import * as mod from './module.js'
```

#### 动态导入模块

```javascript
// import 语句不支持变量
// path = './module.js'
// import {} from path

// 动态导入应使用import函数
import('./module.js').then(function (module) {
                                      
})
```

#### 在MJS模块中导入CJS模块

* 可以在mjs中使用import语句导入cjs模块
* 只支持默认导入，不支持按需导入

#### 在CJS模块中导入MJS模块

* node原生不支持
* 需要webpack支持

#### 在MJS中不能使用CJS环境变量

* require
* module
* exports
* __filename
* __dirname

```js
import { fileURLToPath } from 'url'
const __filename = fileURLToPath(import.meta.url)

import { dirname } from path
const __dirname = dirname(__filename)
```



### CommonJS规范

* node给js新增模块作用域，默认外部无法访问模块作用域内部的变量
* 每一个模块都有一个module对象，用于保存与当前模块有关的信息
* 可以在模块中打印module，`console.log(module)`
* 默认module.exports是一个空对象
* 外部require得到的就是module.exports所指向的对象
* 可以给module.exports添加属性，用于暴露模块属性给外部
* 默认情况下module.exports可以简化为exports，除非手动改变module.exports指向的对象

```javascript
// 给exports添加属性
module.exports.username = 'username';
module.exports.sayHello = function() {
    console.log('say hello!')
};

// 将内部属性挂在到exports上
const age = 20;
module.exports.age = age

// 直接改变exports指向的对象
module.exports = {
    username = 'new_username',
    syaHello() {
        console.log('New say hello!')
    }
}

// 简写为exports
exports = {}
```

### API的mjs、cjs版本示例

* .mjs和.cjs文件是两种不同的模块格式
* .mjs文件是ES模块
* .cjs文件是CommonJS模块。
* ES模块是ECMAScript 6引入的一种新的模块格式，它使用import和export语句来导入和导出模块。
* CommonJS模块是Node.js最初使用的一种模块格式，它使用require和module.exports语句来导入和导出模块。

#### mjs示例

```javascript
import { unlink } from 'node:fs';

unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('successfully deleted /tmp/hello');
});

```

#### cjs示例

```javascript
const { unlink } = require('node:fs');

unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('successfully deleted /tmp/hello');
});
```

### API的Promise、Callback、Synchronous版本示例

#### Promise版本

```javascript
import { unlink } from 'node:fs/promises';

try {
  await unlink('/tmp/hello');
  console.log('successfully deleted /tmp/hello');
} catch (error) {
  console.error('there was an error:', error.message);
}
```

#### Callback版本

```javascript
import { unlink } from 'node:fs';

unlink('/tmp/hello', (err) => {
  if (err) throw err;
  console.log('successfully deleted /tmp/hello');
});
```

#### Synchronous版本

```javascript
import { unlinkSync } from 'node:fs';

try {
  unlinkSync('/tmp/hello');
  console.log('successfully deleted /tmp/hello');
} catch (err) {
  // handle the error
}
```

## NPM

### 包安装

* 包安装会覆盖之前安装，不需要卸载

```bash
# 安装包
npm install 包名称

# 安装包缩写
npm i 包名称

# 安装指定版本包
npm install 包名称@版本号
```

### 语义化版本规范

* 以`2.24.0`为例
* 第一位：大版本
* 第二位：功能版本
* 第三位：Bug修复版本
* 以点分隔
* 默认从`1.0.1`开始
* 前面的版本号更新，则后后面版本号归零

### NPM包管理规范

* npm默认将包安装在项目目录的node_modules文件夹
  * 需注意此目录不要作为源代码进行版本管理

* 使用package-lock.json管理包的下载信息
* 在项目根目录中必须提供一个名为package.json的包管理配置文件
* package.json记录一些与项目有关的配置信息
  * 项目名称
  * 版本号
  * 描述
  * 入口js
  * 依赖的其他包

* 在项目目录中`npm install`时，会自动把依赖的包和版本号记录在package.json中

```bash
# 创建package.json

# 在新项目的文件夹中执行
# 项目文件夹不能包含中文和空格
npm init
npm init -y
```

### 入口JS

* package.json中main参数指向一个js，默认为index.js
* require导入时，如果不指定到js文件路径，只指向目录名称，则会使用main指向的js作为入口
* 可以在index.js中暴露子模块

```javascript
const data = require('./src/dataFormat')
const escape = require('./src/htmlEscap')

module.export = {
    ...date,
    ...escape,
}
```

### 下载所有依赖

```bash
# 在项目文件夹中执行

npm install
npm i
```

### 卸载包

* 卸载包会自动从package.json中移除

```bash
npm uninstall 包名称
```

### 依赖

* dependencies：开发和项目上线后都需要用到的包
* devDependencies：只在开发阶段会用到，项目上线之后不会用到的包

```bash
# 将安装的包记录到devDependencies中
npm install 包名 --save-dev

# 简写
npm i 包名 -D # 旧版
npm i 包名 -S # 新版

# 例如官网会给出提示
npm install --save-dev webpack
```

### 淘宝NPM镜像

#### 修改npm仓库地址

```bash
# 查看当前的仓库地址
npm config get registry

# 设置npm仓库地址
npm config get registry=https://registry.npm.taobao.org/
```

#### nrm切换仓库地址

```bash
# 安装
npm i nrm -g

# 查看所有镜像源
nrm ls

# 切换镜像源
nrm use taobao
```

### 发布NPM包

```bash
# 切换到官方服务器
nrm ls
nrm use npm

# 登录npm
npm login
# 交互式命令行

# 进入需要发布的包的目录

# 发布包
npm publish

# 删除已发布的包（72小时之内）
npm unpublish 包名 --force
```

### NPM加载包的机制

#### 包名匹配机制

* 把包名当做确切文件名
* 包名 + .js
* 包名 + .json
* 包名 + .node
* 加载失败报错

#### 包查找机制

* 如果require包名不是内置模块，也不带路径
* 查找当前目录node_modules文件夹
* 在上一层父目录nodel_modules文件夹中查找
* 直到根目录
* 报错

#### 目录包加载机制

* 在目录中查找package.json文件，加载main属性指向的js
* 如果失败则加载目录下index.js文件
* 报错

## fs，文件系统模块

```
```







