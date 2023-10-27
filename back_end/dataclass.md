# dataclass



## 概念

* 什么是数据类
* 使用数据类有什么好处



## 数据类相关库

* dataclasses
  * python原生
* attrs
  * 原创
* cattrs
  * 



## dataclasses

* 功能
  * 自动编写`__init__`用于初始化
  * 自动编写`__repr__`用于打印、调试
  * 自动编写`__eq__`、`__ne__`用于做key
  * 自动编写`__hash__`用于做key
  * 自动编写`__lt__`、`__le__`、`__gt__`、`__ge__`等用于排序



* 设计文档
  * PEP 557



* 数据类型
  * types，PEP 526



### 入门示例

```py
from dataclasses import dataclass

@dataclass
class InventoryItem:
    """Class for keeping track of an item in inventory."""
    name: str
    unit_price: float
    quantity_on_hand: int = 0

    def total_cost(self) -> float:
        return self.unit_price * self.quantity_on_hand
```



* 自动生成如下初始化函数

```py
def __init__(self, name: str, unit_price: float, quantity_on_hand: int = 0):
    self.name = name
    self.unit_price = unit_price
    self.quantity_on_hand = quantity_on_hand
```





### 定义默认初始值

* 定义

```py
@dataclass
class C:
    a: int       # 'a' has no default value
    b: int = 0   # assign a default value for 'b'
```

* 自动生成如下初始化

```py
def __init__(self, a: int, b: int = 0):
```

* 注意
  * 默认值规则同函数参数默认值规则
  * 默认值后，必须都具有默认值
  * 即，默认值参数之后不能有无默认值的参数





### dataclass功能

* dataclass装饰器原型

```py
@dataclasses.dataclass(*, init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False, match_args=True, kw_only=False, slots=False, weakref_slot=False)
```



* 装饰器用法

```py
@dataclass
class C:
    ...

@dataclass()
class C:
    ...

@dataclass(init=True, repr=True, eq=True, order=False, unsafe_hash=False, frozen=False,
           match_args=True, kw_only=False, slots=False, weakref_slot=False)
class C:
    ...
```



#### 自动生成`__init__`

* `init=True`，默认生成
* 如果有自定义则不生成



#### 自动生成`__repr__`

* `repr=True`，默认生成
* 如果有自定义则不生成
* 生成规则
  * 类名(字段1=值1，字段2=值2)
  * `InventoryItem(name='widget', unit_price=3.0, quantity_on_hand=10)`
* 排除字段
  * 在字段定义的field参数中排除



#### 自动生成`__eq__`

* `eq=True`，默认生成
* 如果有自定义则不生成
* 比较规则
  * 字段值按定义顺序组成元组比较
* 约束
  * 只能比较相同类型



#### 自动生成比较方法

* `order=False`，默认不生成
* 约束
  * 如果有自定义`__lt__()`，`__le__()`，`__gt__()`，`__ge__()`，则报错
  * 如果`order=True`，而`eq=False`，则报错
* 生成规则
  * 自动生成`__lt__()`，`__le__()`，`__gt__()`，`__ge__()`
* 比较规则
  * 字段值按定义顺序组成元组比较



#### 自动生成`__hash__`

* `unsafe_hash=False`，默认不生成，不推荐生成
* 约束
  * hash需要做等值比较，必须实现`__eq__`，`eq=True`
  * hash必须为不可变的值，`frozen=True`



#### 初始化后不可变

* `frozen=False`，默认关闭，初始化后可变
* 约束
  * 如果有自定义`__setattr__`，`__delattr__`则报错，因为内置实现使用了这两个魔术方法



#### 自动生成`__match_args__`

* `match_args=True`，默认生成
* 如果有自定义则不生成
* 生成规则
  * 根据`__init__`形参列表生成



#### 关键字初始化

* `kw_only=False`，默认关闭，不使用关键字初始化字段



#### 自动生成`__slots__`

* `slots=False`，默认不生成`__slots__`
* 如果有自定义`__slots__`，则报错



#### 自动弱引用

* `weakref_slot=False`，默认不生成`__weakref__`
* 约束
  * 自动生成`__weakref__`时，必须`slots=True`





### field

* 功能
  * 用于对特定属性（字段）提供更细节的定义



* 必须使用的场景

```py
# 错误
# 错误在于所有类的实例会共享同一个list

@dataclass
class C:
    mylist: []

c = C()
c.mylist += [1, 2, 3]


# 正确
# 每个类的实例会创建一个list

@dataclass
class C:
    mylist: list[int] = field(default_factory=list)

c = C()
c.mylist += [1, 2, 3]
```





#### 属性默认值

* 属性默认没有默认值
* `default=MISSING`是因为`None`对于属性来说，可能是一个有效的默认值

```py

```





### 其他功能

