# icecream



## 介绍

* 自动打印表达式、变量名称
* 自动梅花数据结构
* 自动语法高亮
* 自动打印当前文件名、行号、父函数



## 快速入门



### 打印变量和表达式

* 使用`ic()`打印

```py
from icecream import ic

# 数值

ic(1)

'''
ic| 1
'''


# 变量

a = 1
ic(a)

'''
ic| a: 1
'''


# 表达式
a = 1
b = 1
ic(a+b)

'''
ic| a+b: 2
'''


# 数组字典表达式

arr = {1: ['j', 'k', 'l']}
ic(arr[1][1])

'''
ic| arr[1][1]: 'k'
'''


# 类属性对象属性

class C:
    V = 1
    
    def __init__(self) -> None:
        self.a = 1

ic(C.V)

'''
ic| C.V: 1
'''

ic(C().a)

'''
ic| C().a: 1
'''


# 函数调用

a = 1
def func(arg):
    return arg * 2
ic(func(a))
'''
ic| func(a): 2
'''


# 打印字典

j = {
    "name": "Alice",
    "age": 25,
    "address": {
        "city": "Beijing",
        "street": "Chaoyang Road"
    },
    "hobbies": [
        {
            "name": "reading",
            "books": ["Harry Potter", "The Lord of the Rings"]
        },
        {
            "name": "playing games",
            "games": ["Tetris", "Super Mario"]
        }
    ]
}
ic(j)

'''
ic| j: {'address': {'city': 'Beijing', 'street': 'Chaoyang Road'},
        'age': 25,
        'hobbies': [{'books': ['Harry Potter', 'The Lord of the Rings'],
                     'name': 'reading'},
                    {'games': ['Tetris', 'Super Mario'], 'name': 'playing games'}],
        'name': 'Alice'}
'''

```



### 定位执行过程

* 直接使用`ic()`

```py
from icecream import ic


def func_a(x) -> int:
    ic()
    return x // 2


def func_b(x) -> int:
    ic()
    return func_a(x) // 2


def func_c(x) -> int:
    ic()
    return func_b(x) // 2


func_c(8)


'''
ic| test.py:15 in func_c() at 14:24:49.805
ic| test.py:10 in func_b() at 14:24:49.813
ic| test.py:5 in func_a() at 14:24:49.818
'''

```



### 追溯执行过程

* 使用`ic()`的返回值

```py
from icecream import ic


def func_a(x) -> int:
    return ic(x // 2)


def func_b(x) -> int:
    return ic(func_a(x) // 2)


def func_c(x) -> int:
    return ic(func_b(x) // 2)


ic(func_c(8))


'''
ic| x // 2: 4        
ic| func_a(x) // 2: 2
ic| func_b(x) // 2: 1
ic| func_c(8): 1
'''

```



### 获取字符串

* 使用`ic.format(*args)`获取字符串，而不是写入stderr

```py
from icecream import ic

s = 'sup'
out = ic.format(s)
print(out)

'''
ic| s: 'sup'
'''
```



### 控制打印开关

* `disable()`
* `enable()`

```py
from icecream import ic

ic(1)

ic.disable()
ic(2)

ic.enable()
ic(3)

'''
ic| 1: 1
ic| 3: 3
'''
```



### 导入成内置函数

* `install()`，将`ic()`添加到内置模块，该模块在解释器导入的所有文件之间共享
* `uninstall()`

```py
from icecream import install
install()
```



