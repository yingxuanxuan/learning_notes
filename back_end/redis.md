# Redis



## 应用场景





## 安装



### docker安装



### centos安装

```bash
yum install epel
yum install redis
```



## 组件介绍

redis-server
reids-sentinel # 监视器，监视、故障转移
redis-cli # 命令行客户端
redis-benchmark # 性能测试工具
redis-check-aof redis-check-dump # 故障修复



## redis客户端

* 自带redis-cli

```bash
```

* RedisInsight，官方UI，提供性能监控、统计分析、图形操作界面、命令行terminal



## 配置

配置文件：/etc/redis.conf

```
daemonize yes # 守护模式，默认为no
loglevel # 日志级别
pidfile /var/run/redis/redis.pid # pid
logfile /var/log/redis/redis.log # 日志
dbfilename dump.rdb # 持久化文件名
dir /var/lib/redis # 持久化目录
bind 127.0.0.1 # 绑定监听IP
port 6379
requirepass password # 设置密码，无用户名  
```

## 其他

### 测试命令

```bash
# 命令式
redis-cli ping

# 交互式
redis-cli
ping

# 参数
-h <hostname>  # 默认localhost
-p <port>      # 默认6379
-a <password>  # 默认无
```







## 键操作，key

```bash
# 查询匹配的key
keys pattern

# 是否存在key
exists key

# 查看值的数据类型
type key

# 删除键
del k1 k2 k3

# 查询键有效期
ttl key

# 设置键有效期
ex key 5
expire key 5

# 重命名
rename k1 k2

# 切换数据库
select dbid
```

### 



## 数据结构



### 字符串，string

#### 设置键值



#### 追加值



#### 不可修改（分布式锁）



#### 设置过期键



#### 批量设置

```bash
mset k1 v1 k2 v2 k3 v3
```



#### 自增自减（对数字字符串操作）

```bash
incr key
desc key
incrby key incnum

```



#### 比特流操作

```bash
# 设1
setbit key offset 1

# 设0
setbit key offset 0

# 获取
getbit key offset 

# 数比特为1的数，默认为0
bitcount 
```



### 列表，list，数组

```bash
# 左插
lpush k v1 v2

# 右插
rpush k v1 v2

# 切片取
lrange k

# 匹配左插入
linsert 

# 左查找


# 获取长度
llen

# 修改
lset

# 删除
lpop k
rpop k


```



## python操作redis

### redis-py，官方

```bash
# 主库
pip install redis

# 编译加速
pip install redis[hiredis]
```



### 案例

#### KV缓存





#### 分布式锁

```
# 原子性，在一条指令中设置nx、ex
redis.set('key', '', nx=True, ex=10)
```





#### 延迟队列，生产消费





#### 订阅发布







#### 定时任务





