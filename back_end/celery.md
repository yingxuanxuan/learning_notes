# Celery

## 入门

### 入门示例

* 需求
  * 完成任务

* 步骤
  * 安装python，略
  * 安装celery
  * 使用推荐Broker Rabbitmq
  * 编写任务
  * 启动任务Worker
  * 发布任务



* 安装celery

```sh
pip install celery
```



* docker安装rabbitmq

```sh
docker run --name rabbitmq -p 5672:5672 -p 15672:15672 -d --restart=unless-stopped rabbitmq:3-management
```



* 任务

```py
# task.py

from celery import Celery

broker_url = 'amqp://guest:guest@192.168.4.101:5672/'
app = Celery('tasks', broker=broker_url)

@app.task
def add(x, y):
    return x + y
```



* 启动Worker

```sh
# celery -A 文件名 worker --loglevel=INFO
celery -A tasks worker --loglevel=INFO
```



* 发布任务

```sh
# publisher.py

from task import add
add.delay(4, 4)
```



* windows不支持celery，启动worker时需要使用eventlet或gevent

```sh
# eventlet
pip install eventlet
celery -A  worker -l info -P eventlet

# gevent
pip install gevent
celery -A  worker -l info -P gevent
```



* worker中显示结果

```
[2023-09-25 16:15:39,595: INFO/MainProcess] Task task.add[645f6a7a-ddeb-40dd-948c-0bc01ee14c11] received
[2023-09-25 16:15:39,616: INFO/MainProcess] Task task.add[645f6a7a-ddeb-40dd-948c-0bc01ee14c11] succeeded in 0.03200000000651926s: 8
```



### 后端使用示例

* 步骤
  * 以入门示例为基础
  * 修改任务配置
  * 修改后端结果获取代码
  * 启动Worker
  * 发布任务



* 任务代码


```py
# task.py

from celery import Celery

broker_url = 'amqp://guest:guest@192.168.4.101:5672/'

# 使用rabbitmq的rpc模式
backend_url = 'rpc://'

app = Celery('tasks', broker=broker_url, backend=backend_url)

@app.task
def add(x, y):
    return x + y
```



* 发布任务


```py
# publisher.py

from task import add
result = add.delay(4, 4)

# get，阻塞等待结果
# timeout，阻塞超时时间
# propagate，传播异常

print(result.get(propagate=True, timeout=5))
```



### 垃圾评论过滤

* 需求
  * 收到评论的同时，生成检查评论任务
  * 通过开源API确认后，标记垃圾评论



* 生成任务

```py
from django import forms
from django.http import HttpResponseRedirect
from django.template.context import RequestContext
from django.shortcuts import get_object_or_404, render_to_response

from blog import tasks
from blog.models import Comment


class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment


def add_comment(request, slug, template_name='comments/create.html'):
    post = get_object_or_404(Entry, slug=slug)
    remote_addr = request.META.get('REMOTE_ADDR')

    if request.method == 'post':
        form = CommentForm(request.POST, request.FILES)
        if form.is_valid():
            comment = form.save()
            # Check spam asynchronously.
            tasks.spam_filter.delay(comment_id=comment.id,
                                    remote_addr=remote_addr)
            return HttpResponseRedirect(post.get_absolute_url())
    else:
        form = CommentForm()

    context = RequestContext(request, {'form': form})
    return render_to_response(template_name, context_instance=context)
```



* 任务

```py
from celery import Celery

from akismet import Akismet

from django.core.exceptions import ImproperlyConfigured
from django.contrib.sites.models import Site

from blog.models import Comment


app = Celery(broker='amqp://')


@app.task
def spam_filter(comment_id, remote_addr=None):
    logger = spam_filter.get_logger()
    logger.info('Running spam filter for comment %s', comment_id)

    comment = Comment.objects.get(pk=comment_id)
    current_domain = Site.objects.get_current().domain
    akismet = Akismet(settings.AKISMET_KEY, 'http://{0}'.format(domain))
    if not akismet.verify_key():
        raise ImproperlyConfigured('Invalid AKISMET_KEY')


    is_spam = akismet.comment_check(user_ip=remote_addr,
                        comment_content=comment.comment,
                        comment_author=comment.name,
                        comment_author_email=comment.email_address)
    if is_spam:
        comment.is_spam = True
        comment.save()

    return is_spam
```



## 中介，Broker

* 概念
  * broker，经纪人，中介
  * 生产者消费者模型中的队列



* Celery原生支持的broker
  * rabbitmq
  * redis
  * AWS SQS
  * sqlalchemy，使用数据库作为broker



* broker建议

| **Name**     | **Status**   | **Monitoring** | **Remote Control** |
| ------------ | ------------ | -------------- | ------------------ |
| ==RabbitMQ== | Stable       | Yes            | Yes                |
| *Redis*      | Stable       | Yes            | Yes                |
| *Amazon SQS* | Stable       | No             | No                 |
| *Zookeeper*  | Experimental | No             | No                 |



### rabbitmq

* 默认连接格式
* 无需安装依赖包

```py
# 格式
# broker_url = 'amqp://myuser:mypassword@localhost:5672/myvhost'

```





### redis

* 需要安装依赖

```py
# 安装
# pip install -U "celery[redis]"

# 格式
# redis://:password@hostname:port/db_number

app.conf.broker_url = 'redis://localhost:6379/0'

```



* 哨兵连接



* 集群连接





### sqlalchemy

* 不建议，不稳定，官方不支持，文档已删除

```py
# sqlite (filename)
BROKER_URL = 'sqla+sqlite:///celerydb.sqlite'
CELERY_BROKER_URL = "sqla+sqlite:///" + os.path.join(BASE_DIR, 'your_database.db')

# mysql
BROKER_URL = 'sqla+mysql://scott:tiger@localhost/foo'

# postgresql
BROKER_URL = 'sqla+postgresql://user:pwd@127.0.0.1:5432/dbname'
BROKER_URL = 'sqla+postgresql://scott:tiger@localhost/mydatabase'
BROKER_URL = 'sqlalchemy+postgresql://postgres:passwd@127.0.0.1:5432/postgres'

# oracle
BROKER_URL = 'sqla+oracle://scott:tiger@127.0.0.1:1521/sidname'
```



### django ORM

* 依赖kombu

```sh
broker = 'django://'

# 需要配置django app
INSTALLED_APPS = ('kombu.transport.django', )

# 需要在django-admin中配置数据迁移
python manage.py migrate kombu_transport_django
```



## 后端，Backend

* 内置后端
  * SQLAlchemy，使用数据库ORM
  * Django ORM，使用数据库ORM
  * Memcached
  * Redis
  * RabbitMQ/QPid ( rpc），发送消息



* 其他支持的后端
  * MongoDB
  * Cassandra
  * Elasticsearch
  * IronCache
  * Couchbase
  * ArangoDB
  * CouchDB
  * 文件
  * s3



* 消费结果
  * 不消费掉结果会导致一直膨胀无法老化
  * `AsyncResult.get()`
  * `AsyncResult.forget()` 



### SQLAlchemy

```py
# 格式
# result_backend = 'db+scheme://user:password@host:port/dbname'

# 连接示例

# sqlite (filename)
result_backend = 'db+sqlite:///results.sqlite'

# mysql
result_backend = 'db+mysql://scott:tiger@localhost/foo'

# postgresql
result_backend = 'db+postgresql://scott:tiger@localhost/mydatabase'

# oracle
result_backend = 'db+oracle://scott:tiger@127.0.0.1:1521/sidname'
```



### redis

* 需要安装redis库

```py
# 安装
pip install celery[redis]

# 格式
result_backend = 'redis://username:password@host:port/db'

# 默认
result_backend = 'redis://localhost/0'

# 默认
result_backend = 'redis://'

```



### Memcached



### rpc

```py
```



### django

* django-celery-result，使用 Django ORM 或 Django Cache 框架提供结果后端



## 工作进程，Worker

- worker模式
  - Celery worker是一个执行异步任务的进程，它可以使用不同的并发模式来处理任务，例如多进程（prefork）或协程（eventlet、gevent）
  - Celery worker的默认并发模式是prefork，也就是多进程的方式，这种方式可以利用多核CPU的优势，但也会占用更多的内存
  - Celery worker的默认并发数是根据所在机器的CPU数量来决定的，如果不指定-c参数，那么worker会启动和CPU数量相同的子进程。我们也可以通过-c参数来指定启动worker的进程数，例如`celery -A tasks worker -c 4`表示启动4个子进程



- 多进程
  - 一个Worker可以是一个进程，也可以是多个进程
  - 当一个Worker有多个进程时，有一个进程负责接收任务，其他进程负责执行任务




- eventlet对比gevent
  - 相同点
    - eventlet和gevent都是Python的协程库，它们可以让你用同步的方式写异步的代码，提高程序的性能和并发能力
  - 区别：
    - eventlet是基于greenlet的，它定义了一个独立的反应器接口，并使用select，epoll或Twisted反应器来实现事件循环。eventlet还提供了一个协程池，可以限制并发数
    - gevent是基于libev的，它通过Cython调用libev来实现一个高效的事件循环。gevent还使用monkey patch的方法来替换Python标准库中的socket，select等模块，使它们支持协程切换
    - eventlet和gevent都可以用于网络编程，但是gevent更适合于IO密集型的场景，而eventlet更适合于CPU密集型的场景。gevent的性能通常比eventlet更好，但是gevent不支持pypy，而eventlet可以在pypy上运行



```sh
# eventlet
pip install eventlet
celery -A  worker -l info -P eventlet

# gevent
pip install gevent
celery -A  worker -l info -P gevent
```



### 代码启动Worker

```py
from celery import Celery
app = Celery()

@app.task
def add(x, y): return x + y

if __name__ == '__main__':
    args = ['worker', '--loglevel=INFO']
    app.worker_main(argv=args)
```







## 配置，config

### 配置项文档

* 文档地址：<https://docs.celeryq.dev/en/stable/userguide/configuration.html#configuration>
* 4.0之前使用大写配置
* 4.0及以后使用小写配置
* 有转换工具



### 使用文件配置

```py
# 方法一，传入字符串
# `config_from_object`会查找`celeryconfig.py`文件作为配置

app.config_from_object('celeryconfig')

# 方法二，传入模块
import celeryconfig
app.config_from_object(celeryconfig)

# 方法三，传入类
class Config:
    enable_utc = True
    timezone = 'Europe/London'

app.config_from_object(Config)
```



* `celeryconfig.py`文件

```py
# 最小配置

# 任务Broker
broker_url = 'amqp://guest:guest@localhost:5672//'

# 任务导入
imports = ('myapp.tasks',)

# 任务后端
result_backend = 'db+sqlite:///results.db'

# 任务配置
task_annotations = {'tasks.add': {'rate_limit': '10/s'}}



# 其他配置

task_serializer = 'json'
result_serializer = 'json'
accept_content = ['json']
timezone = 'Europe/Oslo'
enable_utc = True

```



* 测试py文件配置语法

```sh
python -m celeryconfig
```



* 配置打印
  * `with_defaults=False`，打印默认值
  * `censored=True`，配置审查，避免API、TOKEN、SECRET等外泄

```py
# 打印成列表
app.conf.humanize(with_defaults=False, censored=True)

# 打印成字典
app.conf.table(with_defaults=False, censored=True)
```



### 在代码中修改配置

```sh
app.conf.task_serializer = 'json'
```



### 批量更新配置

```py
app.conf.update(
    task_serializer='json',
    accept_content=['json'],  # Ignore other content
    result_serializer='json',
    timezone='Europe/Oslo',
    enable_utc=True,
)
```



## 应用，Application

* 什么是应用
  * celery.Celery()实例化时候就是一个应用



* 多应用
  * 应用是线程安全的
  * 一个程序可以包含多个应用



* 应用名称
  * `Celery()`，不指定名称，应用会使用当前python模块的名称作为应用名称
  * `Celery('tasks')`，指定名称为tasks
  * 应用名称会作为消息标识



```py
from celery import Celery
app = Celery()


@app.task
def add(x, y):
    return x + y


# 不指定名称，会使用模块名称作为应用名称
print(app)
# <Celery __main__:0x100469fd0>

# 任务名称是 应用名称+函数名
print(add)
# <@task: __main__.add>

# 任务名称是 应用名称+函数名
print(add.name)
# __main__.add

# 任务名称是 应用名称+函数名
print(app.tasks['__main__.add'])
# <@task: __main__.add>
```



* 任务名称
  * 从模块导入的任务，会使用模块名称作为任务名称

```py
from tasks import add

print(add.name)
# tasks.add
```



## 任务，Task

* 默认配置任务

```py
from .models import User

@app.task
def create_user(username, password):
    User.objects.create(username=username, password=password)
```



* 传入配置参数

```py
@app.task(serializer='json')
def create_user(username, password):
    User.objects.create(username=username, password=password)
```



* 多装饰器，task必须是最后一个

```py
@app.task
@decorator2
@decorator1
def add(x, y):
    return x + y
```



* 绑定任务，传入当前任务对象Task为self参数

```py
@app.task(bind=True)
def add(self, x, y):
    print(self.request.id)

    print(f'task id {self.request.id}')
    
    # !r 是一种转换标志，它表示使用 repr() 函数来获取变量的字符串表示
    print(f'args: {self.request.args!r}')
    print(f'kwargs: {self.request.kwargs!r}')
```



* 指定任务名称

```py
@app.task(name='sum-of-two-numbers')
def add(x, y):
    return x + y

print(add.name)
# 'sum-of-two-numbers'
```



* 使用日志
  * 写入标准输出/-err 的任何内容都将被重定向到日志记录系统
  * 可以使用配置禁用此功能
  * 使用python内置的日志模块后，print将无效

```py

# 使用print打印日志
print('日志')


# 使用python内置的日志模块
from celery.utils.log import get_task_logger
logger = get_task_logger(__name__)

```



* 任务重试，retry

```py
@app.task(bind=True)
def send_twitter_status(self, oauth, tweet):
    try:
        twitter = Twitter(oauth)
        twitter.update_status(tweet)
    except (Twitter.FailWhaleError, Twitter.LoginError) as exc:
        raise self.retry(exc=exc)
```



## 发布任务



### 发布异步任务

* 带配置发布任务，apply_async

```py
# 原型
# apply_async(args[, kwargs[, …]])

# 示例
T.apply_async((arg,), {'kwarg': value})
T.apply_async(countdown=10)

# 10秒后执行
T.apply_async(eta=now + timedelta(seconds=10))

# 延迟60秒，120后过期的任务
T.apply_async(countdown=60, expires=120)

# 两天后过期
T.apply_async(expires=now + timedelta(days=2))

```



* 不带配置发布任务，delay

  * 使用时不用加`*`

  * 由于展开了参数，所以无法再带任务配置

```py
# 原型
# delay(*args, **kwargs)

# 示例
T.delay(arg, kwarg=value)

```



* delay对比apply_async

```py
# delay
task.delay(arg1, arg2, kwarg1='x', kwarg2='y')

# apply_async
task.apply_async(args=[arg1, arg2], kwargs={'kwarg1': 'x', 'kwarg2': 'y'})
```



### 同步调用任务

* 直接调用相当于调用`Task.__call__`

```py
@app.task
def add(x, y):
    return x + y

add(2, 2)
```



### 使用名称发布任务

```py
 send_task()
```



### 任务签名与任务链接

* `s`，用于固定任务的参数，生成新的任务
* `link`，用于链接任务，当前一个任务完成后产生回调

* 详见Canvas

```py
task.s(arg1, arg2, kwarg1='x', kwargs2='y').apply_async()

```



* 结果链接示例

```py
@app.task
def add(x, y):
    return x + y

# 第一个任务的结果链接到第二个任务的第一个参数
# 相当于 (2 + 2) + 16
add.apply_async((2, 2), link=add.s(16))

```



* 错误链接

```py
@app.task
def error_handler(request, exc, traceback):
    print('Task {0} raised exception: {1!r}\n{2!r}'.format(
          request.id, exc, traceback))
    
add.apply_async((2, 2), link_error=error_handler.s())
```



* 多链接

```py
add.apply_async((2, 2), link=[add.s(16), other_task.s()])
add.apply_async((2, 2), link_error=[add.s(16), other_task.s()])

```



### 长时间任务状态推送

* 在长时间任务中更新状态
  * `Task.update()`

```py
@app.task(bind=True)
def hello(self, a, b):
    time.sleep(1)
    self.update_state(state="PROGRESS", meta={'progress': 50})
    time.sleep(1)
    self.update_state(state="PROGRESS", meta={'progress': 90})
    time.sleep(1)
    return 'hello world: %i' % (a+b)
```

* 捕获任务状态更新
  * `on_message=on_raw_message`

```py
def on_raw_message(body):
    print(body)

a, b = 1, 1
r = hello.apply_async(args=(a, b))
print(r.get(on_message=on_raw_message, propagate=False))
```



### 预定任务，eta、countdown

* eta是指定一个时间点，任务开始执行
* countdown是eta的快捷方式，指定从现在开始，多长时间之后执行任务
* worker会先收到消息而不确认，worker中判断当前时间符合要求后才开始执行，因此不能设置过长时间
* RabbitMQ中 `consumer_timeout` 的默认值为 15 分钟

```py
# countdown
result = add.apply_async((2, 2), countdown=3)

# eta
# 接收一个datetime对象
tomorrow = datetime.utcnow() + timedelta(days=1)
add.apply_async((2, 2), eta=tomorrow)
```



### 规划任务队列和Worker

* 指定任务队列

```py
add.apply_async(queue='priority.high')
```

* 给Worker指定队列

```py
celery -A proj worker -l INFO -Q celery,priority.high
```



### 不保存任务结果

```py
result = add.apply_async((1, 2), ignore_result=False)
```



## 工作流，Canvas



### 固化任务

* 固化任务是工作流程的基础
* 任务固化之后，才可以作为其他任务的参数，使得多个任务组成工作流程

```py
# 方法1
from celery import signature
signature('tasks.add', args=(2, 2), countdown=10)
# tasks.add(2, 2)


# 方法2
add.signature((2, 2), countdown=10)
# tasks.add(2, 2)


# 星号参数快捷方式
add.s(2, 2)
add.s(2, 2, debug=True)

```



* 对比signature和s

```py
add.signature((2, 2), {'debug': True}, countdown=10)

# s不能固化任务参数
add.s(2, 2, debug=True)

# s链式调用，固化任务参数
add.s(2, 2).set(countdown=1)

# s执行时设置任务参数
add.s(2, 2).apply_async(countdown=1)
```



### 使用固化任务

* 与发布任务相同
* delay和apply_async中的参数，会覆盖signature中的参数

```py
# 直接执行固化任务
add(2, 2)()

# delay发布任务
add.s(2, 2).delay()

# apply_async发布任务
add.s(2, 2).apply_async()

# 参数覆盖
s = add.s(2, 2)

# 相当于add(2, 2, debug=True)
s.delay(debug=True)
s.apply_async(kwargs={'debug': True})
```



### 不接收前一个任务的结果作为参数

* 使用参数`signature(immutable=True)`
* 快捷方式`si`

```py
add.apply_async((2, 2), link=reset_buffers.signature(immutable=True))
add.apply_async((2, 2), link=reset_buffers.si())

# 在任务链中，不接受前一个任务的结果作为参数
from celery import chain
res = chain(add.si(2, 2), add.si(4, 4), add.si(8, 4))()
res.get()
```



### 任务链，chain

```py
from celery import chain

# 任务链
# 2 + 2 + 4 + 8

# 使用函数编写任务链
res = chain(add.s(2, 2), add.s(4), add.s(8))()
res.get()

# 使用管道符号编写任务链
(add.s(2, 2) | add.s(4) | add.s(8))().get()

# 在任务链中，不接受前一个任务的结果作为参数
res = chain(add.si(2, 2), add.si(4, 4), add.si(8, 4))()

# 获取任务链的中间结果
res.get()
16

res.parent.get()
8

res.parent.parent.get()
4

# 获取任务链的列表
res.children
[<AsyncResult: 8c350acf-519d-4553-8a53-4ad3a5c5aeb4>]

# 获取任务链结果集
list(res.collect())
[(<AsyncResult: 7b720856-dc5f-4415-9134-5c89def5664e>, 4),
 (<AsyncResult: 8c350acf-519d-4553-8a53-4ad3a5c5aeb4>, 64)]
```



### 并行任务，group

```py
from celery import group
res = group(add.s(i, i) for i in range(10))()
res.get(timeout=1)
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```



### 并行完成后，再执行一个任务，chord

```py
from celery import chord

@app.task
def tsum(numbers):
    return sum(numbers)

# 示例1
# 分别相加后，求总和

res = chord((add.s(i, i) for i in range(10)), tsum.s())()
res.get()
# 90


# 示例2
# group + chain = chrod
(group(add.s(i, i) for i in range(10)) | tsum.s())

```



### 映射任务，map、starmap

* map和startmap接受序列为参数，依次将对这些参数执行定义的任务，实际只是一个任务
* group会生成多条任务
* map和starmap只会生成一个管理任务，执行多个子任务
* starmap只是map的快捷版，接受`*`参数而非list

```py
from proj.tasks import add

@app.task
def tsum(numbers):
    return sum(numbers)


# 传入一个列表，列表里有两个元素作为参数
tsum.map([list(range(10)), list(range(100))]).delay().get()

# 等价于执行任务
@app.task
def temp():
    return [tsum(range(10)), tsum(range(100))]
```



### 分块任务，chunks

* 接收一个序列作为参数，将这些参数分块，每个分块分配为一个任务

```py
from proj.tasks import add

res = add.chunks(zip(range(100), range(100)), 10)()
res.get()
[[0, 2, 4, 6, 8, 10, 12, 14, 16, 18],
 [20, 22, 24, 26, 28, 30, 32, 34, 36, 38],
 [40, 42, 44, 46, 48, 50, 52, 54, 56, 58],
 [60, 62, 64, 66, 68, 70, 72, 74, 76, 78],
 [80, 82, 84, 86, 88, 90, 92, 94, 96, 98],
 [100, 102, 104, 106, 108, 110, 112, 114, 116, 118],
 [120, 122, 124, 126, 128, 130, 132, 134, 136, 138],
 [140, 142, 144, 146, 148, 150, 152, 154, 156, 158],
 [160, 162, 164, 166, 168, 170, 172, 174, 176, 178],
 [180, 182, 184, 186, 188, 190, 192, 194, 196, 198]]
```



* 存在一个总任务，将任务拆分成多个任务，并汇总结果

```py
add.chunks(zip(range(100), range(100)), 10).apply_async()
```



* 可以将分块任务转换为组任务

```py
group = add.chunks(zip(range(100), range(100)), 10).group()
```



## 定时任务

* 是什么
  * celery beat是一个独立的调度程序
  * 按配置定期执行任务
  * 任务默认使用配置`beat_schedule`，也可以保存在数据库中
* 时区
  * 默认使用UTC时区
  * 可以通过配置改变时区，`Asia/Shanghai`



### 代码中管理定时任务

```py
from celery import Celery
from celery.schedules import crontab

app = Celery()

@app.on_after_configure.connect
def setup_periodic_tasks(sender, **kwargs):
    # Calls test('hello') every 10 seconds.
    sender.add_periodic_task(10.0, test.s('hello'), name='add every 10')

    # Calls test('hello') every 30 seconds.
    # It uses the same signature of previous task, an explicit name is
    # defined to avoid this task replacing the previous one defined.
    sender.add_periodic_task(30.0, test.s('hello'), name='add every 30')

    # Calls test('world') every 30 seconds
    sender.add_periodic_task(30.0, test.s('world'), expires=10)

    # Executes every Monday morning at 7:30 a.m.
    sender.add_periodic_task(
        crontab(hour=7, minute=30, day_of_week=1),
        test.s('Happy Mondays!'),
    )

@app.task
def test(arg):
    print(arg)

@app.task
def add(x, y):
    z = x + y
    print(z)
```



### 在配置中管理定时任务

```py
app.conf.beat_schedule = {
    'add-every-30-seconds': {
        'task': 'tasks.add',
        'schedule': 30.0,
        'args': (16, 16)
    },
}
```



### 类cron定时任务

* cron存在缺陷，如果下次cron触发时，前次cron还未完成，将同时存在两个cron任务
* 需要人为避免

```py
from celery.schedules import crontab

app.conf.beat_schedule = {
    # Executes every Monday morning at 7:30 a.m.
    'add-every-monday-morning': {
        'task': 'tasks.add',
        'schedule': crontab(hour=7, minute=30, day_of_week=1),
        'args': (16, 16),
    },
}
```



* cron配置示例

| **例子**                                                     | **说明**                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `crontab()`                                                  | 每分钟执行一次。                                             |
| `crontab(minute=0, hour=0)`                                  | 每天午夜执行。                                               |
| `crontab(minute=0, hour='*/3')`                              | 每三个小时执行一次：午夜、凌晨 3 点、上午 6 点、上午 9 点、中午、下午 3 点、下午 6 点、晚上 9 点。 |
| `crontab(minute=0, hour='0,3,6,9,12,15,18,21')`              | 每三个小时执行一次：午夜、凌晨 3 点、上午 6 点、上午 9 点、中午、下午 3 点、下午 6 点、晚上 9 点。 |
| `crontab(minute='*/15')`                                     | 每 15 分钟执行一次。                                         |
| `crontab(day_of_week='sunday')                               | 周日每分钟执行一次（！）。                                   |
| `crontab(minute='*', hour='*', day_of_week='sun')`           | 周日每分钟执行一次（！）。                                   |
| `crontab(minute='*/10', hour='3,17,22', day_of_week='thu,fri')` | 每十分钟执行一次，但仅在周四或周五的上午 3-4 点、下午 5-6 点和晚上 10-11 点之间执行。 |
| `crontab(minute=0, hour='*/2,*/3')`                          | 每个偶数小时以及每个被三整除的小时执行一次。这意味着：每个小时，除了：上午 1 点、上午 5 点、上午 7 点、上午 11 点、下午 1 点、下午 5 点、晚上 7 点、晚上 11 点 |
| `crontab(minute=0, hour='*/5')`                              | 执行可被 5 整除的小时。这意味着它在下午 3 点触发，而不是下午 5 点（因为下午 3 点等于 24 小时时钟值“15”，可被 5 整除）。 |
| `crontab(minute=0, hour='*/3,8-17')`                         | 每小时可被 3 整除一次，办公时间（上午 8 点至下午 5 点）每小时执行一次。 |
| `crontab(0, 0, day_of_month='2')`                            | 每月第二天执行。                                             |
| `crontab(0, 0, day_of_month='2-30/2')`                       | 在每个偶数日执行。                                           |
| `crontab(0, 0, day_of_month='1-7,15-21')`                    | 在每月的第一周和第三周执行。                                 |
| `crontab(0, 0, day_of_month='11', month_of_year='5')`        | 每年5月11日执行。                                            |
| `crontab(0, 0, month_of_year='*/3')`                         | 每季度第一个月的每天执行。                                   |



### 启动celery beat

* 独立启动

```sh
celery -A proj beat
```



* 合并在其中一个worker中启动
  * 一般只有在测试环境中这么用
  * 测试环境中只有一个worker

```py
celery -A proj worker -B
```



* 自定义临时文件存储位置
  * 存储任务最后运行时间

```sh
celery -A proj beat -s /home/celery/var/run/celerybeat-schedule
```





## 路由任务





## 结合Django使用



### django集成celery

* 原始工程结构如下

```ini
- proj/
  - manage.py
  - proj/
    - __init__.py
    - settings.py
    - urls.py
```

* 创建celery应用实例

```py
# proj/proj/celery.py

import os

from celery import Celery

# 给celery设置环境变量
# 使得celery使用django的配置文件
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'proj.settings')

app = Celery('proj')

# Celery 配置选项必须以大写而不是小写指定，并以 CELERY_ 开头
app.config_from_object('django.conf:settings', namespace='CELERY')

# 自动发现任务
app.autodiscover_tasks()

# 定义任务
@app.task(bind=True, ignore_result=True)
def debug_task(self):
    print(f'Request: {self.request!r}')
```

* 项目启动是加载celery应用实例

```py
# proj/proj/__init__.py

from .celery import app as celery_app

__all__ = ('celery_app',)
```

* 在每个django app下编写任务

```ini
- app1/
    - tasks.py
    - models.py
- app2/
    - tasks.py
    - models.py
```



### django-celery-beat

* `celery beat`允许指定自定义调度类，默认的调度程序是`celery.beat.PersistentScheduler`
* `django-celery-beat`使用django ORM实现了`django_celery_beat.schedulers:DatabaseScheduler`



#### django集成

* 安装

```sh
pip install django-celery-beat
```

* 添加应用
```py
# settings.py

INSTALLED_APPS = (
    ...,
    'django_celery_beat',
)
```

* 迁移数据库

```sh
python manage.py migrate
```

* 启动celery beat

```sh
celery -A proj beat -l INFO --scheduler django_celery_beat.schedulers:DatabaseScheduler
```



#### 数据模型

* `django_celery_beat.models.PeriodicTask`
  * 记录定时任务
* `django_celery_beat.models.IntervalSchedule`
  * 以特定间隔运行的任务
* `django_celery_beat.models.CrontabSchedule`
  * cron任务
* `django_celery_beat.models.PeriodicTasks`
  * 记录定时任务的改变
  * `celery beat`发现定时任务改变，则会重新从数据库加载定时任务



### 不依赖具体应用的共享任务

* 使用`@shared_task`装饰器

```py
# Create your tasks here

from demoapp.models import Widget

from celery import shared_task


@shared_task
def add(x, y):
    return x + y


@shared_task
def mul(x, y):
    return x * y


@shared_task
def xsum(numbers):
    return sum(numbers)


@shared_task
def count_widgets():
    return Widget.objects.count()


@shared_task
def rename_widget(widget_id, name):
    w = Widget.objects.get(id=widget_id)
    w.name = name
    w.save()
```



### django-celery-result

* 使用 Django ORM 或 Django Cache 框架作为结果后端



#### django集成

* 安装

```sh
pip install django-celery-results
```

* 添加APP到django项目配置

```py
INSTALLED_APPS = (
    ...,
    'django_celery_results',
)
```

* 迁移数据库

```sh
python manage.py migrate django_celery_results
```

* 相关配置

```py
# 数据库
CELERY_RESULT_BACKEND = 'django-db'

# cache默认
CELERY_CACHE_BACKEND = 'django-cache'


# cache自定义
CELERY_CACHE_BACKEND = 'default'

CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table',
    }
}
```



#### 数据模型

* 定义了两个数据模型
  * `django_celery_results.models.TaskResult` 
  * `django_celery_results.models.GroupResult`



## 部署守护进程





## Flower







