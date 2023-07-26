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

#### h标签示例

代码：

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

效果：

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### p标签，paragraph，段落标签

* 默认多个文本之间的换行、空格会空白折叠，缩减成单个空格
* 使用p标签包裹的内容占据整块布局显示，具有段落间距和首行缩进

#### p标签示例

代码：

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

效果：

<figure><img src=".gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

### strong标签，强调，em标签，emphasis，着重标签

* strong比em标签更加强调
* strong默认使用粗体显示
* em默认使用斜体显示
* strong更加常用
* strong与b标签比较，strong带有逻辑意义，b仅标识物理样式
* em与i标签同上

#### strong，em示例

代码：

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

效果：

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### br标签，换行

* 文本换行存在空白折叠，多个空格、换行会替换为一个空格，达不到换行效果
* xhtml中规范协作`<br/>`，html5中写作`<br>`

#### br示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>
        第一行
        第二行
        第三行
        第四行
    </p>
    <p>
    第一行<br/>
    第二行<br/>
    第三行<br/>
    第四行<br/>
    </p>
</body>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

### hr标签，horizontal，水平分割线

* 默认样式为水平线，html5标准将hr定义修改为分割结构上主题内容的转换
* 仅效果上的水平分割线应使用css实现

#### hr示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>
        第一行
        <hr>
        第二行
    </p>
    <p>
    第一行
    </p>
    <hr>
    <p>
    第二行
    </p>
</body>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

### ul标签，unordered list，无序列表

* 可以通过css属性list-style-type设置列表前的圆点样式
* ul自身可以无限嵌套
* ul和ol可以交替使用，并无限嵌套

#### ul示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <li>第一项</li>
        <li>第二项</li>
        <li>第三项</li>
    </ul>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

### ol标签，ordered list，有序列表

#### ol示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ol>
        <li>第一项</li>
        <li>第二项</li>
        <li>第三项</li>
    </ol>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

### dl标签，description list，描述列表

* dl也可以解释为definition list，定义列表
* dt，description term，描述项
* dd，description details，描述细节

#### dl示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>Cryptids of Cornwall:</p>

    <dl>
        <dt>Beast of Bodmin</dt>
        <dd>A large feline inhabiting Bodmin Moor.</dd>
    
        <dt>Morgawr</dt>
        <dd>A sea serpent.</dd>
    
        <dt>Owlman</dt>
        <dd>A giant owl-like creature.</dd>
    </dl>
    
</html>
```

效果：

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

### table标签，表格

* \<table>\</table> 最外层表格标签
* \<thead>\</thead> 表头结构，可选
* \<tbody>\</tbody> 表身结构，可选
* \<tfoot>\</tfoot> 表尾结构，可选
* \<tr>\</tr> 表的一行
* \<td>\</td> 行中的一列
* \<th>\</th> 标题项，可选，可以在首行中均为th，也可以在各行中第一列为th
* \<caption> 表标题，可选
* \<colgroup> 列组，可选，给列分组，整体设置属性

#### 完整表格示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table>
        <colgroup>
            <col style="background-color: #0f0" />
            <col span="2" />
        </colgroup>
        <caption>表标题</caption>
        <thead>
            <tr>
                <th>第一行第一列，纵向表头</th>
                <th>第一行第二列，纵向表头</th>
                <th>第一行第三列，纵向表头</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <th>第二行第一列，横向表头</th>
                <td>第二行第二列，内容</td>
                <td>第二行第三列，内容</td>
            </tr>
            <tr>
                <th>第三行第一列，横向表头</th>
                <td>第三行第二列，内容</td>
                <td>第三行第三列，内容</td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <th>第四行第一列，表尾横向表头</th>
                <td>第四行第二列，表尾内容</td>
                <td>第四行第三列，表尾内容</td>
            </tr>
        </tfoot>
    </table>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### 最简表格示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <table>
        <tr>
            <td>第一行第一列</td>
            <td>第一行第二列</td>
            <td>第一行第三列</td>
        </tr>
        <tr>
            <td>第二行第一列</td>
            <td>第二行第二列</td>
            <td>第二行第三列</td>
        </tr>
    </table>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

#### 跨行跨列示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        table, td{
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <table>
        <tr>
            <td>1x1</td>
            <td colspan="3">1x2</td>
            <!-- <td>1x3</td> -->
            <!-- <td>1x4</td> -->
            <td>1x5</td>
            <td>1x6</td>
        </tr>
        <tr>
            <td>2x1</td>
            <td>2x2</td>
            <td>2x3</td>
            <td>2x4</td>
            <td>2x5</td>
            <td>2x6</td>
        </tr>
        <tr>
            <td>3x1</td>
            <td>3x2</td>
            <td rowspan="3">3x3</td>
            <td>3x4</td>
            <td>3x5</td>
            <td>3x6</td>
        </tr>
        <tr>
            <td>4x1</td>
            <td>4x2</td>
            <!-- <td>4x3</td> -->
            <td>4x4</td>
            <td>4x5</td>
            <td>4x6</td>
        </tr>
        <tr>
            <td>5x1</td>
            <td>5x2</td>
            <!-- <td>5x3</td> -->
            <td>5x4</td>
            <td>5x5</td>
            <td>5x6</td>
        </tr>
        <tr>
            <td>6x1</td>
            <td>6x2</td>
            <td>6x3</td>
            <td>6x4</td>
            <td>6x5</td>
            <td>6x6</td>
        </tr>
    </table>
</html>
```

效果：

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### 单像素边框设置css

```css
/* 打开所有table外层边框，td内层边框 */
table, td{
    border: 1px solid red;
}
/* table内所有td边框间距为0 */
table{
    border-spacing: 0;
}
/* 关闭单个td上边和左边（只保留下边和右边） */
td {
    border-top-width: 0;
    border-left-width: 0;
}
/* 关闭所有tr中最后一个td的右边（使用table的右边） */
td:last-child{
    border-right-width: 0;
}
/* 关闭最后一行tr中所有td的下边（使用table的下边） */
tr:last-child td{
    border-bottom-width: 0;
}
```

### a标签，anchor，锚点

#### target属性

* 通过target可以设置打开链接的方式
* target默认值是`_self`，在本页面打开连接
* 所有选项：
  * `_self`：当前页面加载。（默认）
  * `_blank`：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
  * `_parent`：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 `_self` 相同。
  * `_top`：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 `_self` 相同。

#### 相对路径

```markup
<a href="//example.com">相对于协议的 URL</a>
<a href="/zh-CN/docs/Web/HTML">相对于源的 URL</a>
<a href="./p">相对于路径的 URL</a>
```

#### 页面内跳转

```html
<!-- <a> 元素链接到id所在的位置 -->
<p><a href="#Section_further_down">跳转到id所在位置</a></p>

<!-- 要链接到的标签 -->
<h2 id="Section_further_down">id所在位置</h2>
```

#### a标签示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>You can reach Michael at:</p>

    <ul>
        <li><a href="https://example.com">Website</a></li>
        <li><a href="mailto:m.bluth@example.com">Email</a></li>
        <li><a href="tel:+123456789">Phone</a></li>
    </ul>
    
</html>
```

效果：

## ![](<.gitbook/assets/image (8).png>)

### img标签，image，图片

* alt属性，在获取不到图片时，在图片位置替代显示
* width和height只设置一个时，会自动保持宽高比

#### img标签示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <img src="https://www.baidu.com/img/PCtm_d9c8750bed0b3c7d089fa7d55720d6cf.png">
</html>
```

效果：

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

### form标签，表单

#### form常用属性

* action属性，值为表单提交的路径，这个值可被 `<button>`、`<input type="submit">` 或`<input type="image">` 元素上的 `formaction` 属性覆盖。
* method属性，值为表单提交的http方法，默认为get，可能的值：
  * get，表单数据附加在action指向的提交路径url后，以?分隔
  * post，表单数据附加在表单体内
  * dialog，略
* enctype属性，值为post上报HTTP头中content-type的类型：
  * `application/x-www-form-urlencoded`：未指定属性时的默认值
  * `multipart/form-data`：当表单包含 `type=file` 的`<input>`元素时使用此值
  * `text/plain`：出现于 HTML5，用于调试。这个值可被 `<button>`、`<input type="submit">` 或 `<input type="image">` 元素上的 `formenctype` 属性覆盖。

#### input常用属性

* `type`属性默认值为`text`，即不填写属性时`type="text"`
* `type="text"`时，输入的文字正常显示
* `type="password"`时，输入文字显示为`*`
* `type`为`"text"`或`"password"`时，可以通过设置value属性为input填写默认内容
* `type="submit"`时，input标签展示为提交控件，按钮内容默认为`"提交"`
* `type="submit"`时，可以设置value属性修改按钮内容
* 通过设置input的placeholder属性设置无内容时input控件显示的提示词
* 通过设置intput的name属性为表单输入项设置名称

#### input类型示例

[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input#input\_%E7%B1%BB%E5%9E%8B](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input#input\_%E7%B1%BB%E5%9E%8B)

#### form get提交示例

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything">
        <input type="text" name="username">
        <input type="password" name="password">
        <input type="submit" value="发送">
    </form>
</html>
```

界面：

![](<.gitbook/assets/image (4).png>)

url:

```url
http://192.168.4.101:5555/anything?username=yx&password=123456
```

#### form post提交示例

代码：

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything" method="post">
        <input type="text" name="username">
        <input type="password" name="password">
        <input type="submit" value="发送">
    </form>
</html>
```

界面：

![](<.gitbook/assets/image (4).png>)

http post内容：

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### label关联input

* 点击label，定位到input
* label标签的for属性设置为关联input标签的id属性

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /* 给label设置固定宽度 */
        form label{
            display: inline-block;
            width: 4em;
        }
    </style>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything" method="post">
        <div>
            <label for="u">用户名：</label>
            <input type="text" name="username" id="u">
        </div>
        <div>
            <label for="m">密码：</label>
            <input type="password" name="password" id="m">
        </div>
        <div>
            <input type="submit" value="发送">
        </div>
    </form>
</html>
```

效果：

<figure><img src=".gitbook/assets/test.gif" alt=""><figcaption></figcaption></figure>

### \<input type="radio"> 单选

* name属性值相同的radio标签为相同单选的不同选项
* 相同单选的不同选项互斥
* 可以结合label标签for属性为input.radio设置关联标签
* 设置`input.checked="checked"`的单选input标签为默认选项，或者仅添加`input.checked`属性
* `input.value`属性的值为`input.radio`当前值，即表单提交时`input.radio.name`对应表单项的值

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything" method="post">
        <div>
            <label for="m">男</label>
            <input type="radio" name="sex" id="m" value="male" checked>
        </div>
        <div>
            <label for="f">女</label>
            <input type="radio" name="sex" id="f" value="female">
        </div>
        <div>
            <input type="submit">
        </div>
    </form>
</html>
```

效果：

<figure><img src=".gitbook/assets/test (1).gif" alt=""><figcaption></figcaption></figure>

### \<input type="checkbox"> 多选

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything" method="post">
        <div>
            <label for="h1">跑步</label>
            <input type="checkbox" name="hobby" id="h1" value="running">
            <label for="h2">游泳</label>
            <input type="checkbox" name="hobby" id="h2" value="swimming">
            <label for="h3">跳舞</label>
            <input type="checkbox" name="hobby" id="h3" value="dancing">
        </div>
        <div>
            <input type="submit">
        </div>
    </form>
</html>
```

效果1：无选择时无payload

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>



效果2：单选

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

效果3：多选

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### \<select> 下拉列表

* select默认是单选，下拉列表
* 给select标签添加multiple即可变成多选，样式变为选择框，按住ctrl键即可多选
* 多选情况下可以添加多个selected作为默认选项

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="http://192.168.4.101:5555/anything" method="post">
        <div>
            <select name="skill" multiple>
                <option value="html">HTML</option>
                <option value="css">CSS</option>
                <option value="javascript" selected>JavaScript</option>
            </select>
        </div>
        <div>
            <input type="submit">
        </div>
    </form>
</html>
```

效果：单选下拉列表

<div align="left">

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

</div>

效果：多选框

<div align="left">

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

</div>

效果：payload

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

### \<textarea> 文本区域

* 支持多行输入显示
* 自动换行
* 可以调整区域大小
* 可以设置默认宽高

代码：





样式：



## HTML特殊符号

* 空格，`&nbsp;`，不会空白折叠，一个`&nbsp;`占一个英文宽度，两个英文宽度占一个中文宽度
* 版权，`&copy;`，copyright
* 标准文档：[https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references](https://html.spec.whatwg.org/multipage/named-characters.html#named-character-references)
* 简化文档：[https://tool.chinaz.com/tools/htmlchar.aspx](https://tool.chinaz.com/tools/htmlchar.aspx)

































