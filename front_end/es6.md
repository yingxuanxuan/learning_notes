# ES6

## ES6详细历史

https://es6.ruanyifeng.com/#docs/intro

## ES5不足

* 变量提升可能造成困惑
* 污染全局变量
* 内置对象的方法不灵活
* 模块化实现不完善

## Babel

* 是一种JavaScript编译器
* 可以将ES6转换成ES5，从而获得老版本浏览器支持

## let和const

* let不会变量提升
  * var声明变量，存在变量提升问题，使用在声明之前不会报错。
  * let声明变量，使用在声明之前会报错。

```js
// var变量提升，不会报错
console.log(a) // undefined
var a = 2

// let不会变量提升，会报错
console.log(b) // 报错
let b = 2
```

* let有块级作用域

```js
// 场景一，let在花括号代码块中有独立作用域
if (true) {
    let a = 10;
    var b = 10;
}

console.log(b);
console.log(a);


// 场景二，let在for循环中有块级作用域
var arr1 = [];
for (var x = 0; x < 10; x++) {
    arr1[x] = function() {
        // x的作用域在for外部，return x保存的是变量
        return x;
    }
}

for (var y = 0; y < 10; y++) {
    console.log(arr1[y]());  // 10, 10, ....
}

var arr2 = []
for (let i = 0; i < 10; i++) {
    arr2[i] = function() {
        // i的作用域仅在for内部，return i保存的是当前值，否则将会引发找不到变量的语法错误
        return i;
    }
}

for (var j = 0; j < 10; j++) {
    console.log(arr2[j]()); // 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
}


var arr3 = []
let m;
for (m = 0; m < 10; m++) {
    arr3[m] = function() {
        // m的作用域在for外部，return m保存的是变量
        return m;
    }
}

for (var n = 0; n < 10; n++) {
    console.log(arr3[n]()); //10, 10, ...
}

```

* let在for初始化变量的作用域在单独的一层

```js
for (let i = 0; i < 3; i++) {
    // 这里i在内部作用域，不报错
    let i = 'abc';
    console.log(i);
}
```

* let不能重复声明

```js
var a = 1;
var a = 2;
let b = 1;
let b = 2; // 报错，b已经被定义
```

* let不会污染全局变量

```js
// 浏览器环境下

console.log(RegExp); // RegExp
console.log(window.RegExp); // RegExp
var RegExp = 10; // var 定义污染了默认全局空间的变量定义而没有报错
console.log(RegExp); // 10
console.log(window.RegExp); // RegExp

console.log(RegExp);
console.log(window.RegExp);
let RegExp = 10; // let定义报错
```

* const在拥有let所有特性的前提下，初始化后无法被改变

```js
// 错误，const声明时必须先初始化
const a;
a = 1;

// 错误，对const变量赋值
const a = 1;
a = 2;
```

## 模板字符串

```js
let a = 1
console.log(`--${a}--`) // --1--
```

## 函数默认值和剩余参数

* ES5不支持函数默认值

```js
// ES5不支持函数默认值，需要自行处理
function add_es5(a, b) {
    a = a || 10;
    b = b || 20;
    return a + b;
}
console.log(add_es5());

// ES6支持函数默认值，未传参时自动使用默认值
function add_es6(a = 10, b = 20) {
    return a + b;
}
console.log(add_es6());
```

* ES6支持默认参数是表达式，包括函数

```js
function defaultVal() {
    return 1;
}

function add(a, b = defaultVal()) {
    return a + b;
}

console.log(add(3)); // 4
```

* ES6支持rest其余参数

```js
// 在函数内使用arguments参数
// 手动获取多余参数
function f(a, b) {
    var rest = [];
    if (arguments.length > 2) {
        for (var i = 2; i < arguments.length; i++) {
            rest.push(arguments[i]);
        }
    }
    console.log(a); // 1
    console.log(b); // 2
    console.log(rest); // 3, 4, 5
}
foo(1, 2, 3, 4, 5);

// 在函数内使用rest参数
// 自动获取多余参数
function foo(a, b, ...rest) {
    console.log(a); // 1
    console.log(b); // 2
    console.log(rest); // 3, 4, 5
}
foo(1, 2, 3, 4, 5);
```

* ES6支持扩展运算符

```js
// ES5不支持扩展运算符，Math.max不支持数组参数，使用apply方法调用Math.max
const arr = [1, 2, 3, 4, 5]
console.log(Math.max.apply(null, arr));

// 使用ES6 ... 语法将数组拆散传参
console.log(Math.max(...arr))
```

* 箭头函数

```js
// ES5匿名函数
let add_es5 = function (a, b) {
    return a + b;
};

// ES6箭头函数
let add_es6 = (a, b) => {
    return a + b;
};

// ES6箭头函数，只有一个参数时，可以省略括号
let add = val => {
    return val + 5;
};

// ES6箭头函数，只有一个语句作为返回值是，可以省略花括号
let add = val => (val + 5);
let add = (val1, val2) => (val1 + val2);
let getObj = id => ({
    id: id,
    name: "name"
});

// ES5闭包
let fn = (function() {
 	return function () {
        console.log('es5');
    }   
})();
fn();

// ES6闭包
let fn = (() => {
 	return () => {
        console.log('es6');
    }   
})();
fn();
```

* 箭头函数与匿名函数的this指向的区别
  * 箭头函数没有this绑定
  * es5中this指向取决于调用该函数的上下文对象

```js
// ES5错误示例
let PageHandler = {
    id: 123,
    init: function() {
        document.addEventListener('click', function (event) {
            // 这里this指向document
            this.doSomThings(event.type);
        }, false);
    },
    doSomeThings: function (type) {
        // 这里this指向PageHandler
        console.log(this.id);
    }
}

// ES5修改函数this指向
let PageHandler = {
    id: 123,
    init: function() {
        document.addEventListener('click', function (event) {
            // 这里this指向document
            this.doSomThings(event.type);
            
        // 修改绑定后this，指向PageHandler
        }.bind(this), false);
    },
    doSomeThings: function (type) {
        // 这里this指向PageHandler
        console.log(this.id);
    }
}

// ES6没有this指向，只能通过查找作用域链确定this指向
let PageHandler = {
    id: 123,
    init: function() {
        document.addEventListener('click', (event) => {
            // 这里this指向PageHandler
            this.doSomThings(event.type);
        }, false);
    },
    doSomeThings: function (type) {
        // 这里this指向PageHandler
        console.log(this.id);
    }
}

// ES6错误示例
let PageHandler = {
    id: 123,
    
    // 由于这里使用了箭头函数
    init: () => {
        document.addEventListener('click', (event) => {
            // 这里this只能向PageHandler外部查找，找不到则报错
            this.doSomThings(event.type);
        }, false);
    },
    doSomeThings: function (type) {
        // 这里this指向PageHandler
        console.log(this.id);
    }
}
```

* 箭头函数不存在arguments对象

```js
// 错误示例
let getVal = (a, b) => {
    console.log(arguments);
}
```

* 箭头函数不能使用new关键字实例化对象

```js
// 匿名函数
// 匿名函数是函数，是对象
let Person = function(){};
let p = new Person();

// 箭头函数，错误示例
// 箭头函数不是函数，不是对象
let Person = () => {};
let p = new Person();
```

## 解构赋值

* 对赋值运算符的一种扩展
* 一般针对数组和对象操作

```js
// ES5拆对象
let node = {
    type: 'type',
    name: 'name'
};
let type = node.type;
let name = node.name;

// ES6完全拆对象
// 注意不能重命名
let {type, name} = node;

// ES6部分拆对象
let {type} = node;

// ES6使用其余运算符拆对象
// ...res是其他属性的数组
let {type, ...res} = node;

// ES6拆对象，属性不存在时使用默认值
// other不存在，为默认值10
let {type, name, other = 10} = node;

// ES6拆数组
let arr = [1, 2, 3];
let [a, b] = arr;

// ES6嵌套拆数组
let [a, [b], c] = [1, [2], 3];
```

## 对象扩展

* ES6中简化了ES5定义对象的操作，可以直接写入变量和函数作为对象属性

```js
let name = 'name';
let age = 10;

// ES5
var p = {
    name = name,
    age = age,
    function print() {}
}

// ES6
let p = {
    name,
    age,
    print() {
        
    }
}

```



