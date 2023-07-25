# HTML

## HTML的历史发展过程

* 1993年6月，超文本标记语言第一版
* 1995年11月，HTML2.0
* 1997年01月14日，HTML3.2，W3C推荐版本
* 1999年12月24日，HTML4.01，相对前一版本微小更改
* **2000年05月，HTML4.01严格版（主要流行版本）**
* **2014年10月28日，HTML5（当前标准）**

## HTML是什么

* HyperText Markup Language的缩写，超文本标记语言
* 超文本指的是可以存放、展示图片、音频、视频、链接等
* 标记指的是可以利用标签包裹文本，给文本添加额外属性

## HTML结构规范

### HTML5结构

```html
<!-- 协议版本声明 -->
<!DOCTYPE html>

<!-- html开始结束标签 -->
<html lang="en">

    <!-- 头部声明，非页面内容 -->
    <head>
        <meta charset="UTF-8">
        <title>页面标题</title>
    </head>
    
    <!-- 页面内容 -->
    <body>
    </body>
</html>
```

### HTML4.01文档声明

```html
!<DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 
Transitional//EN" "http://www.w3.org/TR/
xhtml1/DTD/xhtml1-transitional.dtd">
```



## HTML常用标签

### h标签，heading，标题标签

* 不要为了调整大小跳过标签级别使用标签
* 不要利用h标签调整字体大小

h标签示例：

````html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    普通文本
    <h1>标题一</h1>
    <h2>标题二</h2>
    <h3>标题三</h3>
    <h4>标题四</h4>
    <h5>标题五</h5>
    <h6>标题六</h6>
    普通文本
</body>
</html>
```
````

h标签示例效果：

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### p标签，paragraph，段落标签

* 默认多个文本之间的换行、空格会缩减成单个空格
* 使用p标签包裹的内容占据整块布局显示，具有段落间距和首行缩进

p标签示例：

````markup
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一，普通文本段落一
    普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二，普通文本段落二
    
    <p>
    p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一，p标签文本段落一
    </p>
    <p>
    p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，p标签文本段落二，
    </p>
</body>
</html>
```
````

p标签示例效果：

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### strong标签，强调，em标签，emphasis，着重标签

* strong比em标签更加强调
* strong默认使用粗体显示
* em默认使用斜体显示
* strong更加常用

strong，em示例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>普通段落<em>着重</em>普通段落<strong>强调</strong>普通段落</p>
</body>
</html>
```

strong，em示例效果：

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>











































