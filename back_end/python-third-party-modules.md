# Python third-party modules



## Pillow

* PIL（Python Imaging Library）是Python事实上的图像处理标准库，但是仅支持到Python2.7，无法维护。
* Pillow是第三方志愿者在PIL基础上维护的兼容版本。



### 安装

```
pip install pillow
```



### 操作图像



## requests
* urllib用起来比较麻烦，缺少高级功能
* requests用起来更简单，可以自动检测编码



### 安装

```
pip install requests
```



### 文档

<https://requests.readthedocs.io/en/master/>



* requests 示例

```
import requests

# 不带参数GET
r = requests.get('https://www.douban.com')
r.status_code # 200
r.text # '<!DOCTYPE HTML>\n<html>\n<head>\n...'

# 自动检测编码
r.encoding

# 带参数GET
r = requests.get('https://www.douban.com/search', params={'q': 'python', 'cat': '1001'})
r.url # 'https://www.douban.com/search?q=python&cat=1001'

# 自动解析json()
r = requests.get('https://json.com')
r.json() # {'query': {'count': 1}}

# 为request添加HTTP Header
r = request.get('https://www.douban.com', headers={'User-Agent': 'Mozilla/5.0 AppleWebKit'})
r.text

# POST（data部分默认为form, application/x-www-form-urlencoded）
r = request.post('https://accounts.douban.com/login', data={'form_email': 'abc@example.com', 'form_password': 123456})

# POST（JSON）
params = {'key': 'value'}
r = request.post(url, json=params) # 发送时会自动把dict序列化为json

# 上传文件，必须用'rb'二进制读取，这样获取的bytes长度才是文件的长度
upload_files = {'file': open('report.xls', 'rb')}
r = request.post(url, files=upload_files)

# requests对HTTP响应的头部信息自动进行解析
r.headers # {'Content-Type': 'text/html; charset=utf-8', 'Transger-Encoding': 'chunked', 'Content-Encoding': 'gzip', ...}
# 可以直接按dict key取值
r.headers['Content-Type'] # 'text/html; charset-utf-8'


# 携带Cookie
cs = {'token': '12345', 'status': 'working'
r = request.get(url, cookies=cs)

# 设置响应超时
r = requests.get(url, timeout=2.5) # 单位：秒
```

## chardet
* 不规范的网页无法获取编码类型
* 对位置编码bytes解码，需要收集各种编码特征字符，进行判断猜测
* chardet就是用来检测编码



### 安装

```
pip install chardet
```



* chardet示例

```
# 自动监测编码类型
chardet.detect(b'Hello, world!') # {'encoding': 'ascii', 'confidence': 1.0, 'language': ''}
'''
encoding: 编码类型
confidence：置信程度，1.0为100%
language: 语言，如中文
'''

# GBK检测
data = '人生苦短，我用python'.encode('gbk')
chardet.detect(data) # {'encoding': 'GB2312', 'confidence': 0.740740740, 'language': 'Chinese'}
# GBK是GB2312的超集，两者是同一种编码，检测正确的概率是74%

```



### 支持的编码类型

<https://chardet.readthedocs.io/en/latest/supported-encodings.html>



## psutil
* Linux下有ps、top、free等系统监控命令，python可以通过subprocess模块调用并获取结果，但需要解析文本
* psutil（process and system utilities）提供支持跨平台的系统运维工具



### 安装

```
pip install psutil
```



### 文档

<https://github.com/giampaolo/psutil>



### 获取CPU信息

```python
import psutil

# 逻辑cpu数量
psutil.cpu_count()

# 物理核心个数
psutil.cpu_count(logical=False)

# cpu主频
psutil.cpu_freq(percpu=False)

# cpu占用率
psutil.cpu_percent(interval=None, percpu=Flase)

# 循环显示cpu占用率
# interval=1，显示间隔
# percpu=True，分别显示各cpu逻辑核使用占比
for x in range(10):
    psutil.cpu_percent(interval=1, percpu=True)

# 统计CPU用户/系统/空闲时间
psutil.cpu_times() 

# 状态统计
psutil.cpu_stats()

# 负载均衡
psutil.getloadagv()
```



### 内存信息

```python
# 查询虚拟内存
psutil.virtual_memory() 

# 返回svmem对象
svmem(total=8589934592, available=2866520064, percent=66.6, used=7201386496, free=216178688, active=3342192640, inactive=2650341376, wired=1208852480)

# 查询交换内存
psutil.swap_memory() 

# 返回sswap对象
sswap(total=1073741824, used=150732800, free=923009024, percent=14.0, sin=10705981440, sout=40353792)
```



### 获取磁盘信息

```python
# 磁盘分区信息
psutil.disk_partitions(all=False)

# windows下返回对象示例
[sdiskpart(device='C:\\', mountpoint='C:\\', fstype='NTFS', opts='rw,fixed'), sdiskpart(device='D:\\', mountpoint='D:\\', fstype='NTFS', opts='rw,fixed'), sdiskpart(device='E:\\', mountpoint='E:\\', fstype='NTFS', opts='rw,fixed')]

# 查看分区使用率
psutil.disk_usage('E:\\') # 双斜杠为转译斜杠
psutil.disk_usage('E:')

# 返回对象
sdiskusage(total=120030490624, used=108329594880, free=11700895744, percent=90.3)

# 物理磁盘io统计
psutil.disk_io_counters(perdisk=False, nowrap=True)

# 返回对象
sdiskio(read_count=886693, write_count=421645, read_bytes=20292710400, write_bytes=6467171840, read_time=543, write_time=677)

# 注意
# windows下可能需要先执行 `diskperf -y`
```



### 获取网络信息

```python
# 获取网络读写字节／包的个数
psutil.net_io_counters(pernic=False, nowrap=True)

snetio(bytes_sent=3885744870, bytes_recv=10357676702, packets_sent=10613069, packets_recv=10423357, errin=0, errout=0, dropin=0, dropout=0)

# 获取网络接口信息
psutil.net_if_addrs()
{
  'lo0': [snic(family=<AddressFamily.AF_INET: 2>, address='127.0.0.1', netmask='255.0.0.0'), ...],
  'en1': [snic(family=<AddressFamily.AF_INET: 2>, address='10.0.1.80', netmask='255.255.255.0'), ...],
  'en0': [...],
  'en2': [...],
  'bridge0': [...]
}

# 获取网络接口状态
psutil.net_if_stats()

{
  'lo0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=16384),
  'en0': snicstats(isup=True, duplex=<NicDuplex.NIC_DUPLEX_UNKNOWN: 0>, speed=0, mtu=1500),
  'en1': snicstats(...),
  'en2': snicstats(...),
  'bridge0': snicstats(...)
}

# 获取当前所有网络连接信息
psutil.net_connections(kind='inet')

[
    sconn(fd=83, family=<AddressFamily.AF_INET6: 30>, type=1, laddr=addr(ip='::127.0.0.1', port=62911), raddr=addr(ip='::127.0.0.1', port=3306), status='ESTABLISHED', pid=3725),
]

# 按类型查询连接
"inet" # IPv4 and IPv6
"inet4" # IPv4
"inet6" # IPv6
"tcp"
"tcp4"
"tcp6"
"udp"
"udp4"
"udp6"
"unix"
"all"
```



### 其他系统信息

```python
# 启动时间
psutil.boot_time() # 1389563460.0

# 解析timestamp
dt = datetime.datetime.fromtimestamp(psutil.boot_time())
dt.strftime("%Y-%m-%d %H:%M:%S") # '2014-01-12 22:51:00'

# 用户
psutil.users()

[suser(name='giampaolo', terminal='pts/2', host='localhost', started=1340737536.0, pid=1352),
 suser(name='giampaolo', terminal='pts/3', host='localhost', started=1340737792.0, pid=1788)]
```



### 进程信息

```python
# 当前所有pid
psutil.pids()

# 判断pid是否存在
psutil.pid_exists(pid)

# 迭代所有进程（attrs可配置属性）
psutil.process_iter(attrs=None, ad_value=None)

# 构造属性列表
for proc in psutil.process_iter(['pid', 'name', 'username']):
    print(proc.info)

{'name': 'systemd', 'pid': 1, 'username': 'root'}
{'name': 'kthreadd', 'pid': 2, 'username': 'root'}
{'name': 'ksoftirqd/0', 'pid': 3, 'username': 'root'}

# 构造属性字典
procs = {p.pid: p.info for p in psutil.process_iter(['name', 'username'])}

{1: {'name': 'systemd', 'username': 'root'},
 2: {'name': 'kthreadd', 'username': 'root'},
 3: {'name': 'ksoftirqd/0', 'username': 'root'},
 ...}

# 等待进程结束
# 杀死并等待进程结束示例
def on_terminate(proc):
    print('{} terminated with exit code {}'.format(proc, proc.returncode))

procs = psutil.Process().children()
for p in procs:
	p.terminate()
gone, alive = psutil.wait_procs(procs, timeout=3, callback=on_terminate)

# ps命令效果
psutil.test()

"""
USER         PID  %MEM     VSZ     RSS  NICE STATUS  START   TIME  CMDLINE
SYSTEM         0   0.0   60.0K    8.0K        runni         55:02  System Idle Process
SYSTEM         4   0.0  196.0K  148.0K        runni         04:44  System
             120   0.4    5.7M   67.0M        runni  17:57  00:00  Registry
             424   0.0    1.2M    1.2M        runni  17:57  00:00  smss.exe
yingxuanx    448   0.4   33.8M   59.4M    32  runni  19:55  00:10  C:\Program Files (x86)\Google\Chrome\Application\chro
             468   0.1    2.9M   11.8M        runni  17:57  00:00  winlogon.exe
             644   0.0    1.7M    5.1M        runni  17:57  00:02  csrss.exe
"""

# 进程对象示例
>>> p = psutil.Process(3776) # 获取指定进程ID=3776，其实就是当前Python交互环境
>>> p.name() # 进程名称
'python3.6'
>>> p.exe() # 进程exe路径
'/Users/michael/anaconda3/bin/python3.6'
>>> p.cwd() # 进程工作目录
'/Users/michael'
>>> p.cmdline() # 进程启动的命令行
['python3']
>>> p.ppid() # 父进程ID
3765
>>> p.parent() # 父进程
<psutil.Process(pid=3765, name='bash') at 4503144040>
>>> p.children() # 子进程列表
[]
>>> p.status() # 进程状态
'running'
>>> p.username() # 进程用户名
'michael'
>>> p.create_time() # 进程创建时间
1511052731.120333
>>> p.terminal() # 进程终端
'/dev/ttys002'
>>> p.cpu_times() # 进程使用的CPU时间
pcputimes(user=0.081150144, system=0.053269812, children_user=0.0, children_system=0.0)
>>> p.memory_info() # 进程使用的内存
pmem(rss=8310784, vms=2481725440, pfaults=3207, pageins=18)
>>> p.open_files() # 进程打开的文件
[]
>>> p.connections() # 进程相关网络连接
[]
>>> p.num_threads() # 进程的线程数量
1
>>> p.threads() # 所有线程信息
[pthread(id=1, user_time=0.090318, system_time=0.062736)]
>>> p.environ() # 进程环境变量
{'SHELL': '/bin/bash', 'PATH': '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:...', 'PWD': '/Users/michael', 'LANG': 'zh_CN.UTF-8', ...}
>>> p.terminate() # 结束进程
Terminated: 15 <-- 自己把自己结束了
```



### 编程示例



#### 完全匹配查找进程，进程名、程序路径、启动命令

```python
import psutil
import os

def find_procs_by_name(name):
	ls = []
	for p in psutil.process_iter(['name']):
		if p.info['name'] == name or \
         p.info['exe'] and os.path.basename(p.info['exe']) == name or \
         p.info['cmdline'] and p.info['cmdline'][0] == name:
			ls.append(p)
	return ls
```



#### 按pid杀进程树

```python
import os
import signal
import psutil

def kill_proc_tree(pid, sig=signal.SIGTERM, include_parent=True, timeout=None, on_terminate=None):
    assert pid != os.getpid(), "won't kill myself"
    parent = psutil.Process(pid)
    children = parent.children(recursive=True)
    if include_parent:
        children.append(parent)
    for p in children:
        try:
            p.send_signal(sig)
        except psutil.NoSuchProcess:
            pass
    gone, alive = psutil.wait_procs(children, timeout=timeout, callback=on_terminate)
```



#### 进程筛选、排序

```python
# 获取当前用户的进程
import getpass # 获取用户名或者密码
cur_user = getpass.getuser()
all_procs = psutil.process_iter(['name', 'username'])
cur_user_procs = [(p.pid, p.info['name']) for p in all_procs if p.info['username'] == cur_user]

# 按状态过滤进程
[(p.pid, p.info) for p in psutil.process_iter(['name', 'status']) if p.info['status'] == psutil.STATUS_RUNNING]

# 按打开文件（日志）过滤进程
for p in psutil.process_iter(['name', 'open_files']):
     for file in p.info['open_files'] or []:
         if file.path.endswith('.log'):
              print("%-5s %-10s %s" % (p.pid, p.info['name'][:10], file.path))

# 按使用内存用量过滤进程
[(p.pid, p.info['name'], p.info['memory_info'].rss) for p in psutil.process_iter(['name', 'memory_info']) if p.info['memory_info'].rss > 500 * 1024 * 1024]

# 按CPU使用率过滤进程
[(p.pid, p.info['name'], sum(p.info['cpu_times'])) for p in sorted(psutil.process_iter(['name', 'cpu_times']), key=lambda p: sum(p.info['cpu_times'][:2]))][-3:]
```



#### 人易读的动态数据单位

```python
def bytes2human(n):
    symbols = ('K', 'M', 'G', 'T', 'P', 'E', 'Z', 'Y')
    prefix = {}
    for i, s in enumerate(symbols):
        prefix[s] = 1 << (i + 1) * 10 # 1 << ((i+1)*10)
    for s in reversed(symbols):
        if n >= prefix[s]:
            value = float(n) / prefix[s]
            return '%.1f%s' % (value, s)
    return "%sB" % n

usage_bytes = psutil.disk_usage('/').total
usage_human = bytes2human(usage_bytes)
```





## aiohttp


### doc

doc: <http://aiohttp.readthedocs.io/en/stable/>



### 安装

``` sh
pip install aiohttp
pip install aiodns
pip install cchardet (optional)
```



### 客户端示例

``` py
import asyncio
import aiohttp

async def fetch_page(session, url):
    with aiohttp.Timeout(10):
        async with session.get(url) as response:
            if 200==response.status:
                return await response.read()
            else
                return None

loop = asyncio.get_event_loop()
with aiohttp.ClientSession(loop=loop) as session:
    content = loop.run_until_complete(fetch_page(session, 'http://www.capefearit.com/'))
    print(content)
```



### 服务器示例

``` py
from aiohttp import web
import asyncio

async def handle(request):
    name = request.match_info.get('name', 'Anonymous')
    text = 'Hello, ' + name
    return web.Response(body = text.encode('utf-8'))

async def init(loop):
    app = web.Application(loop=loop)
    app.router.add_route('GET', '/{name}', handle)
    srv = await loop.create_server(app.make_handler(), '', 8888)
    print('server started at http://0.0.0.0:8888....')
    return srv

loop = asyncio.get_event_loop()
loop.run_until_complete(init(loop))
loop.run_forever()
```



## fabric



### 安装

```
pip install fabric
pip3 install fabric3（不支持windows）
```



### 默认读取指令文件

fabfile.py



### 示例

```
from fabric.api import run
def host_type():
    run('uname -s')
$fab -H host1, host2 host_type
```



### 带参数任务

```
def hello(name):
    print("Hello %s!" % name)
$fab hello:name=world
$fab hello:world
```



### 交互

默认允许交互



### 基本命令
```
cd #远程cd
lcd #本地cd
run #远程执行
local #本地执行
put #上传文件
```



### 全局环境变量

#### 引用
```
fabric.state.env
fabric.api.env
```



#### 重要env

```
env.hosts #主机列表
env.user #默认使用本地用户名，统一指定用户名
env.password #主机密码或sudo密码
env.port #指定端口
env.sudo_user
env.use_ssh_config = False #使用本地ssh配置
env.ssh_config_path = ~/.ssh/config #默认ssh配置路径
env.passwords #指定主机密码对{'username@hostname:port':'password'}
env.key_filename #指定sshkey
env.parallel = False #并行化
env.timeout = 10 #连接等待时间
env.connection_attempt = 1 #连接尝试次数
env.skip_bad_hosts = False #坏的连接中断任务
```



#### 临时设置env

```
with settings(env.key=value):
    command
```



#### 任务间数据共享

```
env.key = value
```



#### Execution strategy 执行策略

通过fab的参数顺序执行任务
每个任务按host列表顺序执行
没有给出host的任务在本地执行



#### Roles 角色
```
env.roledefs['webservers'] = ['www1', 'www2', 'www3']
env.roledefs = {'web'：['www1','www2','www3'], 'dns':['ns1', 'ns2']}
env.roledefs = {
    'web':{
        'hosts':[],
        'foo':'bar'
    }
    'dns':{
        'hosts':[],
        'foo':'baz'
    }
}
```



#### 全局Hosts

```
方法一：覆盖方法三
env.hosts=['host1', 'host2']
方法二：
def set_hosts():
    env.hosts = ['host1', 'host2']
fab set_hosts,task
方法三：
fab -H host1, host2 task
fab -R role task
fab --hosts host1 task
fab --roles role task
方法四：
与命令行host list相加
env.hosts.extend(['host3','host4'])
```



#### 任务特定hosts：

```
方法一：
fab mytask:hosts="host1;host2"
fab mytask:roles="role1"
方法二：覆盖env，但不覆盖方法一
from fabric.api import hosts
@host('host1', 'host2')
@roles('role1')
```



#### 排除特定hosts：

fab -x/--exclude-hosts



#### python执行任务

from fabric.api import execute
execute(task)
execute(task, hosts=hostlist)



#### 错误处理

from fabric.api import local, settings, abort
from fabric.crontrib.console import confirm
def test():
    with settings(warn_only=True):
        result = local('command', capture=True)
    if result.failed and not confirm('continue?'):
        abort('abort')



## sqlalchemy



### 安装

pip install sqlalchemy



### 驱动



#### pymysql

pip install pymysql
'mysql+pymysql://'



#### mysql-python 封装c接口（sqlalchemy默认）

``` sh
# 需要编译
pip install MySQL-python
```

``` sh
# 直接下载 http://www.lfd.uci.edu/~gohlke/pythonlibs/#mysql-python
pip install wheel
pip install mysqlclient-1.3.8-cp36-cp36m-win_amd64.whl
pip install mysqlclient-1.3.8-cp27-cp27m-win_amd64.whl
```

'mysql://' # 默认
'mysql+mysqldb://'



#### mysql-connector （mysql官方）
https://dev.mysql.com/get/Downloads/Connector-Python/mysql-connector-python-2.1.3-py2.7-winx64.msi
'mysql+mysqlconnector'



* Sqlalchemy示例

``` py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy import Column
from sqlalchemy.types import String, Integer
from sqlalchemy.ext.declarative import declarative_base

engine = create_engine('postgresql://test@localhost:5432/test')
DBSession = sessionmaker(engine)
session = DBSession()

BaseModel = declarative_base()

class User(BaseModel):
    __tablename__ = 'user'
    
    id = Column(String, primary_key=True)
    username = Column(String, index=True)

class Session(BaseModel):
    __tablename__ = 'session'
    
    id = Column(String, primary_key=True)
    user = Column(String, index=True)
    ip = Column(String)
    
query = session.query(Session, User.username).join(User, User.id == Session.user)
for i in query:
    print dir(i)
```



## aiomysql



## aiopg



## watchdoc 文件消息监控



## 创建windows服务



## python-pptx



### 简介

* 安装：`pip3 install python-pptx`
* 文档：<https://python-pptx.readthedocs.io/en/latest/index.html>
* 示例：<https://python-pptx.readthedocs.org/en/latest/user/quickstart.html>
* 功能：
    * 解析pptx文件及XML
    * 修改pptx文件，不支持ppt文件
    * 添加页面 slides
    * 添加图片 image
    * 添加文本框 textbox
    * 添加表格 table
    * 添加 autoshape 如多边形、箭头
    * 添加图标
* 长度单位，English Metric Units(EMU)
    * Inches(1) = 914400
    * 914400可以被许多因数整除，可以在厘米、像素之间整数转换
    * `from pptx.util import Inches, Pt`
    * `length = Inches(1)`
    * `length # 914400`
    * `length.inches # 1.0`
    * `length.cm # 2.54`
    * `length.pt # 72.0`



### pptx文档结构、概念：

* Presentation，根，ppt对象
* prs.slide_layouts，页面模板列表，必须使用slide layout创建slide
    * 常用6号模板，空白，由于该值
    * slide_layouts[0]: Title (presentation title slide)
    * slide_layouts[1] Title and Content
    * slide_layouts[2] Section Header (sometimes called Segue)
    * slide_layouts[3] Two Content (side by side bullet textboxes)
    * slide_layouts[4] Comparison (same but additional title for each side by side content box)
    * slide_layouts[5] Title Only
    * slide_layouts[6] Blank
    * slide_layouts[7] Content with Caption
    * slide_layouts[8] Picture with Caption
* SlideShapes，页面列表，保存所有页面
    * Presentation自动创建slides
    * slide中的一切元素都是shape，除了slide background
    * `SlideShapes.add_chart(chart_type, x, y, cx, cy, chart_data)`
    * `SlideShapes.add_connector(connector_type, begin_x, begin_y, end_x, end_y)`
    * 
* Shape，slide中的元素，保存在`slide.shapes`中树形嵌套结构
    * auto shape，可以带边框，可以填充
        * textbox
        * rectangle，长方形
        * ellips，椭圆形
        * block arrow，箭头
        * ...
    * picture
    * graphic frame，可以容纳table表格、chart图表、smart art diagram智能流程图、media clip多媒体
    * group shape，shape组合
    * line/connector，直线，链接
    * content part，嵌入外部XML、SVG
* auto shape，自动形状，`MSO_SHAPE`，`MSO_AUTO_SHAPE_TYPE`
    * 所有形状枚举：<https://python-pptx.readthedocs.io/en/latest/api/enum/MsoAutoShapeType.html#msoautoshapetype>
    * 创建方法`slide.shapes.add_shape(MSO_SHAPE.RECTANGLE, left, top, width, height)`
    * 快接文本块`slide.shapes.add_textbox(left, top, width, height)`
* Placeholder, 预定义的shape类型
    * <https://python-pptx.readthedocs.io/en/latest/api/placeholders.html#layoutplaceholder-objects>
    * Title, Center Title, Subtitle, Body
    * Content
    * Picture, Clip Art
    * Chart, Talbe, Smart
    * Media Clip
    * Data, Footer, Slide Number
    * Header
    * Vertical Body, Vertical Object, Vertical Title
* text_frame
    * 每个Shape内部文本由text_frame管理
        * `shape.has_text_fame`
        * `shape.text_frame`
    * text_frame管理一个paragraph的列表
        * `text_frame.paragraphs[0]`
        * `p = text_frame.add_paragraph()`
    * paragraph管理一个run列表
        * `paragraph.runs[0]`
        * `paragraph.add_run()`
        * `paragraph.level控制段落层级`
        * `paragraph.font`
    * run
        * `shape.text`, `text_frame.text`, `paragraphs.text`都是对`run.text`的快捷方式
        * run可以针对单个字符设置样式
        * 只有run可以真正保存文本`run.text`
        * `run.font`
        * `run.hyperlink`



### 示例

```
from pptx import Presentation

# ppt对象，可以从文件创建
prs = Presentation()

# 0号模板，内容为一个主标题，一个副标题
title_slide_layout = prs.slide_layouts[0]

# 从模板创建一个页面
slide = prs.slides.add_slide(title_slide_layout)
slide.shapes.title.text = 'Hello, World!'
slide.placeholders[1].text = 'python-pptx was here!'

# 保存文件
prs.save('name.pptx')
```

```
from collections import namedtuple
from pptx import Presentation
from pptx.util import Pt, Cm, Emu, Mm, Inches
from pptx.dml.color import ColorFormat, RGBColor
from pptx.enum.text import MSO_ANCHOR, MSO_AUTO_SIZE, PP_ALIGN

INPUT = 'input.txt'
OUTPUT = 'out.pptx'

SLIDE_SIZE = namedtuple('SLIDE_SIZE', ['W', 'H'])
SLIDEA4 = SLIDE_SIZE(Cm(27.51), Cm(19.05))

def set_text_style(textbox):
    frame = textbox.text_frame
    frame.vertical_anchor = MSO_ANCHOR.MIDDLE
    frame.auto_size = MSO_AUTO_SIZE.TEXT_TO_FIT_SHAPE
    frame.word_wrap = False

    run = frame.paragraphs[0].runs[0]
    run.font.bold = True
    run.font.italic = None
    run.font.name = 'MS Mincho'

def new_slide(prs, spell, soundmark, tone):

    # 幻灯片类型, 6号为空页面
    blank_layout = prs.slide_layouts[6]

    # 添加幻灯片
    slide = prs.slides.add_slide(blank_layout)

    # textbox1 align
    left = SLIDEA4.W * 0.05
    top =  SLIDEA4.H * 0.05
    width = SLIDEA4.W * 0.8
    height = SLIDEA4.H * 0.35
    textbox1 = slide.shapes.add_textbox(left, top, width, height)

    # textbox2
    left = SLIDEA4.W * 0.80
    top =  SLIDEA4.H * 0.05
    width = SLIDEA4.W * 0.15
    height = SLIDEA4.H * 0.35
    textbox2 = slide.shapes.add_textbox(left, top, width, height)

    # textbox3
    left = SLIDEA4.W * 0.05
    top =  SLIDEA4.H * 0.4
    width = SLIDEA4.W * 0.9
    height = SLIDEA4.H * 0.55
    textbox3 = slide.shapes.add_textbox(left, top, width, height)

    # 设置内容，自动添加run[0]
    # 以下都实际指向 textbox1.text_frame.paragraphs[0].run[0].text
    # textbox1.text_frame.text
    # textbox1.text_frame.paragraphs[0].text
    textbox1.text_frame.text = soundmark
    textbox2.text_frame.text = tone
    textbox3.text_frame.text = spell

    # text style
    set_text_style(textbox1)
    set_text_style(textbox2)
    set_text_style(textbox3)

    textbox1.text_frame.paragraphs[0].font.size = Pt(100)
    textbox1.text_frame.paragraphs[0].alignment = PP_ALIGN.CENTER

    textbox2.text_frame.paragraphs[0].font.size = Pt(150)
    textbox2.text_frame.paragraphs[0].font.color.rgb = RGBColor(255, 0, 0)
    textbox2.text_frame.paragraphs[0].alignment = PP_ALIGN.LEFT
    
    textbox3.text_frame.paragraphs[0].font.size = Pt(150)
    textbox3.text_frame.paragraphs[0].alignment = PP_ALIGN.CENTER

def run():
    # 文件所有幻灯片长宽
    prs = Presentation()
    prs.slide_height = SLIDEA4.H
    prs.slide_width = SLIDEA4.W

    lines = None
    try:
        with open('./in.txt', 'r', encoding='utf-8') as f:
            lines = f.readlines()
    except Exception:
        print('文件{0}读取失败'.format(INPUT))
        return -1

    if lines:
        for line in lines:
            items = line.split()
            if len(items) == 3:
                new_slide(prs, *items)
            else:
                print('Error items: {}'.format(len(items)))
                print('Data :' + ' '.join(items))

    prs.save(OUTPUT)

if __name__ == "__main__":
    run()
```



## nginx+supervisord多进程部署



### Supervisord 四进程配置（端口14001 - 14004）

```ini
[program:myprogram] 
process_name=MYPROGRAM%(process_num)s
directory=/var/www/apps/myapp 
command=/var/www/apps/myapp/virtualenv/bin/python index.py --PORT=%(process_num)s
startsecs=2
user=youruser
stdout_logfile=/var/log/myapp/out-%(process_num)s.log
stderr_logfile=/var/log/myapp/err-%(process_num)s.log
numprocs=4
numprocs_start=14000

# 说明：
# numproces # 进程数（一般为内核数的2倍）
# numprocs_start # 起始进程号
# --PORT=%(process_num)s # 程序参数
```



### Nginx多后端进程配置

```ini
upstream myappbackend {
    server 127.0.0.1:14001 max_fails=3 fail_timeout=1s;
    server 127.0.0.1:14002 max_fails=3 fail_timeout=1s;
    server 127.0.0.1:14003 max_fails=3 fail_timeout=1s;
    server 127.0.0.1:14004 max_fails=3 fail_timeout=1s;
}

server {
    listen 4.5.6.7:80;
    server_name example.com;
    access_log /var/log/nginx/myapp.log main;
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-Ip $remote_addr;
        proxy_pass http://myappbackend/;
    }
}      
```



### supervisor virtualenv配置

```ini
[program:diasporamas]
command=/var/www/django/bin/gunicorn_django
directory=/var/www/django/django_test
environment=PATH="/var/www/django/bin"
```