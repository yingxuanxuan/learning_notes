---
coverY: 0
---

# asyncio



## aioconsole





#### 解决的问题

* 在python原生解释器中，无法直接执行异步函数
* 该库用于在命令行调试中执行异步函数，\`await func()\`，方便调试



#### 安装方法

* 注意，由于`aioconsole安装时会导入script`，命令行需要有管理员权限

<pre class="language-python"><code class="lang-python"># 直接使用pypi安装
<strong>pip3 install aioconsole
</strong>
# 使用软件包安装
python3 setup.py install
</code></pre>



#### 使用方法

```python
# 方法一，模块启动法
python -m aioconsole

# 方法二，脚本启动法

# 注意安装时需要有管理员权限
apython
```



