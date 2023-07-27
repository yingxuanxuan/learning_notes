# CSS

## CSS基础

* Cascading Style Sheet的缩写，层叠样式表
* 用来控制网页样式，将样式与内容、结构的配置表
* html发展早期通过元素属性来控制样式，但是结构与样式混用会使网页代码难以维护，逐步从html中解耦出来
* CSS2.1之后，标准分模块制定，进展差异很大，所以放弃版本控制，采用定期发布标准

### CSS基本语法

* 使用`/*注释*/`注释

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

#### 元素（标签）选择器



#### ID选择器



#### 类选择器



#### 属性选择器



#### 伪类选择器



### 组合（关系，Combinator）选择器

#### 后代选择器



#### 子代关系选择器





#### 邻接兄弟选择器



#### 通用兄弟选择器





## CSS生效优先级

### 继承



### 权重

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

以上代码效果为红色，class选择器权重大于元素选择器\


### 层叠

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

