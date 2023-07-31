# git

## git简介

* 分布式
* 版本控制

### 版本控制发展历程

1. 保存多份文件
2. 本地版本控制
3. 集中式版本控制（Server），只有服务器保存全部版本
4. 分布式版本控制，每个节点都保存全部版本

## git本地使用方法

###  首次使用版本库

```bash
# 个人信息配置
git config --global user.email "your@email.com"
git config --global user.name "your name"

# 初始化目录

# 在目标目录执行
git init

# 初始化目标目录为git管理目录
# 生成.git隐藏目录，存放版本信息
```

### 查看当前目录中文件的状态

* 进入目标目录
* `git status`，文件三种状态
  * new file，绿色，
  * untracked files，未追踪且未忽略的文件，modified，已追踪已修改但是未提交的文件，红色，
  * 已追踪未被修改
* 三个区域
  * 工作区，用户可见的目录，通过`git add` 提交到暂存区
    * 已管理
    * 新文件，已修改的
  * 暂存区，通过`git commit`提交到版本库
  * 版本库

![image-20230731101324411](.gitbook/assets/image-20230731101324411.png)

### 管理文件

* `git add file` 添加file为管理文件
* `git add .` 添加所有未追踪的文件为管理文件

### 提交版本

`git commit -m '提交信息'`

### 查看提交记录

`git log`

### 回滚到之前版本

```bash
git log # 查找版本后
git reset --hard 之前版本号
```

### 回滚到之后版本

```bash
git reflog
git reset --hear

```

### 其他

```bash
# 将所有文件恢复到暂存区状态
git checkout
git checkout -- file

# 将暂存区恢复到未追踪
git reset HEAD

```



## git分支使用方法

### 查看所有分支（及当前所在分支）

```bash
git branch
```

### 创建分支

```bash
git branch 分支名称
```

### 切换到分支

```bash
git checkout 分支名称
```

### bug合并到主干

```bash
# 先切换到主干
git checkout master
# 主干合并分支
git merge 分支名称

# 无冲突时，git merge 分支名称 已经合并成功
# 有冲突时，手动处理冲突文件后再commit，才能完成合并
git add .
git commit -m '合并'
```









