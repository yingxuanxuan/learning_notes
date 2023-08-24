# vue.js.v2

## vue基础

### 导入vue

#### html script导入

开发版：

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
```

生产版本：

```html
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
```

#### NPM导入

```bash
npm i vue
```

### 完整基础结构

```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <title>CSS</title>

    </head>
    <body>
        <!-- 被绑定的基础元素 -->
        <div id="app">
            <h2>{{ msg }}</h2>
        </div>

        <!-- 导入 -->
        <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
        <script>
            const vm = new Vue(
                // 入参是一个对象
                {
                    // 被绑定的元素id
                    el: '#app',
                    // 数据
                    data: {
                        msg: "Hello World!"
                    }
                }
            );
        </script>
    </body>
</html>
```

效果：
<div align="left">
    <figure>
    	<img src=".gitbook/assets/image-20230801135056780.png" alt="">
        <figcaption></figcaption>
    </figure>
</div>
### 文本渲染、模板、插值

```html
<div id="app">
    <!-- 插入vm绑定的数据 -->
    <h2>{{ msg }}</h2>

    <!-- 插入vm绑定的方法 -->
    <h2>{{ getContent() }}</h2>

    <!-- 插入js表达式 -->
    <h2>{{ 1 + 1 }}</h2>
    <h2>{{ 1 > 0 ? 'Y': 'N' }}</h2>
    <h2>{{ '123'.split('').reverse().join('')  }}</h2>

    <!-- 插入对象（表达式的一种） -->
    <!-- 注意三个大括号不能连着写 -->
    <h2>{{ {a: 1} }}</h2>

    <!-- 不能插入HTML，不会解释html -->
    <div>{{ <h2>不会解释html</h2> }}</div>

    <!-- 这是语句，不是表达式 -->
    <!-- {{ var a = 1 }} -->

    <!-- 流控制也不会生效，请使用三元表达式 -->
    <!-- {{ if (ok) { return message } }} -->
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js">
</script>

<script>
    const vm = new Vue({
            el: '#app',
            data: {
                msg: "Hello World !",
                content: 'C O N T E N T'
            },
            methods: {
                getContent() {
                    return this.content
                }
            }
        }
    )
</script>
```

效果：
<div align="left">
    <figure>
    	<img src=".gitbook/assets/image-20230801151139124.png" alt="">
        <figcaption></figcaption>
    </figure>
</div>
### v-text，绑定文本

* 文本渲染的指令形式
* 不会渲染html标签
* 相当于修改innerText

```html
<div id='app'>
	<h2 v-text='msg'></h2>    
</div>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            msg: "Hello World !"
        }
    })
</script>
```

### v-html，绑定HTML

* 插入html
* 会渲染html标签
* 相当于修改innerHTML

```html
<div id='app'>
    <div v-html="html"></div>
</div>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            html: "<h2>HHHHHHHH</h2>"
        }
    })
</script>
```

### v-bind，绑定属性

* 属性不能使用双大括号绑定
* 用于绑定属性

示例：

```html
<div id='app'>
    <a v-bind:href = "res.url" :title = "res.name">{{ res.name }}</a>
</div>
<script>
	const vm = new Vue({
        el: '#app',
        data: {
            res: {
                url: 'http://www.baidu.com',
                name: '百度'
            }
        }
    });
</script>
```

效果：
<div align="left">
    <figure>
    	<img src=".gitbook/assets/image-20230801164334815.png" alt="">
        <figcaption></figcaption>
    </figure>
</div>


### 动态参数

* 使用中括号包裹变量
* 绑定不特定属性

v-bind:[attributeName]



v-on:[eventName]



移除动态参数：

```html
<a v-bind:[null]></a>
```

注意事项：

* 中括号中尽量不要使用表达式
* 如果有计算需要可以使用计算属性
* 属性变量不能使用大写，大写会被自动转换为小写在vm数据模板中查找



### 修饰符



### html class绑定

* vue对class和style做了专门的增强，可以使用对象语法和数组语法
* 短横线必须用引号括起来

#### 对象语法

```html
<!-- 仅使用v-bind绑定 -->
<h3 v-bind:class = "{active: isActive}"></h3>

<!-- 同时使用原生class和v-bind绑定 -->
<h3 class="name" v-bind:class = "{active: isActive}"></h3>
```

#### 数组语法

```html
<div v-bind:class="[activeClass, errorClass]"></div>

<!-- 根据条件的数组写法 -->
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

#### 数组对象结合语法

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### html内联样式style绑定

* 短横线必须用引号括起来
* style的驼峰写法不用加引号，vue会自动转换成短横线写法

#### 对象语法

```html
<!-- 直接写对象，支持表达式 -->
<h3 v-bind:style="{color: color, fontSize: fontSize + 'px'}"></h3>
data: {
  activeColor: 'red',
  fontSize: 30
}

<!-- 直接写对象名称 -->
<div v-bind:style="styleObject"></div>
<script>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
</script>
```

#### 数组语法

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### 条件渲染

#### v-if，v-else-if，v-else

```html
<div id="app">
    <h1 v-if = "getRandom() < 0.3">小于0.3</h1>
    <h1 v-else-if = "0.6 > getRandom() > 0.3">大于0.3小于0.6</h1>
    <h1 v-else>大于0.6</h1>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
<script>
    const vm = new Vue({
        el: '#app',
        methods: {
            getRandom() {
                return Math.random()
            }
        }
    })
</script>
```

#### v-show

* 控制是否显示标签
* 相当于`style="display: none"`
* v-if 则标签不存在
* 开销较小，性能更好

### 事件绑定，v-on

```bash


```

* 缩写为at符号

#### 事件修饰符



### 列表渲染，v-for

#### 迭代数组

示例：

```html
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
</ul>

<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</script>
```

获取当前索引id：

```html
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

if of 通用

```html
<div v-for="item of items"></div>
```

#### 迭代对象

示例：

```html
<ul id="v-for-object" class="demo">
    <!-- 迭代的是object的value -->
    <li v-for="value in object">
    {{ value }}
	</li>
</ul>
<script>
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
</script>
```

获取key：

```html
<div v-for="(value, name) in object">
  {{ name }}: {{ value }}
</div>
```

获取key和index

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```





### 双向绑定，v-model

* v-model会忽略表单元素的value、checked、selected的初始值，使用已vue示例的数据作为数据来源
* v-model为不同的表单元素映射了不同的，html属性和事件
  * text、textarea使用value属性，input事件
  * checkbox、radio使用checked属性，change事件
  * select使用value属性，change事件

#### 单行文本，input-text

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

#### 多行文本，textarea

```html
<textarea v-model="message" placeholder="add multiple lines"></textarea>
<!-- 作用是将段落元素中的空格和换行符保留，而不是忽略它们 -->
<p style="white-space: pre-line;">{{ message }}</p>
```

#### 复选框，checkbox

* 单个复选框绑定到布尔值

```html


```

* 多个复选狂绑定到数组

```html


```

#### 单选按钮，radio

* 绑定到value文本

#### 选择框，select

* 单选绑定value到文本



* 多选绑定value到数组



* 结合v-for渲染option



### 计算属性，computed

```html
<div id="example">
	<p>Original message: "{{ message }}"</p>

    <!-- 像使用data数据一样使用计算属性，类似于python property -->
	<p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
});
</script>
```

#### 为什么不使用methods

* 计算属性是有缓存的，仅当依赖的data发生变化时才重新计算，methods每次都会计算

#### 计算属性setter

* 反向更新给data中的数据

```html
<script>
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
</script>
```







### 侦听器，watch



### 过滤器，filter



### 缩写

#### v-bind缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

#### v-on缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```



## vue组件化开发

### 组件基础

#### 使用局部组件



#### 注册全局组件



#### data工厂函数



#### 从外向内传递数据：props

* props是字符串时，只传单一值
* props是数组时，传递多个值

```html


```

#### 从内向外传递数据：emit（监听子组件事件）

* 父子均需绑定，函数调用emit

```html


```

* 简写emit

```html


```

#### 平行组件间传递数据，bus方案

* new 一个vue对象bus，作为中央总线
* 触发源bus.$emit('')
* 接受者在created中注册bus.$on('eventname', (n)=>{ this.action })

* vuex



#### 多层嵌套组件简历通讯关系，provide、inject





#### 内外双向绑定





#### 组件有且只有一个根元素



### 插槽，组件中的用户自定义区域

#### 匿名插槽

```html
<slot></slot>
```



#### 具名插槽

* 按名称调用使用哪个插槽

```html
# 定义
<slot name="xxx"></slot>

# 使用
<templte slot="xxx">
</templte>
```



#### 作用域插槽

```html
```

### 生命周期



### 异步组件，import



### $refs

#### 给标签添加ref

#### 给子组件添加ref



### 异步更新队列

* 主动调用nexttick立即更新组件

* 



### 动态添加属性，$set，Object.assign



### 复用技术，mixins

* 属性重复时，vue优先，minin劣后



## vue-cli3单文件组件

### 基础

#### script开发的缺陷

* 组件名称不能重复
* es6引进模板字符串不能自动提示
* 不支持CSS
* 没有构建步骤
  * 不能使用装饰器等语法
  * 不支持.vue单文件组件

#### 安装cnpm淘宝加速镜像

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

#### 安装vue cli

```bash
cnpm install -g @vue/cli
```

#### 检查vue cli版本

```bash
vue --version
```

#### 快速原型开发

* 使用`vue serve`和`vue build`命令对单个`*.vue`文件开发
* 需要安装插件`cnpm install -g @vue/cli-service-global`
* 使用的是全局包依赖，在不同及其上一致性无法保证
* npm init生成项目

```bash
# 初始化项目目录
npm init

# 填写项目信息

# 生成package.json项目配置


# 安装vue插件
cnpm install -g @vue/cli-service-global

# 编写App.vue

npm run serve
```

```vue

<template>
    <div>
        <h3>{{ msg }}</h3>
    </div>
</template>

<script>
export default {
    data(){
        return {
            msg: "HHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHH#"
        }
    }
}
</script>

<style scoped>
    h3 {
        color: red;
    }
</style>
```

#### vue生成项目

```bash
vue create 项目名称

# 进入vue交互式配置


```



#### 模拟数据，Mock server，webpack devServer

* 编辑vue.config.js

```javascript
module.exports = {
    devServer: {
        // mock数据模拟
        before(app, server) {
            app.get('/api/cartList', ()=>{
                res.json({
                    result: [
                        {id: 1},
                        {id: 2}
                    ]
                })
            })
        }
    }
}
```

#### 使用axios

import axios from 'axios'

Vue.prototype.$http = axios



#### 使用element-ui

* 完整导入

```javascript
# node安装element-ui
# cnpm install element-ui

import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(ElementUI);

# vue-cli导入
# vue add element
```

* 按需导入

```javascript
```







## vue-router





## vuex



## Demo

### 音乐播放器

* 功能
  * audio下5个mp3文件
  * 后端提供一个歌曲列表，保存每个歌曲的信息
  * 打开页面后自动播放第一首歌曲
  * 鼠标点击播放列表能切换歌曲
  * 歌曲列表中能看到当前播放的歌曲被特殊标记

### 轮播图

* 功能

  * images目录下5个jpg文件

  * 初始化显示第一张图片

  * 点击按钮可以切换上一张，下一张

```html
<!-- vue绑定的目标根元素 -->
<div id="app">
  <!-- 绑定事件click和处理methods -->
  <button @click="toPrevImg()">上一张</button>
  <button @click="toNextImg()">下一张</button>
  <!-- 绑定img.src属性 -->
  <div>
    <img :src="curImgSrc" />
  </div>
</div>
<!-- 导入vue -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
<script>
  const vm = new Vue({
    // 绑定目标根元素app
    el: "#app",

    // 数据模型
    data: () => {
      return {
        cur_idx: 1,
      };
    },

    // 计算属性根据当前id值计算图片相对路径
    computed: {
      curImgSrc() {
        return `./images/${this.cur_idx}.jpg`;
      },
    },

    // 点击时触发改变id值，越界复位
    methods: {
      toPrevImg() {
        if (this.cur_idx >= 5) {
          this.cur_idx = 1;
        } else {
          this.cur_idx++;
        }
      },
      toNextImg() {
        if (this.cur_idx <= 1) {
          this.cur_idx = 5;
        } else {
          this.cur_idx--;
        }
      },
    },
  });
</script>
```



