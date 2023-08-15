# Data Structures and Algorithms

目录
----------
[toc]

数据结构
----------
### 概念
* 数据以何种方式组织，存储
* 按是否有序分类：
    * 有序数据结构
        * 数组
        * 链接链表
        * 堆栈
        * 队列
        * 矩阵
    * 无序数据结构
        * 二叉树
        * 堆
        * 哈希表
        * 图表
* 按元素关系分类：
    * 线性结构：一对一数据结构
    * 树结构：一对多数据结构
    * 图结构：多对多数据结构


### Python内置抽象容器
* 文档：<https://docs.python.org/zh-cn/3/library/collections.abc.html>
* 魔术方法：<https://docs.python.org/zh-cn/3/reference/datamodel.html>


### 列表，List
* python内置列表list
* list保存内存地址，地址指向存储的数据
* list插入数据会自动扩展内存
* list插入时间复杂度
* 列表/数组的基本操作：
    * 下标 O(1)
    * 插入 O(n)
    * 删除 O(n)

#### array，数组
* typcode，数组中内容的类型
    * b：1字节有符号
    * B：1字节无符号
    * C：1字节字符
    * i：2字节有符号
    * I：2字节无符号
    * F：4字节浮点数
    * d：8字节浮点数
    * l：4字节有符号
    * L：4字节无符号
    * q：8字节有符号
    * Q：8字节无符号


```python
from array import *

# 创建数组
# arrayName = array(typecode, [Initializers])
a = array('i', [1, 2, 3, 4, 5])

# 切片访问
# a[0]
# a[-1]
# a[0:3]
# a[2] = 80
print(a)
print(a[0])
print(a[-1])
print(a[0:3])
a[2] = 80
print(a)

# 插入
# a.insert(index, item)
a.insert(0, 0)
print(a)

# 按value删除
# a.remove(item)
a.remove(1)
print(a)

# 按value查找
# a.index(item)
print(a.index(80))
```

#### numpy.array，二维数组
```python
from numpy import *
a = array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
```

### 栈，Stack
* 定义：
    * 只能在一端插入、删除操作的列表
* 特点：
    * 后进先出，LIFO，（last-in, first-out)
* 基本操作：
    * 进栈，压栈，入栈，push
    * 出栈，pop
    * 栈顶，top
    * 栈底，bottom
* 列表（数组）实现栈：
```python
# 进栈：list.append
# 出栈：list.pop
# 栈顶：list[-1]

# 组合封装
class Stack:
    def __init__(self):
        self._l = list()

    def push(self, item):
        return self._l.append(item)

    def pop(self):
        return self._l.pop()

    def empty(self):
        return len(self._l) == 0

    def top(self):
        '''
        考虑空数组时越界
        '''
        if self.empty():
            return None
        else:
            return self._l[-1]

    def bottom(self):
        '''
        考虑空数组时越界
        '''
        if self.empty():
            return None
        else:
            return self._l[0]

# 继承封装
class Stack(list):
    def __init__(self, *args, **kwargs):
        list.__init__(self, *args, **kwargs)

    def push(self, item):
        return self.append(item)

    def pop(self):
        return list.pop(self)

    def empty(self):
        return not bool(len(self))

    def top(self):
        if self.empty():
            return None
        return self[-1]

    def bottom(self):
        if self.empty():
            return None
        return self[0]

    
if '__main__' == __name__:
    s = Stack([])
    print(s.empty())
    print(s.top())
    print(s.bottom())
    
    s = Stack([1,2])
    print(s.empty())
    print(s.top())
    print(s.bottom())
    
    s.push(3)
    print(s)
    
    s.pop()
    print(s)

```

#### 括号匹配问题，Brace match
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

PAIRS = '()[]{}\'\'""（）【】｛｝‘’'
L_TO_R_DICT = dict(zip(PAIRS[::2], PAIRS[1::2]))
R_TO_L_DICT = dict(zip(PAIRS[1::2], PAIRS[::2]))

def is_brace_match(s):
    stack = []
    empty = lambda stack: 0 == len(stack)
    top = lambda stack: None if empty(stack) else stack[-1]

    for i in s:
        if i in PAIRS:
            if i in L_TO_R_DICT:
                stack.append(i)
            else:
                if top(stack) == R_TO_L_DICT[i]:
                    stack.pop()
                else:
                    return False

    if empty(stack):
        return True
    else:
        return False

if '__main__' == __name__:
    s1 = '()(){}[]' # 匹配
    s2 = '([{()}])' # 匹配
    s3 = '[]('      # 不匹配
    s4 = '[(])'     # 不匹配

    t1 = '('
    t2 = ')'
    t3 = '\'\''     # bug，两个引号相同，都会入栈不会出栈，中文引号不存在这个问题
                    # 引号中内容失效，这个函数也无法解决
    t4 = '‘’'
    t5 = '“”'

    print(is_brace_match(t1))
    print(is_brace_match(t2))
    print(is_brace_match(t3))
    print(is_brace_match(t4))
    print(is_brace_match(t5))

    print(is_brace_match(s1))
    print(is_brace_match(s2))
    print(is_brace_match(s3))
    print(is_brace_match(s4))
```

### 队列，Queue
* 定义：
    * 仅允许在一端插入，另一端删除的队列
* 操作：
    * 在队尾rear插入，入队，进队
    * 在队首front删除，出队
* 性质：
    * 先进先出，First in, First out
* 环形队列，有限队列
    * 空队：front == rear
    * 出队：front = (front + 1) % length
    * 入队：rear  = (rear + 1) % length
    * 满队：(rear + 1) % length == front

#### 环形队列实现1
```python
class Queue:
    def __init__(self, size):
        self._l = [None for _ in range(size)]
        self._size = size
        self._head = 0 # 指向队首
        self._tail = 0 # 指向队尾的后一个位置(浪费一个位置用于判断队列满）

        # 修改代码可改成
        # self._head = 0 # 指向队首的前一个位置
        # self._tail = 0 # 指向队尾
        
    def is_empty(self):
        return self._head == self._tail

    def is_full(self):
        return (self._tail+1) % self._size == self._head

    def push(self, item):
        if self.is_full():
            return
        self._l[self._tail] = item
        self._tail = (self._tail+1) % self._size

    def pop(self):
        if self.is_empty():
            return
        rt = self._l[self._head]
        self._head = (self._head+1) % self._size
        return rt

q = Queue(10)
for i in range(9):
    q.push(i)
while not q.is_empty():
    print(q.pop())
```

#### 环形队列实现2
```python
class MyQueue:
    def __init__(self, queue_length):
        self._queue_length = queue_length
        self._list_length = self._queue_length + 1
        self._list = [None for _ in range(self._list_length)]
        self._head = 0
        self._tail = 0

    def __len__(self):
        if self._tail < self._head:
            return self._tail + self._list_length - self._head
        else:
            return self._tail - self._head

    def _next_id(self, cur):
        return (cur + 1) % self._list_length

    def _prev_id(self, cur):
        return (cur - 1) % self._list_length

    def full(self):
        return self._next_id(self._tail) == self._head

    def empty(self):
        return self._head == self._tail

    def head(self):
        if self.empty():
            raise Exception('Queue is empty')
        return self._list[self._head]

    def tail(self):
        if self.empty():
            raise Exception('Queue is empty')
        return self._list[self._prev_id(self._tail)]

    def push(self, item):
        if self.full():
            raise Exception('Queue is full')
        self._list[self._tail] = item
        self._tail = self._next_id(self._tail)

    def pop(self):
        if self.empty():
            raise Exception('Queue is Empty')
        head = self._list[self._head]
        self._head = self._next_id(self._head)
        return head

if '__main__' == __name__:
    q = MyQueue(2)
    print(len(q))
    print(q.full())
    print(q.empty())
    print()

    q.push(1)
    print(len(q))
    print(q.full())
    print(q.empty())
    print(q.head())
    print(q.tail())
    print()

    q.push(2)
    print(len(q))
    print(q.full())
    print(q.empty())
    print(q.head())
    print(q.tail())
    print()
    
    q.pop()
    print(len(q))
    print(q.full())
    print(q.empty())
    print(q.head())
    print(q.tail())
    print()
    
    q.push(3)
    print(len(q))
    print(q.full())
    print(q.empty())
    print(q.head())
    print(q.tail())
    print()

    q.pop()
    q.pop()
```

* 内置队列：collections.deque
    * 队尾入队：deque.append(item)
    * 队首出队：item = deque.popleft()
    * 队首入队：deque.appendleft(item)
    * 队尾出队：item = deque.pop()

```python
# 创建
deque([iterable[, maxlen]])

# 长度
len(deque)

# 队首，队尾
deque[0]
deque[-1]

# 入队
deque.append(item)
deque.appendleft(item)

# 出队
deque.pop(item)
deque.poplift(item)

# 支持中间插入，查找，计数
deque.clear()
deque.count(value)
deque.index(value)
deque.remove(value)
deque.insert(index, value)

# 环形移位
deque.rotate()
deque.rotate(1)
deque.rotate(-1)
```

* 内置队列应用：文件后5行
```python
for collections import deque

def tail(n):
    with open('file.txt', 'r') as f:
        # deque接受一个iterable作为输入，n限制queue大小
        q = deque(f, n)
        return q

for line in tail(5):
    print(line)
```

#### 迷宫问题
* 问题：
    * 给一个二维列表，表示迷宫，0表示通道，1表示围墙。给出算法，求一条走出迷宫的路径。
```python
MAZE = [
    [1,1,1,1,1,1,1,1,1,1],
    [1,0,0,1,0,0,0,1,0,1],
    [1,0,0,1,0,0,0,1,0,1],
    [1,0,0,0,0,1,1,0,0,1],
    [1,0,1,1,1,0,0,0,0,1],
    [1,0,0,0,1,0,0,0,0,1],
    [1,0,1,0,0,0,1,0,0,1],
    [1,0,1,1,1,0,1,1,0,1],
    [1,1,0,0,0,0,0,0,0,1],
    [1,1,1,1,1,1,1,1,1,1]
]

def print_maze(maze):
    for line in maze:
        print(line)

DIR = [
    lambda x: (x[0], x[1] + 1), # 右
    lambda x: (x[0] + 1, x[1]), # 下
    lambda x: (x[0], x[1] - 1), # 上
    lambda x: (x[0] - 1, x[1])] # 左

print(find_path(begin=(1, 1), end=(1,8)))
```


* 栈解法：深度优先搜索，回溯法
    * 从一个节点开始，任意找下一个能走的点，找不到时，退回上一个点找是否有其他方向的点
    * 使用栈存储当前路径
```python
def find_path(maze=MAZE, begin=(1,1), end=(8,8)):
    def path(s):
        l = []
        while not s.empty():
            l.append(s.pop())
        return list(reversed(l))
    
    s = Stack()
    s.push(begin)

    while not s.empty():
        cn = s.top()
        # 尝试过的节点标记为8
        maze[cn[0]][cn[1]] = 8
        
        if cn == end: # 出口
            return path(s)
        else:         # 非出口
            # 遍历4个方向
            for d in DIR:
                nn = d(cn) # 计算方向位置
                # 这个方向可以走
                if maze[nn[0]][nn[1]] == 0:
                    # 尝试走这个方向
                    s.push(nn)
                    break
            else:
                # 这个位置四个方向都无法走通，则回到上一个节点
                s.pop()
    else:
        # 遍历所有通路都未找到end点
        return []

print_maze(MAZE)
print(find_path(begin=(1, 1), end=(1,8)))
print_maze(MAZE)
```


* 队列解法：广度优先搜索
    * 从一个节点开始，寻找所有接下来能继续走的点，直到找到出口
    * 使用队列存储当前正在考虑的节点
    * 需要存储是新节点入队是哪个旧节点出队
```python
from collections import deque

def find_path(maze=MAZE, begin=(1,1), end=(8,8)):
    def find_all_node(q, n):
        l = deque()
        l.appendleft(n)
        # 倒序查找队列，找到当前节点的前一个节点
        for i in reversed(q):
            if i[0] == n:
                n = i[1]
                l.appendleft(i[1])
        return list(l)

    his_q = deque()
    cur_q = deque()
    cur_q.append(begin)
    while len(cur_q) > 0:
        cn = cur_q.popleft()
        maze[cn[0]][cn[1]] = 8

        # 找到结束点，开始回溯
        if cn == end: 
            return find_all_node(his_q, cn)

        # 计算所有下一步节点
        for d in DIR:
            nn = d(cn)
            if maze[nn[0]][nn[1]] == 0:
                cur_q.append(nn)
                his_q.append((nn, cn))

    return []

print_maze(MAZE)
print(find_path(begin=(1,1), end=(8,8)))
```


### 链表
* 定义：
    * 节点之间使用指针连接的一对一数据结构
* 类型：
    * 头插法：只记录头节点，新节点在头部插入
    * 尾插法：记录头尾，新节点插在尾部
    * 双链表：每个节点都记录前后节点
* 时间复杂度：
    * 按值查找: O(n)
    * 按下标查找: O(n)
    * 插入: O(1)
    * 删除: O(1)


#### 单链表，Single Linked List，可以实现队列、堆栈，不适合从中间删除
* 操作：
    * 创建
    * 插入，需要知道插入点前的节点
    * 删除，需要知道删除点前的节点，一般需要从头遍历
    * 查找
    * 逆序，无法逆序遍历
```python
from collections.abc import Collection, Iterator, Iterable

class SLLNode:
    def __init__(self, value=None):
        self._value = value
        self._next = None

class SLLIter(Iterator):
    def __init__(self, cur):
        self._cur = cur

    def __iter__(self):
        return self

    # 返回当前值并移动到下一个节点
    def __next__(self):
        if self._cur == None:
            raise StopIteration
        else:
            value = self._cur._value
            self._cur = self._cur._next
            return value

class SLList(Collection):
    # 后进前出，实现队列Queue
    # 前进前出，实现堆栈Stack
    # 删除需要已知前一个节点，不方便从头以外的节点删除

    def __init__(self, it=None):
        # 头尾指向同一个空节点，使insert、delete操作统一，判空条件简化
        self._head = SLLNode()
        self._tail = self._head
        # _size是非必要操作，不使用_size作为操作条件
        self._size = 0
        # 使用一个可迭代对象初始化链表
        if isinstance(it, Iterable):
            for i in it:
                self.append(i)

    def __iter__(self):
        # self._head永不为空，_next空链表时为空
        return SLLIter(self._head._next)

    def __contains__(self, value):
        i = self._head._next
        while i:
            if i._value == value:
                return True
            i = i._next
        return False

    def __len__(self):
        return self._size

    def insert(self, node, value):
        # value插入node之后
        new_node = SLLNode(value)
        new_node._next = node._next
        node._next = new_node
        # 插入点是最后一个节点之后，则更新尾节点
        if self._tail == node:
            self._tail = new_node

        self._size += 1

    def delete(self, prev_node, node):
        # 头尾指向同一个空节点作为空链表条件
        if self._head == self._tail:
            return None
        prev_node._next = node._next
        node._next = None
        # 删除的是最后一个节点，则更新尾节点
        if self._tail == node:
            self._tail = prev_node

        self._size -= 1
        # 插入时记录值，删除时返回值
        return node._value

    def append(self, value):
        return self.insert(self._tail, value)

    def appendleft(self, value):
        return self.insert(self._head, value)

    def popleft(self):
        # self._head永不为空
        return self.delete(self._head, self._head._next)

l = SLList([-1, 0])
for i in l:
    print(i)
print()
l.append(1)
l.append(2)
for i in l:
    print(i)
print(2 in l)
print()
l.popleft()
l.popleft()
for i in l:
    print(i)
print()
l.appendleft(0)
l.appendleft(-1)
for i in l:
    print(i)
```

##### 单链表翻转问题

```python


```





#### 双链表，Double Linked List，可以方便从中间删除

```python
from collections.abc import Collection, Iterator, Iterable, Reversible

class DLLNode:
    def __init__(self, value=None):
        self._value = value
        self._next = None
        self._prev = None

class DLLIter(Iterator):
    def __init__(self, cur, reverse=False):
        self._cur = cur
        self._reverse = reverse

    def __iter__(self):
        return self

    # 返回当前值并移动到下一个节点
    def __next__(self):
        if self._reverse:
            # tail之前永远多一个空节点
            if self._cur._prev == None:
                raise StopIteration
            else:
                value = self._cur._value
                self._cur = self._cur._prev
                return value
        else:
            if self._cur == None:
                raise StopIteration
            else:
                value = self._cur._value
                self._cur = self._cur._next
                return value

class DLList(Collection, Reversible):
    def __init__(self, it=None):
        # 头尾指向同一个空节点，使insert、delete操作统一，判空条件简化
        self._head = DLLNode()
        self._tail = self._head
        # _size是非必要操作，不使用_size作为操作条件
        self._size = 0
        # 使用一个可迭代对象初始化链表
        if isinstance(it, Iterable):
            for i in it:
                self.append(i)

    def __iter__(self):
        # self._head永不为空，self._head._next空链表时为空，self._head._prev永远为空
        return DLLIter(self._head._next)

    def __reversed__(self):
        # self._tail永不为空，self._tail._prev空链表时为空，self._tail._next永远为空
        return DLLIter(self._tail, True)

    def __contains__(self, value):
        i = self._head._next
        while i:
            if i._value == value:
                return True
            i = i._next

        return False

    def __len__(self):
        return self._size

    def insert(self, node, value):
        # value插入node之后
        new_node = DLLNode(value)
        new_node._prev = node
        new_node._next = node._next
        if node._next is not None:
            node._next._prev = new_node
        node._next = new_node
        # 插入点是最后一个节点之后，则更新尾节点
        if self._tail == node:
            self._tail = new_node

        self._size += 1

    def delete(self, node):
        # 头尾指向同一个空节点作为空链表条件
        if self._head == self._tail:
            return None
        # 删除的是最后一个节点，则更新尾节点
        if self._tail == node:
            self._tail = node._prev

        node._prev._next = node._next
        if node._next is not None:
            node._next._prev = node._prev
        node._prev = None
        node._next = None
        self._size -= 1
        # 插入时记录值，删除时返回值
        return node._value

    def append(self, item):
        return self.insert(self._tail, item)

    def appendleft(self, item):
        return self.insert(self._head, item)

    def pop(self):
        return self.delete(self._tail)

    def popleft(self):
        return self.delete(self._head._next)

l = DLList([-1, 0])
for i in l:
    print(i)
print()
l.append(1)
l.append(2)
for i in l:
    print(i)
print(2 in l)
print()
l.popleft()
l.popleft()
for i in l:
    print(i)
print()
l.pop()
l.pop()
l.appendleft(0)
l.appendleft(-1)
for i in reversed(l):
    print(i)
```


### 哈希表
* 定义：
    * 通过哈希函数来存储、提取数据的数据结构
* 操作：
    * 插入，insert(key, value)
    * 查找，get(key)
    * 删除，delete(key)
* 哈希冲突解决办法：
    * 开放寻址法：如果已经被占用，则向后探查新位置
        * 线性探查：`+= 1`
        * 二次探查：`+= 1^2, -= 1^2`
        * 二度哈希：更换哈希函数
    * 拉链法（常用）：使用链表存在哈希桶后面
* 常见哈希函数：
    * MD5（实际常用）: h(k) = md5(k)
    * 除法哈希：h(k) = h % m
    * 惩罚哈希：h(k) = floor(m x (A x key%1))  A是个小数
    * 全域哈希：h_a_b(k) = (a x key + b) % m) % n
* 应用：
    * 集合
    * 字典
* 实现：
```python
from collections.abc import Hashable

class HashTable:
    def __init__(self, bucket_size):
        self._bucket_size = bucket_size
        self._buckets = [DLList() for _ in range(bucket_size)]

    def _bucket(self, key):
        if isinstance(key, Hashable):
            return self._buckets[hash(key) % self._bucket_size]
        else:
            raise TypeError('Key is not Hashable.')

    def get(self, key, default=None):
        '''
        注意不存在时抛出异常，因为值可能为None
        '''
        bucket = self._bucket(key)
        for i in bucket:
            if i[0] == key:
                return i[1]
        else:
            return default

    def insert(self, key, value):
        bucket = self._bucket(key)
        for i in bucket:
            if i[0] == key:
                i[1] = value
                return
        else:
            bucket.append((key, value))

    def delete(self, key):
        '''
        注意不存在时抛出异常
        '''
        
        bucket = self._bucket(key)
        # 为避免两次遍历，使用链表类内部操作
        node = bucket._head._next
        while node:
            if key == node._value[0]:
                v = node._value[1]
                bucket.delete(node)
                return v
            node = node._next
        else:
            return None


h = HashTable(7)
print(h.get(1))
print(h.get(1, 'NotExist!'))
h.insert(1, 'a')
h.insert('c', 'b')
h.insert(8, 'z')
print(h.get(8))
print(h.get('c'))
print(h.delete(1))
print(h.get(1))
```

### 树，Tree
略

### 二叉树，Binary Tree
* 定义：
    * 度不超过2的树
* 存储方式：
    * 顺序存储：只适用于完全二叉树
    * 链式存储：节点之间使用指针连接
* 遍历算法：
    * （根）前序遍历，pre order：根->左子树->右子树
    * （根）中序遍历, in order：左子树->根->右子树
    * （根）后续遍历，post order：左子树->右子树->根
    * （根）层次遍历，level order：按层访问根节点，使用队列，子节点入队
* 遍历问题：
    *  已知两个遍历方式，可以确定一个树
    *  已知一个遍历方式，无法确定一个树
* 遍历实现：
```python
class BNode:
    def __init__(self, value):
        self._v = value
        self._l = None
        self._r = None
    def __str__(self):
        return self._v


a = BNode('a')
b = BNode('b')
c = BNode('c')
d = BNode('d')
e = BNode('e')
f = BNode('f')
g = BNode('g')

e._l = a
e._r = g
a._r = c
c._l = b
c._r = d
g._r = f

def pre_order(root):
    if root:
        print(root, end=',')
        pre_order(root._l)
        pre_order(root._r)

def in_order(root):
    if root:
        in_order(root._l)
        print(root, end=',')
        in_order(root._r)

def post_order(root):
    if root:
        post_order(root._l)
        post_order(root._r)
        print(root, end=',')

from collections import deque

def level_order(root):
    q = deque()
    q.append(root)

    while len(q) > 0:
        node = q.popleft()
        print(node, end=',')
        if node._l:
            q.append(node._l)
        if node._r:
            q.append(node._r)

pre_order(e)
print()
in_order(e)
print()
post_order(e)
print()
level_order(e)
```

### 二叉搜索树，BST，Binary Search Tree
* 定义：
    * 左子树的值都比父节点小，右子树的值都比父节点大
* 性质：
    * 中序遍历是排好序的（堆层级遍历有序）
* 查找算法：
    * 比父节点小、递归左子树，比父节点大、递归右子树
* 操作：
    * 查询，平均O(logn)
    * 插入，平均O(logn)
    * 删除
        * 叶子节点：直接删
        * 只有一个孩子：把该节点的子节点和父节点相连
        * 有两个孩子：找左子树里最大的数，或者右子树里最小的树，替换该节点
* 缺陷：
    * 偏斜，极端退化为链表
    * 解决方案1：随机化插入
    * 解决方案2：AVL树

### 自平衡二叉树，AVL树，AVL（三个科学家名字缩写）
* 定义：
    * 自平衡的二叉搜索树
* 性质：任意节点左右子树高度差不超过1
    * 左右子树高度差绝对值不能超过1
    * 根的左右子树都是平衡二叉树
* 平衡因子，Balance Factor:
    * 左子树的高度减去右子树的高度
* 插入算法：
    * 按插入的路径，回溯，找出第一个被破坏平衡的节点
    * 不平衡的四种情况：
        * K节点的右孩子C的右子树插入导致不平衡：左旋
        * K节点的左孩子C的左子树插入导致不平衡：右旋
        * K节点的右孩子C的左子树插入导致不平衡：先右旋，再左旋
        * K节点的左孩子C的右子树插入导致不平衡：先左旋，再右旋
    * 左旋算法：
        * K的右孩子C提升为父节点
        * K下降为左节点
        * K的右孩子改为C的左子树
    * 左旋理解：
        * K为父节点时，C为K的右节点，C比K大，C的左子树节点都比C小
        * K < C的左子树 < C
        * C为父节点时，K比C小，K为C的左节点，C的左子树整体作为K的右子树
    * 右旋算法：
        * K的左孩子C提升为父节点
        * K下降为右节点
        * K的左孩子改为C的右子树

#### B树，BTree, B-Tree
* 用于数据库索引


算法简介
----------
### 算法
Algorithm，一个计算过程，解决问题的方法

### 时间复杂度
* 常见时间复杂度：
    * O(1) < O(log(n, 2)) < O(n) < O(n*log(n,2)) < O(n^2) < O(n^2*log(n,2)) < O(n^3)
* 复杂问题时间复杂度：
    * O(n!) < O(2^n) < O(n^n)
* 快速判断算法复杂度：
    * 确定问题规模 n
    * 循环减半 log(n, 2)
    * k层关于n的循环 n^k


* O(1)
```python
print('H') 
```

* O(n)
```python
for _ in range(n):
    print('H')
```

* O(n^2)
```python
for _ in range(n):
    for _ in range(n):
        print('H')
```

* O(log(n,2)*n) 或 O(log(n,2))
```python
# 循环减半
# 2^6 = 64
while n > 1:
    print(n)
    n = n // 2
```

### 空间复杂度
* 概念：
    * 用来评估算法内存占用大小的式子。
    * 可以用空间换时间
* O(1)：几个变量
* O(n)：一维列表
* O(m*n)：m行n列

### 时间评估
```python
from functools import wraps
import time

from functools import wraps
import time

def ptime(func):
    @wraps(func)
    def wrapper(*args, **kws):
        t1 = time.time()
        rt = func(*args, **kws)
        t2 = time.time()
        print('{} running time: {} seconds.'.format(func.__name__, t2 - t1))
        return rt 
    return wrapper

@ptime
def t():
    time.sleep(1)

t()
```


递归问题
--------
### 执行顺序

* 递归前执行，结果：1, 2, 3
```python
def fun(x):
    if x < 10:
        print(x)
        fun(x + 1)
```

* 递归后执行，结果：3, 2, 1
```python
def fun(x):
    if x < 10:
        fun(x + 1)
        print(x) # 递归最内层开始执行
```

### 汉诺塔问题
* 问题：
    * 整体从a移动到c
* 简化：
    * 将n-1个盘子看作整体
    * 递归子问题：n-1 a 通过空柱子 b 移动到 c
    * n   a 移动到 b
    * 递归子问题：n-1 c 通过空柱子 a 移动到 b
* 问题复杂度：
    * h(n) = 2h(n) + 1 ~= O(2^64)
```python
def hanoi(n, a, b, c):
    if n > 0:
        hanoi(n - 1, a, c, b)
        print('{} -> {}'.format(a, c))
        hanoi(n - 1, b, a, c)

hanoi(3, 1, 2, 3)
```

查找问题
----------
### 顺序查找，Linear Search
* 概念：
    * 线性查找，从头找到尾
* 复杂度：
    * 最差O(n)

### 二分查找、折半查找、Binary Sea5rch

* 复杂度：
    * 循环减半 O(logn)

#### 测试用例

```python
l = list(range(0, 10**7, 2))
print(search(l, 10**7 // 2))
print(binary_search(l, 10**7 // 2))
print()

l0 = []
l1 = [1]
l2 = [0, 1, 1, 1, 2]
l3 = [0 ,1, 1, 1, 1, 2]
l4 = [0 ,0, 1, 1, 1, 1, 2]
l5 = [0 ,0, 1, 1, 1, 1, 2, 3]

print(binary_search(l0, 1))
print(binary_search(l1, 1))
print(binary_search(l2, 1))
print(binary_search(l3, 1))
print(binary_search(l4, 1))
print(binary_search(l5, 1))
print()

print(left(l0, 1))
print(left(l1, 1))
print(left(l2, 1))
print(left(l3, 1))
print(left(l4, 1))
print(left(l5, 1))
print()

print(right(l0, 1))
print(right(l1, 1))
print(right(l2, 1))
print(right(l3, 1))
print(right(l4, 1))
print(right(l5, 1))
print()

from bisect
# 查找左插入点
print(bisect.bisect_left(l0, 1))
print(bisect.bisect_left(l1, 1))
print(bisect.bisect_left(l2, 1))
print(bisect.bisect_left(l3, 1))
print(bisect.bisect_left(l4, 1))
print(bisect.bisect_left(l5, 1))
print()

# 右插入点
print(bisect.bisect_right(l0, 1))
print(bisect.bisect_right(l1, 1))
print(bisect.bisect_right(l2, 1))
print(bisect.bisect_right(l3, 1))
print(bisect.bisect_right(l4, 1))
print(bisect.bisect_right(l5, 1))
print()
```



#### 直接查找相等（仅适用于无重复元素的查找，重复元素找到第几个不确定）

```python
def binary_search(l, search_value):
    start_index = 0
    end_index = len(l)

    # 此处需注意结束条件，不能带等于
    while(start_index < end_index):
        mid_index =  (start_index + end_index) // 2
        mid_value = l[mid_index]
        if search_value == mid_value:
            return mid_index
        elif search_value < mid_value:
            # 此处需要注意，end是list后一个位置，不会被比较
            end_index = mid_index
        else:
            # 此处需要注意，mid已经比较过，所以加一
            start_index = mid_index + 1
    else:
        return -1

# 速写版二分查找
def binary_search(l, v):
    b = 0
    e = len(l)
    
    while b < e:
        mid = (b + e) // 2
        mid_value = l[mid]
        
        if v == mid_value:
            return mid
        elif v < mid_value:
            e = mid
        elif v > mid_value:
            b = mid + 1
    
    return None
```

#### 查找左侧插入位（左值都小于，或者返回0，空list也可以list.insert(0)）

```python
def left(search_list, search_value, begin=0, end=None):
    end = end or len(search_list)

    while begin < end:
        mid = (begin + end) // 2
        mid_value = search_list[mid]
        if search_value > mid_value: # 左边必须小于目标值，否则begin向右移动
            begin = mid + 1
        else:
            end = mid
    return begin
```

#### 查找右侧插入位（右值都大于，或者返回len(list)，list可以insert(len(list)))

```python
def right(search_list, search_value, begin=0, end=None):
    end = end or len(search_list)

    while begin < end:
        mid = (begin + end) // 2
        mid_value = search_list[mid]
        if search_value >= mid_value: # 右边必须大于目标值，否则end向左移动
            begin = mid + 1
        else: 
            end = mid
    return begin
```



### python内置库 bisect

* 封装：<https://code.activestate.com/recipes/577197-sortedcollection/>
* bisect只能查找插入点，查找插入点是比返回value位置更加通用、更加抽象的接口
* bisect接口分为左侧插入点和右侧插入点
  * **左侧插入点更适合用于查找，如果值相等，则index下标相同**
  * 右侧插入点更适合用于插入，左侧相同的值不用移位
* bisect内置库：
```python
import bisect

# 寻找左侧插入点，返回index
bisect.bisect_left(array, value，low=0, high=len(array))

# 寻找右侧插入点，插入从最右侧最高效，但是插入效率为O(n)
bisect.bisect(array, value，low=0, high=len(array))
bisect.bisect_right(array, value，low=0, high=len(array))

# 左插入
bisect.insort_left(a, x, lo=0, hi=len(a))

# 右插入，插入从最右侧最高效，但是插入效率为O(n)
bisect.insort(a, x, lo=0, hi=len(a))
bisect.insort_right(a, x, lo=0, hi=len(a))
```
* bisect组合封装示例：
```python
# 查找最左边等于x的item
def index(a, x):
    'Locate the leftmost value exactly equal to x'
    i = bisect_left(a, x)
    if i != len(a) and a[i] == x:
        return i
    raise ValueError

# list中最右边小于x的item，查找x的最左端插入点，取前一个index
def find_lt(a, x):
    'Find rightmost value less than x'
    i = bisect_left(a, x)
    if i:
        return a[i-1]
    raise ValueError

# list中最右边小于等于x的item，查找x的最右端插入点，取前一个index
def find_le(a, x):
    'Find rightmost value less than or equal to x'
    i = bisect_right(a, x)
    if i:
        return a[i-1]
    raise ValueError

# list中最左边大于x的item，查找x的最右端插入点，一定小于右边
def find_gt(a, x):
    'Find leftmost value greater than x'
    i = bisect_right(a, x)
    if i != len(a):
        return a[i]
    raise ValueError

# list中最左边大于等于x的item，查找x的最左端插入点，一定大于等于左边
def find_ge(a, x):
    'Find leftmost item greater than or equal to x'
    i = bisect_left(a, x)
    if i != len(a):
        return a[i]
    raise ValueError
```


排序问题
----------
### 概念
* 将一组无序的记录序列调整为有序记录序列

* 分类：
    * LowB o(n**2)：
        * 冒泡排序
        * 选择排序
        * 插入排序
    * NiuB o(n*log(n))：
        * 快速排序
        * 堆排序
        * 归并排序
    * 其他：
        * 希尔排序
        * 计数排序
        * 基数排序

### 排序问题总结、动图
<https://www.runoob.com/w3cnote/ten-sorting-algorithm.html>

### 冒泡排序，Bubble Sort

* 列表中相邻的两个数，如果前面比后面大，则交换
* 遍历一次后，无序区减少一个数，有序区增加一个数
* 时间复杂度：O(n^2)，两次关于n的循环
* 改进：
    * 一趟中没有任何交换，则已经排好序

#### 冒泡排序while写法

```python
def bubble(ls, reverse=False):
    cursor = 0
    boundary = len(ls) - 1
    
    while boundary > 0:
        while cursor < boundary:
            curnext = cursor+1
            if (ls[cursor] > ls[curnext] if reverse else ls[cursor] < ls[curnext]):
                ls[cursor], ls[curnext] = ls[curnext], ls[cursor]
            cursor += 1
        cursor = 0
        boundary -= 1
```

#### 冒泡排序for写法

```python
def bubble_sort(l, reverse=False):
    # 注意：外层是迭代次数，由于一次比较两个数，最后一个数-1，前两个数-1
    for x in range(len(l) - 1 - 1):
        # 首次迭代得到最大数l[-1]
        # 倒数第二次迭代得到次大数l[-2]
        # 以此类推
        for y in range(len(l) - x - 1):
            if l[y] < l[y+1] if reverse else l[y] > l[y+1]:
                l[y], l[y+1] = l[y+1], l[y]
```

### 选择排序、Selection Sort

* 遍历一遍找到最小（或最大）的数
* 交换最大数与末尾数
* 时间复杂度：O(n^2)
* 选择排序时间复杂度较冒泡排序高，但是实测选择排序性能高，因为读多写少

#### 选择排序while写法
```python
@ptime
def selection_sort(ls, reverse=False):
    boundary = len(ls) - 1

    while boundary > 0:
        cursor = 0

        idx = 0
        val = ls[idx]

        while cursor <= boundary:
            if ls[cursor] < val if reverse else ls[cursor] > val:
                idx = cursor
                val = ls[idx]
            cursor += 1

        if idx != boundary:
            ls[idx], ls[boundary] = ls[boundary], ls[idx]

        boundary -= 1
```

#### 选择排序for写法
```python
def selection_sort(l, reverse=False):
    for x in range(len(l)-1): # 最后一趟不用比
        min_idx = x
        for y in range(x+1, len(l)): # 起点从x+1开始
            if l[min_idx] < l[y] if reverse else l[min_idx] > l[y]:
                min_idx = y
        if x != min_idx:
            l[x], l[min_idx] = l[min_idx], l[x]
```

#### 选择排序简单写法
* 实测使用min，max反而可以提高性能
```python
@ptime
def selection_sort(li, reverse=False):
    li_new = []
    for i in range(len(li)):
        val = max(li) if reverse else min(li)
        li_new.append(val)
        li.remove(val)
    li[:] = li_new
```


### 插入排序, Insertion Sort
* 遍历序列，每个item都按值插入到已排序表的适当位置
* 时间复杂度O(n^2)：
* 插入排序比冒泡排序、选择排序效率更高

* 顺势倒插，下一个item与之前的item顺势比较，逐个交换
```python
@ptime
def insertion_sort(l, reverse=False):
    length = len(l)

    for i in range(1, length):
        value = l[i]
        index = i
        
        while index-1 >= 0 and (value > l[index-1] if reverse else value < l[index-1]):
            l[index] = l[index-1] 
            index -= 1
        l[index] = value
```


### 快速排序，Quick Sort
* 算法：
    * 取一个元素p，作为基准
    * 列表被元素p分成两部分，左边都比p小，右边都比p大
    * 递归两部分作为子表
* 时间复杂度：O(n*log(n))
* 最坏情况：完全逆序的排序，O(n**2)
* 优化点：
    * 可以随机化选择分界值，避免最坏情况
    * 优化递归为循环，递归有深度限制
* 实现方法：
    * 实现1，最左边取一个值，从右边开始找比基准值小的值，交换两个值，再从右边开始找
        * 优点，可以原地排序
    * 实现2，最左边取一个值，从左边开始找比基准值小的值，将最小值插入基准值左边
        * 优点，实现思路简单
        * 缺点，插入比交换效率低

#### 递归实现1：左边取值，最右开始，交换两值，再从左起

```python
@ptime
def quick_sort(l, reverse=False):

    def partition(l, p_left, p_right):
        tmp = l[p_left]
        nonlocal reverse

        while p_left < p_right:
            while p_left < p_right and (l[p_right] <= tmp if reverse else l[p_right] >= tmp):
                p_right -= 1
            l[p_left] = l[p_right]

            while p_left < p_right and (l[p_left] >= tmp if reverse else l[p_left] <= tmp):
                p_left += 1
            l[p_right] = l[p_left]

        l[p_left] = tmp
        return p_left

    def quick_sort_recursive(l, left, right):
        if left < right:
            mid = partition(l, left, right)
            quick_sort_recursive(l, left, mid-1)
            quick_sort_recursive(l, mid+1, right)

    quick_sort_recursive(l, 0, len(l)-1)
```

#### 递归实现2：左边取值，最左开始，交换两值，再从左起

```python
pass
```



#### 循环实现

```python
@ptime
def quick_sort(l, reverse=False):

    def partition(l, p_left, p_right):
        tmp = l[p_left]
        nonlocal reverse

        while p_left < p_right:
            while p_left < p_right and (l[p_right] <= tmp if reverse else l[p_right] >= tmp):
                p_right -= 1
            l[p_left] = l[p_right]

            while p_left < p_right and (l[p_left] >= tmp if reverse else l[p_left] <= tmp):
                p_left += 1
            l[p_right] = l[p_left]

        l[p_left] = tmp
        return p_left

    p_list = []
    p_list.append((0, len(l) - 1))

    while p_list:
        p = p_list.pop()
        mid = partition(l, p[0], p[1])

        if p[0] < mid-1:
            p_list.append((p[0], mid-1))

        if mid+1 < p[1]:
            p_list.append((mid+1, p[1]))
```

### 堆排序

* 树
    * 根节点
    * 树的深度：根节点到最远节点的节点数
    * 树的度：分叉最多的节点的分叉数
    * 孩子节点，父节点
    * 子树


* 二叉树
    * 度为2的数
    * 孩子节点只有左节点和有节点
    * 满二叉树：每一层的节点数都达到最大
    * 完全二叉树：叶子节点只能出现在最下层和次下层，并且最下层节点都集中在最左边
    * 存储方式：
        * 顺序存储
        * 链式存储
    * 顺序存储（根节点从0计算）：
        * 父节点与左子节点关系：i -> 2i + 1
        * 父节点与右子节点关系：i -> 2i + 2


* 堆：一种特殊的完全二叉树
    * 大根堆：任意节点都比孩子节点大（中序遍历从大到小）
    * 小根堆：任意节点都比孩子节点小（中序遍历从小到大）
    * 堆的向下调整：
        * 如果：节点左右子树都是堆，自身不是堆
        * 那么：可以通过一次向下调整调整成一个堆
    * 堆出数：
        * 根节点出数
        * 把末尾节点填入根节点
        * 向下调整
        * 递归根节点出数
    * 构造堆：
        * 先调整最后一个非叶子节点
        * 再调整倒数第二个非叶子节点
        * 逆序向上


* 堆排序过程：
    * 构造堆：
        * 从最后一个非叶子节点，倒序遍历所有子树执行sift（堆向下调整）
    * 堆出数：出数最大值存到末尾叶子节点，末尾叶子结点放到根节点

#### python内置实现，heapq
* 排序
```python
import heapq
@ptime
def heap_sort_i(l, reverse=False):
    new_l = []
    if reverse:
        heapq._heapify_max(l)
        for i in range(len(l)):
            new_l.append(heapq._heappop_max(l))
    else:
        heapq.heapify(l)
        for i in range(len(l)):
            new_l.append(heapq.heappop(l))
    l[:] = new_l
```

* 最大的子列表：
    * sorted(list, reverse=True)[:n]
    * heapq.nlargest(n, list)
* 最小的子列表:
    * sorted(list)[:n]
    * heapq.nsmallest(n, list)
* 排序效率:
    * n = 1时：
        * max(), min()效率最高
    * n << len(list) 时：
        * heapq.nlargest, heapq.nsmallest效率最高
    * n = len(list)，或者需要复用时：
        * sorted(list) 效率最高
    * list内部排序时:
        * list.sort()效率最高
* 排序复杂度：n个数求前k个最大的数（k < n)
    * 排序+切片: 排序的最高效率为O(nlogn)，切片效率为O(n)
    * LowB三人组：O(nk)
    * 建k堆：O(nlogk)
        * 取前k个数建一个堆，取后续数调整堆

#### 堆的数组（list）实现
```python
import math
class Heap:
    '''
       0
     1   2
    3 4 5 6
    '''

    def head(l):
        # 堆顶：列表头
        return 0

    def tail(l):
        # 堆尾：列表尾
        return len(l) - 1

    def prev(current):
        # 前一个节点（包括前一个父节点）
        # 不保护越界
        return current - 1

    def next(current):
        # 后一个节点，包括后一个父节点
        # 不保护越界
        return current + 1

    def left_child(parent):
        # 左子节点
        return 2 * parent + 1

    def right_child(parent):
        # 右子节点
        return 2 * parent_idx + 2
    
    def parent(child):
        # 父节点
        return (child_idx - 1) // 2

    def last_parent(l):
        # 最后一个父节点
        return (len(l)-2) // 2

    def has_left_child(l, parent):
        # 判断是否有子节点
        return 2 * parent + 1 < len(l)

    def has_right_child(l, parent):
        # 判断是否有子节点
        return 2 * parent + 2 < len(l)

    def height(l):
        # 树高
        return math.floor(math.log(len(l), 2))

    def sub_tree_tail(l, node):
        # 求子树，返回子树末尾节点
        left = right = node
        length = len(l)
        
        if node < length:
            child = node*2+1
            if child < length:
                left = Heap.sub_tree_tail(l, child)

            child = child + 1
            if child < length:
                right = Heap.sub_tree_tail(l, child)

            return max(left, right)
        else:
            return node

    def print(l, low=0, high=None):
        if high is None:
            high = len(l)-1

        if low >= high:
            return

        length = high - low + 1
    
        # 打印树
        height = math.floor(math.log(length, 2) + 1)
        print('height', height)

        for h in range(height):
            for i in range(2**h):
                index = low + 2 ** h + i - 1
                if index > high:
                    break
                print(l[index], end=' ')
            print()


    def sift(l, low, high, max_root=False):
        # 向下调整
        # 假设：除树顶之外，其他都满足堆

        parent = low
        child = 2 * parent + 1
        top_value = l[low]

        while child <= high:
            # 大根堆：总选择大的孩子节点，这样可以保证替换上来的节点比其余节点都大
            # 小根堆：总选择小的孩子节点，这样可以保证替换上来的节点比其余节点都大
            if child+1 <= high and (l[child+1] > l[child] if max_root else l[child+1] < l[child]):
                child = child + 1

            # 大根堆：父节点小于所有子节点则替换
            # 小根堆：父节点大于所有子节点则替换
            if (l[child] > top_value if max_root else l[child] < top_value):
                l[parent] = l[child]
                parent = child
                child = 2 * parent + 1
            # 大根堆：父节点大于所有子节点则结束
            # 小根堆：父节点小于所有子节点则结束
            else:
                break

        # 结束时写回向下调整值，避免重复交换效率更高
        l[parent] = top_value


    def heapify(l, head, tail, max_root=False):
        # 最后一个父节点
        last_parent = (tail - 1) // 2
        
        # 从最后一个父节点循环向前进行sift，向下调整
        for node in range(last_parent, -1, -1):
            # 这里tail值严格来说应该设置为node子树的最后一个节点
            # 由于计算困难，可以传入整个堆的tail替代
            Heap.sift(l, node, tail, max_root)


    def push(l, item, max_root=False):
        # 堆是完全二叉树，又是顺序存储，在中间插入元素会造成堆变成非完全二叉树
        # 算法：插入堆尾，从堆尾开始调整所有父节点

        l.append(item)
        tail = len(l) - 1
        parent = (tail - 1) // 2
        while parent >= 0:
            Heap.sift(l, parent, tail, max_root)
            parent = (parent - 1) // 2
    
    def pop(l, max_root=False):
        # 堆顶是最大值或者最小值，从堆顶取值才有意义
        # 堆是完全二叉树，又是顺序存储，从中间取走元素会造成堆需要重新排序
        # 算法：取走堆顶，将堆尾放到堆顶，向下调整。

        l[0], l[-1] = l[-1], l[0]
        Heap.sift(l, 0, len(l)-2, max_root)

        return l.pop()
        

    @ptime
    def sort(l, reverse=False, low=0, high=None):
        length = len(l)
        if high is None:
            high = length-1

        if low >= high:
            return

        max_root = not reverse

        Heap.heapify(l, low, high, max_root)

        for i in range(high, 0, -1):
            l[0], l[i] = l[i], l[0]
            Heap.sift(l, 0, i-1, max_root)

    @ptime
    def top_n(l, n, reverse=False):
        # 最大值算法：取前n项建立小根堆，比最小值小的堆项被替换

        length = len(l)
        n_tail = n - 1

        Heap.heapify(l, 0, n_tail, max_root=reverse)
        for idx in range(n, length):
            if (l[idx] < l[0] if reverse else l[idx] > l[0]):
                l[0], l[idx] = l[idx], l[0]
                Heap.sift(l, 0, n_tail, max_root=reverse)

        return l[:n]
```

### 归并排序
* 概念：
    * 归并：合并两个有序列表
        * low、high中较小的值写入新列表，其中一个表空时结束
        * 写入非空表剩余值
    * 归并排序算法：
        * 递归分解一个列表
        * 每个列表是一个元素时一定为有序
        * 递归合并
    * 时间复杂度：
        * O(n*log(n)) = O(n)，O(log(n))，每层归并复杂度O(n)，一共O(log(n))层
    * team排序，归并排序的升级版，python内置排序算法

* 实现
```python
def merge(l, low, mid, high, reverse=False):
    tmp_l = []
    tmp_low = low
    tmp_mid = mid + 1
    
    while tmp_low <= mid and tmp_mid <= high:
        if (l[tmp_low] > l[tmp_mid] if reverse else l[tmp_low] < l[tmp_mid]):
            tmp_l.append(l[tmp_low])
            tmp_low += 1
        else:
            tmp_l.append(l[tmp_mid])
            tmp_mid += 1

    while tmp_low <= mid:
        tmp_l.append(l[tmp_low])
        tmp_low += 1

    while tmp_mid <= high:
        tmp_l.append(l[tmp_mid])
        tmp_mid += 1

    l[low:high+1] = tmp_l

@ptime
def merge_sort(l, reverse=False):

    def devide(l, low, high):
        if low < high:
            mid = (low + high) // 2
            devide(l, low, mid)
            devide(l, mid+1, high)
            merge(l, low, mid, high, reverse)

    devide(l, 0, len(l)-1)
```

### 希尔排序，Shell Sort
* 概念：
    * 通过分段，使用插入排序逐步降低乱序程度
* 算法：
    * 希尔排序是把序列分成lenght/2段，每一段进行插入排序
    * gap = length//2, gap = gap // 2
    * 实现的时候不必真的分组进行排序，仍然可以按照自然顺序，只是排序时按gap跳跃
* 时间复杂度：
    * 与gap选择相关

* 实现：
```python
@ptime
def shell_sort(l, reverse=False):
    length = len(l)
    gap = length // 2

    while gap > 0:
        for i in range(gap, length):
            cur_val = l[i]
            cur_idx = i
            
            while cur_idx - gap >= 0 and (cur_val > l[cur_idx-gap] if reverse else cur_val < l[cur_idx-gap]):
                l[cur_idx] = l[cur_idx-gap]
                cur_idx -= gap
            l[cur_idx] = cur_val
        gap = gap // 2
```

### 计数排序，Counting Sort
* 概念：
    * 已知需要排序的序列值在一个范围内，统计每个值出现的次数，依次展开
* 时间复杂度：
    * O(n) = O(n) + O(n)
* 空间复杂度：
    * O(n)
* 限制：
    * 必须知道数值范围，且数值范围必须较小
* 实现：
```python
# 需要排序的数，值必须分布在[0, 100)之间
@ptime
def counting_sort(l, max_num=99):
    buckets = [0 for _ in range(max_num+1)]

    for i in l:
        buckets[i] += 1

    l.clear()
    for index, count in enumerate(buckets):
        for _ in range(count):
            l.append(index)
```


### 桶排序，Bucket Sort，Bin Sort
* 概念：
    * 桶排序是计数排序的升级版，扩大了可以排序的数值范围
* 算法：
    * 将元素均匀映射到bucket中，再对每个桶中的元素排序
    * 合并桶中的数
* 时间复杂度：
  
* 实现：
```python
@ptime
def bucket_sort(l, bucket_num=101, max_num=10000):
    buckets = [[] for _ in range(bucket_num)]
    # 0 - 10000 会落到 0 - 100 共 101个桶里
    tmp = max_num // (bucket_num - 1)
    
    for value in l:
        idx = value // tmp

        insert = 0
        length = len(buckets[idx])-1
        while insert <= length and buckets[idx][insert] < value:
            insert += 1
        buckets[idx].insert(insert, value)

    l.clear()
    for bucket in buckets:
        l.extend(bucket)
```


### 基数排序，Radix Sort:
* 概念：
    * 基数排序在桶排序的基础上从数值高位向数值低位排序。
* 算法：
    * 1、计算最大数值的位数
    * 2、从高位开始循环，插入桶，取出
    * 3、最后一次取出则排好序
* 时间复杂度：
    * O(kn)，k等于位数，n等于序列长度
    * O(k+n)
    * 当k较大时，即数值范围过大时，效率会降低
* 实现
```python
@ptime
def radix_sort(l, reverse=False, redix=2):
    redix = 10**redix

    # 需要提前获知最大数有几位数
    max_value = max(l)
    max_radix = math.ceil(math.log(max_value, redix))
    
    # 计算位数可以使用while循环代替
    # r = 1
    # while redix**r <= max_value
    #     r += 1
    
    buckets = [[] for _ in range(redix)]
    for r in range(max_radix):
        for value in l:
            # 除以redix**r将高位变到最低位，余数作为桶序号
            buckets[value//(redix**r)%(redix)].append(value)
        l.clear()

        for bucket in buckets:
            l.extend(bucket)
            bucket.clear()
```

### 查找排序习题
* 给两个字符串s、t，判断t是否是s的重新排列后组成的单词
    * s = 'anagram', t = 'nagaram', # return True
    * s = 'rat', t = 'car' # return False
* 实现1:
    * s、t分别排序，比较相同则相同
```python
def isAnagram(self, s, t):
    ss = list(s)
    tt = list(t)
    ss.sort()
    tt.sort()
    return ss==tt
```
* 实现2：
    * 对s、t中的字母计数，依次比较个数是否相同
```python
def isAnagram(self, s, t):
    d1 = {}
    d2 = {}
    for ch in s:
        d1[ch] = d1.get(ch, 0) + 1
    for ch in t:
        d2[ch] = d2.get(ch, 0) + 1
    return d1 == d2
```


* 给定一个m*n的二维列表，查找一个数是否存在，列表有以下性质：
    * 每一行从左到右已经排好序
    * 每一行第一个数比上一行最后一个数大
* 实现1：遍历
```python
def searchMatrix(self, matrix, target)
    for line in matrix:
        if target in line:
            return True
    return False
```
* 实现2：二分查找
```
1. m*n指定了是等宽的列表
2. 宽度固定就可以直接计算二维数组下标，可以使用二分查找
```


* two sum问题：
    * 给定一个列表和一个整数，设计算法找到两个数的下标，使得两个数之和为给定的整数。保证肯定仅有一个结果。
        * 例如，列表[1,2,5,4]与目标整数3，1+2=3，结果为(0,1)。
        * 必须找两个数
        * 输入列表是无序的
    * 实现：
```
1. 无序的需要先排序
2. 只需查找当前值后面的值，正反顺序相加值相同，不必执行两次
```


* 数值分段问题
    * 排序后生成数值段数组，连续数字只显示首位数字
    * 输入：[404, 401, 403, 405, 407, 409, 408]
    * 输出：[[401], [403, 405], [407, 409]]
    * 解题思路：
        1. 排序方法中，选择排序每次从列表中选择最小值，方便集成此题检查连续数值的过程，但是复杂度并未降低。
        2. 使用O(log(n,2)n)复杂度的排序算法排序后，再遍历一次即可解决此题，复杂度比选择排序低。
    * 实现：
```python
def append_range(min_num, max_num, return_l):
    if max_num == min_num:
        return_l.append([min_num])
    else:
        return_l.append([min_num, max_num])

def sort(l: list):
    # 选择排序
    length = len(l)
    l.sort()
    return_l = []

    min_num = l[0]
    max_num = l[0]
    i = 1

    while i < length:
        if l[i] == max_num + 1:
            max_num = l[i]
        else:
            append_range(min_num, max_num, return_l)
            min_num = l[i]
            max_num = l[i]
        i += 1

    append_range(min_num, max_num, return_l)

    return return_l
```

算法进阶
----------
### 贪心算法、贪婪算法
* 概念：
    * 在对问题求解时，总是做出在当前看来最好的选择。不从整体上加以考虑。
* 理解：
    * 贪心算法是最自然智慧的算法
    * 用一种局部最功利的标准，总是作出在当前看来是最好的选择
    * 难点在于证明每次局部最功利的标准可以得到全局的最优解
    * 对于贪心算法的学习主要以增加阅历和经验为主
* 示例：
    * 例如每次只做当前看来最紧急的事情
    * 例如每次只选择最赚钱的项目
    * 例如只满足最高消费的用户群体
* 性质：
    * 不保证会得到最优解，但是在某些问题上贪心算法就是最优解。
* 效率：
    * 不使用贪心算法：O(2^n), n个项，选择or不选择
    * 使用贪心算法：O(n)

#### 找零问题
* 问题：
    * 假设老板需要找零n元钱，铅笔的面额有100，50，20，5，1。求解如何找零所需钱币数量最少。
* 解法：
    * 使用最大的面值开始找，找不开再选择较小面值，得出结果就是最优解。
* 实现：
```

```


#### 背包问题，backpack
* 问题：
    * 一个小偷在商店发现n个商品，第i个商品价值vi元，重wi千克。他希望拿走的价值尽量高，但最多只能容纳W千克东西。应该拿走哪些商品
    * 0-1背包：一个商品，要么只能完整拿走，要么留下。
    * 分数背包：一个商品，可以拿走任意一部分。
* 举例：
    * 商品1：v=60， w=10
    * 商品2：v=100， w=20
    * 商品3：v=120， w=30
    * 背包重量：W=50
* 解法：
    * 先计算单位商品价格
    * 按单位价格最高往包里装。
    * **分数背包是最优解**
    * **0-1背包不是最优解**
```python
# (价格，重量）
goods = [(60, 10), (100, 20), (120, 30)]
goods.sort(key=lambda x: x[0]/x[1], reverse=True)

def fractional_backpack(goods, w):
    m = [0 for _ in range(len(goods))]
    for i, (prize, weight) in enumerate(goods):
        if w >= weight:
            m[i] = 1
            w -= weight
        else:
            m[i] = w / weight
            w = 0
            break
    return m

print(fractional_backpack(goods, 50))
```

#### 拼接最大数字问题，number_join
* 问题：
    * 有n个非负整数，将其按照字符串拼接为一个整数，如何拼接可以得到最大的整数。
* 例题：
    * 32，94，128，1286，6，71可以拼接除的最大整数
* 解法：
    * 相同位数，大的放前面
    * 不同位数，字符串拼接后比较（拼接后长度相同）
    * x+y < y+x
```python
from functools import cmp_to_key

li = [32, 94, 128, 1286, 6, 71]

def xy_cmp(x, y):
    if x+y < y+x:
        return 1
    elif x+y > y+x:
        return -1
    else:
        return 0

def number_join(li):
    li = list(map(str, li))
    li.sort(key=cmp_to_key(xy_cmp))
    return ''.join(li)

print(number_join(li))
```

#### 活动选择问题，Activity Selection
* 问题：
    * 假设有n个活动，这些活动要占用同一个片场地，而场地在一个时刻只能供一个活动使用。
    * 每个活动占用时间段[si, fi)
    * 问：安排哪些活动能够使该场地举办的活动的个数最多？

| --   | ---  | ---  | ---  | ---  | ---  | ---  | ---- | ---- | ---- | ---- | ---- |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| i    | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   |
| si   | 1    | 3    | 0    | 5    | 3    | 5    | 6    | 8    | 8    | 2    | 12   |
| fi   | 4    | 5    | 6    | 7    | 9    | 9    | 10   | 11   | 12   | 14   | 16   |

* 解法：
    * 最先结束的活动一定是最优解的一部分
```python
activities = [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11), (8,12), (2,14), (12,16)]

activities.sort(key = lambda x:x[0])

def activity_selection(a):
    res = [a[0]]
    for i in range(1, len(a)):
        if a[i][0] >= res[-1][1]:
            res.append(a[i])

    return res

res = activity_selection(activities)
print(res)
print(len(res))
```

### 动态规划
* 概念：
    * 一个总问题可以分为多个子问题
    * 每个子问题都有最优解

#### 裴波那契函数

* 递归的斐波那契解法存在子问题的重复计算

```python
def fibnacci(n):
    if n <= 2:
        return 1
    else:
        return fibnacci(n-1) + fibnacci(n-2)

# 递归方式存在重复计算问题
# f(6) = f(5) + f(4)
# f(5) = f(4) + f(3)
# f(4) = f(3) + f(2)
# f(4) = f(3) + f(2) # f(4) 重复计算2次
# f(3) = f(2) + f(1)
# f(3) = f(2) + f(1)
# f(3) = f(2) + f(1) # f(3) 重复计算3次
    
def fibnacci_no_recurision(n):
    f = [0,1,1]

    if n > 2:
        for i in range(n-1):
            num = f[-1] + f[-2]
            f.append(num)
    return f[n]

# print(fibnacci(100))
print(fibnacci_no_recurision(100))
```

#### 钢条切割问题
* 问题：
    * 某公司出售钢条，出售价格与钢条长度之间的关系如下：
    * 长度i ：01, 02, 03, 04, 05, 06, 07, 08, 09, 10
    * 价格pi：01, 05, 08, 09, 10, 17, 17, 20, 24, 30
    * 现有一段长度为n的钢条，如何切割售价最高？
* 示例：
    * 从最小长度开始计算最优解：
    * 长度：  00, 01, 02, 03, 04, 05, 06, 07, 08, 09, 10
    * 最优：  00, 01, 05, 08, 10, 13, 17, 18, 22, 25, 30
    * 切割点：00, 01, 02, 03, 02, 02, 06, 01, 02, 03, 10
* 递推式：
    * 


#### 最长公共子序列，LCS
* 问题：
    * 给定两个序列X和Y，求X和Y长度最大的公共子序列。
* 例:
    * X="ABBCBDE" Y="DBBCDB" LCS(X, Y) = "BBCD"
* 应用场景：
    * 字符串相似度对比（模糊查询，模糊搜索）
* 解题思路
    * 

### 欧几里得算法（辗转相除法）
* 问题：
    * 12和16的最大公约数是4
* 应用：
    * 分数计算
* 示例：
```python
#!/usr/bin/env python3
# coding: utf-8
from __future__ import annotations
# <https://stackoverflow.com/questions/33533148/how-do-i-specify-that-the-return-type-of-a-method-is-the-same-as-the-class-itsel>
# python4：支持成员函数使用类自身作为type hint
# python3.7以上：使用`from __future__ import annotations`支持成员函数使用类自身作为type hint
# python3.7以下：使用字符串替代类支持成员函数使用类自身作为type hint

class RationalNum(object):
    def __init__(self, fenzi: int, fenmu: int):
        # 实现类的构造函数
        # 我们用 RationalNum(1,3) 来表示1/3，而不是0.333333

        self._fenzi = fenzi
        self._fenmu = fenmu
        self._gcd = self.gcd(self._fenzi, self._fenmu)
        self._fenzi = self._fenzi // self._gcd
        self._fenmu = self._fenmu // self._gcd

    @staticmethod
    def gcd(a: int, b: int) -> int:
        if a < b:
            a, b = b, a
        if b == 0:
            return a
        else:
            return RationalNum.gcd(b, a % b)

    @staticmethod
    def op(a, opt: str, b: RationalNum) -> RationalNum:
        # 实现两个RationalNum类instance（a和b）的加减乘除，并返回一个instance
        # opt in '+', '-', '*', '/'
        # 例如 a=RationalNum(1,2), b=RationalNum(1,3), opt='+', 
        # 输出 RationalNum(5,6)

        gcd = RationalNum.gcd(a._fenmu, b._fenmu)
        a_fenzi = a._fenzi * gcd
        a_fenmu = a._fenmu * gcd
        b_fenzi = b._fenzi * gcd
        b_fenmu = b._fenmu * gcd

        if opt=='+':
            return RationalNum(a_fenzi + b_fenzi, b_fenmu)
        elif opt=='-':
            return RationalNum(a_fenzi - b_fenzi, b_fenmu)
        elif opt=='*':
            return RationalNum(a._fenzi * b._fenzi, a._fenmu * b._fenmu)
        elif opt=='/':
            return RationalNum(a._fenzi * b._fenmu,  b._fenzi * a._fenmu)
        else:
            raise ValueError('opt error!')

    def equal(self, b: RationalNum) -> bool:
        # 判断self和给定实数类instance b，是否相等。
        # 例如 RationalNum(1,2) = RationalNum(2,4)
        return (self._fenzi == b._fenzi) and (self._fenmu == b._fenmu)

    def __add__(self, b: RationalNum) -> RationalNum:
        return RationalNum.op(self, '+', b)

    def __sub__(self, b: RationalNum) -> RationalNum:
        return RationalNum.op(self, '-', b)

    def __mul__(self, b: RationalNum) -> RationalNum:
        return RationalNum.op(self, '*', b)
    
    def __truediv__(self, b: RationalNum) -> RationalNum:
        return RationalNum.op(self, '/', b)

    def __eq__(self, b: RationalNum) -> RationalNum:
        return self.equal(b)

    def __str__(self) -> str:
        return f"{self._fenzi}/{self._fenmu}"

def test_rational_num() -> None:
    print(RationalNum(2, 4) == RationalNum(2, 4))
    print(RationalNum(2, 4) + RationalNum(2, 4))
    print(RationalNum(2, 4) - RationalNum(2, 4))
    print(RationalNum(2, 3) * RationalNum(2, 3))
    print(RationalNum(2, 3) / RationalNum(2, 3))

    print(RationalNum(2, 4) == RationalNum(1, 2))
    print(RationalNum(2, 3) + RationalNum(2, 4))
    print(RationalNum(3, 4) - RationalNum(1, 3))
    print(RationalNum(1, 4) * RationalNum(2, 1))
    print(RationalNum(6, 3) / RationalNum(2, 3))
```

### RSA算法
* 选取两个质数
* 无法解密的原因
    * 两个质数相乘容易
    * 大数拆分很难（无有效算法）
