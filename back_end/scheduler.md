# scheduler

### 任务调度器

schedule

apscheduler

rocketry



#### 延迟（定时任务）处理调度

**循环sleep检查**

```
 import time
 ​
 def timer(n):
     while 1:
         print(time.time())
         time.sleep(n)
 ​
 timer(2)
```

**threading.Timer回调（Timer是一个新线程）**

```
 import time
 import threading
 ​
 def timer(n):
     print(time.time())
     t = threading.Timer(n, timer, (n,))
     t.start()
 ​
 timer(2)
```

**sched模块（线程内定时任务）**

* 初始化：scheduler(时间函数=time.time, 睡眠函数=time.sleep)
* 在绝对时间执行一个任务：scheduler.enterabs(绝对时间, 优先级, 回调, 参数)
* 在相对时间执行一个任务：scheduler.enter(相对时间, 优先级, 回调, 参数)
* 开始执行：scheduler.run()
* enter一次只执行一次任务，没有任务时scheduler.run()退出

```
 import time
 import sched
 ​
 scheduler = None
 ​
 def timer(sleep_time, priority):
     global scheduler
 ​
     print('timer', sleep_time, ': ', time.time())
     scheduler.enter(sleep_time, priority, timer, (sleep_time, priority))
     
 scheduler = sched.scheduler()
 scheduler.enter(1, 0, timer, (1, 0))
 scheduler.enter(2, 0, timer, (2, 0))
 scheduler.run()
```

**第三方库 APScheduler**

* 持久化任务
* 以daemon方式运行
* 支持crontab类型的任务
* `pip install apscheduler`

**协程调度示例，定制一次性触发器示例**

```
 from apscheduler.schedulers.asyncio import AsyncIOScheduler
 from apscheduler.triggers.base import BaseTrigger
 ​
 # 定制一次性触发器
 class OnceNowTrigger(BaseTrigger):
     def __init__(self):
         self._triggered = False
 ​
     def get_next_fire_time(self, previous_fire_time, now):
         if self._triggered:
             return None
         else:
             self._triggered = True
             return now
 ​
 # 异步任务处理
 class AsyncTaskExecutor(object):
     def __init__(self):
         self._scheduler = None
     
     def running(self):
         return self._scheduler and self._scheduler.running
     
     def start(self):
         t = threading.Thread(target=asyncio.run, args=[self.main_task()], daemon=True)
         t.start()
         signal.signal(signal.SIGINT, self.stop)
         signal.signal(signal.SIGTERM, self.stop)    
     
     def stop(self):
         if self._scheduler:
             self._scheduler.remove_all_jobs()
             self._scheduler.shutdown()
     
     async def main_task(self):
         self._scheduler = AsyncIOScheduler()
         self._scheduler.start()
         
         while True:
             await asyncio.sleep(1)
     
     def add_job(self, async_func):
         return self._scheduler.add_job(async_func, OnceNowTrigger())
     
     def remove_job(self, job):
         self._scheduler.remove_job(job.id)
```

\

## apscheduler

### AsyncIOScheduler

```
import asyncio
import os
from datetime import datetime

from apscheduler.schedulers.asyncio import AsyncIOScheduler

async def tick():
    print('Tick! The time is: %s' % datetime.now())

if __name__ == '__main__':
	# 注意，AsyncIOScheduler在创建时获取loop，注意loop位置
    scheduler = AsyncIOScheduler()
    scheduler.add_job(tick, 'interval', seconds=3)
    scheduler.start()
    print('Press Ctrl+{0} to exit'.format('Break' if os.name == 'nt' else 'C'))

    # Execution will block here until Ctrl+C (Ctrl+Break on Windows) is pressed.
    try:
        asyncio.get_event_loop().run_forever()
    except (KeyboardInterrupt, SystemExit):
        pass

```



## 
