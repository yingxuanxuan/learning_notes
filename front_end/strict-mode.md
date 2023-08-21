# Strict Mode

## 设计目的

* JavaScript有很多设计不合理的地方，但是为了兼容老的代码，所以无法删除
* 严格模式从ES5引入标准
* 严格模式可以解决以下问题
  * 明确禁止一些不合理、不严谨的语法，减少JavaScript语言的一些怪异行为
  * 增加更多报错的场合，消除代码运行的一些不安全之处，保证代码运行的安全
  * 提高编译效率，增加运行速度
  * 为未来新版本的JavaScript语法做好铺垫

## 启用方法

```js
// 不兼容严格模式的旧版浏览器只会当做一行普通字符串处理
'use strict';
```

### 整个脚本严格模式

```html
<script>
    // 必须写在最前面
    'use strict';
    // 严格模式
</script>

<script>
    // 非严格模式
</script>
```

### 单个函数严格模式

```js
function strict() {
  'use strict';
  return '这是严格模式';
}

function strict2() {
  'use strict';
  function f() {
    return '这也是严格模式';
  }
  return f();
}

function notStrict() {
  return '这是正常模式';
}
```

## 显式报错

* 很多在普通模式下默默失败不会报错的错误，在严格模式下会显式报错

### 只读属性不可写

```js
// 设置不可变类型的属性
'use strict';
'abc'.length = 5;
// TypeError: Cannot assign to read only property 'length' of string 'abc'

// 对只读属性赋值会报错
'use strict';
var obj = Object.defineProperty({}, 'a', {
  value: 37,
  writable: false
});
obj.a = 123;
// TypeError: Cannot assign to read only property 'a' of object #<Object>

// 删除不可配置的属性会报错
'use strict';
var obj = Object.defineProperty({}, 'p', {
  value: 1,
  configurable: false
});
delete obj.p;
// TypeError: Cannot delete property 'p' of #<Object>
```

### 只设置了取值器的属性不可写

```js
// 对只有getter、没有setter的属性赋值报错
'use strict';
var obj = {
  get v() { return 1; }
};
obj.v = 2;
// Uncaught TypeError: Cannot set property v of #<Object> which has only a getter
```

### 禁止扩展的对象不可扩展

```js
// 对禁止扩展的对象添加型属性会报错
'use strict';
var obj = {};
Object.preventExtensions(obj);
obj.v = 1;
// Uncaught TypeError: Cannot add property v, object is not extensible
```

### eval、arguments不可用作标识名

```js
// 以下每一行都会报错
// SyntaxError: Unexpected eval or arguments in strict mode

'use strict';
var eval = 17;
var arguments = 17;
var obj = { set p(arguments) { } };
try { } catch (arguments) { }
function x(eval) { }
function arguments() { }
var y = function eval() { };
var f = new Function('arguments', "'use strict'; return 17;");
```

### 函数不能有重名的参数

* 正常模式下，如果函数有多个重名的参数，可以用`arguments[i]`读取。严格模式下，这属于语法错误。

```js
function f(a, a, b) {
  'use strict';
  return a + b;
}
// Uncaught SyntaxError: Duplicate parameter name not allowed in this context
```

### 禁止八进制的前缀0表示法

```js
// 正确的八进制前缀是 0o

'use strict';
var n = 0100;
// Uncaught SyntaxError: Octal literals are not allowed in strict mode.
```

## 增强安全措施

### 全局变量显式声明

* 非严格模式中，如果一个变量没有声明就赋值，默认是全局变量，不报错，可能污染全局命名空间

```js
'use strict';

v = 1; // 报错，v未声明

for (i = 0; i < 2; i++) { // 报错，i 未声明
  // ...
}

function f() {
  x = 123;
}
f() // 报错，未声明就创建一个全局变量
```

### 禁止this关键字指向全局对像

* 未绑定到上下文的函数，this指向undefined

```js
// 正常模式
function f() {
  console.log(this === window);
}
f() // true

// 严格模式
function f() {
  'use strict';
  console.log(this === undefined);
}
f() // true
```

* 严格模式构造函数未加new时，会因为this指向undefined报错，非严格模式下会正常执行

```js
function f() {
  'use strict';
  this.a = 1;
};

f();// 报错，this 未定义
```

* 非严格模式可以将null和undefined绑定到this上，严格模式下会报错

```js
// 正常模式
function fun() {
  return this;
}

fun() // window
fun.call(2) // Number {2}
fun.call(true) // Boolean {true}
fun.call(null) // window
fun.call(undefined) // window

// 严格模式
'use strict';
function fun() {
  return this;
}

fun() //undefined
fun.call(2) // 2
fun.call(true) // true
fun.call(null) // null
fun.call(undefined) // undefined
```

### 禁止使用fn.callee、fn.caller

* 严格模式禁止在函数内部访问调用栈

```js
function f1() {
  'use strict';
  f1.caller;    // 报错
  f1.arguments; // 报错
}

f1();
```

### 禁止使用arguments.callee、arguments.caller

* `arguments.callee`和`arguments.caller`是没有标准化过，现在已经取消，非严格模式下调用它们没有作用
* 严格模式下报错

```js
'use strict';
var f = function () {
  return arguments.callee;
};

f(); // 报错
```

### 禁止删除变量

* 严格模式下无法删除变量
* 只有对象的属性，且属性的描述对象的`configurable`属性设置为`true`，才能被`delete`命令删除

```js
'use strict';
var x;
delete x; // 语法错误

var obj = Object.create(null, {
  x: {
    value: 1,
    configurable: true
  }
});
delete obj.x; // 删除成功
```

## 静态绑定

* JavaScript允许“动态绑定”，某些属性和方法到底属于哪一个对象，不是在编译时确定的，而是在运行时确定的。
* 严格模式对动态绑定做了一些限制，某些情况下，只允许静态绑定。也就是说，属性和方法到底归属哪个对象，必须在编译阶段就确定。这样做有利于编译效率的提高，也使得代码更容易阅读，更少出现意外。

### 禁止使用with语句

* 严格模式下，使用`with`语句将报错。因为`with`语句无法在编译时就确定，某个属性到底归属哪个对象，从而影响了编译效果。

```js
'use strict';
var v  = 1;
var obj = {};

with (obj) {
  v = 2;
}
// Uncaught SyntaxError: Strict mode code may not include a with statement
```

### 创设eval作用域

* 非严格模式下，JavaScript 语言有两种变量作用域，全局作用域和函数作用域。严格模式引入了第三种作用域，`eval`作用域。
* 非严格模式下，`eval`语句的作用域，取决于它处于全局作用域，还是函数作用域。
* 严格模式下，`eval`语句本身就是一个作用域，不再能够在其所运行的作用域创设新的变量了，即`eval`所生成的变量只能用于`eval`内部。

```js
(function () {
  'use strict';
  var x = 2;
  console.log(eval('var x = 5; x')) // 5
  console.log(x) // 2
})()
```

* eval使用严格模式有两种方式

```js
// 方式一
function f1(str){
  'use strict';
  return eval(str);
}
f1('undeclared_variable = 1'); // 报错

// 方式二
function f2(str){
  return eval(str);
}
f2('"use strict";undeclared_variable = 1')  // 报错
```

### arguments不再追踪参数变化

* 严格模式下，改变参数不会再反映到arguments数组中

```js
function f(a) {
  a = 2;
  return [a, arguments[0]];
}
f(1); // 正常模式为[2, 2]

function f(a) {
  'use strict';
  a = 2;
  return [a, arguments[0]];
}
f(1); // 严格模式为[2, 1]
```

## 下一个版本的JavaScript过度

* 为了平稳过渡到ES6，严格模式引入了一些 ES6 语法

### 非函数代码块不得声明函数

* ES5 的严格模式只允许在全局作用域或函数作用域声明函数。即，不允许在非函数的代码块内声明函数

```js
'use strict';
if (true) {
  function f1() { } // 语法错误
}

for (var i = 0; i < 5; i++) {
  function f2() { } // 语法错误
}
```

### 保留字

* 严格模式新增了一些保留字（implements、interface、let、package、private、protected、public、static、yield等）。使用这些词作为变量名将会报错。
