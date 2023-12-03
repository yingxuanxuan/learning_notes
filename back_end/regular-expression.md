# regular expression



## 正则表达式

```python
\d # 数字  
\w # 数字orWord  
\s # 空白字符(空格，Tab)  
\- # 转译-  
\, # 转译,  
\; # 转译;  
. # 任意字符  
* # 0-n个字符  
? # 0-1个字符  
+ # 1-n个字符  
{n} # n个字符  
{n,m} # n-m个字符 * 逗号后面不能有空格  
{n,} # n到无线个  
[0-9a-zA-Z] # 范围内所有字符  
[a|A] # a或A  
^ # 行首  
$ # 行尾  
r'' # 正则表达式字符串  
```


```python
import re
re.match(r'') # 匹配则返回Match对象  
re.compile(r'').match() # 编译
re.split(r'\s+\,\;', 'a b  c') # 切分字符串
m = re.match(r'(1)(2)(3)', 'rawstr') # 括号分组
m.group(0) = rawstr
m.group(1) = 1
m.group(2) = 2
m.group(3) = 3
m.groups() = ('1', '2', '3')
```


```python
+ # 默认贪婪匹配  
+? # 非贪婪匹配   
```



### match fullmatch search

* match 只匹配开头，后面有其他字符也匹配
* search 搜索整个字符串，前后有其他字符也匹配
* fullmatch 必须全部字符串匹配



