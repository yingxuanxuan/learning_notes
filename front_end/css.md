# CSS

## CSS基础

* Cascading Style Sheet的缩写，层叠样式表
* 用来控制网页样式，将样式与内容、结构的配置表
* html发展早期通过元素属性来控制样式，但是结构与样式混用会使网页代码难以维护，逐步从html中解耦出来
* CSS2.1之后，标准分模块制定，进展差异很大，所以放弃版本控制，采用定期发布标准
* 浏览器会忽略不存在的属性和无效的属性值

### CSS基本语法

* 使用`/*注释*/`注释
* 属性名称和属性值均不区分大小写

#### 行内CSS基本语法

* 在html元素style属性内使用纯文本书写CSS
* 多个属性用分号分割

```html
<h1 style="color: blue;background-color: yellow;border: 1px solid black;">
  Hello World!
</h1>
```

#### 独立CSS基本语法

* 选择器（Selector） + 大括号 + 多条声明组成属性集、规则集、规则
* 大括号内多条声明（Declaration）用分号分割
* 属性（Properties）和属性值（Property value）使用冒号分隔
* 一个属性集使用多个选择器时，使用逗号分隔

<div align="left">

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

</div>

示例：

```html
<style>
  h1 {
    color: blue;
    background-color: yellow;
    border: 1px solid black;
  }
  p {
    color: red;
  }
</style>
```

### 在HTML中使用CSS的方式

#### 内联样式表

* 页面多个元素之间会重复编码，不易维护
* 与html结构代码糅合在一起，不易阅读

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">
      Hello World!
    </h1>
    <p style="color:red;">这是我的第一个 CSS 示例</p>
  </body>
</html>
```

#### 内部样式表

* 多个页面之间会重复编码，不易维护

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }
      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>这是我的第一个 CSS 示例</p>
  </body>
</html>
```

#### 外部样式表

示例：

```html
<link rel="stylesheet" href="styles.css" />
```

完整示例：

```html
<!doctype html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8" />
    <title>我的 CSS 测试</title>
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>这是我的第一个 CSS 示例</p>
  </body>
</html>
```



## CSS选择器

### 原子选择器

#### 元素选择器（类型选择器、标签选择器、标签名选择器）

示例：

```css
h1 {
}
```

特殊元素选择器，全局选择器：

```css
* {
    margin: 0;
}
```

全局选择器特殊用途，区别自带选择器和伪类选择器：

```css
/* 
    artical元素选择器与:first-child中间有一个空格
    表示选择artical元素的后代的第一个元素
*/
article :first-child {
}
/*
    artical元素选择器与:first-child中间无空格
    表示选择artical作为其他元素的第一个子元素
*/
article:first-child {
}
/*
    使用*:first-child可以与伪类选择器显著区别
*/
article *:first-child {
}
```

#### 类选择器

```css
.highlight {
    background-color: yellow;
}
```

#### ID选择器

```css
#unique {
}
```

#### 标签属性选择器

根据标签上属性是否存在选择：

```css
a[title] {
}
```

根据标签上属性的值选择：

```css
a[href="https://example.com"] {
}
```

#### 伪类、伪元素选择器

* 伪类
  * 选择元素的某个状态
  * 使用单冒号

```css
a:hover {
}
```

* 常见伪类：
  * :first-child
  * :last-child
  * :only-child
  * :invalid
* 用户行为伪类
  * :hover
  * :focus
  * :link
  * :visited
* 伪元素
  * 选择元素的某个部分，或者某个位置
  * 使用双冒号



* 常用伪元素：
  * ::first-line
  * ::before，特殊伪元素
  * ::after，特殊伪元素



### 组合（关系，Combinator）选择器

#### 后代选择器



#### 子代关系选择器





#### 邻接兄弟选择器



#### 通用兄弟选择器





## CSS生效优先级

### 继承

* 某些css属性会被内部元素集成，如font-size，font-wight，color
* 某些css属性不会被内部元素继承，如width、margin、padding、border

#### 默认值

* 协议中会规定每个属性的默认值（初始值）
* 浏览器（user-agent）为了使得html结构在默认效果下能够区分，给属性设置了一部分默认值

#### 继承控制

* 5个特殊的属性值：
  * `inherit`使子元素属性和父元素相同。开启继承。
  * `initial`设置为该属性的初始值。协议初始。
  * `revert`重置为浏览器的默认样式。在许多情况下，此值的作用类似于 `unset`。
  * `revert-layer` 重置为在上一个层叠层中建立的值。
  * `unset`将属性重置为自然值，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial` 一样

示例：

```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <title>CSS</title>
        <style>
            body {
                color: green;
            }
            .my-class-1 a {
                color: inherit;
            }
            .my-class-2 a {
                color: initial;
            }
            .my-class-3 a {
                color: unset;
            }
        </style>
    </head>
    <body>
        <ul>
            <li>Default <a href="#">link</a> color</li>
            <li class="my-class-1">Inherit the <a href="#">link</a> color</li>
            <li class="my-class-2">Reset the <a href="#">link</a> color</li>
            <li class="my-class-3">Unset the <a href="#">link</a> color</li>
        </ul>
    </body>
</html>   
```

效果：

<div align="left">

<figure><img src=".gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

</div>

说明：

* 浏览器对\<a>标签有默认样式绿色，默认会覆盖继承自父元素\<body>的绿色
* 强制使用inherit属性值，则强制继承父元素\<body>的绿色
* 强制使用initial属性值，color属性的协议默认值为黑色
* 强制使用unset属性值，color属性为自然继承，则继承\<body>元素的绿色

#### 重置所有属性

* 使用all关键字指代所有属性

```css
p {
    all: unset;
}
```

### 优先级（specificity），权重

* 指向更具体的选择器权重越高，如id选择器>类选择器>元素选择器
* 选择器中有一个id选择器，记100分
* 选择器中有一个类选择器，记10分
* 选择器中有一个元素选择器，记1分
* 内联样式记1000分
* 计分不进位
* 继承优先级低于所有选择器

HTML：

```html
<p class="special">我是什么颜色的？</p>
```

CSS：

```css
.special {
  color: red;
}

p {
  color: blue;
}
```

以上代码效果为红色，class选择器权重大于元素选择器



#### !important强制优先，尽量避免使用

```css
.better {
    background-color: gray;
    border: none !important;
}
```

### 层叠（cascade）

```css
p {
  color: red;
}
p {
  color: blue;
}
```

以上代码效果为绿色，样式存在层叠覆盖，后出现的覆盖先出现的



## 常见CSS属性



## HTML元素分类



## 布局示例



## 盒模型

* `padding`（内边距）：是指内容周围的空间。在下面的例子中，它是段落文本周围的空间。
* `border`（边框）：是紧接着内边距的线。
* `margin`（外边距）：是围绕元素边界外侧的空间。

<div align="left">

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

</div>

## 浮动



## 定位



## 背景属性及雪碧图



## 圆角及阴影



## 布局



## 元素居中

