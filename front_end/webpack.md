# webpack

## 基础

### 介绍

* 是一个静态资源打包工具
* 输出的文件叫做bundle
* 以一个或者多个文件作为打包的入口，输出为一个或者多个文件
  * 把零散的资源打包到一起，减少请求数
  * 将一个资源拆散，异步加载提高首次加载的速度

* 开发模式，编译js中的ES module语法，浏览器不识别module语法
* 生产模式，编译js中的ES module，并且压缩js代码

### 原则

* 建议使用import方式导入所有资源，明确依赖关系，按需加载
* JavaScript驱动前端

### 基本示例

```bash
# 在包目录下安装webpack
npm i webpack webpack-cli -D

# npx会在node_modules/.bin目录下查找命令

# 将js打包成开发版
npx webpack ./src/main.js --mode=development

# 将js打包成发布版
npx webpack ./src/main.js --mode=production

# 打包好的文件生成在dist目录下
./dist/

```

### 使用webpack配置文件

```javascript
// 在项目目录中创建 webpack.config.js

module.exports = {
    mode: 'development'
    // mode: 'production'
    // mode: none
}

// 在package.json下编辑scripts属性
{
	"scripts" : {
        "dev": "webpack"
    }
}

// 在命令行中运行
// npm run dev

// 或无需编辑package.json
// npx webpack
```

### 设置webpack的输入输出路径

```javascript
// webpack4.0之后支持零配置启动
// 默认输入 src/index.js
// 默认输出 dist/main.js

const path = require('path')

module.export = {
    entry: path.join(__dirname, './src/index.js'),
    output: {
    	path: path.join(__dirname, './dist'),
        filename: 'bundle.js'
	}
}

module.export = {
    entry: './src/main.js',
    outpus: {
        // 这里path必须是一个绝对路径
		path: path.join(__dirname, './dist'),
        filename: 'bundle.js'
    }
}
```

### 使用webpack自动编译功能

* 检测文件修改自动重新编译
* 生成的输出在项目根目录，并且不可见
* 生成的js中内置自动刷新页面功能

```javascript
// npm i -D webpack-dev-server

// 在package.json下编辑scripts属性
{
	"scripts": {
		"dev": "webpack-dev-server"
	}
}

// 在命令行中运行
// npm run dev

// 或无需编辑package.json
// npx webpack-dev-server
```

### 配置webpack自动复制预览页面

* 自动复制预览页面到根目录

```javascript
// npm i html-webpack-plugin -D

// 在webpack.config.js添加配置
const HtmlWebpackPlugin = require('html-webpack-plug')

// 创建插件
const htmlPlugin = new HtmlWebpackPlugin({
	// 源文件
	template: './src/index.html',
	// 目标文件
	filename: 'index.html'
})

// 添加插件
module.exports = {
    plugin: [htmlPlugin]
}
```

### 配置webpack自动在浏览器中打开页面

```javascript
// 编辑package.json的scripts属性
{
    "scripts": {
        // --open 自动打开
        // --host
        // --port
        "dev": "webpack-dev-server --open --host 127.0.0.1 --port 8080"
    }
}
```

### 配置loader打包非js模块

* webpack默认只能打包js模块
* 通过调用加载器，可以打包其他文件
  * less-loader 打包.less
  * sass-loader 打包.scss
  * url-loader 打包css中url相关文件
  * babel打包ES6模块

#### 打包css文件

```javascript
// 默认无法处理
import './css/test.css'

// 安装
// npm i style-loader css-loader -D

// 在webpack.config.js中配置规则
{
    module: {
        rules: [
            {test: /\.css$/, use: ['style-loader', 'css-loader']}
        ]
    }
}
```

#### 打包less文件

```javascript
// 默认无法处理
import './css/test.less'

// 安装
// npm i less-loader less -D

// 在webpack.config.js中配置规则
{
    module: {
        rules: [
            {test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader']}
        ]
    }
}
```

#### 打包scss文件

```javascript
// 默认无法处理
import './css/test.scss'

// 安装
// npm i sass-loader node-sass -D

// 在webpack.config.js中配置规则
{
    module: {
        rules: [
            {test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader']}
        ]
    }
}
```

#### 配置postCSS（解决浏览器兼容性）

```javascript
// 安装
// npm i postcss-loader autoprefixer -D

// 创建postcss.config.js
const autoprefixer = require('autoprefixer')
module.exports = {
    plugins: [autoprefixer]
}

// 在webpack.config.js中配置规则
// 修改css规则
{
    module: {
        rules: [
            {test: /\.css$/, use: ['style-loader', 'css-loader', 'postcss-loader']}
        ]
    }
}
```

#### 打包CSS url中的文件

```javascript
// 安装
// npm i url-loader file-loader -D

// 在webpack.config.js中配置规则
{
    module: {
        rules: [
            {
                test: /\.jpg|pgn|gif|bmp|ttf|eot|svg|woff|woff2$/, 
                
                // 其中limit标识小于这个值时，转换成BASE64
                use: 'url-loader?limit=16940'
            }
        ]
    }
}
```

#### 打包JS中的高级语法

```javascript
// 安装
// babel转换器相关
// npm i babel-loader @babel/core @babel/runtime -D

// babel语法插件相关
// npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D

// 创建babel.config.js配置文件
module.exports = {
    presets: ['@babel/preset-env'],
    plugins: ['@babel/plugin-transform-runtime', '@babel/plugin-proposal-class-properties']
}

// 修改webpack.config.js中配置规则
{
    module: {
        rule: [
            {
                test: /\.js$/,
                use: 'babel-loader',
                
                // 排除第三方包
                exclude: /node_modules/
            }
        ]
    }
}
```

### 配置打包Vue单文件组件

* vue非单文件组件存在的问题
  * 全局组件名称不能重复
  * 字符串模板没有语法高亮，字符串需要斜杠换行
  * 不支持CSS
  * 没有构建步骤，只能使用ES5

* vue单文件组件组成
  * 后缀名.vue的文件
  * template，组件模板区域
  * script，业务逻辑区域
  * style，样式区域

单文件组件示例：

```vue
<template>
	<!-- 组件模板 -->
</template>

<script>
    // 组件逻辑
    export default {
        // 私有数据
        data: () { return {} },
        
        // 组件方法
        methods: {}
    }
</script>

<style scroped>
	/* 组件样式 */
</style>
```

导入组件示例：

```javascript
import App from './components/App.vue'
```

配置vue loader：

```javascript
// 安装
// npm i vue-loader vue-template-complier -D

const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            }
        ]
    },
    plugins: [
        new VueLoaderPlugin()
    ]
}
```

### 使用Vue

```javascript
// 安装vue
// npm i vue -S

// 导入vue包
import Vue from 'vue'

// 导入组件
import App from './complnents/App.vue'

// 使用组件
const vm = new Vue({
    el: '#app',
    
    // 在node中只能使用render
    // 不支持component
    // 不支持template
    render: h=>h(App)
})

```

### 打包发布webpack

```javascript
// 直接打包
// npx webpack -p

// 配置webpack.config.js
{
    "scripts": {
        "build": "webpack -p"
    }
}

// npm build
```



