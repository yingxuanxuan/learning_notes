## 获取帮助

### whatis，显示命令功能介绍

* 显示命令的简短介绍，实际为man手册中的简介。
* command 只能为完整命令

```bash
# whatis command
whatis whatis
```

### which，查找命令绝对路径

```bash
# which command
which which
```

### whereis，定位二进制文件、源文件、手册文件

```bash
# whereis command
whereis ssh
# ssh: /usr/bin/ssh /etc/ssh /usr/share/man/man1/ssh.1.gz
```

### file，查看文件类型

```bash
# file file
file .
# .: directory

file /etc/passwd
# /etc/passwd: ASCII text
```

### man

* 说明
  * manual简写，手册，文档，说明书
  * 手册分类为多个章节，出现歧义时需要输入章节号
    * 用户指令（1）：用户在shell环境可操作的命令或执行文件
    * 系统调用（2）：系统内核可调用的函数与工具等
    * 核心函数库（3）：一些常用的函数(function)与函数库(library)，大部分为C的函数库(libc)
    * 设备（4）：设备文件说明，通常在/dev下的文件 
    * 配置文件、文件格式（5）：配置文件或某些文件格式 
    * 游戏（6）
    * Linux惯例、协议、杂项（7）：如Linux文件系统，网络协议，ASCII code等说明 
    * 管理员命令、系统指令（8）：系统管理员可用的管理命令 
    * 内核相关文件（9）：跟kernel有关的文件
    * Tcl或Tk指令（n）
  * 默认路径：`/usr/share/man`
  * 配置路径：`/etc/main_db.conf`
* 命令示例：

```shell
# 获取帮助
# man command
man man

# 有歧义时，需要加上章节号获取帮助
# man n command
man 1 man

# 按命令搜索帮助
# man -f command 
# 完全等同于 whatis xxx
man -f man

# 按关键字搜索帮助
# man -k keyword 
# 按关键字搜索文档，完全等同于apropos keyword
man -k etc
```

### apropos

```

```

### info

* 搜索路径：`/usr/share/info`

### 软件文档路径

/usr/share/doc

## Linux标准规范（LSB）

* Linux标准规范（Linux Standard Base，简称LSB）
* LSB的目的是为了开发和促进一系列开放式的标准，这些标准将会提升各个不同的Linux发行版之间的兼容性，并让软件应用程序可以在任意满足该条件的系统上运行（在不修改二进制文件的情况下都可以）
* LSB包含Linux内核中所使用的文件系统标准（FHS）
* LSB定义的内容包括
  - 标准库
  - 一系列命令和工具（在POSIX基础上进行了扩展）
  - 文件系统层级结构
  - 运行级别
  - 打印系统
  - 包含假脱机（spoolers），诸如CPUS和类似Foomatic这样的工具
  - 若干对X Window系统的扩展

```bash
# 打印当前lsb相关信息
yum install -y redhat-lsb
lsb_release -a
```

### 文件系统标准（FHS）

* FHS(Filesystem Hierarchy Standard 文件系统层次化标准)
* 多数Linux 版本采用这种文件组织形式
* FHS 采用树形结构组织文件。FHS 定义了系统中每个区域的用途、所需要的最小构成的文件和目录，同时还给出了例外处理与矛盾处理

#### local目录的作用

* `/usr/bin`下面的都是系统预装的可执行程序，会随着系统升级而改变。
* `/usr/local/bin`目录是给用户放置自己的可执行程序的地方，推荐放在这里，不会被系统升级而覆盖同名文件

#### FHS

```bash
FHS(Filesystem Hierarchy Standard 文件系统层次标准)目录命名标准  
<http://www.pathname.com/fhs/>  
/bin #所有用户可执行文件 #binary  
/sbin #root用户可执行的命令  
/boot #引导目录  
/dev #device 设备  
/etc #配置  
/home #除root用户外的其他用户文件  
/lib #库 *.so  
/lib/modules/ #核心驱动  
/media #自动挂载cdrom  
/mnt #正常挂载目录  
/opt #通常用来装第三方大型软件  
/run #/var/run内存虚拟硬盘  
/srv #service /srv/www  
/proc #系统实时信息 内存接口  
/sys #底层硬件信息  
/tmp #临时信息 自动删除  
/usr #应用软件 Unix Software Resource  
/var #经常变化的信息  
/lost+found #ext错误片段  
```

* /proc

```bash
/proc/cpuinfo  
/proc/meminfo  
/proc/partitions #分区信息  
/proc/interrupts  
/proc/ioports #硬件端口（地址）  
/proc/net/*  
/proc/swaps #交换分区  
/proc/filesystems #已经加载的文件系统  
```

* 获取启动时间示例

```bash
cat /proc/uptime  
== uptime  
== w first line  
```

* /usr

```bash
/usr/bin = /bin  
/usr/lib = /lib  
/usr/local #覆盖/usr下的bin etc include lib  
/usr/sbin = /sbin  
/usr/share /usr/share/man /usr/share/doc /usr/share/zoneinfo  
/usr/include  
/usr/games  
/usr/libexec #XWindow操作指令等  
/usr/libxx = /libxx  
/usr/src /usr/src/linux  
```

* /var

```bash
/var/cache  
/var/lib /var/lib/mysql /var/lib/rpm  
/var/lock = /run/lock  
/var/log /var/log/wtmp #登录日志 /var/log/message #系统日志  
/var/mail = /var/spool/mail  
/var/run = /run  
/var/spool #队列数据 /var/spool/mqueue /var/spool/cron  
```

* /etc

```bash
/etc/fstab #自动挂载配置  
/etc/yum.repos.d/  
/etc/samba  
/etc/sysconfig/network-scripts/  
/etc/sysconfig/network-scripts/ifcfg-* #网卡配置文件  
/etc/sysconfig/network #主机名配置文件  
/etc/resolv.conf #DNS配置  
/etc/hosts #静态主机名配置  
/etc/bashrc  
/etc/rc.local #开机执行命令 chmod +x /etc/rc.d/rc.local  
```

* /dev

```bash
/dev/null #丢弃  
/dev/sdx #磁盘  
/dev/vdx #虚拟机磁盘  
/dev/mdx #软RAID  
/dev/VGNAME/LVNAME #LVM  
[device命名标准](https://www.kernel.org/doc/Documentation/devices.txt)  

/dev/sd[a-p] SCSI/SATA/USB硬盘/USB软盘
/dev/vd[a-p] VirtIO虚拟硬盘
/dev/fd[0-1] 软盘
/dev/lp[0-2] 25针接口打印机
/dev/usb/lp[0-15] USB接口打印机
/dev/input/mouse[0-15] 鼠标
/dev/psaux PS/2接口鼠标
/dev/mouse 当前默认鼠标
/dev/scd[0-1] CDROM/DVDROM
/dev/sr[0-1] CentOS CDROM/DVDROM
/dev/cdrom 当前默认CDROM
/dev/ht0 IDE接口磁带机
/dev/st0 SATA/SCSI接口磁带机
/dev/tape 当前默认磁带机
/dev/hd[a-d] IDE接口硬盘（淘汰后可能会被模拟成/dev/sd[a-p]
```

文件权限
----------

### 文件权限

* 用户继承组的权限

| 字符 | 含义 | 用在文件含义     | 用在目录含义                                |
| ---- | ---- | ---------------- | ------------------------------------------- |
| r    | 读取 | 可读取文件内容   | 可列出目录内容                              |
| w    | 写入 | 可修改文件内容   | 可在目录中创建、删除文件                    |
| x    | 执行 | 可以作为命令执行 | 可访问目录内容 (目录必须有执行权限才能读取) |

### chown # 修改文件目录所属用户

```bash
# 修改所属用户
chown user file|dir

# 同时修改用户和组
chown user:group file|dir

## 只修改组
chown .group file|dir

# 参数
-R # 递归目录  
-h # 改变link的属性  
```

### chgrp # 修改文件、目录所属组

```bash
chgrp group file
-R # 递归目录
```

### chmod # 修改权限

```bash
### 格式
chmod u[+|-|=][r|w|x],g[+|-|=][r|w|x],o[+|-|=][r|w|x],a[+|-|=][r|w|x] file
chmod [4+2+1][4+2+1][4+2+1] file

### 参数
-R #递归目录  
```

### umask

* umask各用户分别配置
* 普通用户默认umask：002 # rw-rw-r--
* root用户默认umask：022 # rw-r--r--
* 文件最终权限为 666 - umask，**文件默认不能拥有执行权限**
* 目录最终权限为 777 - umask

```bash
# 获取新建文件及文件夹默认权限
umask # 0002

# 字符显示掩码
umask -S 
# u=rwx, g=rwx, o=rx

# 临时设置umask
umask xxxx 
# 目录默认权限 = rwxrwxrwx - umask  
# 文件默认权限 = rw-rw-rw- - umask  

# umask默认值设置
# /etc/bashrc  
```

### 特殊权限

```bash
# suid(4)
# 用途：运行程序的进程用户为文件自身用户，如用于passwd修改root拥有的/etc/passwd等
# suid 占用Ux位为s，如/bin/passwd文件
1、只对可执行文件有效  
2、同时需要执行者具有可执行权限  
3、仅在运行过程中有效  
4、执行时执行者具有拥有权限  

# sgid(2)
# 用途：
# sgid 占用Gx位为s  

# 用在文件上
# 用途：运行程序的 进程组 为 文件自身的 进程组
1、仅对二进制程序有效  
2、执行者需要对文件具备可执行权限  
3、执行者在执行过程中将会获得组权限  

# 用在目录上时
# 用途：在此目录中创建的文件和目录为 此目录的用户组
1、使用者若对此目录有rx权限时，使用者能够进入此目录  
2、使用者在目录下的有效群组将会变成该目录的群组  
3、用途：使用者在此目录下具有w的权限（可以新建文件），则使用者所建立的新文件，该新文件的群组与目录群组相同  

# sticky(1)
# 用途：公共临时文件目录，每个用户都能创建、删除属于自己的文件，但是不能删除别人创建的文件
# stick 占用Ox位为t，如/tmp的other权限为t
仅可以删除自己的文件，不可以删除其他用户文件  
1、只对目录有效  
2、当使用者对此目录有wx权限时，使用者在此目录下简历的文件或目录只有自己与root有权删除  

# 设置特殊权限
chmod u+s file # 原有执行权限显示为s,否则为S
chmod g+s file  
chmod g+s dir  
chmod o+t dir
```

## 用户管理

```bash
# 显示当前用户（linux特色，命令越长，信息越少）
whoami # root

# 显示已经登录的用户，登录时间
who

# 显示当前登录用户所有信息
w

# 显示所有用户登录信息
lslogin

# 显示登录历史记录
last
last | less
last | head
```

### 权限配置文件

* /etc/passwd # 用户信息  
* /etc/shadow # 用户密码  
* /etc/group # 用户组信息  

### 切换用户ID组ID为USER

```bash
# switch user, 默认切换为root  
su 

# 切换为指定用户
su root

# ubuntu下切换为root用户
sudo su

# 使用shell登录切换用户（切换时携带环境变量）
su - # su -l, su --login
```

### 用户编号

* 用户ID，32位，0-60000
* 用户ID = 0，root
* 用户ID = 1~499，系统用户，没有shell，为服务创建的用户
* 用户ID = 500+，普通用户  

### 创建用户，useradd

```bash
# 创建用户
useradd yingxuanxuan
# 不指定-g会自动创建同名用户组

# 创建用户并设置用户组
useradd -G minor_group1, minor_group2 -g main_group yingxuanxuan

# 参数
-g, --gid GROUP # 指定主用户组名
-G, --groups 附属组1, 附属组2
-N, --no-user-group # 不建立以用户名称为名的用户组

-c, --comment COMMENT # 备注
-u, --uid userid # 指定用户id
-e, --expiredate EXPIRE_DATE # 用户过期时间

-d, --home-dir HOME_DIR # 家目录，指定登录目录
-m, --create-home # 自动建立登录目录
-M, --no-create-home # 不自动建立用户登录目录
```

### 删除用户，userdel

```bash
# 删除用户
userdel yinguanxuan

# 强制删除用户，即使当前已经登录
userdel -f

# 慎用
# 删除用户同时删除家目录及其中文件
userdel -r
```

### 修改用户，usermod

* 用户在线时不允许修改
* 修改user id时必须确保没有任何该id的程序正在运行
* crontab和at需要手动修改

```bash
# 修改用户的用户名
usermod -l new_username old_username

# 添加用户到用户组
usermod -G group user

# 锁定用户
usermod -L user

# 解除锁定
usermod -U user
```

### 修改用户组信息，gpasswd

* 修改/etc/group 和/etc/gshadow

```bash
# 设置组密码
gpasswd groupname  

# 取消用户组密码
gpasswd -r groupname 

# 将用户从用户组删除
gpasswd -d user group

# 添加普通用户到用户组
gpasswd -a user wheel

# 添加管理员到用户组
gpasswd -A user1 user2 user3 wheel

# 添加用户组成员
gpasswd -M user1 user2 user3 wheel 
```

### 修改密码 # passwd

* 只有管理者才能指定用户，普通用户只能修改自己

```bash
# 修改密码
passwd # 修改当前用户密码  
passwd username # 修改指定用户密码  
sudo passwd # 设置root默认密码  

# 删除密码
passwd -d username

# 查看密码状态
passwd -S username

# 锁定用户不能修改密码
passwd -l username
```

### 批量修改密码 # chpasswd

```bash
# 重定向修改密码
echo "user:pwd" | chpasswd

# 从文件导入密码
chpasswd < passwd.txt

# 参数
-e # 导入的密码是加密后的密文
```

### 创建用户组 # groupadd

```bash
groupadd groupname

# 参数
-g # 指定组id
-r # 创建系统工作组（ID小于500）
```

### 修改组信息 # groupmod

```bash
# 修改组名
groupmod -n new_name 
```

### 删除用户组 # groupdel

```bash
# 删除组
groupdel [groupname]
```

### 





## 服务管理

### Init程序的类型（历史）

* SysV: init, CentOS 5, /etc/inittab
* Upstart: init, CentOS 5, /etc/inittab, /etc/init/*.conf
* Systemd: systemd, CentOS7, /usr/lib/systemd/system, /etc/systemd/system



### 比较System V与Systemd

- Linux操作系统的开机过程是，从BIOS开始，然后进入Boot Loader，再加载系统内核，然后内核进行初始化，最后启动初始化进程。初始化进程作为Linux系统的第一个进程，它需要完成Linux系统中相关的初始化工作，为用户提供合适的工作环境。红帽RHEL 7系统已经替换掉了熟悉的初始化进程服务System V init，正式采用全新的systemd初始化进程服务。如果您之前学习的是RHEL 5或RHEL 6系统，可能会不习惯。

- 优点：
  systemd初始化进程服务采用了并发启动机制，开机速度得到了不小的提升。

- 缺点：
  * 1：systemd初始化进程服务的开发人员Lennart Poettering就职于红帽公司，这让其他系统的粉丝很不爽。
  * 2： systemd初始化进程服务仅仅可在Linux系统下运行，“抛弃”了UNIX系统用户。
  * 3：systemd接管了诸如syslogd、udev、cgroup等服务的工作，不再甘心只做初始化进程服务。
  * 4：使用systemd初始化进程服务后，RHEL 7系统变化太大，而相关的参考文档不多，令用户着实为难。

### 运行级别与target概念比较

- 无论怎样，RHEL 7系统选择systemd初始化进程服务已经是一个既定事实，因此也没有了“运行级别”这个概念，Linux系统在启动时要进行大量的初始化工作，比如挂载文件系统和交换分区、启动各类进程服务等，这些都可以看作是一个一个的单元（Unit），systemd用目标（target）代替了System V init中运行级别的概念，这两者的区别如下：

| System V init运行级别 | systemd目标名称   | systemd 目标作用 |
| --------------------- | ----------------- | ---------------- |
| 0                     | poweroff.target   | 关机             |
| 1                     | rescue.target     | 单用户模式       |
| 2                     | multi-user.target | 多用户的文本界面 |
| 3                     | multi-user.target | 多用户的文本界面 |
| 4                     | multi-user.target | 多用户的文本界面 |
| 5                     | graphical.target  | 多用户的图形界面 |
| 6                     | reboot.target     | 重启             |
| emergency             | emergency.target  | 救援模式         |

* target则直接使用软连接设置默认启动方式，如想要将系统默认的运行目标修改为“多用户，无图形”模式，可直接用ln命令把多用户模式目标文件连接到/etc/systemd/system/目录


```bash
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target
```



### 服务管理概念比较

systemctl管理服务的启动、重启、停止、重载、查看状态等常用命令：

| 老系统命令          | 新系统命令              | 作用                           |
| ------------------- | ----------------------- | ------------------------------ |
| service foo start   | systemctl start httpd   | 启动服务                       |
| service foo restart | systemctl restart httpd | 重启服务                       |
| service foo stop    | systemctl stop httpd    | 停止服务                       |
| service foo reload  | systemctl reload httpd  | 重新加载配置文件（不终止服务） |
| service foo status  | systemctl status httpd  | 查看服务状态                   |

systemctl设置服务开机启动、不启动、查看各级别下服务启动状态等常用命令：

| 老系统命令        | 新系统命令                             | 作用                               |
| ----------------- | -------------------------------------- | ---------------------------------- |
| chkconfig foo on  | systemctl enable httpd                 | 开机自动启动                       |
| chkconfig foo off | systemctl disable httpd                | 开机不自动启动                     |
| chkconfig foo     | systemctl is-enabled httpd             | 查看特定服务是否为开机自启动       |
| chkconfig --list  | systemctl list-unit-files --type=httpd | 查看各个级别下服务的启动与禁用情况 |

## 网络连接

### nmtui 图形化网卡设置（centos6使用setup）

```bash
# 安装
yum install NetworkManager-tui

# 使用
nmtui

setup
```

### hostnamectl，主机信息

* 默认使用/etc/hostname
* 不存在时则使用DNS反响解析获取主机名
* 主机名默认为 localhost.localdomain

```bash
# 查看主机名
hostname 

# 设置主机名
hostname name

# 查看主机信息
hostnamectl
hostnamectl status

# 设置主机名称，写入配置文件 /etc/hostname
hostnamectl set-hostname centos7
```

### nmcli # NetworkManager服务器客户端

```bash
# 控制NetworkManager
nmcli networking on
nmcli networking off
nmcli networking connectivityh # none / portal / limited / full / unknown

# 控制无线网
nmcli radio all on
nmcli radio all off
nmcli radio wifi on
nmcli radio wwan on

# 查看所有设备状态
nmcli device status

# 显示设备详细状态
nmcli device show enp0s3

# 控制设备状态
nmcli device connect enp0s3
nmcli device disconnect enp0s3

# 显示所有连接
nmcli connection

# 显示连接所有信息
nmcli connection show "有限连接 1"

# 重新载入配置
nmcli connection reload

# 控制连接
nmcli connection delete enp0s3
nmcli connection down enp0s3
nmcli connection up enp0s3

# 显示所有活动连接
nmcli connection show --active

# 增减ip
nmcli connection modify enp0s3 +ipv4.addresses 192.168.1.1/24
mncli connection modify enp0s3 -ipv4.addresses 192.168.1.1/24

# 设置DNS
nmcli connection modify enp0s3 ipv4.dns 114.114.114.114
nmcli connection modify enp0s3 -ipv4.dns 114.114.114.114

# 设置网关
nmcli connection modify enp0s3 ipv4.gateway 192.168.1.1

# 设置网络自动/手动
nmcli connection modify enp0s3 ipv4.method manual # BOOTPROTO=none
nmcli connection modify enp0s3 ipv4.method auto # BOOTPROTO=dhcp


```

#### 专题：设置自动获取IP

```
nmcli connection modify eth0 connection.autoconnect yes
nmcli connection modify eth0 ipv4.method auto
nmcli connection up eth0
```



### Systemd 网卡命名方式

```bash
# 传统网卡命名机制
传统的网卡命名方式：
以太网卡 eth [0,1,2,...]
无线网卡 wlan [0,1,2,...]

# Redhat7网卡命名机制
Systemd对网卡的命名方式：
规则1：如果从BIOS中能够取到可用的，板载网卡的索引号，则使用这个索引号命名。例如：eno1
规则2：如果从BIOS中能够取到可以用的，网卡所在的PCI-E热插拔插槽的索引号，则使用这个索引号命名。例如：ens1
规则3：如果硬件接口的位置信息可用，则根据此信息进行命名。例如：enp2s0
规则4：根据MAC地址命名，默认不开启。例如：enx2387a1dc56
规则5：上述均不可用时，则使用传统命名机制。
注意：上述命名机制中，有的需要biosdevname程序的参与。所以必须安装biosdevname程序且启用它，默认启用。

# 网卡名称的组成格式：
前两个字母标识固件
以太网卡以 en 开头
无线网卡以 wl 开头
后一个字母标识设备结构
o：主板上集成的设备的设备索引号
s：扩展槽的索引号
p s：基于拓扑的命名。如enp2s1，表示PCI总线上第2个总线的第1个插槽的设备索引号
x：基于MAC地址的命名
```

### ip

#### ip link # 链路层配置

```bash
# 显示所有网络接口
ip link
ip link show

# 显示网口
ip -s link show
ip -s link show enp0s3

# 开关网卡
ip link set enp0s3 up  
ip link set enp0s3 down  

# 组播开关
ip link set dev enp0s3 multicast on
ip link set dev enp0s3 arp on
ip link set dev enp0s3 arp off

# 设置网卡
ip link set enp0s3 address aa:aa:aa:aa:aa:aa  
ip link set enp0s3 name eth0
ip link set enp0s3 mtu 1500  

# 过滤所有网络接口
ip link | grep -E '^[0-9]' | awk -F: '{print $2}'
```

#### ip neigh # arp表配置

```bash
# 查看arp表
ip neigh
ip neigh show
ip neighbor dev eth0
ip neighbor show 192.168.0.0/24

# 添加
ip neighbor add 192.168.1.1 dev enp0s3

# 删除
ip neighbor del 192.168.1.1 dev enp0s3
```

#### ip address # 网络层配置

```bash
# 显示网络层
ip a
ip addr
ip address show  
ip address show eth0  

# 设置IP
ip address add 192.168.1.1 dev enp0s3
ip address add 192.168.1.1/24 broadcat 192.168.1.255 dev eth0 label eth0:newip  

# 删除IP
ip address del 192.168.1.1 dev enp0s3
ip address del 192.168.1.1/24 dev eth0
```

#### ip route # 路由配置

```bash
# 显示路由
ip route
ip route show  
ip route show dev enp0s3

# 添加路由
ip route [add|del] [ip|net] [var gatewayip] [dev eth]  
ip route add 192.168.1.1/24 dev eth0  
ip route add 192.168.1.1/24 var 192.168.1.254 dev eth0  

# 添加默认路由
ip route add default var 192.168.1.254 dev eth0  

# 修改默认路由
ip route chg default var 192.168.1.1

# 删除路由
ip route del 192.168.1.1/24  

# 删除默认路由
ip route del default

# 清楚路由表
ip route flush
```

#### 专题：通过端口号查找进程ID

```

```

### ss，Socket Statics，获取socket统计信息，替代netstat

* ss可以显示更多有关tcp和连接状态的信息，比netstat更加快速高效
* 当服务器socket连接数量非常大时，使用`netstat`和`cat /proc/net/tcp`速度都会特别慢，而ss使用tcp_diag保证了ss的快捷高效。

```bash
# 默认，输出已建立的连接，包括tcp，udp，unix socket
ss 

# 显示统计信息
ss -s # --summary

# tcp查询
ss -t # 显示已连接状态的tcp连接
ss -t -a # 包括显示listen状态的tcp连接
ss -tl # 只显示监听状态的tcp连接

# udp查询
ss -uanp

# 查询unix socket
ss -x

# 参数
-p, --processes # 显示用户，进程名称，pid，fd，需要root用户
-r, --resolve # 尝试解析地址，端口号
-n, --numeric # 不解析服务名称
-a, --all # listening and non-listening(for TCP means established connections)
-l, --listening # 显示监听状态socket，默认不显示
-4, --ipv4
-6, --ipv6

# 状态过滤器
# 按状态过滤
ss state established '( dport = :ssh or sport = :ssh)'
ss state fin-wait-1 '( sport = :http or sport = :https )' dst 193.233.7/24
# 按源目的地址过滤
ss dst 192.168.25.100
ss dst 192.168.25.100:50460
ss src 192.168.25.140

# tcp 状态
# 全部状态 
all

# 分解状态
established
syn-sent
syn-recv
fin-wait-1
fin-wait-2
time-wait
closed
lose-wait
last-ack
listen
closing

# 连接状态
connected # 包括 listen, closed
```

### netstat（废弃），使用ss替代

* netstat -r 被 ip route替代：netstat --route / netstat -r，显示核心路由表，等同于route -e
* netstat -i 被 ip -s link 替代：显示指定接口的网络状态
* netstat -g 被 ip maddr替代：netstat --groups 或 netstat -g，显示多播分组 
* 整体功能分类：

```bash
# netstat不带参数：显示所有协议打开状态的socket
netstat

# 显示进程 PID/名称
netstat -p

# 持续刷新显示
netstat -c # 可结合grep持续监测

# 不解析 主机、端口、用户名
netstat -n
netstat -an | grep ':80'
netstat -a --numeric-ports # 不解析端口
netstat -a --numeric-hosts # 不解析主机
netstat -a --numeric-users # 不解析用户名

# 列出所有端口（包括监听的，未监听的（tcp已经建立连接的））
netstat -a
netstat -at # 列出所有tcp端口
netstat -au # 列出所有udp端口

# 列出所有监听端口
netstat -l
netstat -lt # tcp
netstat -lu # udp
netstat -lx # unix

# 显示统计信息
netstat -s
netstat -st # tcp
netstat -su # udp
```


### ifconfig（废弃），临时修改网络设置（修改内存）

```bash
# 安装
yum install net-tools

# 显示所有活动网卡信息
ifconfig
ifconfig -s # 简要网口信息

# 查看所有网口信息
ifconfig -a

# 查看某网口的信息
ifconfig enp0s3

# 开启网口
ifup enp0s3
ifconfig enp0s3 up 

# 关闭网口
ifdown enp0s3
ifconfig enp0s3 down 

# 设置mtu
ifconfig enp0s3 mtu 1500 

# 设置越点数
ifconfig enp0s3 metric 3

# 设置arp协议
ifconfig enp0s3 arp # 开启arp
ifconfig enp0s3 -arp # 关闭arp

# 设置混杂模式
ifconfig enp0s3 promisc
ifconfig enp0s3 -promisc

# 设置子网掩码
ifconfig enp0s3 192.168.1.3/24  # 24为子网掩码
ifconfig enp0s3 192.168.1.3 netmask 255.255.255.0 

# 修改/设置 IP地址
ifconfig enp0s3 192.168.1.3

# 单网卡，多ip设置
ifconfig enp0s3:0 192.168.1.100
ifconfig enp0s3:1 192.168.1.101

# 临时修改mac地址，会关闭网口
ifconfig enp0s3 hw ether ab:cd:ef:gh:ij:kl
ifconfig enp0s3 up
```

### MAC层

#### arp协议

* ARP，Address Resolution Protocal，地址解析协议
* 协议原理：
  1. 初始时源主机不知道目的主机MAC。
  2. 源主机发送一条MAC地址广播，目的IP为目的主机IP的报文
  3. 目的主机收到与自身IP相同的报文，响应一条ARP应答，携带自身MAC地址。
  4. 源主机收到这条报文，在ARP表中建立ARP表项。
  5. 目的主机与源主机不在同一广播区的话，只找网关。

#### arp（废弃），修改arp缓存，使用`ip neigh`替代

```bash
# 显示当前地址映射表
# /proc/net/arp
arp
arp -e # linux style
arp -a # BSD style
arp ip
arp host

# 添加ARP记录（绑定mac地址）
arp -s IP MAC

# 删除ARP记录
arp -d host
arp -d ip

# 参数
-n, --numeric # 不解析地址
-v, --verbose
-i, --device interface # 指定网卡
```

#### 专题：arp攻击

* 什么是ARP攻击
  * arp协议利用网络包获取IP与MAC地址映射关系
  * 黑客不断发送包含错误MAC地址的网络包，导致被攻击服务器的地址映射表错误
* 如何预防ARP攻击
  1. ARP双向绑定，在pc端绑定 IP + MAC，网络设备端绑定 IP + MAC + 端口。
  2. 使用DHCP，并建立ARP过滤防火墙
  3. 划分安全区域，使得ARP广播包不能跨子网传播。
* 如何解决arp攻击

```bash
# 1. 查看当前地址映射表
arp -a

# 2. 清除arp记录
arp -d hostname
arp -d ip

# 3. 手动绑定网关
arp -s 1.1.1.1 ab-cd-ef-gh-ij-kl
arp -s 1.1.1.1 ab:cd:ef:gh:ij:kl
```

#### arping 检测连通性，发送、接受arp消息

```bash
# 检测目的IP的MAC地址
arping -I enp0s3 REMOTE_IP

# 重复检测模式
arping -D -I enp0s3 REMOTE_IP

# 更新邻近主机的本机MAC地址
arping -A -I enp0s3 LOCAL_IP
arping -U -I enp0s3 LOCAL_IP

# 参数
-f # 首次响应停止
-c count # 指定次数
```

### 网络层

#### traceroute 网络跟踪

```bash
# 默认使用icmp协议进行路由探测
traceroute www.baidu.com
traceroute -I www.baidu.com

# 参数
-m max_ttl, --max-hops=max_ttl # 限制跳数
-n # 不反查主机名
-p port, --port=port # udp检测
-T # 使用tcp协议80端口的syn进行路由探测
-i interface # 指定网络接口
-w waittime # 指定等待应答时间，默认5s
-g gate # 指定网关

```

#### tracepath 

```bash

```





#### mtr，my trace route，连通性直观检测

```bash
# 查看到网关的连接状态
mtr

# 查看连接路径
mtr www.baidu.com
mtr 192.168.1.1

# 报告模式，根据-c count输出报告
mtr -r -c 1 www.baidu.com
mtr -w -c 1 www.baidu.com
```

#### route（废弃），显示内核路由表，使用 ip route替代

```bash
# 显示所有路由
# /proc/net/ipv6_route
# /proc/net/route
# /proc/net/rt_cache
route 

# 显示详细信息
route -ee 

# 添加环回路由，省略掩码255.0.0.0
route add -net 127.0.0.0

# 想eth0添加一条指向网络192.168.100.0的路由
# 可省略掩码255.255.255.0
# 可省略关键字dev
route add -net 192.168.100.0 netmask 255.255.255.0 dev eth0  

# 添加默认网关
route add default gw 192.168.1.250

# 添加一条阻塞路由（屏蔽一条路由）
route add 10.0.0.0 netmask 255.0.0.0 reject

# 删除网关
route del -[net|host] netmask [gw|dev]  
route del -net 192.168.100.0 netmask 255.255.0.0 dev eth0  

# 添加网关/设置网关
route [-A family] add -[net|host] target_ip [netmask NM] [gw GW] [metric N] [reject] [[dev] IF]
route [-A family] del -[net|host] target_ip [gw GW] [netmask NM] [metric N] [[dev] IF]

# 参数
-A [inet | inet6] # 指定地址族
-n # 不解析主机名称
-net # 路由目标为网络
-host # 路由目标为主机

### Flags  
U：（route is up），表示启动状态的路由
H：（target is a host），表示此网关为一主机
G：（use gateway），表示此网关为一路由器
R：（reinstate route for dynamic routing），使用动态路由重新初始化的路由
D：（dynamically installed by daemon or rederict），此路由是动态地写入
M：（modified from routing daemon or rederict），此路由由路由守护程序或导向器动态修改
!: （reject route）此路由当前为关闭状态（阻塞路由）

```

#### ping，检测网络是否科大，需要目标支持ICMP协议

```bash
# 检测指定次数
ping -c count ip  

# 极限测试
ping -f ip

# 广播测试
ping -b 192.168.1.255

### MTU检测
ping -M do/dont # 侦测mtu，do，发送don't fragment标记，dont，不发送don't fragment标记  
ping -s 1472 -M do ip #检测MTU是否为1500，ip报头20+icmp报头8+icmp长度1472  

# 参数
-i # 指定间隔时间
-I # 指定网卡
-s # 指定ping包大小
-t # 指定ttl
-n # 不解析
```

### 应用层

* host 和 nslookup 命令在 bind-utils包中

#### host

```bash
# 解析域名
host www.baidu.com   
host 8.8.8.8

# 显示详细主机信息
host -a www.baidu.com
host -t any www.baidu.com

# 使用特定dnsip查询  
host www.baidu.com 8.8.8.8

# 参数
-t # 使用-t参数指定查询类型
-v # 显示详细信息
```

#### nslookup 互动式查询

``` bash
# 查询域名
nslookup www.baidu.com  
```

#### dig 查询域名服务器

```bash

```



## 网络安全

```
tcpdump # 抓包工具
wget url # 下载

mii-tool / 
/etc/resolv.conf # 设置dns
/etc/hosts # 域名地址映射表

# CentOS6
iptables -A INPUT -p tcp --dport 22 -j ACCEPT  
iptables -I INPUT 1 -p tcp --sport 53 -j ACCEPT  
iptables -I INPUT 1 -p udp --sport 53 -j ACCEPT  
iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT  
iptables -A INPUT -j DROP  

# CentOS7
firewall-cmd --zone=public --add-port=80/tcp --permanent  
firewall-cmd --reload
firewall-cmd --list-all
```



## 磁盘管理

```bash
df -hT path # 检查文件系统的磁盘占用
-a # 列出所有文件系统，/proc等
-k -m # 以k、m为单位显示
-h # human readable 文件大小单位
-H # 以1000为进制显示文件大小
-T # type，显示格式化类型
-i # inode，显示inode数量

du -ah path # 显示文件的大小
-a # 列出所有文件和目录的容量，默认仅文件
-h # human readable
-s # 仅显示总量，不分目录显示
-S # 仅显示总量，不包含目录下的占用
-k -m -b# 以k、m、byte显示

mount -t type -L Label -o other -n device mountpoint
mount /dev/hdc6 /mnt/hdc6
umount /dev/hdc6
umount -f /dev/hdc6
/etc/fstab # 设置开机挂载
```

## 软件管理

### 软件包管理

- 在RPM（红帽软件包管理器）公布之前，要想在Linux系统中安装软件只能采取源码包的方式安装。早期在Linux系统中安装程序是一件非常困难、耗费耐心的事情，而且大多数的服务程序仅仅提供源代码，需要运维人员自行编译代码并解决许多的软件依赖关系，因此要安装好一个服务程序，运维人员需要具备丰富知识、高超的技能，甚至良好的耐心。而且在安装、升级、卸载服务程序时还要考虑到其他程序、库的依赖关系，所以在进行校验、安装、卸载、查询、升级等管理软件操作时难度都非常大。
- RPM机制则为解决这些问题而设计的。RPM有点像Windows系统中的控制面板，会建立统一的数据库文件，详细记录软件信息并能够自动分析依赖关系。目前RPM的优势已经被公众所认可，使用范围也已不局限在红帽系统中了。
- rpm是一个包管理器，全称Red Hat Package Manager，用于生成、安装、查询、核实、更新、卸载单个软件包。
- 一个软件包通常包括一个文档一级包的信息，比如名字，版本，描述等。
- 使用rpm的发行版包括Red Hat，CentOS，Fedora，SuSE等。
- rpm只检查依赖关系，yum能解决依赖关系。

#### 基础知识

* 两大包管理器
  * dpkg，debian，B2D，Ubuntu包管理器
  * rpm，redhat package manager，fedora，centos，suse
* 依赖管理器
  * yum，rpm依赖管理器，rpmbuild
  * apt，dpkg依赖管理器，apt-get
* rpm与srpm
  * rpm是已经编译好的，特定平台的包，*.rpm
  * srpm是未编译的源代码包，*.src.rpm
* srpm与tar包区别
  * srpm含有依赖性说明，tar包没有依赖性说明
* 软件包命名
  * rp-pppoe-3.11-5.el7.x86_64.rpm
  * 软件名称 - 版本号 - 特定平台编译版本次数（bug修复）- 硬件平台 -  后缀名
* 硬件平台
  * i386 # 适用于所有386平台
  * i586 # 针对586编译优化，如奔1 Intel MMX，AMD K5/K6 
  * i686 # 针对686编译优化，如奔2，AMD K7  
  * x86_64 # 针对64位CPU编译优化，如Intel Core2，AMD Athlon64  
  * noarch # 没有硬件限制，无二进制文件，如shell script   
* 二进制版本与开发用途版本
  * pam-x.x.rpm
  * pam-devel-x.x.rpm，开发用途版本

#### rpm包搜索

<http://rpmfind.net/>

<https://pkgs.org/>

<https://www.rpmseek.com/>

<http://rpm.pbone.net/>

#### rpm -i / rpm -U / rpm -F # 安装和升级模式 

```bash
# 安装包
rpm -ivh packagename 
rpm -ivh http://url/pkg.rpm

# 升级或安装包
rpm -Uvh packagename

# 只升级包，不存在则不会安装
rpm -Fvh packagename
rpm [-F|--freshen] [install-options] <package_files>+

# 参数解释
-i # install
-v # visual
-h --hash # 如果没有被解压，打印50个破折号
--percent # 显示解压百分比
--force # 强制安装
--nodeps # 不检查依赖
--noscripts # 不执行安装前、安装后脚本
--notriggers # 不执行安装触发脚本
--test # 不安装包，只检查冲突
--oldpackage # 同--force，允许使用旧版本的包取代较新的版本
--replacefiles # 同--force，允许取代已安装的文件
--replacepkgs # 同--force，安装已经安装的包

# 路径修改
--excludepath <path> # 不安装以路径<path>开头的文件
--prefix <path> # 修改安装路径到<path>
--relocate <oldpath>=<newpath> # 修改默认安装路径
```

#### rpm -q # 查询模式 

```bash
# 列出所有已安装的rpm包
rpm -qa
rpm -qa | grep keyword

# 查询已安装的rpm包的，详细信息
rpm -qi pkg
rpm -qil python
rpm -qlicdR pkg

# 查询未安装的rpm包的，详细信息
rpm -qpi pkg.rpm

# 列出未安装的rpm包的，所含文件
rpm -qpl pkg.rpm

# 列出已安装的rpm包的，所含文件
rpm -ql package

# 查询文件（全路径）属于哪个已安装的rpm包
rpm -qf filepath
rpm -qf /etc/yum.conf

# 参数解释
-a, --all # 查询所有安装的包
-i # information, 显示包的信息，包括名字、版本、描述
-R, --requires # 显示依赖包
--provides # 列出包含的命令
--changelog # 显示包变更信息

-l, --list # 列出包含的所有文件
-d, --docfiles # 文档，包含-l
-c, --configfiles # 配置，包含-l
--scripts # 脚本
--triggers, --triggerscripts # 触发脚本

-f, --file <file> # 查询包含文件<file>的包
-p <package_file> # 查询一个没有安装的rpm中的信息，加载包头，支持ftp和http在线查询
```

#### rpm -e # 卸载模式

```bash
# 卸载
rpm -e packagename # erase

# 强行卸载
rpm -e --nodeps yum

# 参数
--test # 测试卸载
-vv # 配合--test显示卸载详细信息
```

### 软件仓库管理 # yum/dnf工具

* 尽管RPM能够帮助用户查询软件相关的依赖关系，但问题还是要运维人员自己来解决，而有些大型软件可能与数十个程序都有依赖关系，在这种情况下安装软件会是非常痛苦的。Yum软件仓库便是为了进一步降低软件安装难度和复杂度而设计的技术。Yum软件仓库可以根据用户的要求分析出所需软件包及其相关的依赖关系，然后自动从服务器下载软件包并安装到系统。
* 全称 Yellowdog Updater Modifed
* yum在未完成安装的情况下强行终止安装过程，下次再安装时将无法解决依赖关系，所以使用dnf替代yum。

#### yum 安装

```bash
yum install package1

# 无需同意安装软件包
yum install -y package 

# 安装本地的rpm包, 如果有依赖关系, 会自动从软件仓库中下载所需依赖
yum localinstall /mnt/cdrom/Packages/httpd-2.4.6-67.el7.x86_64.rpm 

# 安装网络上rpm包
yum install https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/vsftpd-3.0.2-25.el7.x86_64.rpm

# 安装指定库中软件包
yum --disablerepo* --enablerepo-Local package

# 重新安装软件包
yum reinstall package

# 安装整组软件
yum groupinstall group1

# 参数说明
-y, --assumeyes
--nogpgcheck # 禁用公钥签名验证
--installroot=/path # 安装到非默认路径
--enablerepo=epel
--disablerepo=*
--noplugins # 禁用插件
```

#### yum更新

```bash
# 更新全部
yum update
yum update -y package

# 安全更新
yum --security check-updaste
yum --security update # 更新安全级别
yum --security update-minimal

# 更新指定包
yum update package
yum upgrade package

# 更新到指定版本
yum update-to package-version
yum downgrade package

# 检查更新
yum check-update

# 更新组
yum groupupdate group
```

#### yum查找、显示信息

```bash
# 查找软件包
yum search keyword # 按关键词查询软件包的名字和简介
yum search all keyword # 按关键词查询软件包全部信息
yum provides feature # 查找软件包
yum whatprovides feature

# 列出全部软件
yum list
# 按关键字列出软件，支持通配符
yum list python*

# 按分类列出软件包
yum list installed
yum list all
yum list available
yum list updates

# 查看包信息
yum info python

# 查看组信息
yum groupinfo group

# 查看当前使用的仓库
yum repolist

# 查看所有的仓库列表
yum repolist all
yum repolist enabled
yum repolist disabled

```

#### 卸载软件

```bash
# 卸载软件包
yum remove package
yum remove -y package
yum erase package

# 整组卸载
yum groupremvoe group
```

#### 其他

```bash
# 查看软件包依赖
yum deplist package

# 构建元信息缓存
yum makecache

# 清除所有仓库缓存
# /var/cache/yum
yum clean all

# 清除缓存软件包
yum clean packages
# 清除软件包信息头
yum clean headers
# 清除旧软件包信息头
yum clean oldheaders
```

#### 组管理

```bash
# 查看系统中已经安装的软件包组
yum grouplist 

# 安装软件包组
yum groupinstall group

# 移除软件包组
yum groupremove group

# 查看软件包组信息
yum groupinfo group

# 也可以在yum.conf中配置默认选项
yum --setopt=group_package_types=mandatory,default,optional groupinstall "Performance Tools"   
```

#### 专题：安装第三方软件仓库epel

```bash
yum install epel-release
```

#### 专题：repoquery 从软件仓库查询软件仓库

```bash
# 查询软件包中所有文件
repoquery -q -l dhcp

# 参数
-l， --list # 列出包中所有文件
```





#### 专题：自定义软件仓库

```bash
# 仓库公共配置
/etc/yum.conf
/etc/dnf.conf

# 软件仓库地址配置
/etc/yum.repo.d/*.repo

# 配置光盘为软件仓库
## 挂载光盘
mount -o ro /dev/sr0 /mnt
echo "mount -o ro /dev/sr0 /mnt" >> /etc/rc.local; chmod u+x /etc/rc.d/rc.local # 或
echo "/dev/sr0 /mnt iso9660 defaults,ro 0 0" >> /etc/fstab
## 配置软件仓库
```local.repo
[local] # 仓库名称
baseurl=file:///mnt
enabled=1
gpgcheck=0
```

## 配置网络源软件仓库

```aliyun.repo
[aliyum]
name=aliyum
baseurl=http://mirrors.aliyun.com/centos/6/os/x86_64/
enabled=1
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/6/os/x86_64/RPM-GPG-KEY-CentOS-6
```

```
### 专题：yum-cron，定期自动更新软件

```bash
# 安装
yum install yum-cron

# 打开服务
# centos7
systemctl enable yum-cron.service
systemctl start yum-cron.service
systemctl status yum-cron.service
# centos6
chkconfig yum-cron on
service yum-cron start

# 配置
# 一般小时cron什么都不做
# 日cron只会下载更新，不会安装
/etc/yum/yum-cron.conf # 如使用次配置，则每天自动下载更新文件，但是并不安装
/etc/yum/yum-cron-hourly.conf # 默认cron使用此配置文件，只检查更新，什么都不做
/etc/cron.daily/0yum-daily.cron # 修改cron改变更新时间

# 修改配置使得自动更新
apply_updates = yes

# 禁用内核更新
exclude=kernel*

# 日志
cat /var/log/cron | grep yum # 执行日志
cat /var/log/yum.log | tail # 更新日志
```

#### 专题： 自动选择最快软件仓库源 yum-plugin-fastestmirror 

```bash
# centos 5
yum install yum-fastestmirror

# centos 7 会自动安装并使用最快软件仓库源
yum install yum-plugin-fastestmirror

# 配置文件
/etc/yum/pluginconf.d/fastestmirror.conf

# 镜像地址列表
/var/cache/yum/x86_64/7/timedhosts.txt

# 镜像测试结果
/var/cache/yum/x86_64/7/timedhosts
```

#### 专题：添加中文语言支持

```bash
yum groupinstall "Chinese Support"
LANG=zh_CN.utf8
```

#### 专题：从软件仓库下载包

```bash
# centos7自带，低版本手动安装
yum install yum-plugin-downloadonly
yum install --downloadonly --downloaddir=/tmp package

# 下载源代码
yumdownloader package
yumdownloader --source package

--urls # 只显示下载地址，不真实下载
--destdir dir # 指定下载地址
--resolve # 解决依赖，一并下载
```

#### 专题：管理软件源 # yum-config-manager

```bash
# 开启源
yum-config-manager --enable epel

# 关闭源
yum-config-manager --disable epel

# 从文件添加源
yum-config-manager --add-repo ./repository.repo 

# 从网络添加源
yum-config-manager --add-repo https://www.example.com/repository.repo 

# 修改设置（永久修改--save）
yum-config-manager --setopt=option=value --save
```

vi/vim
----------

* root没有`alias vi=vim`
* 软连接共享配置文件`ln -s /home/yingxuanxuan/.vimrc /root/.`

### 模式切换状态机

```bash
一般模式 -> i, o, a, r, I, O, A, R -> 编辑模式  
一般模式 -> : / ? -> 命令行模式  
编辑模式 -> ESC -> 一般模式
指令模式 -> ESC -> 一般模式
```

### 一般模式

```bash
j k # 下上
h l # 前后

10j # 下10
10k # 上10
10h # 前10
10l # 后10

ctrl + f or PageDown # forward 下一页  
ctrl + b or PageUp # back 上一页  
ctrl + d # down 下半页  
ctrl + u # up 上半页  

Space # 向右移动
10Space # 向右移动10个字符  
Enter # 向下移动一行
10Enter #向下移动10行  

0 or Home # 行首
$ or End # 行尾

G # 文件末  
gg # 文件头 =1G  
10G # 移动到第10行


shift + h # head 最上行首字符  
shift + m # middle 中间行首字符  
shift + l # last 最下行首字符

x # 向后删除 == del  
X # 向前删除 == backspace  
10x # 向后删除10个字符  

# 删除，剪切
dd # 删除行，删除同时复制，同剪切
10dd # 向下删除10行，删除同时复制，同剪切
d1G # 从本行删到文章首
dG # 从本行删到文件末
d0 # 从行首删到本字符
d$ # 从本字符删到行末

yy # 复制行  
nyy # 复制n行  
p # 粘贴到下一行  
P # 粘贴到上一行  
J # 下一行合并到本行  

# 撤销，重做
u # undo  
ctrl + r # redo  
. # 重复前一个动作  
```

### 命令模式

```bash
/key # 向下查询  
?key # 向上查询  
n # 查询下一个  
N # 查询上一个  
:n1,n2s/key1/key2/g # 替换n1到n2间的key1为key2  
:1,$s/key1/key2/g # 替换全文的key1为key2  
:1,$s/key1/key2/gc # confirm 互动模式  

:w! #write  
:q! #quit  
ZZ #变更才存储  
:w filename #另存为  
:r filename #append文件内容到光标后  
:n1,n2 w file #部分另存为  
:!command # bash  
:!./% # 执行本文件
:set nu/nonu # 显示行号  
:e! #恢复原始状态  
:colorscheme blue # 更换主题为blue
```

### 切换到编辑模式

```bash
i # insert 插入到光标前 
a # append 插入到光标后  

I # Insert 插入到行首  
A # Append 插入到行尾  

o # 插入下一行  
O # 插入上一行  

r # replace 替换单个字符  
R # replace 替换  
```

### 区块模式

```bash
v # 进入字符选择模式
V # 进入行选择模式

Ctrl + v # 进入列编辑模式
Ctrl + v # 块选择，选择后使用//或#注释块
Ctrl + v # 快选择注释符号，按d取消注释

# 选择之后按
y # 复制  
d # 删除  
p # 粘贴  
```

### 多文档编辑

vim file1 file2 file3  
:n # next  
:N # prev  
:files # list  

### 多窗口

:sp #同文件分割窗口  
:sp file #不同文件分割窗口  
ctrl + w, w # tab  
ctrl + w, j # ctrl + w, down  
ctrl + w, k # ctrl + w, up  
ctrl + w, q # quit    

### 命令补全

ctrl + x, ctrl + n # 以上文的内容进行补齐  
ctrl + x, ctrl + f # 以当前目录下的文件名补齐 
ctrl + x, ctrl + o # 以后缀名相关的内建语法进行补齐  

### 配置文件

#### ~/.viminfo # 使用记录，上次编辑位置，搜索关键字  

:set # 显示与default不同的设置  
:set all # 查看当前设置  

#### ~/.vimrc # 配置文件 

* 注意：配置文件无需冒号，单个双引号注释 *  
  :set nu/nohu # 显示行号  
  :set hlsearch/nohlsearch # 高亮搜索关键词  
  :set backup # default nobackup filename~  
  :set ruler # 状态栏行列标尺  
  :set showmode # 显示当前模式  
  :set backspace=(012) #2:删除原本的字符 01:只能删除刚刚输入的字符  
  :syntax on/off #开启关闭语法颜色  
  :set bg=dark/light #default light  

#### tab设置

```bash
# 设置 table 键 
filetype plugin indent on 

# show existing tab with 4 spaces width
set tabstop=4             

# when indenting with '>', use 4 spaces width
set shiftwidth=4

# On pressing tab, insert 4 spaces
set expandtab

# 文件类型的相应设置
filetype plugin indent on
filetype detect

# makefile 文件时，按下 tab 键则输入一个 tab
autocmd FileType make set noexpandtab               
```

### 换行符转换

dos2unix   
unix2dos  
dos2unix -k file #保留文件mtime  
dos2unix -n oldfile newfile #另存为  

### 编码转换

#### iconv

iconv --list  
iconv -f fromcodec -t tocodec -o outputfile inputfile  

#### 繁简互转

iconv -f big5 -t gb2312  

#### utf8繁简互转

iconv -f utf8 -t big5 vi.utf8 |iconv -f big5 -t gb2312 | iconv -f gb2312 -t utf8 -o vi.gb.utf8  

### 修复ubuntu小键盘乱码

apt-get remove vim-common  
apt-get install vim  

## 文本处理

### seq，生成序列

```bash
# 格式
seq LAST
seq FIRST LAST
seq FIRST INCREMENT LAST

# 示例
seq 1
seq 3
seq 2 4 # 2 3 4
seq 1 2 5 # 1 3 5

# 参数
-s, --separator=STRING # 分隔符，默认\n
```



文件与目录管理
----------

### 当前工作目录

pwd  
pwd -P 显示链接真实路径  

### 建立目录

mkdir -m 775 dir #建立目录时设置权限  
mkdir -p /dir/dir/dir/dir #建立多级目录  

### 删除目录

rmdir -p #连同上层空目录一起删除  

### 查看环境变量

env  
echo $PATH  

### 添加PATH变量

PATH="${PATH}:/ADDPATH"  

### ls 默认按文件名排序

ls -R #递归显示子目录  
ls -a #all 显示隐藏文件  
ls -d #dir 显示目录本身  
ls -h #human readable  
ls -i #列出inode号码  
ls -l #long  
ls -r #逆序  
ls -S #按文件大小排序  
ls -t #按时间按排序  
ls --full-time #完整时间  
ls --time={atime,ctime} 默认mtime  
atime access time #访问时间  
mtime modify time #内容修改时间  
ctime status time #权限属性修改时间  
ls --color=auto #默认写进alias  

### cp # 复制

#### 权限

cp默认使用执行者的权限

#### 参数 

cp -d #将软连接任然复制为软链接，默认复制原文件  
cp -r #递归复制目录  
cp -p #连同权限，用户，时间一起复制，而非使用预设属性  
cp --preserve=all #除了cp -p外的属性还保留SELinux属性、links，xattr等  
cp -a # =cp -dr --preserve=all #全备份  
cp -f #强制覆盖无法打开的文件  
cp -l #硬链接  
cp -s #软连接  
cp -u #update，更新，比备份新才覆盖  
cp -i #存在时询问  
cp -n #存在时不覆盖，默认覆盖  

### rm

rm -f #强制  
rm -i #删除前询问  
rm -r #递归删除  

### mv

mv -f #强制  
mv -i #询问是否覆盖  
mv -u #update  

### rename

用于批量改名  
rename .htm .html *.htm  

### 临时取消alias

\alias 反斜杠  

### 取得文件名

basename path  

### 取得目录名

dirname path  

### cat(Concatenate)

cat -v #显示不可见字符  
cat -E #显示结尾换行字符$  
cat -T #显示TAB为^|  
cat -A # = cat -vET  
cat -n #显示行号，空白行有行号  
cat -b #显示行号，空白行无行号  

### tac

逆向cat  

### nl(numbering line)

-b --body-numbering=正文编号样式  
-d --section-delimiter=分割页行数  
-n --number-format=行号格式 ln rn rz  
-w --number-width=行号宽度  
nl -b a == cat -n  
nl -b t == cat -b #default  
nl -n ln #靠左显示，不加零  
nl -n rn #靠右显示，不加零  
nl -n rz #靠右显示，不加零  
nl -w num#固定补零位数  

### more

不能向前翻页  

### less

space #向下翻页   
pagedown #向下翻页  
pageup #向上翻页  
/ #向下搜索  
? #向上搜索  
n #重复前一个搜索，/向下，?向上  
N #逆向重复搜索  
j #向下一行  
k #向上一行  
g #首行  
G #末行  
q #退出  
h #help  

### head

default 10行  
head -n num file #显示前n行  
head -n -num file #显示直到倒数n行  

### tail

default 10行  
tail -n num file #显示后n行  
tail -n +num file #显示直到正数n行  
tail -f #持续检测  

### od(octal dump)

od [OPTION] [FILE] [[+|-]OFFSET[.][b]]
-A --address-radix=[doxn] #地址（行号）显示方式  
-Ad #decimal  
-Ao #octal  
-Ax #hex  
-An #none  
-w[BYTES] --width=BYTES #每行宽度（字节数） 
-t --format=[acdfoux]  
-t a #named character, ignoring hight-order bit  
-t c #printable character or bachslash escape  
-t dn #每n个字节用有符号十进制显示  
-t un #每n个字节用无符号十进制显示  
-t fn #每n个字节用浮点数显示  
-t on #每n个字节用八进制显示  
-t xn #每n个字节用十六进制显示  
n=C for sizeof(char)  
n=S for sizeof(short)  
n=I for sizeof(int)  
n=L for sizeof(long)  
n=F for sizeof(float)  
n=D for sizeof(double)  
n=L for sizeof(long double)  

### touch

默认修改modify和access  
touch -a #修改accesstime  
touch -m #修改modifytime  
touch -c #只修改时间，不创建  
touch -d #--date="时间或者日期"  
touch -t YYYYMMDDhhmm #修改为指定时间  

### chattr

改变文件系统属性  
\+ #增加参数  
\- #删除参数  
= #设定参数  
A #存取不修改atime（mount时修改更为合理）  
S #不缓存，同步写入磁盘  
a #只能增加数据，不能删除修改资料  
c #自动压缩  
i #禁止删除，改名，软连接，写入，新增  
s #完全删除，无法恢复  
u #假删除，可恢复  

### lsattr

-a #显示隐藏文件的属性  
-d #显示目录的属性，默认显示目录内的文件  
-R #递归  

### file

检测文件类型  
检测压缩类型  
解析mbr、gpt分区备份  

### which

which command #查找命令位置  
which -a command #查找所有命令位置  
type command #查找bash指令  

### whereis # 命令搜索

只查找bin man src  
whereis -l #列出whereis查找的目录  
whereis -b #只查找binary格式文件  
whereis -m #只查找manual文件  
whereis -s #只查找srouce文件  
whereis -u #搜索除bin man src之外的其他特殊文件  

### locate # 索引搜索

默认匹配全路径中包含关键字  
locate keyword #从索引中查找文件，索引每天更新一次  
locate -b keyword #只搜索basename文件名  
locate -i keyword #忽略大小写差异  
locate -c keyword #只计算找到的数量  
locate -l n keyword #只输出n行  
locate -S #输出locate数据库相关信息  
locate -r regexp #正则表达式查询（注意：正则表达式匹配整个路径，只匹配文件名需要加-b）  
updatedb #更新文件数据库  
/var/lib/mlocate/mlocate.db  

### find # 全盘搜索

find path1 path2 ... option -exec ls -l {} \; #查找并执行  

find path -[mtime|atime|ctime] n #第前n天的文件  
find path -[mtime|atime|ctime] +n #第前n天之前的文件，不含前第n天  
find path -[mtime|atime|ctime] -n #第前n天之内的文件，含前第n天  
find path -newer file #列出比file还要新的文件  
find path -mtime 0 #24小时之内修改过的文件  

find path -uid n #使用id查找 /etc/passwd  
find path -user name == find path -uid `id -u name` #使用用户名查找  
find path -gid n #使用gid查找 /etc/group  
find path -group name #使用用户组查找  
find path -nouser #查找passwd不存在的用户文件  
find path -nogroup #查找group不存在的用户组的文件  

find path -name filename #使用文件名查找  
find path -size +-num{c|k|m} #使用文件大小查找  
find path -type {b|c|d|p|f|l|s|D} #按文件类型查找  
b block (buffered) special  
c character (unbuffered) special  
d directory  
p named pipe (FIFO)  
f regular file  
l symbolic link  
s socket  
D door(Solaris)  

find path -perm mode #按权限查找  
find path -perm -mode #按必须包含的权限查找  
find path -perm /mode #按包含任意的权限查找  

#### 多个后缀名查询

find /mnt/f \( -name *.mp4 -o -name \) -exec ./ffprobe {} \;

磁盘与文件系统
----------

### MBR与GPT

#### MBR

MBR（Master Boot Record)是MSDOS的分区格式，早期Linux兼容磁盘分区方式也使用MBR  
MBR区域为512Bytes，前446Bytes为MBR主开机记录区，后64Byte为分割表（partition table）区，64Byte记录4个主分区（Primary)或延展分区（Extended）或逻辑分区logical partition，每个分区16字节，限制磁盘容量大小为2.2T。延展分区分区表记录在延展分区之前（或主分区之后）。  
/dev/sda[1-4]为主分区设备名，若有逻辑分区则越过。  
/dev/sda[5-n]为逻辑分区设备名。  

#### GPT(GUID partition table)


### dumpe2fs 显示分区格式信息

dump ext2/3/4 filesystem information  
dumpe2fs /dev/sda1 #列出文件系统信息  
dumpe2fs -h /dev/sda1 #只列出superblock  

### blkid

blkid #列出目前挂载的已经格式化的设备的UUID  

### 系统支持的文件系统

cat /proc/filesystems  
ls -l /lib/modules/$(uname -r)/kernel/fs  

### df

df -option path  
df -a #列出全部文件系统，包括虚拟文件系统  
df -{k|m} # == df -B{K|M|G|T|P|E|Z|Y} --block-size=SIZE  
df -h #humanreadable  
df -T #列出文件系统格式  
df -i #列出inode数量信息，默认列出容量信息  

### du

du -option path  
du -a #列出目录与文件大小，默认不显示文件  
du -h #humanreadabe  
du -s --summarize  
du -S --separate-dirs #不统计子目录  

### lsblk # list block device  

lsblk -option path  
lsblk -d #disk only  
lsblk -f #列出文件系统类型及UUID  
lsblk -i #使用ASCII输出关系符号  
lsblk -m #输出设备权限信息  
lsblk -p #列出设备完整名称  
lsblk -t #列出设备详细资料  

### parted 命令式分区工具

parted -l #显示所有分区  
mkpart [primary|logical|extended] [ext4|vfat|xfs] startMB endMB  
parted rm [partition] #删除  
parted device print #显示  
parted device mklabel gpt|msdos|loop #修改分区类型  

### gpt交互式分区工具

gdisk /dev/device  
? #help  
d #delete  
n #new  
p #print  
q #quit  
o #重建GPT分区表  
w #write  

### mbr交互式分区工具

fdisk /dev/device  
fdisk -l #列出已经安装的硬盘及其信息  
fdisk /dev/sdb #分区模式  
m #help  
p #print  
n #new   
n->p # new primary partition  
n->e->l # new extend ->logic partition  
t #change id  
g #重建GPT分区表  
o #重建MSDOS分区表  
w #write  

### 立即更新分区表

partprobe  
partprobe -s #打印信息  

### 查看分区信息

cat /proc/partitions  

### 备份还原MBR

#### 备份硬盘主引导记录   

``` sh
dd if=/dev/hda of=/disk.mbr bs=512 count=1  
```

#### 还原硬盘主引导记录  

``` sh
dd if=/disk.mbr of=/dev/hda bs=512 count=1  
```

### mkfs.xfs

mkfs.xfs -option /dev/device  
mkfs.xfs -b blocksize #512 - 4K  
mkfs.xfs -d parms #data section高级参数  
mkfs.xfs -f #强制格式化已经被格式化的分区  
mkfs.xfs -i parms #inode相关高级参数  
mkfs.xfs -L labelname  
mkfs.xfs -r parms #realtime section相关参数   

### mke2fs

创建分区系统/格式化  
mke2fs -t ext4 /dev/sdb1  
-b #blocksize 块大小  
-c #check badblocks 坏块  
-L #label 指定卷标  
-j #journal 建立文件系统日志  

#### 格式化相关默认值

/etc/mke2fs.conf   

#### 其他

mkfs.ext3 /dev/sdb1  
mkfs.ext4 -b size -L label /dev/sdb1  
mkfs.vfat /dev/sdb1  

#### 显示/修改lable标签

e2label /dev/sdb1 NEWNAME  

### xfs_repair

xfs_repair /dev/device  
xfs_repair -f file #检查文件   
xfs_repair -n #只检查而不修复   
xfs_repair -d #在单人维护模式下修复根目录   

### xfs_growfs

expands an existing xfs filesystem  

### fsck.ext4

fsck.ext4 -pf -b superblock /dev/device  
fsck.ext4 -p /dev/device #自动确认修复  
fsck.ext4 -f /dev/device #强制检查  

### fsck

fsck -t ext4 /dev/sdb1 #检查分区 -t指定文件系统格式，损坏严重时使用  
fsck -y /dev/sdb1 #自动修复  

### mount

mount #显示所有挂载  
mount -a #按/etc/fstab挂载  
mount -t type device mountpoint #指定挂载文件系统类型  
mount UUID=uuid dstdir  
mount LABEL=label dstdir  
mount -o option #指定挂载选项  
async, sync #非同步,同步  
atime, noatime #是否更新访问时间  
ro, rw #readonly readwrite  
auto, noauto #是否允许mount -a 自动挂载  
dev, nodev #是否允许建立dev设备  
suid, nosuid #是否允许suid sgid  
exec, noexec #是否允许执行binary  
user, nouser #是否允许一般用户执行mount  
defaults #默认值rw,suid,dev,exec,auto,nouser,async  
remount #出错后是否重新挂载  
username=Everyone  
password=''  

#### mount替硬连接

硬链接不能连接目录，不能跨文件系统  
mount --bind oldfile new file  
mount --bind olddir newdir  
umount卸载时必须使用挂载点  

#### mount samba

mount.cifs //192.168.1.5/d /mnt/d -o username=yx,password=H  
mount -t cifs //192.168.1.5/e /mnt/e -o username=yx,password=H,iocharset=utf8  

### umount

umount -f #强制卸载  
umount -l #立即卸载  
unmount device  
unmount dir  

### fuser

yum install psmisc   
identify processes using files or sockets   
fuser -m /dev/sdb1 #列出文件打开的用户   

### lsof

yum install lsof  
lsof /mnt/testsdb #list open file 列出打开文件  

### mknod

mknod device [bcp] [Major] [Minor]  
修改设备类型、设备代码  
b block  
c char  
p fifo  

### xfs_admin

xfs_admin [-lu] [-L label] [-U uuid] device  
修改xfs系列文件系统信息  
修改xfs Label UUID等配置  
xfs_admin -l #显示设备Label  
xfs_admin -u #显示设备uuid  

### uuidgen

生成uuid  

### tune2fs

tune2fs [-l] [-L label] [-U uuid] device    
修改ext系列文件系统信息  
tune2fs -l device#dumpe2fs -h 显示superblock信息  
tune2fs -L label #修改label  
tune2fs -U uuid #修改uuid  

### /etc/fstab

#### 限制：  

1、根目录必须挂载，且必须首先挂载  
2、挂载点必须已经建立目录  
3、一个挂载点只能挂载一次  
4、一个设备只能挂载一次  

#### 格式

设备/uuid/label  挂载点  文件系统类型  选项  dump是否归档  fsck  
/dev/sdb    /mnt    ext4    default    0    0  

#### fstab带帐号密码格式

//server/share /mount/point smbfs username=[username],password=[password] 0 0  

### 重新挂载root

单人模式中根目录是readonly模式使用以下命令修改文件  
mount -o remount,rw,auto /  

### 挂载光盘镜像

mount -o loop isofile dstdir  

### 文件虚拟磁盘  

dd if=/dev/zero of=filepath bs=1M count=512  
mkfs.xfs -f filepath  
mount -o loop filepath mountpoint  

### swap

分区ID设置为8200  
mkswap device  
mkswap file #文件做swap  
swapon device  
swapon -s #show  
swapon -a #加载swap  
swapoff file|device  

#### /etc/fstab  

device swap swap defaults 0 0  

### 综合：vultr磁盘分区、格式化、挂载

#### 查看

``` sh
lsblk
```

#### 分区

``` sh
parted -s /dev/vdb mklabel gpt
parted -s /dev/vdb unit mib mkpart primary 0% 100%
```

#### 格式化

``` sh
mkfs.ext4 /dev/vdb1
```

#### 挂载

``` sh
mkdir /mnt/blockstorage
echo >> /etc/fstab
echo /dev/vdb1               /mnt/blockstorage       ext4    defaults,noatime 0 0 >> /etc/fstab
mount /mnt/blockstorage
```

压缩打包备份
--------------------------------------------------

### 后缀名

*.Z #compress（淘汰）  
*.zip #zip  
*.gz #gzip  
*.bz2 #bzip2  
*.xz #xz  
*.tar #tar  
*.tar.gz #tar gzip  
*.tar.bz2 #tar bzip2  
*.tar.xz #tar xz  

### gzip

支持.Z .gz 对目录内的内容分别压缩  
gzip file  #压缩并替换原文件  
gzip -c file > file.gz #输出压缩后的数据流  
gzip -d file #decompress  
gzip -t file #测试gz文件格式是否正确  
gzip -v file #verbose  
gzip -1 file #压缩等级，-1最快，压缩比最差，-9最慢，压缩比最好，default -6  

#### gzip压缩相关命令

zcat  
zmore  
zless  
zgrep  
egrep # 支持.gz  
znew # .Z 转 .gz  

### bzip2

bzip2 -c file > file.bz2 #输出压缩后的数据流  
bzip2 -d file.bz2 #decompress  
bzip2 -k file #keep original file  
bzip2 -z option file #option  
bzip2 -v file #verbose  
bzip2 -1 file #-1 --fast -9 --best  

#### bzip相关命令

bzcat  
bzmore  
bzless  
bzgrep  

### xz

xz -c file > file.xz  
xz -d file.xz #decompress  
xz -t file.xz #test  
xz -l file.xz #list  
xz -k file #keep original file  
xz -1  

#### xz相关命令

xzcat  
xzmore  
xzless  
xzgrep  

### tar

tar -cv[z|j|J] -f tarname.tar.* file #create  
tar -xv[z|j|J] -f tarname.tar.* [仅解压包中的某个文件] -C dir #extract  
tar -t #test  
-z #gzip *.tar.gz  
-j #bzip2 *.tar.bz2  
-J #xz *.tar.xz  
-v #verbose  
-f filename  
-f /dev/st0 #备份到磁带机  
-f - #输出到管道，从管道输入tarball  
-C dstdir #解压路径  
-p #保留权限与属性（解压时必须有备份文件的用户权限）  
-P #保留完整路径  
--exclude=File #排除文件  
--newer [file|2015/01/01] #仅打包mtime ctime更加新的文件  
--newer-mtime  

### xfsdump

1、不支持没有挂载的文件系统备份  
2、必须使用root权限  
3、只能备份xfs文件系统  
4、只能用xfsrestore还原  
5、使用UUID作为标识  
xfsdump [-L session_label] [-M media_label] [-l 0-9] [-f file] filesystem  
-l 0-9 # increamental backup level  
xfsdump -I #i info /var/lib/xfsdump/inventory  

* 注意增量备份时label务必不同 * 

### xfsrestore

xfsrestore -I #i info == xfsdump -I  
xfsrestore -f dumpfile -L session_label -s spesific_file_or_dir restoredir  
依次恢复level0到level9  
xfsresotre -f dumpfile -i restoredir #互动模式  

### mkisofs  

光盘镜像  
mkisofs [-o file.iso] [-Jrv] [-V vol] [-m file] file and dir  
默认将目录下的内容放在光盘的根目录下  
-graft-point /要制作的目录 = /光盘下的目录   
相当于将所有文件复制到零时目录，将零时目录制作成光碟  
-o #outpust  
-J #兼容windows  
-r #兼容Linux权限  
-v #verbose  
-V vol #Volume  
-m file #exclude  
mkisofs -o /custom.iso -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -V 'CentOS 7 x86_64' -boot-load-size 4 -boot-info-table -R -J -v -T . #可启动光碟  

### isoinfo

显示iso信息  
isoinfo -d -i file.iso  
-d #discription  
-i #image  

### CentOS7 wodim 光盘刻录

略

### CentOS6 cdrecord 光盘刻录

略

### dd

dd if="input_file" of="output_file" bs="blocksize" count="number"  
bs #default 512bytes  
dd if=file1 of=file2 #复制文件  
dd if=/dev/sr0 of=file.iso #制作镜像（镜像大小等于设备容量）  
dd if=file.iso of=/dev/sda #制作usb启动盘  
dd if=/dev/urandom of=filepath bs=1M count=1 # 创建随机文件  
dd if=/dev/zero of=filepath bs=1M count=1 # 创建空文件

### cpio

cpio -ovcB > [file|device] #备份  
cpio -ivcdu < [file|device] #还原  
cpio -ivct < [file|device] #查看  
-o #output  
-B #default:512bytes B:5120bytes  
-i #input  
-d #自动简历新目录  
-u #自动覆盖旧文件  
-t #配合-i查看  
-v #verbose  
-c #使用portable formate方式存储  

* 输入的路径中，不能含有根目录，否则恢复时会覆盖根目录  *  
* /boot/initramfs-xxxx 是cpio文件 *  



定时任务，crontab、at
--------------------------------------------------

### at

#### 简介

需要服务atd，centos默认启动  
at产生执行文本存入/var/spool/at/等待atd调用  

#### 执行优先级

优先级1、/etc/at.allow使用权限白名单  
优先级2、/etc/at.deny使用权限黑名单，默认预留空at.deny允许所有用户执行at  
优先级3、没有执行文件则只有root可以执行at  

at -l == atq # list all at  
at -v # list verbose  
at -m # 执行完成后mail通知用户，默认有输出时才发送mail到mailbox  
at -d jobid == atrm jobid #delete  
at -c jobid # 列出jobid实际执行代码  
at time # 进入任务编辑交互模式  
HH:MM # 04:00  
HH:MM YYYY-MM-DD #04:00 2015-07-30  
HH:MM[am|pm] [Month] Date #04pm July 30  
time + n [minutes|hours|days|weeks] #now +5 minutes #04 +3days  

### batch # 使用方法同at

uptime load average低于1时执行  

### crontab

#### 简介

需要crond服务  

#### 优先级

优先级1、/etc/cron.allow使用权限白名单  
优先级2、/etc/cron.deny使用权限黑名单，默认预留空cron.deny允许所有用户执行  

#### 相关位置

crontab命令建立的任务存入/var/spool/cron/username  
执行日志 /var/log/cron  

#### 格式、参数

crontab [-u username] [-l|-e|-r]  
-u # 给指定用户色画质  
crontab -e #vi edit  
crontab -l #list  
crontab -r #remove all, 删除其中一项用-e编辑  

#### 格式

分钟    小时    日期    月份    周    command  
0-59    0-23    1-31    1-12    0-7（0和7都代表星期天，周与日月不能并存）  

#### 通配符

星号* # 忽略这个参数，符合其他参数就执行  
, # 列举  
减号- # 范围  
*/n # 每隔n时间单位执行一次  

#### 示例

59    23    1    5    *    command #每年五月一号23点59分执行  
*/5    *    *    *    *    command #每隔5分钟执行  
30    16    *    *    5    command #每周五16点30分执行  

#### 系统crontab

/etc/crontab 
/etc/cron.d/*

* 某些系统会把crontab加载到内存，需要重启crond重新读取crontab *  
  /etc/cron.d/0hourly每小时定时执行/etc/cron.hourly里的脚本  
  /etc/cron.hourly/0anacron定时执行daily weekly monthly任务  
  /etc/cron.hourly /etc/cron.daily /etc/cron.weekly /etc/cron.monthly 里直接放执行脚本  

#### 各种cron区别

个人用户使用crontab -e其他用户看不到  
系统维护使用/etc/crontab方便管理追踪  
自己开发使用/etc/cron.d/file方便升级修改时直接替换文件  
每小时/每日/每周/每天/每月固定执行使用/etc/cron.hourly|daily|weekly|monthly，系统会从目录中随机抽取任务执行，避免同时执行  

### anacron

#### 简介

检查cron.daily及以上的任务是否因关机而未执行，为执行则定期执行  
/etc/cron.hourly/0anacron #每小时定时检查程序是否定时执行anacron -s  

anacron -s [job] # 根据/var/spool/anacron/*判断是否已经执行，未执行则执行  
anacron -f [job] # 不判断执行情况，强制执行   
anacron -n [job] # now，立即检查执行情况，未执行的立即执行，而不按配置延迟  
anacron -u [job] # update，只更新执行时间记录而不执行任何任务  

#### 配置文件

/etc/anacrontab  
RANDOM_DELAY=45 #随机给与最大延迟时间，单位分钟  
START_HOURS_RANGE=3-12 #延迟多少个小时执行  

格式：天数    延迟时间（分钟）    工作名称定义    实际要执行的command（利用cron的run-parts)  
天数：与/var/spool/anacron/*记录的时间差，超过则执行  
延迟时间：按天数判断超时后，延迟START_HOURS_RANGE+延迟时间 执行，避免同时执行  

程序管理
----------

### 基础知识

子程序会继承父程序权限  
程序 program  
进程 process  
fork and exec # Linux进程会先fork复制一份进程作为临时程序，然后再exec执行子程序。  

### 工作管理

#### 限制登录个数

/etc/security/limits.conf  

#### 在后台执行任务

job control只能管理一个bash下的工作  
工作状态有stop和running  
使用&符号启动后台任务  
立即返回值为[JobID]ProcessID，JobID只跟当前bash有关  
执行完成后返回在bash中返回执行结果  
执行过程中会将信息打印在bash，可以使用 > output.txt 2>&1 &，将输出、错误导出到文件  

#### 在后台暂停任务

[ctrl]+[z]  

#### 恢复后台暂停任务

bg  
bg %jobid  

#### 查看后台任务状态

jobs [-lrs]  
-l # 列出所有后台任务的JobID Job状态 命令 PID  
-r # running 列出正在运行状态的工作  
-s # stop 列出正在暂停的工作  
加号+代表最近的一个任务，使用fg即可恢复  
减号-代表倒数第二个任务   

#### 恢复后台任务

fg  
fg %jobid  

#### 停止后台任务

kill -signalid %jobid # 对job发出信号，使用man 7 signal查询信号  
kill -l # 列出所有signal信号  

信号：  
-1 # 重新读取一次参数设定，类似reload  
-2 # 与[ctrl]+[c]相同  
-9 # 强制立即停止  
-15 # 信号默认值，以正常方式终止程序  

* 例如使用-9，vi会来不及删除swp文件，使用-15，vi会以正常流程结束  
* 使用signal名称与使用signal数字相同  

#### bash后台与Linux系统后台

仅以&结尾的命令在bash后台运行，bash退出则命令停止运行  
使用at可以使命令在系统后台运行与bash无关  
使用nohup可以使命令在系统运行与bash无关  

nohup command # 在bash前台运行，会打印到前台  
nohup command & # 在bash后台运行  
输出会存储在~/nohup.out  

### 程序管理

#### ps  

参数（带-不带-有区别）：  
-A # 显示所有进程，等于-e  
-a # 显示与terminal无关的进程  
-u # 显示有效使用者（effective user）相关进程  
x # 显示完整信息  
l # 显示较长，较详细的信息  
J # 工作的格式（jobs format）  
-f # 更加完整的信息  

常用指令：  



#### 特殊文件与权限

## SELinux



systemd # service daemon 服务 守护进程
----------

### 基础知识

daemon = service  
daemon进程通常以d结尾  

#### init管理方式（CentOS6）

/etc/rc.d/init.d # System V控制脚本  
systemd不使用/etc/inittab文件。  

服务管理方式：  
/etc/init.d/daemon start # 启动  
/etc/init.d/daemon stop # 关闭    
/etc/init.d/daemon restart # 重启  
/etc/init.d/daemon status # 状态  
service daemon start  
service daemon stop  
service daemon restart  
service daemon status  
service --status-all  

服务类型：  
独立启动模式，stand alone  
总管模式，super daemon，由xinetd或inetd管理socket和port，有socket连接请求则启动服务进程  

服务依赖管理：  
无，人工控制，固化在脚本中  

有执行等级的分类：  
/etc/rc/d/rc[0-6]/SXXdaemon -> /etc/init.d/daemon # XX为启动顺序，通过软连接控制等级所包含的服务器  

修改启动配置：  
chkconfig --list # 查看所有服务  
chkconfig --list daemon # 查看状态  
chkconfig daemon on # 设置为开机启动服务  
chkconfig daemon off # 关闭开机启动服务  

切换启动等级：  
init 0 # 关机  
init 1 # 单人模式  
init 3 # 文字模式  
init 5 # 图形界面  
init 6 # 重启  

#### systemd管理方式（CentOS7）

优点：  
并行启动所有服务  
自动检查依赖，递归启动  
按功能分为不同unit，如service、socket、target、path、snapshot、timer  
多个daemon合成target组，相当于systemv的启动等级  
向下兼容init脚本  

配置文件：  
/usr/lib/systemd/system/ # 系统启动脚本  
/run/systemd/system/ # 执行过程中产生的服务脚本，优先级高  
/etc/systemd/system/ # 管理员配置的脚本，优先级最高，软链接到/usr/lib/systemd/system/  
/etc/sysconfig/* # 服务相关配置  
/etc/sysconfig/network-scripts # 网络配置  
/var/lib # 服务数据库  
/run # 服务临时文件，如lock file，PID file  
/etc/services # 服务对应tcp/udp端口  

服务类型：  
.service # 一般服务  
.socket # IPC socket，内部数据交换等  
.target # 执行环境类型，服务集合  
.mount / .automount # 自动挂在，NFS  
.path # 文件侦测，打印机  
.timer # 循环执行服务  

### systemctl

#### 控制命令

systemctl start postfix.service # 启动  
systemctl stop postfix.service # 停止  
systemctl restart postfix.service # 重启  
systemctl reload postfix.service # 动态加载配置  
systemctl enable postfix.service # 开机启动  
systemctl disable postfix.service # 开机不启动  
systemctl status postfix.service # 获取当前状态  
systemctl is-active postfix.service;echo $?; # 判断当前是否运行  
systemctl is-enabled postfix.service;echo $?; # 判断当前是否开机启动  
systemctl mask postfix.service # 注销，无论如何也不会启动，不会被其他服务启动  
systemctl unmask postfix.service # 取消注销  

#### 服务状态

active(running) # 正在活动、正在执行  
active(exited) # 正在活动，之行结束  
active(waiting) # 正在活动，等待触发  
inactive # 服务当前没有活动  
enabled # 开机时被执行  
disabled # 开机时不执行  
static # 只能被其他服务唤醒，不能开机执行  
mask # 设置为无法被启动，注销。  

#### 查询服务

参数：  
--all # 列出未启动的  
--type=service,socket,target # 列出指定类型  

命令：  
systemctl == systemctl list-units # 列出所有启动（loaded）的unit  
systemctl list-unit-files # 列出/usr/lib/systemd/system/ 下所有服务  
systemctl list-unit-files | grep enabled # 查看已启动的服务列表  

#### target管理

系统target类型：  
graphical.target # 包含了multi-user.target  
multi-user.target # 纯文字模式  
rescue.target # 救援模式，独立系统  
emergency.target # 紧急模式，救援模式失败  
shutdown.target # 关机  
getty.target # 管理tty，配置文件修改tty个数  

systemctl get-default # 获取默认target  
systemctl set-defualt x.target # 设置默认target  
systemctl isolate multi-user.target == systemctl isolate runlevel3.target # 切换到命令行模式， 相当于“运行级别3”  
systemctl isolate graphical.target == systemctl isolate runlevel5.target # 切换到图形界面模式， 相当于“运行级别5”  

#### target切换原理

systemd使用链接来指向默认的运行级别。在创建新的链接前，可以通过下面命令删除存在的链接  
rm /etc/systemd/system/default.target  
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target # 默认启动运行级别3  
ln -sf /lib/systemd/system/graphical.target/etc/systemd/system/default.target # 默认启动运行级别5  

#### target快捷命令

systemctl poweroff  
systemctl reboot  
systemctl suspend # 暂停模式  
systemctl hibernate # 休眠模式  
systemctl rescue # 救援模式  
systemctl emergency # 紧急救援模式  

#### 依赖分析

systemctl list-dependencies x.target # 显示依赖  
systemctl list-dependencies --reverse x.target # 递归显示依赖  

### 配置服务

#### 配置目录  

/usr/lib/systemd/system/vsftpd.service # 官方配置  
/etc/systemd/system/vsftpd.service.d/custom.conf # 建立同名目录放置修改配置.conf  
/etc/systemd/system/vsftpd.service.requires/* # 启动依赖，启动前调用  
/etc/systemd/system/vsftpd.service.wants/* # 建议依赖，启动后调用  

#### 配置说明

`After=xxx` # 赋值  
`1, yes, true, on` # 真  
`0, no, false, off` # 假  
`#, ;` # 注释  

#### 配置项

[Unit] # 设置unit说明  
[Services], [Socket], [Timer], [Mount], [Path] # 设置类型相关配置  
[Install] # 设置归属target  

#### Unit配置项

Description # 服务说明  
Documentation # 文档资料  
Documentation=http://  
Documentation=man:sshd(8)  
Documentation=file:/etc/ssh/sshd_config  
After # 启动后建议启动的服务  
Before # 启动前建议启动  
Requires # 依赖的服务  
Wants # 建议启动  
Conflicts # 冲突服务  

#### Service配置项

Type # 启动方式  
Type=simple # 默认，由ExecStart指令启动，持续运行  
Type=forking # 由ExecStart启动后通过spawns启动新的进程  
Type=onshot # 由ExecStart指令启动，执行完则退出  
Type=dbus # 需要一个D-Bus名称才会运行，依赖BusName配置  
Type=idle # 限制服务，系统启动完成之后才启动  
EnvironmentFile # 配置文件，也可以使用Shell变量配置  
ExecStart # ststemctl start执行命令，可以接受参数，不接受管道  
ExecStartPre # 启动前执行  
ExecStartPost # 启动后执行  
ExecStop # systemctl stop执行指令  
ExecReload # systemctl reload  
Restart=1 # 服务终止后再次启动，如tty  
RemainAfterExit=1 # 所有服务程序都终止之后再次尝试启动，一般用于Type=oneshot  
TimeoutSec # 无法启动或无法结束时，强制执行时间  
KillMode=process # 只终止ExecStart启动的进程  
KillMode=control-group # 终止服务启动的所有进程  
KillMode=none # 没有程序会被终止  
RestartSec # 重启时间间隔，默认100ms毫秒  

#### Install配置项

WantedBy # 归属target，如multi-user.target  
Also # 开启依赖服务，开启时自动enable的服务  
Alias # 别名，例如default.target是multi-user.target  

#### 自定义服务配置示例

示例1：/usr/lib/systemd/system/sshd.service  

```
[Unit]  
Description=OpenSSH server daemon
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

示例2：backup.service  

```
[Unit]
Description=backup my server
Requires=atd.service

[Service]
Type=simple
ExecStart=/bin/bash -c "echo /backups/backup.sh | at now"

[Install]
WantedBy=multi-user.target
```

### getty及相同服务多实例通配

（使用@通配文件中数字，使用%I在脚本中获取通配）  
<http://linux.vbird.org/linux_basic/0560daemons.php#systemd_cfg_repeat>  

### timer相关服务配置

<http://linux.vbird.org/linux_basic/0560daemons.php#systemctl_timer>

### CentOS7预设服务配置

#### 预设服务

abrtd # 异常日志服务  
accounts-daemon # D-Bus接口的账户查询服务，如useradd，usermod，userdel  
alsa-X # 声音相关服务  
atd # 定时服务  
auditd # SELinux检查日志  
avahi-daemon # Zeroconf自动管理网络  
brandbot / rhel-* # 开机环境检测，网络启动关闭等  
chronyd / ntpd / ntpdate # 时间校对服务  
cpupower # cpu运行规范，/etc/sysconfig/cpupower  
crond # 定期服务  
cpus # 打印机服务  
dbus # D-Bus通讯服务  
dm-event / multipathd # 设备监听服务，外设存储等  
dmraid-activation / mdmonitor # 软raid服务  
dracut-shutdown # 处理initramfs相关行为  
ebtables # 桥接方式防火墙  
emergency / rescue # 紧急模式服务  
firewalld # 防火墙服务  
gdm # GNOME登录服务  
getty@ # tty登录服务  
hyper* ksm* libvirt* vmtoolsd # 虚拟化相关服务  
irqbalance # IRQ中断负载均衡服务  
iscsi* # SAN网络磁盘服务  
kdump # Kernel dump服务  
lvm2-* # LVM服务  
microcode # Intel CPU微指令服务  
ModemManager / network / NetworkManager* # 调制解调器、网络相关服务  
quotaon # Quota磁盘使用配额服务  
rc-local # /etc/rc.d/rc.local调用服务  
rsyslog # 日志记录服务，如/var/log/messages  
smartd # 硬盘S.M.A.R.T监控服务  
sysstat # 系统监视服务，CPU/流量/输入输出，记录在日志中  
systemd-* # 系统运行所需服务  
plymount* / upower # 图形界面相关服务  

#### 常用server服务

dovecot # POP3/IMAP服务  
httpd # web server  
named # 域名服务  
nfs / nfs-server # Network Filesystem，NFS网络文件服务  
smb / nmb # Windows文件共享，网络邻居服务  
vsftpd # FTP服务  
sshd # ssh服务  
rpcbind # RPC服务，NFS、NIS依赖RPC  
postfix # 邮件服务  


日志分析
--------------------------------------------------

重要  

网络操作
--------------------------------------------------



### dhcclient # 手动获取ip

dhclient eht0  

#### 检测网络内所有ip

``` sh
#!/bin/bash  
for siteip in $(seq 1 254)
do
    site = "192.168.1.${siteip}"
    ping -c1 -W1 ${site} &> /dev/null
    if ["$?" == "0"]; then
        echo "$site is UP"
    else
        echo "$site is DOWN"
    fi
done
```


### telnet

telnet host  
telnet ip  
telnet ip port # 检测端口是否启动  

### ftp

ftp host/ip port  

```
>anonymous #匿名登录  
>help  
```

### lftp # 命令式ftp

lftp -p port -u user,pwd host/ip #交互模式  
lftp -f ftpscriptfile #ftp脚本模式  
lftp -c "command" #命令模式  

### links # 文字浏览器

略

### wget # 下载

wget --http-user=user --http-password=pwd --quit www.baidu.com  
-i file #从文件中按行读取网址  
--bind-address=ip/host #指定ip下载  
-O name(--output-document=name) #保存为  
-c --continue #断点续传  
-S --server--response #打印响应头部  
--no-proxy #不使用代理  
--secure-protocol=auto/SSLv2/SSLv3/TLSv1  
-m #下载整站  
-r(--recursive)#递归下载  
-l depth(--level=depth) #递归层数  
-k(--convert-links) #将绝对路径转换为相对路径，方便本地浏览链接  
-p #下载网页所需的所有文件图片  
-A c, h #指定要下载的文件样式列表  
-nc #不下载已经存在的文件  
-np #不跟随链接，只下载指定目录及子目录  

### tcpdump

tcpdump [-AennqX] [-i interface] [-w writefile] [-c count] [-r readfile] [filter]  
-A #截取内容以ASCII显示  
-e #使用mac层信息显示  
nn #不解析，使用ip/port显示  
-q #仅列出简短信息  
-X #十六进制 ASCII对照显示  

#### filter

'host baidu.com'  
'host ip'  
'net 192.168'  
'src host 127.0.0.1'  
'dst net 192.168'  
'tcp/udp/arp/ether port 22'  
and #且  
or #或  

### nc/netcat # 端口连接器

nc ip port  
nc -u ip/host port #udp  
nc -l ip/host port #listen  
nc -l ip port -e /bin/bash #将本地bash暴露到端口，需要编译参数GAPING_SECURITY_HOLE  

网络安全
--------------------------------------------------

### 防火墙分类

网络层防火墙：IP Filtering Net Filter  
传输层防火墙：/etc/hosts.allow /etc/hosts/hosts.allow  
应用层防火墙：应用程序权限  
系统级防火墙：SELinux，系统控制的应用程序权限  
文件级防火墙：文件权限  

### nmap

nmap [type] [option] [hosts or nets]  

#### type

-sT #已连接的tcp  
-sS #带有SYN的tcp  
-sP #以ping方式进行扫描  
-sU #以UDP进行扫描  
-sO #以IP进行扫描  

#### option

-PT #使用TCP的ping扫描  
-PI #使用ICMP的ping扫描  
-p portrange #使用特定端口范围扫描  

#### hosts or nets

192.168.1.100  
192.168.1.0/24  
192.168.*.*  
192.168.1.0-50, 60-100, 103, 200  

#### 示例

nmap ip #默认，扫描tcp端口  
nmap -sTU ip #扫描tcp和udp端口  
nmap -sP net #扫描网络内所有开机的机器  

### selinux

略

路由
--------------------------------------------------

### 查看是否开启转发功能

cat /proc/sys/net/ipv4/ip_forward

### 临时开启数据转发功能

echo 1 > /proc/sys/net/ipv4/ip_forward

### 永久开启数据转发功能

/etc/sysctl.conf  
net.ipv4.ip_forward = 1  
sysctl -p # 立即生效  

### 使用linux做路由器

#### 自动路由  

#### arp转发  

防火墙、NAT转发
--------------------------------------------------

域名
--------------------------------------------------

远程连接
--------------------------------------------------

### ssh

ssh ip #使用当前用户登录默认端口ip的ssh    
ssh -p port user@ip #指定端口用户登录   
ssh -f user@ip command #远程执行command不登录   
ssh -o option ip #ConnectTimeout=second, StrictHostKeyChecking=[yes|no|ask]   

#### ssh证书登录

ssh -i x.pem ec2-user@ip  

#### 创建ssh登录密钥

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  

#### server端密钥存储位置

cat 公钥 >> /user/.ssh/authorized_key or authorized_key2  

#### 密钥格式转换

ssh-keygen -e -f ~/.ssh/id_dsa > ~/.ssh/id_dsa_com.pub   
ssh-keygen -i -f ~/.ssh/id_dsa_com.pub > ~/.ssh/id_dsa.pub  

#### client端密钥存储位置

cat 私钥 > /user/.ssh/id_rsa  

#### 多个client私钥存储

方法一：    

```
eval `ssh-agent -s` #使用eval是因为要执行ssh-agent的结果来加入环境变量  
ssh-add ~/.ssh/new_id_rsa    
```

方法二：编辑.ssh/config   

```
Host            shortname  
HostName  long.name.com  
IdentityFile  ~/.ssh/new_id_rsa   
User             username   
```

方法三：  
编辑~/.bash_profile  

```
if [ -z "$SSH_AUTH_SOCK" ] ; then
  eval `ssh-agent -s`
  ssh-add ~/.ssh/
fi
```

方法四：
编辑~/.bash_profile:

```
eval `keychain --eval id_rsa`
```

#### 本机公私钥匙存储路径

/etc/ssh/ssh_host_*  
/etc/ssh/ssh_host_*.pub  

#### 本用户已知公钥存储路径

~/.ssh/known_hosts  

#### 本用户登录公钥存储路径

~/.ssh/authorized_key2  

#### sshd配置

/etc/ssh/sshd_config  
Port 22  
Protocol 2,1 #要支持1需要明确指定   
LoginGraceTime 2m #登录等待时间    
Compression yes/no/delayed #登陆后压缩   
PermitRootLogin no  
PasswordAuthentication no  
AuthorizedKeysFile .ssh/authorized_keys #公钥路径配置  
PermitEmptyPasswords no  
DenyGroups nossh #禁止nossh登录  
DenyUsers testssh #禁止testssh登录  

#### 安全配置

/etc/hosts.allow  
sshd: ip1 ip2/255.255.255.0  
/etc/hosts.deny  
sshd:ALL  

### sftp

sftp user@ip  

### scp

scp [-pr] [-l limitspeed] file user@ip:/dir/file  
scp [-pr] [-l limitspeed] user@ip:/dir/file file  
-p # 传输时保留文件权限及时间戳  
-r # 递归复制目录  
-l # 限制速率为kbps  
-C # 传输时数据压缩  

### xdmcp

#### 组件

X Server负责显示  
X Client负责计算  
Window Manager (GNOME, KDE, XFCE) 管理一组X Client  
Display Manager(DM) 提供登录界面启动Window Manager  
xdmcp监听udp 177等待x server连接  


1，使用命令 yum install xdm  安装XDM  
通过 XDMCP 支持来管理 X 显示器集合  
2，修改/etc/X11/xdm/Xaccess文件，找到下面的语句：# * #any  host  can  get  a  login  window，  
去掉这一行最前面的#号，成为：* #any  host  can  get  a  login  window  
3，修改/etc/gdm/custom.conf文件。  
找到下面的语句：[xdmcp],在这句下面加上以下两行:  
Enable=true  
Port=177  
在[security]下面添加  
AllowRemoteRoot=true  
修改custom.conf后，必须重启gdm才可以生效。具体做法就是kill进程中的gdm，然后重启gdm:  
/usr/sbin/gdm-restart  
4，开启防火墙UDP 177端口  
/etc/sysconfig/iptables  
-A INPUT -m state --state NEW -m udp -p udp --dport 177 -j ACCEPT  
service iptables restart  
5，在X Server运行的及其上开启监听端口，:0 6000, :1 6001  
-A INPUT -m state --state NEW -m tcp -p tcp --dport 6001 -j ACCEPT  

### linux下远程连接桌面

runlevel 5 启动图形界面， :0 在 tty1  
runlevel 3 启动图形界面， :0 在 tty7，tty8  

xhost + 192.168.1.2  
iptales -A INPUT -i $EXTIF -s 192.168.1.2/24 -p tcp --dport 6001 -j ACCEPT  
iptables-save  
X -query 192.168.1.2 :1  

### xnet

在图形界面中远程x window

### xming

windows远程桌面

### vnc

不加密

### xrdp

加密
windows 远程连接协议

### rsync

加密远程同步

DHCP Server
--------------------------------------------------

NFS Server
--------------------------------------------------

NIS Server
--------------------------------------------------

NTP Server
--------------------------------------------------

SAMBA Server
--------------------------------------------------

Proxy Server
--------------------------------------------------

DNS Server
--------------------------------------------------

