# K8S

## 介绍

* 什么是K8S
  * 开源
  * 管理多个主机上容器的应用
  * 目标是让部署容器化应用简单、高效
  * 提供了应用部署、规划、更新、维护的机制



* 历史
  * Kubernetes含义为舵手、飞行员
  * k8s原因是k、s之间有8个字幕
  * k8s源于google使用十几年的borg系统经验，基于最新技术发展而来



* 为什么需要K8S
  * Docker只能解决一台机器下的问题
  * 已经成为实际标准
  * 微服务、云原生的基础



* 应用部署方式的演进
  * 传统部署，应用直接部署在服务器
    * 人工参与多
    * 脚本不灵活
    * 应用间不隔离
    * 硬件利用率低
  * 虚拟机部署
    * 解决了环境隔离
    * 虚拟机过重，资源占用率高
    * 仍然存在人工参与多和流程繁琐的问题
  * 容器化部署
    * 实现了轻量级资源隔离
  * K8S部署
    * 实现了容器管理
    * 监听容器自动修复
    * 检测流量弹性伸缩
    * 服务发现、负载均衡
    * 机密和配置管理
    * 存储编排
    * 批处理任务



* 企业级容器调度平台
  * Apache Mesos
    * 简介
      * 分布式调度系统内核
      * 早于Docker产生
      * 主/从结构工作模式，主节点分配任务，从节点上的Executor执行任务
      * 使用Zookeeper给主节点提供服务注册、服务发现功能
      * 通过Framework Marathon提供容器调度的能力
    * 优势
      * 经过了时间的检验
      * 支持容器化和非容器化的工作服在
      * 支持应用的健康检查
      * 开放的架构
      * 支持多个框架和多个调度器，通过不同Framework可以运行Haddop/Spark/MPI等
      * 支持超大型规模的节点管理，5W+
  * Docker Swarm
    * 简介
      * Docker官方开发的调度框架
      * 每个节点（Node）对应一个代理（Agent）
      * Swarm控制代理还行任务，拉取和运行不同的镜像
    * 优势
      * 支持标准Docker API
      * 与Docker无缝集成
      * 小数量服务器简单易上手
  * Google Kubernetes



## 核心概念

### borg系统架构

* Cell，系统核心
  * BorgMaster，主服务器节点
  * Borglet，从服务器节点
  * scheduler，调度器
* borgcfg、command-line tools、web browsers，外部接口



![Google Borg](.gitbook/assets/15414871166556.jpg)



### K8S系统架构

* kubectl，用户客户端
* Master，主节点
  * APIServer，核心内外接口
  * authentication、authorization，身份认证授权
  * Scheduler，调度器
  * Controller manager，控制管理器
* Node，从节点
  * docker，容器环境
  * kubelet，管理代理
  * proxy，负载代理

![k8s架构](.gitbook/assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MjQ5MjI4MA==,size_16,color_FFFFFF,t_70.png)



### K8S系统架构特点

* 基于主从架构
* 从节点只执行任务
* 主节点负责调度
* 通过配置主节点也可以执行任务
* api-server管理所有接口，所有节点都通过api-server管理



## 组件说明

