# dataclass



## 概念

* 什么是数据类
  * 可以通过简单的声明来定义一个类
  * 自动生成一些数据类常用的方法



* 使用数据类有什么好处
  * 简化代码
  * 提高可读性
  * 提高可维护性
  * 提高代码质量



* 性能
  * 初始化后，性能影响接近于0



## 数据类相关库

* dataclasses
  * python原生
* attrs
  * 原创
  * 功能更加丰富
* cattrs
  * python对象与JSON、dict互转



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



* 使用要点
  * 被改变时抛出`exception dataclasses.FrozenInstanceError`
  * `FrozenInstanceError`是`AttributeError`的子类



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





### field功能

* 功能
  * 用于对特定属性（字段）提供更细节的定义



* 类原型

```py
dataclasses.field(*, default=MISSING, default_factory=MISSING, init=True, repr=True, hash=None, compare=True, metadata=None, kw_only=MISSING)
```



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



* 类属性与对象属性

```py
@dataclass
class C:
    x: int
    y: int = field(repr=False)
    z: int = field(repr=False, default=10)
    t: int = 20

# C.x # 不存在
# C.y # 不存在
C.z = 10 # 保存默认值
C.t = 20 # 保存默认值
```



#### 属性默认值

* 默认没有默认值
* `default=MISSING`是因为`None`对于属性来说，可能是一个有效的默认值



#### 属性默认值工厂

* 默认没有默认值工厂
* `default_factory=MISSING`是因为`None`对于属性来说，可能是一个有效的默认值



#### 是否初始化

* 默认初始化字段，`init=True`



#### 是否包含在repr

* 默认repr中包含字段，`repr=True`



#### 是否包含在hash

* 默认为`None`，`hash=None`，含义为使用`dataclasses.field.compare`的值
* 不建议修改



#### 是否参与比较

* 默认用于比较，`compare=True`
* 将被用于生成，`__eq__`和`__gt__`等



#### metadata

* 默认是None，`metadata=None`
* 用于第三方扩展



#### 是否关键字初始化

* 默认不适用关键字初始化，`kw_only=MISSING`



### 获取数据类的描述

* 方法原型

```py
dataclasses.fields(class_or_instance)
```



### 将数据类转换为一个字典

* 方法原型

```py
dataclasses.asdict(obj, *, dict_factory=dict)
```



* 使用要点
  * 默认使用深拷贝复制内部数据
  * 入参obj必须是一个数据类



* 示例

```py
@dataclass
class Point:
     x: int
     y: int

@dataclass
class C:
     mylist: list[Point]

p = Point(10, 20)
assert asdict(p) == {'x': 10, 'y': 20}

c = C([Point(0, 0), Point(10, 4)])
assert asdict(c) == {'mylist': [{'x': 0, 'y': 0}, {'x': 10, 'y': 4}]}
```



### 将数据类转换为一个元组

* 方法原型

```py
dataclasses.astuple(obj, *, tuple_factory=tuple)
```



* 示例

```py
@dataclass
class Point:
     x: int
     y: int

@dataclass
class C:
     mylist: list[Point]

assert astuple(p) == (10, 20)
assert astuple(c) == ([(0, 0), (10, 4)],)
```



### 判断是否是数据类

* 原型

```py
dataclasses.is_dataclass(obj)
```



* 使用要点
  * 这个函数会同时判断数据类和数据类对象



* 示例

```py
def is_dataclass_instance(obj):
    return is_dataclass(obj) and not isinstance(obj, type)
```



### 关键字参数分隔符

* 原型

```py
dataclasses.KW_ONLY
```



* `_: KW_ONLY`，之前为位置参数，之后为关键字参数

```py
@dataclass
class Point:
    x: float
    _: KW_ONLY
    y: float
    z: float

p = Point(0, y=1.5, z=2.0)
```



### 扩展初始化操作

* 使用要点
  * 在生成的`__init__`之后调用自定义的`__post_init__`



## attrs



### 安装

```sh
pip install attrs
```



### 入门示例

```py
from attrs import asdict, define, make_class, Factory

@define
class SomeClass:
    a_number: int = 42
    list_of_numbers: list[int] = Factory(list)

    def hard_math(self, another_number):
        return self.a_number + sum(self.list_of_numbers) * another_number


# 自动生成 __init__
sc = SomeClass(1, [1, 2, 3])


# 自动生成 __repr__
sc # SomeClass(a_number=1, list_of_numbers=[1, 2, 3])

sc.hard_math(3) 
# 19


# 自动生成 __eq__
sc == SomeClass(1, [1, 2, 3])
# True


# 自动生成 __ne__
sc != SomeClass(2, [3, 2, 1])
# True


# 生成的类可以转换为字典
asdict(sc)
# {'a_number': 1, 'list_of_numbers': [1, 2, 3]}


# 可以定义默认值
SomeClass()
# SomeClass(a_number=42, list_of_numbers=[])


C = make_class("C", ["a", "b"])
C("foo", "bar")
# C(a='foo', b='bar')
```

