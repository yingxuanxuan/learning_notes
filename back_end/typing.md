# typing

## 类型标注

* 类型标注不影响程序执行，只会被第三方工具所使用

### 基本格式

```python
def greeting(name: str) -> str:
    return 'Hello' + name
```

### 类型别名

* 可以重命名类型，用于缩短泛型嵌套


* 示例1，将泛型重命名


```python
from typing import List
Vector = List[float]

def scale(scalar: float, vector: Vector) -> Vector:
    return [scalar * num for num in vector]

new_vector = scale(2.0, [1.0, -4.2, 5.4])
```


* 示例2，避免泛型嵌套


```python
from typing import Dict, Tuple, Sequence

ConnectionOptions = Dict[str, str]
Address = Tuple[str, int]
Server = Tuple[Address, ConnectionOptions]

# 简化后
def broadcast_message(message: str, servers: Sequence[Server]) -> None:
    pass
    
# 简化前
def broadcast_message(
    message: str, 
    servers: Sequence[Tuple[Tuple[str, int], Dict[str, str]]) -> None:
    pass
```


### NewType静态检查

* NewType在静态检查时，创建一个等同于子类的的类型，用于分别相同类型，发现Bug


* 使用NewType创建类型在运行时无效，完全等效于原始类型
* NewType创建的类型在运行时，等效于一个函数，返回原类型原值
* 使用NewType创建类型用于静态检查，父类传入会导致检查失败


```python
from typing import NewType

UserId = NewType('UserId', int)

def get_user_name(user_id: UserId) -> str:
    pass

# 静态检查成功
user_a = get_user_name(UserId(42351)) 

# 静态检查失败，int类型无法匹配子类UserId
user_b = get_user_name(-1)
```


* NewType操作完全等效于原类型，运行结果也一定为原类型


```python
# 返回int而不是UserId
output = UserId(2222) + UserId(3333)
```

* NewType仅可用于静态检查，不能在运行时当作类型继承


```python
# 继承失败，因为UserId运行时是一个函数
class AdminUserId(UserId): pass 
```


### 自定义泛型

* typing已将内部容器类型扩展为支持泛型，使用[]符号限定内容类型


```python
from typing import Mapping, Sequence

def notify_by_email(employees: Sequence[Employee],
                    overrides: Mapping[str, str]) -> None
    pass
```


* 使用typing.TypeVar参数化泛型


```python
from typing import Sequence, TypeVar

T = TypeVar('T') # 如不指定，则匹配任意类型，相当于Any
T = TypeVar('T', int) # 错误，至少两个类型
T = TypeVar('T', float, int) # float 或 Int
```


* 自定义泛型类型，继承typing.Generic
* Generic的每个参数类型必须是不同的，如Generic[T, T]无效


```python
from typing import TypeVar, Generic
from logging import Logger

T = TypeVar('T')

class LoggeredVar(Generic[T]):
    def __init__(self, value: T, name: str, logger: Logger) -> None:
        self.name = name
        self.logger = logger
        self.value = value
    
    def set(self, new: T) -> None
        self.log('Set ' + repr(self.value))
        self.value = new
    
    def get(self) -> T:
        self.log('Get ' + repr(self.value))
        return self.value
    
    def log(self, message: str) -> None:
        self.logger.info('%s: %s', self.name, message)

from typing import Iterable
def zero_all_vars(vars: Iterable[LoggedVar[int]]) -> None:
    for var in vars:
        var.set(0)
```

### 特殊类型

#### Callable

* 格式：`Callable[[Type1, Type2], ReturnType]`
* 省略入参列表：`Callable[..., ReturnType]`


```python
from typing import Callable

def feeder(get_next_item: Callable[[], str]) -> None:
    pass
    
def async_query(on_success: Callable[[int], None], 
                on_error: Callable[int, Exception], None]) -> None:
    pass
```

#### Any，所有类型

#### NoReturn

```python
from typing import NoReturn

def stop() -> NoReturn:
    raise RuntimeError
```

#### Union，并集

* Union[X, Y]，要么X类型，要么Y类型

#### Optional[X]

* X或者None

#### Literal，字面量

* 若不定义枚举类型，可以使用Literal限定字面量，限定字符串


```python
from typing import Literal

MODE = Literal['r', 'rb', 'w', 'wb']
def open_helper(file: str, mode: MODE) -> str:
    pass
    
open_helper('path', 'r') # 正确
open_helper('path', 'a') # 错误
```

#### typing.AnyStr

* 允许任何类型的字符串
* 但是不允许混合
* AnyStr = TypeVar('AnyStr', str, bytes)


```python
def concat(a: AnyStr, b: AnyStr) -> AnyStr:
    return a+b

concat(u'foo', u'bar')
concat(b'foo', b'bar')
concat(u'foo', b'bar') # 错误
```

#### typing.ClassVar

* 限定变量指向类


```python
class Starship:
    stats: ClassVar[Dict[str, int]] = {} # 类
    damage: int = 10                     # 对象
```

#### typing.Final

* 声明变量不可再被赋值


```python
MAX_SIZE: Final = 9000
MAX_SIZE += 1 # 错误

class Connection:
    TIMEOUT: Final[int] = 10

class FastConnector(Connection):
    TIMEOUT = 1 # 错误
```

#### @typing.overload

#### @typing.final 不可继承，可以修饰函数、类

#### @typing.no_type_check 不检查类型

#### @typing.no_type_check_decorator

#### @typing.type_check_only 仅用于静态检查，运行时无效

#### @typing.runtime_checkable


### 内置类型的泛型版本

#### list

* Sequence，一般用来标注参数类型，可以容纳子类
* List，一般用来标注返回类型

#### dict

* Mapping，一般用来标注参数类型
* Dict，一般用来标注返回类型

#### Tuple

* Tuple[int, float, str] 限定tuple内类型为int, float, str

#### 其他

* Iterable(Generic[T_co]) -> collections.abc.Iterable
* Iterator(Iterable[T_co]) -> collections.abc.Iterator
* Reversible(Iterable[T_co]) -> collections.abc.Reversible
* Container(Generic[T_co]) -> collections.abc.Container
* Hashable -> collections.abc.Hashable
* Sized -> collections.abc.Sized
* Collection(Sized, Iterable[T], Container[T]) -> collections.abc.Collection
* AbstractSet(Sized, Collection[T_co]) -> collections.abc.Set
* MutableSet(AbstractSet[T]) -> collections.abc.MutableSet
* Mapping(Sized, Collection[KT], )



### 专题：lambda表达式类型标注

```python
from typing import Callable

func: Callable[[str, str], int] = lambda var1, var2: var1.index(var2)
```

