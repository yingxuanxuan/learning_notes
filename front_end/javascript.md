# JavaScript

## 快速入门

### 基本语法

\### 内嵌JS

&#x20;   alert('Hello, world');

\### 引用JS \<script src="/static/js/abc.js">

\### script 类型省略 \<script type="text/javascript">

&#x20;\# script类型只有一种，可以省略

\### 本地JS安全限制 浏览器的安全限制，以file://开头的地址无法执行如联网等JavaScript代码，最终，你还是需要架设一个Web服务器，然后以http://开头的地址来正常执行所有JavaScript代码。

\### 调试 chrome console.log("")

\### 语句 语句以分号;结束，并不强制要求 如：

``` js
var x = 1;
'Hello, world';
var x = 1; var y = 2;
```

\### 语句块 使用大括号{} 语句块可以嵌套 如：

``` js
if (2>1) {
    x = 1;
    y = 2;
}
```

\### 注释 // # 行注释 /\*\*/ # 块注释

\### 大小写 严格区分大小写

### 数据类型和变量

\### Number Javascript不区分整数和浮点数，统一用Number表示

Number示例：

``` js
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

Number运算：

``` js
1 + 2; // 3
(1 + 2) * 5 / 2; // 7.5
2 / 0; // Infinity
0 / 0; // NaN
10 % 3; // 1
10.5 % 3; // 1.5
```

数据类型转换方法： 1、parseInt(), parseFloat(), 2、Boolean(), Number(), String(),  3、str - 0, str \* 1

八进制： 前缀为0

十六进制： 前缀为0x

\### 字符串 字符串使用单引号''或双引号""括起来的文本。ES6新增烦点\`\`，可以解释变量。

\### 布尔值 布尔值只有true和false

布尔值运算：

``` js
true;
false;
2 > 1; // true
2 >= 3; // false
```

与 或 非：

``` js
true && true;
true && false;
false && true && false;
false || false;
true || false;
false || true || false;
!true;
!false;
! (2>5);
```

\### 比较运算 Number之间比较得到一个布尔值：

``` js
2 > 5; //false
5 >= 2; // true
7 == 7; // true
```

JavaScript允许任意数据类型做比较：

``` js
false == 0; // true
false === 0; // false
```

第一种是==比较，它会自动转换数据类型再比较，很多时候，会得到非常诡异的结果； 第二种是===比较，它不会自动转换数据类型，如果数据类型不一致，返回false，如果一致，再比较。 由于JavaScript这个设计缺陷，不要使用==比较，始终坚持使用===比较。

另一个例外是NaN这个特殊的Number与所有其他值都不相等，包括它自己：

``` js
NaN === NaN; // false
```

唯一能判断NaN的方法是通过isNaN()函数：

``` js
isNaN(NaN); // true
```

浮点数比较相等有误差：

``` js
1 / 3 === (1 - 2 / 3); // false
```

&#x20;浮点数比较：

``` js
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; //true
```

\### null 和 undefined null表示一个“空”的值，它和0以及空字符串''不同，0是一个数值，''表示长度为0的字符串，而null表示“空”。 大多数情况下，我们都应该用null。undefined仅仅在判断函数参数是否传递的情况下有用。

\### 数组 JavaScript的数组可以包括任意数据类型：

``` js
[1, 2, 3.14, 'Hello', null, true];
```

数组用中括号\[]表示，元素之间用逗号,分隔，另一种创建数组的方法是通过Array()函数实现:

``` js
new Array(1, 2, 3)
```

数据使用下标来访问，起始值为0：

``` js
var arr = [1, 2, 3.14, 'Hello', null, true];
arr[0];
arr[5];
arr[6];
```

\### 对象 对象是一组键-值组成的无序集合：

``` js
var person = {
    name: 'Bob',
    age: 20, 
    tags: ['js', 'web', 'mobile'],
    city: 'Beijing',
    hasCar: true,
    zipcode: null
}
```

JavaScript对象的键都是字符串类型，值可以是任意数据类型。

获取对象属性，对像变量.属性名：

``` js
person.name; // 'Bob'
person.zipcode; // null
```

\### 变量 大小写英文、数字、$和\_的组合，且不能用数字开头。 变量名不能包含Javascript关键字，如if、while等。 声明一个变量：

``` js
var a; // undefined
var $b = 1; // 一般只有jquery包装的变量使用$符号开头
var s_007 = '007'; // 可以使用下划线
var Answer = true;
var t = null; // null
```

一个变量只能用var声明一次，但是可以多次赋值，赋值类型可以不同。

\### strict模式 如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量

``` js
i = 10; // i为全局变量
```

使用var声明的变量作用域在函数体内。

使用'use strict';强制使用strict模式，用var定义变量，没有用var定义则不是变量，抛出ReferenceError错误。 不支持的浏览器仅当作一个字符串语句。

### 字符串

字符串使用单引号''或双引号""括起来表示。

\### 转译 字符串包含双引号则使用单引号，字符串包含单引号则使用双引号。 \ # 转译符号 \n # 换行 \t # 制表符 \ # 单斜杠 '\x41' # 转译十六进制转译ASCII '\u4e2d\u6587' # 转译Unicode字符

\### 多行字符串（ES6新增）

``` js
`str
ing` # 使用反点多行
```

\### 模板字符串

``` js
var name='name';
var age = 20;
`${name},${age}`;

// 相当于
name + ',' + age;
```

\### 操作字符串 String.length # 长度 String\[index] # 获取 \* 字符串不可变，可以对索引项赋值，但不会改变也不会出现错误。

String.toUpperCase(); # 转换成大写 String.toLowerCase(); # 转换成小写 String.indexOf('substr'); # 搜索子串返回子串开始index，找不到返回-1 String.substring(startindex); # 使用索引获取子串，从startindex到结尾 String.substring(startindex, endindex); # 使用索引获取子串

### 数组

\### 数组长度 Array.length; # 获取长度 对长度赋值会改变数组长度，不建议修改 长度超过原来的部分值是undefined 长度低于原来的部分会被截断

\### 赋值 Array\[index] = newValue;

\### indexOf # 通过值查找Index Array.indexOf(Object);

``` js
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 0
arr.indexOf(20); // 1
arr.indexOf('30'); // 2
arr.indexOf(30); // -1
```

\### slice # 相当于String.substring() Array.slice(startIndex, endIndex); # 截取startIndex到endIndex Array.slice(startIndex); # 截取startIndex到末尾 Array.slice(); # 复制数组

复制的数组不相等：

``` js
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy == arr; // false
```

\### push 和 pop Array.push(newItem); // 添加到末尾，返回Array新长度 Array.push(newItem1, newItem2); // 添加到末尾 Array.pop(); // 删除末尾，返回Item值，pop空Array返回undefined

\### unshift 和 shift Array.unshift(newItem); // 添加到头部，返回Array新长度 Array.unshift(newItem1, newItem2); // 添加到头部 Array.shift(); // 删除头部（全部左移），返回Item值，shift空Array返回undefined

\### sort 排序 Array.sort();

``` js
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

\### reverse 反转 Array.reverse(); // 反转

``` js
var arr = ['one', 'two', 'three'];
arr.reverse();
arr; // ['three', 'two', 'one']
```

\### splice 删除拼接，万能方法 Array.splice(from, count, add1, add2, addn); # 从from删除count添加add，返回删除的元素

``` js
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];

// 从索引2删除3个元素，再添加两个元素
arr.splice(2, 3, 'Google', 'Facebook'); // 返回['Yahoo', 'AOL', 'Excite']

// 只删除，不添加
arr.splice(2, 2); // 返回['Google', 'Facebook']

// 只添加，不删除
arr.aplice(2, 0, 'Google', 'Facebook'); // 返回[]，没有删除任何元素
```

\### concat 拼接 Array.concat(Array);

``` js
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]); # 返回新数组
added; // ['A', 'B', 'C', 1, 2, 3]

var added2 = arr.concat(1, 2, [3, 4]); # 会自动拆开数组
added2; // ['A', 'B', 'C', 1, 2, 3, 4);
```

\### join 连接字符串 Array.join('devide by');

``` js
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
arr.join(); // 'A,B,C,1,2,3'，默认使用逗号分割
```

### 对象

\### 声明对象 对象是一种无序的集合，由若干键值对组成。 使用逗号,割开，最后一个键值对后不需要加，有些浏览器会报错。

``` js
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};

// 属性名中含有特殊字符，需要用引号
var xiaohong = {
    name: '小红',
    'middle-school': 'No.1 Middle School'
}
```

\### 访问属性 xiaoming.name;  # 使用点.访问属性 xiaoming.birth; xiaohong\['middle-school']; # 属性名称包含特殊字符，只能使用属性名称访问属性 xiaohong.age; # 访问不存在的属性，返回undefined

\### 添加属性 xiaohong.age = 10; # 直接赋值添加属性

\### 删除属性 delete Object.property; delete Object\['property'];

\### 检测是否包含属性 'property' in Object; # 该属性也可能是继承的，返回true或false 'toString' in Object;

Object.hasOwnProperty('property'); # 不包含继承的属性

### 条件

if() {} else if {} else{} # else可选

\### 非布尔值判断条件 null, undefined, 0, NaN, '', false # 这些值视为false，其他所有值都视为true

### 循环

\### for for(;;) # 无限循环

``` js # 下标循环
var arr = ['Apple', 'Goole', 'Microsoft'];
var i, x;
for (i=0; i < arr.length; i++) {
    x = arr[i];
    alert(x);
}
```

\### for in for (var x in Object) # 遍历key for (var i in Array) # i是index string，不建议这样做，为Array添加属性后会被遍历

``` js # 过滤掉继承的属性
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        alert(key);
    }
}
```

\### for of # ES6新增 for (var x of Object) # 遍历interable for (var x of Array) # 遍历数组，x是item

\### while while(){}

\### do while do{}while() interable.forEach(function(element, index, array){});

### Map 和 Set

\### 大括号{}的问题 {}可以视为其他语言中的Map或Dictionary。 但是大括号{}的key必须是字符串，实际上也可以是Number或其他数据类型。

\### Map # ES6新增 var m = new Map(\[\[key, value], \[key, value], \[key, value]]); # 创建 m.get(key); # 获取 m.set(key, value); # 设置，不存在则添加 m.has(key); # 判断 m.delete(key); # 删除

\### Set # ES6新增 var s = new Set(\[1,2,3]); s.add(key); # 添加 s.delete(key); # 删除

### iterable # ES6

\### for of 存在背景：Map和Set无法使用下标循环，Array可以使用下标循环，为统一集合类型，ES6新增iterable类型，使用for of遍历。

\### iterable.forEach() # ES5.1 默认传入三个参数，但是不要求参数个数一致

``` js
// 两个相同参数，一个Set本身
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) { });

// Value, Key, Map本身
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) { });

// Element, Index, Array本身
var a = ['A', 'B', 'C'];
a.forEach(function(element, index, array) { });
a.forEach(function(element) { });
```

## 函数

### 函数定义和调用

\### 定义函数

```
// 命名函数
function abs(x) {}

// 匿名函数
var abs = function (x) { };
```

\### 返回值 如果没有return语句，则返回undefined

\### 调用函数 abs(10); abs(10, 'xxx', 'dddd'); # 参数过多可以正常调用 abs(); # 参数过少返回NaN

\### arguments关键字 类似Array但不是Array：

``` js
function foo(x) {
    alert(x); // 10
    for (var i = 0; i < arguments.length; i++) {
        alert(argument[i]); // 10, 20, 30
    }
}
foo(10, 20, 30);
```

参数交换： // foo(a, \[b,] c); b为可选参数 function foo(a, b, c) {     if (arguments.length === 2) {         c = b;         b = null;     } }

\### rest参数（ES6新增）

``` js
// 不支持rest时实现
function foo(a, b) {
    var i, rest = [];
    if (arguments.length > 2) {
        for (i = 2; i < arguments.length; i++) {
            rest.push(arguments[i]);
        }
    }
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

// 支持rest参数
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
foo(1)
```

\### 参数类型判断 if (typeof i !== 'number') {     throw 'Not a number'; }

\### return自动加分号

``` js
// 错误范例
function foo() {
    return
        {name: 'foo'};
}

// 自动加分号结果，造成返回undefined
function foo() {
    return;
        {name: 'foo'};
}
```

### 变量作用域

\### 变量作用域 变量只在函数内有效。 内部函数可以使用外部函数定义的变量，反过来不行。

\### 变量优先级 内部函数同名变量优先查找内部变量。

\### 变量定义提升 变量定义会在函数内提升到最前，但不会提升变量赋值。 （习惯）在函数内首先使用一个var声明所有变量。

``` js
// 后定义先使用不会报错，输出'Hello undefined'。
function foo() {
    var x = 'Hello, ' + y;
    alert(x);
    var y = 'Bob';
}

// 等价于
function foo() {
    var y; // 自动提升变量声明，值为undefined
    var x = 'Hello, ' + y;
    alert(x);
    y = 'Bob';
}
```

\### 全局作用域 var xxx; # 不在任何函数内定义的变量就具有全局作用域。 window.var; # 等价于绑定到全局对像window window.foo(); # 顶层函数的定义也是全局变量，同样被绑定到window window.alert(); # 内置函数同样被绑定到window

\### 全局变量定义，避免冲突（习惯） var MYAPP = {}; MYAPP.var = xxx; //定义变量 MYAPP.foo = function (){} //定义全局函数

\### let 块级作用域（ES6） for (var i of arr) //作用域函数内 for (let i of arr) //作用域循环内

\### const 定义常量（ES6） var PI = 3.14; # 不支持const特性时使用大写变量名表示常量 const PI = 3.14; 对常量赋值无效果，有些浏览器不会报错

### 方法

\### 对像方法 对像方法可以使用对象属性

``` js
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function() {
        var y = new Date().getFullYear();
        return y = this.birth;
    }
}
```

\### this this指向当前对象 'use strict'; 模式下会触发错误

``` js
// 错误
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};

xiaoming.age(); // this指向xiaoming，返回正常结果
getAge(); // this指向window，返回NaN

var fn = xiaoming.age;
fn(); // this也指向window，返回NaN
```

``` js
// 嵌套错误，嵌套函数内this指向window
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function() {
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - this.birth;
        }
        return getAgeFromBirth();
    }
```

``` js
// 正确示范，提前固化this
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function() {
        var that = this;
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth;
        }
        return getAgeFromBirth();
    }
};
```

\### apply 和 call apply和call可以指定this指向 apply和call第一个参数为this指向，其他参数为实际参数 foo.apply(yourthis, Array); foo.call(yourthis, arg1, arg2, ... );

调用对象示例：

``` js
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
}

xiaoming.age() // 正确
getAge.apply(xiaoming, []); // 正确
getAge.call(xiaoming); // 正确
```

调用普通函数示例：

``` js
Math.max.apply(null, [3, 5, 4]);
Math.max.call(null, 3, 5, 4);
```

\### 装饰器 统计调用次数示例：

``` js
var count = 0;
var oldParseInt = parseInt;

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments);
};

parseInt('10');
parseInt('20');
parseInt('30');
count;
```

### 高阶函数

高阶函数就是让函数的参数能接收别的函数

\### map map是Array的方法

示例1：

``` js
function pow(x) {
    return x * x;
}
var arr = [1, 2, 3, 4, 5, 6, 7, 8 ,9];
arr.map(pow)
```

转化成字符串示例：

``` js
var arr= [1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.map(String);
```

\### reduce reduce是Array的方法 \[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)

累加示例：

``` js
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
});
```

\### filter filter是Array的方法，根据返回值是true还是false决定保留还是丢弃该元素。 filter函数接受三个参数。

删除偶数示例：

``` js
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
```

删除空字符串：

``` js
var arr = ['A', '', 'B', null, undefined, 'C', '   '];
var r = arr.filter(function (x) {
    return s && s.trim();
});
```

三参数示例，去除重复元素：

``` js
var r;
var arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
r = arr.filter(function (element, index, self) {
    return self.indexOf(element) === index;
});
```

\### sort sort()是Array的方法

sort默认会把Array所有元素转换为String再按ASCII码排序

``` js
// 小写字幕在大写字幕后面
['Google', 'apple', 'Microsoft'].sort(); // ['Google', 'Microsoft', 'apple']

// 先转换为字符串再排序
[10, 20, 1, 2].sort() // [1, 10, 2, 20]
```

sort入参函数根据返回值判断大小，返回值为-1，1，0

``` js
var arr = [10, 20, 1, 2];
arr.sort(function(x, y){
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
```

### 闭包

\### 实现基础：函数可以作为返回值

``` js
// 普通调用
function sum(arr) {
    return arr.reduce(function (x, y) {
        return x + y;
    });
}
sum([1, 2, 3, 4, 5)];

// 延迟调用
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}
var f = lazy_sum([1, 2, 3, 4, 5]);
f();
```

\### 用途：封装私有变量

``` js
function create_counter(initial) {
    var x = initial || 0;
    return {
        inc: function() {
            x += 1;
            return x;
         }
    }
}

var c1 = create_counter();
c1.inc(); // 1
c1.inc(); // 2
c1.inc(); // 3

var c2 = create_counter(10);
c2.inc(); // 11
c2.inc(); // 12
c2.inc(); // 13
```

\### 用途：减少参数个数

``` js
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}

var pow2 = make_pow(2);
var pow3 = make_pow(3);
pow2(5);
pow3(6);
```

\### 循环变量错误

``` js
function count() {
    var arr = [];
    for (var i = 1; i <= 3; i++) {
        arr.push(function () {
            return i * i;
        });
    }
    return arr;
}

var results = count(); // 调用时循环已经完成，i = 4
var f1 = results[0]; // 16，预期1
var f2 = results[1]; // 16，预期4
var f3 = results[2]; // 16，预期9
```

解决方法：

``` js
function count() {
    var arr = [];
    for (var i = 1; i <= 3; i++) {
        arr.push((function (n) {
            return function() {
                return n * n;
            };
        })(i));
    }
}

使用(function(x){return x*x;})(i);函数参数固定循环变量
```

### 箭头函数 # ES6新增

#### 定义

箭头函数相当于匿名函数，简化了函数定义。

```js
x => x * x
// 相当于：
function (x) {
    return x * x;
}
```

包含多条语句和return时，不能省略大括号{}

```js
x => {
    if (x > 0) {
        return x * x;
    } else {
        return - x * x;
    }
}
```

两个参数：

```js
(x, y) => x * x + y * y
```

无参数：

```js
() => 3.14
```

可变参数：

```js
(x, y, ...rest) => {
    var i, sum = x + y;
    for (i=0; i<rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}
```

返回对象错误：

```js
x => {foo:x} # SyntaxError，语法冲突
x => ({foo:x}) # 正确写法
```

#### this作用域区别

总是指向词法作用域，即外层调用obj call和apply绑定this无效

### 生成器 generator # ES6

#### 定义

使用function\*定义 使用return返回 使用yield多次返回

```js
function* foo(x) {
    yield x + 1;
    yield x + 2;
    return x + 3;
}
```

#### 使用

var f = foo(6); # 初始调用 f.next(); # 多次调用，返回 {value: 7, done: false}，使用done判断是否调用结束 for ( var x of foo(6)) {} # for of调用，不需要自行判断done结束

## 内置类

### 标准对像

#### typeof获取对象类型

```js
typeof 123; // 'number'
typeof NaN; // 'number'
typeof 'str'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof Math.able; // 'function'
typeof null; // 'object'
typeof []; // 'object'
typeof {}; // 'object'
```

#### 包装对象

包装对象使用new创建，不建议

```js
var n = new Number(123);
var b = new Boolean(true);
var s = new String('str');
```

不使用new时当作普通函数

```js
var n = Number('123');
typeof n; // 'number'

var b = Boolean('true'); // true
var c = Boolean('false'); // true，字符串非空转换为true
var d = Boolean(''); // false
typeof b; // 'boolean'

var s = String(123.45);
typeof s; // 'string'
```

#### 约定

不要使用new Number()，new Boolean()，new String()创建包装对象 用parseInt()或parseFloat()转换任意number 使用String()或调用Object.toString()转换为字符串 typeof可以判断出number、boolean、string、function、undefined 判断Array使用Array.isArray(arr)，typeof array返回'object' 判断null使用 var === null 判断全局变量是否存在使用typeof window.var === 'undefined' 判断函数内部某个变量是否存在使用typeof var === 'undefined' null和undefined没有toString()函数，虽然null是Object

#### number对象toString

```js
123.toString(); // SyntaxError
123..toString(); // '123'
(123).toString(); // '123'
```

### Date

### RegExp

### JSON

## 面向对象

## 浏览器

## 其他

\### 页面加载时执行的js 将页面JS放入window.onlo ad = function(){ }中执行。

\### 页面加载时更新内容 //设置按钮显示 function setButton(){     if(localStorage.rocketswitch == "true")     {         document.getElementById("open").innerHTML="停止抢鱼丸";     } } window.onload = function(){      setButton(); }

\### 获取域名 host = window.location.host; host2=document.domain;&#x20;

\### 获取页面完整地址 url = window.location.href;

\### 空 \[]表示空数组 {}表示空对象  var \_hmt = \_hmt || \[]; 若\_hmt为null(未定义)，则等于空数组

\### 动态加载

var \_hmt = \_hmt || \[]; (function() {   var hm = document.createElement("script");   hm.src = "//hm.baidu.com/hm.js?e99aee90ec1b2106afe7ec3b199020a7";   var s = document.getElementsByTagName("script")\[0];    s.parentNode.insertBefore(hm, s); })();

1\. 宣告并初始化全域变量，一般在动态载入的源码中使用。 2. 节点操作所生的  的优先度比静态资源低，不会阻碍网页载入。

\### JS HTML DOM #通过id查找HTML元素 var x=document.getElementById("intro"); #通过标签名查找HTML元素 var y=x.getElementsByTagName("p"); #通过类名找到HTML元素 var x=document.getElementsByClassName("intro"); #改变HTML内容 document.getElementById(id).innerHTML=new HTML #改变元素属性 document.getElementById(id).attribute=new value #改变CSS document.getElementById(id).style.property=new style #改变事件处理 document.getElementById("myBtn").onclick=function(){displayDate()}; #添加事件监听器，可添加多个 element.addEventListener(event, function, useCapture); useCapture=false;在 冒泡 中，内部元素的事件会先被触发，然后再触发外部元素，即：&#x20;

&#x20;元素的点击事件先触发，然后会触发&#x20;

&#x20;元素的点击事件。 useCapture=true;在 捕获 中，外部元素的事件会先被触发，然后才会触发内部元素的事件，即：  元素的点击事件先触发 ，然后再触发&#x20;

&#x20;元素的 #删除事件监听器 element.removeEventListener("mousemove", myFunction); #增加DOM节点（元素） var para=document.createElement("p"); var node=document.createTextNode("这是一个新段落。"); para.appendChild(node);

var element=document.getElementById("div1"); element.appendChild(para); #删除DOM节点（元素） var parent=document.getElementById("div1"); var child=document.getElementById("p1"); parent.removeChild(child);

\### JS HTML 常用DOM事件 #onload 和 onunload 事件 onload 和 onunload 事件会在用户进入或离开页面时被触发。 onload 事件可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。 onload 和 onunload 事件可用于处理 cookie。 #onchange 事件 onchange 事件常结合对输入字段的验证来使用。 #onmouseover 和 onmouseout 事件 onmouseover 和 onmouseout 事件可用于在用户的鼠标移至 HTML 元素上方或移出元素时触发函数。 #onmousedown、onmouseup 以及 onclick 事件 onmousedown, onmouseup 以及 onclick 构成了鼠标点击事件的所有部分。首先当点击鼠标按钮时，会触发 onmousedown 事件，当释放鼠标按钮时，会触发 onmouseup 事件，最后，当完成鼠标点击时，会触发 onclick 事件。

\### JS BOM 浏览器对象模型 Window Location window.location 对象在编写时可不使用 window 这个前缀。     - location.hostname 返回 web 主机的域名     - location.pathname 返回当前页面的路径和文件名     - location.port 返回 web 主机的端口 （80 或 443）     - location.protocol 返回所使用的 web 协议（http:// 或 https://） Window Location Href location.href 属性返回当前页面的 URL。 Window Location Pathname location.pathname 属性返回 URL 的路径名。 Window Location Assign location.assign() 方法加载新的文档。
