# Docker

[toc]

docker基本信息
----------
### 版本
* 17.03后分为CE版、EE版本
* CE, Community Edition
* EE, Enterprise Edition
```
docker version
docker info
docker --help
```

### 基本概念
* Repository, 镜像的仓库
* Image, 镜像
* Container, Image的实体，在机器中的运行环境
* Host, 运行Docker daemon和容器的服务器
* Client, 命令行

### 安装
```
# 卸载旧版本
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine

# 安装依赖
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
 
# 添加repo
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 解决centos8 container版本冲突
sudo yum install docker-ce docker-ce-cli containerd.io --nobest

# 安装Docker Engine-Community
sudo yum install docker-ce docker-ce-cli containerd.io
```

### 镜像加速
```
/etc/docker/daemon.json
{
    "registry-mirrors": ["https://6qs3kk2z.mirror.aliyuncs.com"]
}
sudo systemctl daemon-reload
sudo systemctl restart docker
```
### 启动
```
sudo systemctl start docker
sudo systemctl status docker # active
sudo docker run hello-world # 测试运行
```

docker基本操作
----------
### 命令模式
```
sudo docker run ubuntu:15.10 /bin/echo "Hello World"
```

### 交互模式
```
sudo docker run -i -t ubuntu:15.10 /bin/bash
exit # 退出
```

### 后台模式（启动容器）
```
# 示例程序
while true; do echo hello world; sleep 1; done 

# 前台运行
sudo docker run ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"

# 后台运行 -d，工作任务不退出docker容器不退出，返回一个容器ID
sudo docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
* 后台运行必须有前台进程，否则立即退出 *

# 查看运行容器
sudo docker ps
sudo docker top

# 查看容器输出
sudo docker logs d35aad423212

# 停止容器
sudo docker stop d35aad423212 
```

容器使用
----------
```
# 获取镜像
sudo docker pull ubuntu

# 启动容器, -i interactive -t terminal
sudo docker run -it ubuntu /bin/bash

# 退出容器
exit
Ctrl + p + q

# 查看所有容器 -a all 包含历史容器
sudo docker ps -a 

# 重新启动已经停止的docker
docker start container_id
docker stop container_id
docker kill container_id
docker restart container_id

# 后台模式
sudo docker run -itd --name name ubuntu /bin/bash
sudo docker attach container_id
sudo docker exec -it container_id /bin/bash

# 导入导出容器
docker export container_id > ubuntu.tar
cat ubuntu.tar | docker import - test/ubuntu:v1

# 删除容器
docker rm -f $(docker ps -a -q)
docker ps -q -qa | xargs docker rm

# 清楚所有终止容器
docker container prune

# 映射端口到本地 -P
docker pull training/webapp  # 载入镜像
docker run -d -P training/webapp python app.py

# 查看容器端口
docker port container_id

# 动态查看容器日志
docker logs -f container_id # -f 动态输出内容

# 查看容器信息
docker inspect container_id

# 无权限时添加权限
--privileged=true
```

镜像使用
----------
```
# 列出所有本地镜像
docker images
docker images --no-trunc # 不截断imageid

# 获取镜像
docker pull Repo:Tag
docker pull nginx:latest
docker pull mysql:5.6

# 搜索镜像 https://hub.docker.com
docker search reponame
docker search --no-trunc reponame # 显示详细信息
docker search -s 30 reponame # star数30以上

# 删除镜像
docker rmi hello-world
docker rmi -f ${docker images -qa}

# 创建镜像
## 更新镜像并保存
docker run -t -i ubuntu:15.10 /bin/bash
apt-get update
docker commit -m="mark" -a="author" container_id user/repo:tag

## 从Dockerfile创建镜像
"""
FROM ubuntu
MAINTAINER author "email@email.com"

RUN /bin/echo 'root:passwd' | chpasswd
RUN useradd user
RUN /bin/echo 'user:passwd' | chpasswd
RUN /bin/echo -e "LANG=\"en_US.UTF-8\"" > /etc/default/local
EXPOSE 22
EXPOSE 80
CMD /usr/sbin/sshd -D
"""
docker build -t user/repo:tag [Dockerfile Dir] 
# -t 指定名称
# docker指令必须大写

```

容器连接
---------
```
# 端口映射

## -P 自动映射端口
docker run -d -P  training/webapp python app.py

## -p docker_port:local_port 手动映射端口
docker run -d -p listen_ip:port:5000 training/webapp python app.py

## 绑定udp端口，默认tcp
docker run -d -p ip:port:docker_port/udp repo/image cmd

## 查看所有端口映射
docker port container_id


# docker网桥

## 创建网桥
docker network create -d bridge testnet # -d 指定网桥类型 bridge / overlay

## 查看docker网络
docker network ls

## 删除网桥
docker network rm net_id

## 使用网桥 --network netname
docker run -itd -name test1 --network testnet ubuntu /bin/bash
docker run -itd -name test2 --network testnet ubuntu /bin/bash

## 测试连接
docker run -it ubuntu /bin/bash
apt-get update
apt install iputils-ping

ping test1
ping test2


# 配置DNS
/etc/docker/daemon.json
{
    "dns" : [
        "144.144.144.144",
        "8.8.8.8"
    ]
}
sudo systemctl restart docker


```

仓库管理
----------
```
docker login --username=yingxuanxuan@hotmail.com registry.cn-shanghai.aliyuncs.com
docker pull registry.cn-shanghai.aliyuncs.com/daniel-hub/nginx-docker:tag
docker tag [ImageId] registry.cn-shanghai.aliyuncs.com/daniel-hub/nginx-docker:tag
docker push registry.cn-shanghai.aliyuncs.com/daniel-hub/nginx-docker:tag
docker logout 

```


容器数据卷
----------
### 数据复制
```
docker cp container_id:/path/to/file /local/path
```

### 数据卷（目录映射）
* 数据双向读写同步
* 效果相当于mount
```
# 命令添加
## 默认读写映射
docker run -it -v /local/path:/docker/path image

## 默认只读映射
docker run -it -v /local/path:/docker/path:ro image


# Dockerfile添加
VOLUME["/path1", "/path2"] # 指定Docker内数据卷，主机不指定在/var/lib/...


# 容器间数据共享

## 数据卷继承（数据双向同步）
docker run -it --name dc01 zzyy/centos
docker run -it --name dc02 --volumes-from dc01 zzyy/centos
docker run -it --name dc03 --volumes-from dc01 dc02 zzyy/centos
```

Dockerfile
----------
* 指令必须大写
* 指令后至少一个参数
* 顺序执行
* 使用#号注释
* 每条指令都会创建一个新的镜像层
```

FROM # 依赖image
FROM image
FROM scratch
FROM centos

MAINTAINER # 维护者
MAINTAINER yingxuanxuan <yingxuanxuan@hotmail.com>

ADD # 解压缩并添加tar包
ADD jdk-8u171-linux-x64.tar.gz /usr/local
ADD apache-tomcat-9.0.8.tar.gz /usr/local

COPY # 只复制
COPY src dst
COPY ["src", "dst"]
COPY c.txt /usr/local/cincontainer.txt

LABEL # 说明

RUN # 执行shell指令

EXPOSE # 暴露端口

VOLUME # 数据卷

WORKDIR # 默认工作目录

ENV # 环境变量

CMD # Default Command 启动后执行命令
* 多个CMD命令只有最后一个生效
* 执行时CMD会被docker run命令替换

ENTRYPOINT # 执行命令，可以追加命令
* 不会被docker run命令替换

ONBUILD # 被子镜像继承时会被触发
ONBUILD RUN echo "Onbuild running"

"""

""""

# 编译 dockerfile
docker build -t user/repo:tag [Dockerfile Dir] 
-f Dockerfile # 指定Dockerfile，默认名称Dockerfile

# 查看构建历史
docker history image_ID

```

推送
----------
```
# 在阿里云创建仓库

# 登录阿里云
sudo docker login --username=yingxuanxuan@hotmail.com registry.cn-shanghai.aliyuncs.com

# 打标签
sudo docker tag [ImageId] registry.cn-shanghai.aliyuncs.com/[namespace]/[repo]:[tag]

# 推送镜像
sudo docker push registry.cn-shanghai.aliyuncs.com/[namespace]/[repo]:[tag]

# 使用镜像
sudo docker pull registry.cn-shanghai.aliyuncs.com/[namespace]/[repo]:[tag]
```

mysql
----------
### 配置项
```
# 密码环境变量（三选一） 
MYSQL_ROOT_PASSWORD=passwd
MYSQL_ALLOW_EMPTY_PASSWORD=1
MYSQL_RANDOM_ROOT_PASSWORD=1

# 目录配置
/etc/mysql/conf.d # 用户自定义配置
/var/lib/mysql # 数据
/logs # 日志


# 配置命令
--character-set-server=utf8mb4
--collation-server=utf8mb4_unicode_ci
```

### 从文件启动
```
docker pull mysql
docker run 
    -p 8888:3306 
    --name mysql
    -v /var/mysql/conf:/etc/mysql/conf.d
    -v /var/mysql/logs:/logs
    -v /var/mysql/data:/var/lib/mysql
    -e MYSQL_ROOT_PASSWORD=passwd
    -d mysql
    
docker exec -it container_Id /bin/bash
mysql -uroot -p
```

### C/S启动
```
### 创建网络连接
docker network create -d bridge dock_net

### 服务端启动
docker run -p 3306:3306 --name mysql_server --network dock_net -v /var/mysql/conf:/etc/mysql/conf.d -v /var/mysql/logs:/logs -v /var/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=passwd -d mysql 

### 客户端启动
sudo docker run --network dock_net -it --rm mysql mysql -hmysql_server -uroot -ppasswd
```




redis
----------
```
docker pull redis
docker run 
    -p 6379:6379
    -v /var/redis/data:/data
    -v /var/redis/conf/redis.conf:/usr/local/etc/redis/redis.conf
    -d redis redis-server /usr/local/etc/redis/redis.conf --appendonly yes
    
vim /var/resis/conf/redis.conf/redis.conf
"""

"""
docker exec -it container_id redis-cli
```
