# Linux

[toc]

# 目录

[toc]

# 获取帮助

## whatis # 显示命令功能介绍

* 显示命令的简短介绍，实际为man手册中的简介。
* command 只能为完整命令
```bash
# whatis command
whatis whatis
```

## which # 查找命令绝对路径

```bash
# which command
which which
```

## whereis # 定位二进制文件、源文件、手册文件

```bash
# whereis command
whereis ssh
# ssh: /usr/bin/ssh /etc/ssh /usr/share/man/man1/ssh.1.gz
```

## file # 查看文件类型

```bash
# file file
file .
# .: directory

file /etc/passwd
# /etc/passwd: ASCII text
```

## man

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

## apropos

```

```

## info 

* 搜索路径：`/usr/share/info`

## 软件文档路径

/usr/share/doc



# 专题



## Linux忘记密码解决方法
### 方法一 单用户模式
1. 重启进入grub
2. 编辑grub kernel启动项，进入单用户模式
3. 在单用户模式下启动，输入passwd修改密码
```
kernel /vmlinuz ro root=LABEL=/
kernel /vmlinuz ro root=LABEL=/ single
```

### 方法二 救援模式
1. F5进入Rescue模式
2. boot: linux rescue # 输入`linux rescue`进入Rescue模式
3. 系统自动挂载旧操作系统到`/mnt/sysimage`
4. 输入`chroot /mnt/sysimage`切换到本地操作系统
5. 输入`passwd`修改密码


Centos-minimal最小化安装
----------
### 1.设置临时IP
```
ifconfig eth0 192.168.199.3 netmask 255.255.255.0
```

### 2.设置临时路由
```
route add default gw 192.168.199.1
```

### 3.配置网卡开机启动并DHCP  
配置文件：`/etc/sysconfig/network-scripts/ifcfg-eth0`  
```
ONBOOT=yes # 网卡开机启动
BOOTPROTO=dhcp # 获取IP的方式
```

CentOS6:
```
service network restart
/etc/init.d/network restart
```
CentOS8:
```

```

### 3.配置网卡静态IP
```
BOOTPROTO=static #获取ip的方式(static/dhcp/bootp)
IPADDR=192.168.199.3 #IP地址
NETMASK=255.255.255.0 #子网掩码
NETWORK=192.168.199.0 #网络地址
BROADCAST=192.168.288.255 #广播地址
```

### 3.无线网卡  
安装：  
```yum install -y wpa_supplicant```  
添加配置项SSID WPA PSK:  
```
wpa_passphrase long 'my password' >> /etc/wpa_supplicant/wpa_supplicant.conf
wpa_passphrase long 'my password' | grep -v '{\|}' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
echo 'WPA=yes' >> /etc/sysconfig/network-scripts/ifcfg-wlan0
```
修改配置项ONBOOT为yes  
添加启动命令:  
```
cat >> /etc/rc.local<<EOF
wpa_supplicant -iwlan0 -B -c /etc/wpa_supplicant/wpa_supplicant.conf
EOF
```
启动无线网卡:
```
ifup wlan0
```


### 4. 配置网关  
配置文件：
```
/etc/sysconfig/network
```
增加:
```
GATEWAY=192.168.199.1
```
执行：
```
service network restart
```

### 5. 配置DNS  
修改```vi /etc/resolv.conf```  
```
# 增加DNS信息
nameserver 192.168.1.1
```


### 更新操作系统
yum update

### 安装配置管理图形界面
```
yum install setuptool # 配置管理工具
yum install ntsysv # 系统服务配置
yum install system-config-firewall # 安装防火墙
yum install system-config-firewall-tui # 防火墙配置
yum install system-config-securitylevel-tui #
yum install system-config-network-tui # 安装setup中配套的网络设置
yum install system-config-keyboard # 键盘设置
```

### 安装常用工具
```
rpmforge #软件包
nslookup
traceroute
wget
man
sudo
ntp
ntpdate
screen
patch
flex
bison
```

### 安装samba文件共享
yum install samba

### 关闭SELinux
/etc/selinux/config文件中设置SELINUX=disabled


Linux常用缩写，名词解释
----------
```bash
pwd # print work directory
ps # process status #
df # disk free
du # disk useage
rpm # RedHat Package Management
rmdir # remove directory
rm # remove
cat # concatenate # cat file1 file2 >> file3
insmod # install module
ln -s # link soft
mkdir # Make Directory
touch # touch
man # manual
su # switch user
cd # change directory
ls # list files
ps # process status
mkfs # make file system
fsck # file system check
uname # unix name
lsmod # list modules
mv # move file
rm # remove file
cp # copy file
fg # foreground
bg # background
chown # change owner
chgrp # change group
chmod # change mode
umount # unmount
dd # convert an copy 缩写cc，cc已经被CComplier占用，所以命名dd
tar # tape archive
ldd # list dynamic dependenicies
.bashrc # rc, resource configuration
.a # archive, static library
.so # shared object, dynamically linked library
.o # Object file, complied result of c/c++ source file
dpkg # debian package manager
apt # advanced package tool
bin # binaries
etc # etcetera, 等等
proc # processes
sbin # superuser binaries
tmp # temporary
usr # unix shared resources
var # variable
FIFO # first in first out
grub # Grand unified bootloader
IFS # Internal Field Seperators
LILO # LInux LOader
bash # bourne again shell
cal # calendar
cmp # compare
```

文件权限与目录配置
==========

权限配置文件
----------
* /etc/passwd # 用户信息  
* /etc/shadow # 用户密码  
* /etc/group # 用户组信息  

切换用户ID组ID为USER
----------
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

用户编号
----------
* 用户ID，32位，0-60000
* 用户ID = 0，root
* 用户ID = 1~499，系统用户，没有shell，为服务创建的用户
* 用户ID = 500+，普通用户  

创建用户，useradd
----------
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

删除用户，userdel
----------
```bash
# 删除用户
userdel yinguanxuan

# 强制删除用户，即使当前已经登录
userdel -f

# 慎用
# 删除用户同时删除家目录及其中文件
userdel -r
```

修改用户，usermod
----------
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

修改用户组信息，gpasswd
----------
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

修改密码 # passwd
----------
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

批量修改密码 # chpasswd
----------
```bash
# 重定向修改密码
echo "user:pwd" | chpasswd

# 从文件导入密码
chpasswd < passwd.txt

# 参数
-e # 导入的密码是加密后的密文
```

创建用户组 # groupadd
----------
```bash
groupadd groupname

# 参数
-g # 指定组id
-r # 创建系统工作组（ID小于500）
```

修改组信息 # groupmod
----------
```bash
# 修改组名
groupmod -n new_name 
```

删除用户组 # groupdel
----------
```bash
# 删除组
groupdel [groupname]
```

文件权限
----------
* 用户继承组的权限
| 字符 | 含义 | 用在文件含义     | 用在目录含义                                |
| ---- | ---- | ---------------- | ------------------------------------------- |
| r    | 读取 | 可读取文件内容   | 可列出目录内容                              |
| w    | 写入 | 可修改文件内容   | 可在目录中创建、删除文件                    |
| x    | 执行 | 可以作为命令执行 | 可访问目录内容 (目录必须有执行权限才能读取) |

chown # 修改文件目录所属用户
----------
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

chgrp # 修改文件、目录所属组
----------
```bash
chgrp group file
-R # 递归目录
```

chmod # 修改权限
----------
```bash
### 格式
chmod u[+|-|=][r|w|x],g[+|-|=][r|w|x],o[+|-|=][r|w|x],a[+|-|=][r|w|x] file
chmod [4+2+1][4+2+1][4+2+1] file

### 参数
-R #递归目录  
```

umask
----------
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

特殊权限
----------
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

FHS
----------
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

## 查询LSB标准
```bash
yum install -y redhat-lsb  
lsb_release -a  
```

# 用户管理

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

# 服务管理

## Init程序的类型（历史）

* SysV: init, CentOS 5, /etc/inittab
* Upstart: init, CentOS 5, /etc/inittab, /etc/init/*.conf
* Systemd: systemd, CentOS7, /usr/lib/systemd/system, /etc/systemd/system



## 比较System V与Systemd
- Linux操作系统的开机过程是，从BIOS开始，然后进入Boot Loader，再加载系统内核，然后内核进行初始化，最后启动初始化进程。初始化进程作为Linux系统的第一个进程，它需要完成Linux系统中相关的初始化工作，为用户提供合适的工作环境。红帽RHEL 7系统已经替换掉了熟悉的初始化进程服务System V init，正式采用全新的systemd初始化进程服务。如果您之前学习的是RHEL 5或RHEL 6系统，可能会不习惯。

- 优点：
  systemd初始化进程服务采用了并发启动机制，开机速度得到了不小的提升。

- 缺点：
  * 1：systemd初始化进程服务的开发人员Lennart Poettering就职于红帽公司，这让其他系统的粉丝很不爽。
  * 2： systemd初始化进程服务仅仅可在Linux系统下运行，“抛弃”了UNIX系统用户。
  * 3：systemd接管了诸如syslogd、udev、cgroup等服务的工作，不再甘心只做初始化进程服务。
  * 4：使用systemd初始化进程服务后，RHEL 7系统变化太大，而相关的参考文档不多，令用户着实为难。

## 运行级别与target概念比较
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



## 服务管理概念比较

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

# 网络连接

## nmtui 图形化网卡设置（centos6使用setup）

```bash
# 安装
yum install NetworkManager-tui

# 使用
nmtui

setup
```

## hostnamectl，主机信息

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

## nmcli # NetworkManager服务器客户端

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

### 专题：设置自动获取IP

```
nmcli connection modify eth0 connection.autoconnect yes
nmcli connection modify eth0 ipv4.method auto
nmcli connection up eth0
```



## Systemd 网卡命名方式

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

## ip

### ip link # 链路层配置
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

### ip neigh # arp表配置

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

### ip address # 网络层配置

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

### ip route # 路由配置
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

### 专题：通过端口号查找进程ID
```


```

## ss，Socket Statics，获取socket统计信息，替代netstat
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

## netstat（废弃），使用ss替代

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


## ifconfig（废弃），临时修改网络设置（修改内存）

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

## MAC层

### arp协议
* ARP，Address Resolution Protocal，地址解析协议
* 协议原理：
    1. 初始时源主机不知道目的主机MAC。
    2. 源主机发送一条MAC地址广播，目的IP为目的主机IP的报文
    3. 目的主机收到与自身IP相同的报文，响应一条ARP应答，携带自身MAC地址。
    4. 源主机收到这条报文，在ARP表中建立ARP表项。
    5. 目的主机与源主机不在同一广播区的话，只找网关。

### arp（废弃），修改arp缓存，使用`ip neigh`替代

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

### 专题：arp攻击

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

### arping 检测连通性，发送、接受arp消息

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

## 网络层

### traceroute 网络跟踪

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

### tracepath 

```bash

```





### mtr，my trace route，连通性直观检测

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

### route（废弃），显示内核路由表，使用 ip route替代

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

### ping，检测网络是否科大，需要目标支持ICMP协议

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

## 应用层
* host 和 nslookup 命令在 bind-utils包中

### host
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

### nslookup 互动式查询
``` bash
# 查询域名
nslookup www.baidu.com  
```

### dig 查询域名服务器

```bash

```



# 网络安全

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



# 磁盘管理

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

# 软件管理

## 软件包管理

- 在RPM（红帽软件包管理器）公布之前，要想在Linux系统中安装软件只能采取源码包的方式安装。早期在Linux系统中安装程序是一件非常困难、耗费耐心的事情，而且大多数的服务程序仅仅提供源代码，需要运维人员自行编译代码并解决许多的软件依赖关系，因此要安装好一个服务程序，运维人员需要具备丰富知识、高超的技能，甚至良好的耐心。而且在安装、升级、卸载服务程序时还要考虑到其他程序、库的依赖关系，所以在进行校验、安装、卸载、查询、升级等管理软件操作时难度都非常大。
- RPM机制则为解决这些问题而设计的。RPM有点像Windows系统中的控制面板，会建立统一的数据库文件，详细记录软件信息并能够自动分析依赖关系。目前RPM的优势已经被公众所认可，使用范围也已不局限在红帽系统中了。
- rpm是一个包管理器，全称Red Hat Package Manager，用于生成、安装、查询、核实、更新、卸载单个软件包。
- 一个软件包通常包括一个文档一级包的信息，比如名字，版本，描述等。
- 使用rpm的发行版包括Red Hat，CentOS，Fedora，SuSE等。
- rpm只检查依赖关系，yum能解决依赖关系。

### 基础知识

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

### rpm包搜索

<http://rpmfind.net/>

<https://pkgs.org/>

<https://www.rpmseek.com/>

<http://rpm.pbone.net/>

### rpm -i / rpm -U / rpm -F # 安装和升级模式 

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

### rpm -q # 查询模式 

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

### rpm -e # 卸载模式

```bash
# 卸载
rpm -e packagename # erase

# 强行卸载
rpm -e --nodeps yum

# 参数
--test # 测试卸载
-vv # 配合--test显示卸载详细信息
```

## 软件仓库管理 # yum/dnf工具

* 尽管RPM能够帮助用户查询软件相关的依赖关系，但问题还是要运维人员自己来解决，而有些大型软件可能与数十个程序都有依赖关系，在这种情况下安装软件会是非常痛苦的。Yum软件仓库便是为了进一步降低软件安装难度和复杂度而设计的技术。Yum软件仓库可以根据用户的要求分析出所需软件包及其相关的依赖关系，然后自动从服务器下载软件包并安装到系统。
* 全称 Yellowdog Updater Modifed
* yum在未完成安装的情况下强行终止安装过程，下次再安装时将无法解决依赖关系，所以使用dnf替代yum。

### yum 安装

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

### yum更新

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

### yum查找、显示信息

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

### 卸载软件

```bash
# 卸载软件包
yum remove package
yum remove -y package
yum erase package

# 整组卸载
yum groupremvoe group
```

### 其他

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

### 组管理

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

### 专题：安装第三方软件仓库epel

```bash
yum install epel-release
```

### 专题：repoquery 从软件仓库查询软件仓库

```bash
# 查询软件包中所有文件
repoquery -q -l dhcp

# 参数
-l， --list # 列出包中所有文件
```





### 专题：自定义软件仓库

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

# 配置网络源软件仓库
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

### 专题： 自动选择最快软件仓库源 yum-plugin-fastestmirror 

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

### 专题：添加中文语言支持

```bash
yum groupinstall "Chinese Support"
LANG=zh_CN.utf8
```

### 专题：从软件仓库下载包

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

### 专题：管理软件源 # yum-config-manager

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
==========
* root没有`alias vi=vim`
* 软连接共享配置文件`ln -s /home/yingxuanxuan/.vimrc /root/.`

模式切换状态机
----------
```bash
一般模式 -> i, o, a, r, I, O, A, R -> 编辑模式  
一般模式 -> : / ? -> 命令行模式  
编辑模式 -> ESC -> 一般模式
指令模式 -> ESC -> 一般模式
```

一般模式
----------
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

命令模式
----------
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

切换到编辑模式
----------
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

区块模式
----------
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

多文档编辑
----------
vim file1 file2 file3  
:n # next  
:N # prev  
:files # list  

多窗口
----------
:sp #同文件分割窗口  
:sp file #不同文件分割窗口  
ctrl + w, w # tab  
ctrl + w, j # ctrl + w, down  
ctrl + w, k # ctrl + w, up  
ctrl + w, q # quit    

命令补全
----------
ctrl + x, ctrl + n # 以上文的内容进行补齐  
ctrl + x, ctrl + f # 以当前目录下的文件名补齐 
ctrl + x, ctrl + o # 以后缀名相关的内建语法进行补齐  

配置文件
----------
### ~/.viminfo # 使用记录，上次编辑位置，搜索关键字  
:set # 显示与default不同的设置  
:set all # 查看当前设置  

### ~/.vimrc # 配置文件 
* 注意：配置文件无需冒号，单个双引号注释 *  
:set nu/nohu # 显示行号  
:set hlsearch/nohlsearch # 高亮搜索关键词  
:set backup # default nobackup filename~  
:set ruler # 状态栏行列标尺  
:set showmode # 显示当前模式  
:set backspace=(012) #2:删除原本的字符 01:只能删除刚刚输入的字符  
:syntax on/off #开启关闭语法颜色  
:set bg=dark/light #default light  

### tab设置
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

换行符转换
----------
dos2unix   
unix2dos  
dos2unix -k file #保留文件mtime  
dos2unix -n oldfile newfile #另存为  

编码转换
----------
### iconv
iconv --list  
iconv -f fromcodec -t tocodec -o outputfile inputfile  

### 繁简互转
iconv -f big5 -t gb2312  

### utf8繁简互转
iconv -f utf8 -t big5 vi.utf8 |iconv -f big5 -t gb2312 | iconv -f gb2312 -t utf8 -o vi.gb.utf8  

修复ubuntu小键盘乱码
----------
apt-get remove vim-common  
apt-get install vim  

# 文本处理

## seq，生成序列

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





# shell内置功能

## shell基础

### 命令提示符（可修改）

* `#`, root用户
* `$`, 普通用户
* 变量`PS1`为命令提示符格式，使用`echo $PS1`查看
* 变量`PS2`为命令换行提示符，使用`echo $PS2`查看

### 当前用户默认shell

```bash
$SHELL
echo $SHELL
```

### 当前系统支持的所有shell

```bash
/etc/shells
cat /etc/shells
```



### tab命令补全

* 命令中第一组字符 + Tab # 命令补全
* 命令中第二组字符 + Tab # 文件补全
* 补全插件：

```bash
yum install bash-completion # 参数补全（影响文件补全）  
yum erase bash-completion  
yum install bash-completion-extras # 参数补全（影响文件补全）  
yum install python-django-bash-completion # django命令补全  
```

### shell快捷键

```bash
Ctrl + l # 清屏，同clear
Ctrl + c # 终止当前命令
Ctrl + z # 挂起命令，使用fg恢复，使用bg查看
Ctrl + r # 搜索history历史命令
Ctrl + d # EOF, End of File, End Of Input, exit

Shift + PageUp / PageDown # bash terminal 翻页

ctrl + a # 行首，同Home
ctrl + e # 行尾，同End

# linux 特色，删除时剪切，可使用Ctrl + y 粘贴
ctrl + u # 删除光标前所有内容，backspace增强
ctrl + k # 删除光标后所有内容，del增强

Ctrl + Alt + F1 - F6 # 在tty之间切换
Ctrl + Alt + F7 # 切换到图形界面
Ctrl + Alt + F1 # Centos7切换到图形界面
```

## shell快捷指令

```bash
!! # 执行上一条命令
!str # 执行上一条str开头的指令
```







## type 判断是shell内建命令还是外部程序

```shell
type man # /usr/bin/man
type type # shell

# type -t 返回标准type类型
type -t command 

alias # 别名
keyword # shell保留字
function # shell函数
builtin # shell内置
file # 磁盘文件
'' # 空，没有找到

# type -p 获取file类型的路径，相当于 which command
type -p command
```



# shell 脚本编程

## 注释

```bash
# 单行注释
# 使用井号

# 多行注释方法1（非主流，改方法利用重定向符号将后续内容读入，但不做任何操作，作为注释）
<< BLOCK
注释行1
注释行2
BLOCK

:<<EOF
注释行1
注释行2
EOF

# 多行注释方法2（有问题，利用冒号，执行一条字符串作为命令，但是其中语句可能会被执行
# centos7下无效
:'
注释行1
注释行2
`echo abc` # 会引发错误
'
```



## 数据类型

* **shell中所有变量都是字符串，没有数值概念**

### 字符串

```bash
 # 获取字符串长度
 var="abcd"
 echo ${#var}  # 4
 
 # 提取字符串子串
 var="abcd"
 echo ${var:0:4} # 从index 0提取4个长度
 
 # 查找子串
 var="abcd"
 echo $(expr index $var "c") # 3
```

### 布尔值

* bash中有布尔值，true/false，不等于0/1

```bash
# bash中，true和false不等于0/1，虽然输出结果为0/1
true; echo $? # 0
false; echo $? # 1

true && true; echo $? # 0
true && false; echo $? # 1
false && true; echo $? # 1
false && false; echo $? # 1

true || true; echo $? # 1
true || false; echo $? # 1
false || true; echo $? # 1
false || false; echo $? # 0
```

### 数组

* bash支持一位数组，不支持多位数组
* bash使用下面提取元素，从0开始

```bash
# bash中使用小括号定义数组，数组使用分隔

# 定义数组1
array1=(1 2 3 4)

# 定义数组2
array2=(
5
6
7
8
)

# 定义数组3
array3[0]=9
array3[1]=10
array3[2]=11
array3[3]=12

# 使用数组
# 使用数组需要使用大括号

# 提取特定元素
echo ${array1[0]}
echo ${array1[1]}
echo ${array1[2]}
echo ${array1[3]}

# 提取所有元素
echo ${array2[*]}
echo ${array2[@]}

# 获取元素长度
echo ${#array3[*]}
echo ${#array3[@]}

```

## 变量

* 变量命名与其他语言相同，由字母、下划线开头，可以包含数字，不能使用shell关键字作为变量名
* **使用未定义的变量不报错，值为空**
* bash变量区分大小写

### 声明变量

```bash
# 使用等号声明变量
# 注意：等号两边不能有空格，有空格时需要使用单引号或者双引号
var=value
var='value' # 此为原样输出最佳实践
var="value" # 此为变量定义最佳实践

# 修改变量的值与定义变量相同，不会报错
var=old
var=new
```

### 使用变量

```bash
# 使用花括号帮助解释器识别变量边界，可省略
var
${var}
$varabc # 不使用花括号会识别为变量varabc，未定义的变量值为空
${var}abc # 使用花括号会导致变量var与字符串abc拼接
```

### 字符串拼接
```bash
"$var"append
${var}append
```

### 变量类型

1. **局部变量：在脚本或命令中定义，仅在当前shell实例中有效，其他shell中不能访问的局部变量**
2. **环境变量：所有程序，包括shell启动的程序，都能访问的变量**
3. shell变量：shell为了正常运行，使用的变量，包含环境变量和局部变量

### 声明为只读变量

```bash
var='old'
readonly var
var='new' # NAME: This variable is read only
```

### 删除变量

```bash
unset var
```

### 引号区别

```bash
# 不加引号

# 1.转译符号斜杠无效
# 2.字符串中不能有空格（变量声明时识别为字符串结束，echo时识别为下一个字符串）
# 3.字符串中空格只
echo \a # a
echo "\a" # \a
echo 111  222   333 # 111 222 333
echo "111  222   333" # 111  222   333，空格被保留

# 单引号
# 引号里内容会原封不动显示
a=abc
echo $a # abc
echo \$a # $a，\转译符号解转译
echo "$a" # abc
echo '$a' # $a

# 双引号
# 双引号中特殊符号“斜杠”，“空格”，“$”，“`”会被解析
# 换言之，双引号支持变量和转译

# 反引号
反引号中内容被执行，用于获取执行结果
```

### 获取命令执行结果

```bash
`反引号`
$(command)

d1=`date` 
d2=$(date) # 此为将命令结果赋值的最佳实践，反引号容易误读为单引号
```

### 特殊变量

```bash
$$ # 当前进程的ID，echo $$
$0 # 当前脚本的文件名，**使用source执行时，不是文件名，是bash**
$1 # 第1个参数
$5 # 第5个参数
$# # 参数个数，不含$0
$* # 所有参数，被双引号包含时，输出"$1 $2 $3 $n"
$@ # 所有参数，被双引号包含时，输出"$1" "$2" "$3" "$n"
$? # 上个命令的状态，或函数的返回值
~ # 家目录

# "$*" 循环1次
echo "print each param from \"\$*\""
for var in "$*"
do
    echo "$var"
done

# "$@" 循环4次
echo "print each param from \"\$@\""
for var in "$@"
do
    echo "$var"
done
```

### 转义字符

```bash
\\ # 反斜杠
\a # 警报
\b # 退格（删除）
\f # 换页
\n # 换行
\r # 回车
\t # 水平制表符
\v # 垂直制表符

echo "\a" # 默认echo不解释转义字符
echo -e "" # 使用-e参数解释转义字符
```

## 运算符

* 原生bash不支持数学运算，只能通过命令awk和expr实现

```bash
var=`expr 2 + 2`
# 1、数字、符号、expr中间均需要空格分隔，英文bash按空格识别参数
# 2、使用反引号获取结果，结果为字符串
# 3、乘号前必须加反引号转译，避免识别为通配符
```

### 算数运算符

| 运算符 | 说明                                          | 举例                          |
| ------ | --------------------------------------------- | ----------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 10。    |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a。  |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |


### 关系运算符

* 关系运算符只支持数字，不支持字符串

| 运算符 | 说明                                                  | 举例                       |
| ------ | ----------------------------------------------------- | -------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 true。  |
| -ne    | 检测两个数是否相等，不相等返回 true。                 | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大等于右边的，如果是，则返回 true。   | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

### 布尔运算符

```bash
if true; then echo "T"; else echo "F"; fi # T
if false; then echo "T"; else echo "F"; fi # F

# 注意，条件判断时方括号等同于test，
# if [ true ]; then echo "T"; else echo "F"; fi
if ! true; then echo "T"; else echo "F"; fi # F
if ! false; then echo "T"; else echo "F"; fi # T

# and
# 注意，-a 是 test的参数，所以必须要价方括号
if [ true -a false ]; then echo "T"; else echo "F"; fi
if [ true -o false ]; then echo "T"; else echo "F"; fi

# 示例
a=10
b=20

# and
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a -lt 100 -a $b -gt 15 : returns true"
else
   echo "$a -lt 100 -a $b -gt 15 : returns false"
fi

# or
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : returns true"
else
   echo "$a -lt 100 -o $b -gt 100 : returns false"
fi

# or
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a -lt 100 -o $b -gt 100 : returns true"
else
   echo "$a -lt 100 -o $b -gt 100 : returns false"
fi
```
| 运算符 | 说明                                                |
| ------ | --------------------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 |
| -o     | 或运算，有一个表达式为 true 则返回 true。           |
| -a     | 与运算，两个表达式都为 true 才返回 true。           |

### 字符串运算符

```bash
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
    echo "a等于b"
else
    echo "a不等于b"
fi

empty=""
if [ $empty ]
then
    echo "非空"
else
    echo "空"
fi
```

| 运算符 | 说明                                      |
| ------ | ----------------------------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。   |
| !=     | 检测两个字符串是否相等，不相等返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。     |
| -n     | 检测字符串长度是否为0，不为0返回 true。   |
| str    | 检测字符串是否为空，不为空返回 true。     |

### 文件测试运算符

* 检测Unix文件的各种属性
* linux文件系统中所有设备都是文件，可以使用ls -l查看

* 文件类型：

```bash
- # 常规文件
c # 特殊档案
p # 命名管道
b # 块设备
s # 套接字
d # 目录
l # 链接
```

* 判断文件类型示例：

```bash
#!/bin/sh

file="/etc/rc.local"

# 测试文件是否有读权限
# 中括号和-r直接需有空格
if [ -r $file ]
then
	echo "文件有读权限"
else
	echo "文件没有读权限"
fi
```



| 操作符  | 说明                                                         |
| ------- | :----------------------------------------------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  |
| -p file | 检测文件是否是具名管道，如果是，则返回 true。                |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            |
| -r file | 检测文件是否可读，如果是，则返回 true。                      |
| -w file | 检测文件是否可写，如果是，则返回 true。                      |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          |

## shell脚本

### 脚本解释器标记
* `#!` 用于标记使用的解释器
* `#!/bin/bash`

### 执行脚本
* 使用source是在本shell进程中运行脚本，运行完之后，所有变量在当前shell有效
* 其他方法在新shell进程中运行脚本，运行完之后，所有变量在当前shell无效，除非export

```bash
# 执行方法1：在新shell中运行脚本

# 为脚本增加执行权限，默认没有权限
# 必须加./符号表示在当前目录下文件，不然bash解释器会在环境变量$PATH中寻找可执行程序
chmod +x ./test.sh
./test.sh # 相对路径执行
/root/test.sh # 绝对路径执行
test.sh # 添加环境变量，或放在$PATH任意目录下

# 使用解释器程序运行脚本
/bin/bash test.sh
sh test.sh
bash test.sh

# 执行方法2：在本shell中运行脚本（依次执行每行命令）
source test.sh
source ~/.bashrc

# 可以使用.代替source
. test.sh
```

### 设置返回值

```bash
exit 0  
```

### 获得输入

```bash
# 读取用户输入到变量
read input

# 带提示读取输入到变量
read -p "Please input: " input  

# 为读取内容设置默认值
var=${input:-"default"}  
```







## 流程控制

### break

* 终止循环
* break n，可以跳出n层循环

### continue

* 跳过本次循环
* continue n，可以跳过第n层本次循环

### if

```bash
# 单行if
if true; then echo "T"; fi # "T"
if false; then echo "T"; fi # ""

# 多行if
if [ expression ]
then
	command
fi

# 单行if else
if true; then echo "T"; else echo "F"; fi # T
if false; then echo "T"; else echo "F"; fi # F

# 多行 if else
if [ expression ]
then
	command
else
	command
fi

# 单行 if elif fi
var=0
if [ 0 = $var ]; then echo "0"; elif [ 1 = $var ]; then echo "1"; else echo "other"; fi

# 多行 if elif fi
if [ 0 = $var ]
then
	echo "0"
elif [ 1 = $var ]
then
	echo "1"
else
	echo "other"
fi
```



### while

* 单行循环

```bash
while true; do echo 1; sleep 1; done
```

* 多行循环
```bash
while true
do
    echo 1
    sleep 1
done
```

### for in

```bash
# 迭代列表
for x in 'a' 'b' 'c'
do
echo $x
done

# 迭代序列
for x in $(seq 3):
do
echo $x
done

# 迭代当前目录所有文件
for file in *
do
echo $file
done
```

### for

```bash
for (( x=1; x<=5; x=x+1 ))
do
echo $x
done
```

### case

```bash
case $var in
    1) 
    	command1
	    command2
    	;;
	2)
		command3
		command4
		;;
	*)
		command5
		command6
		;;
esac

# 示例1：读取输入
read -p "请输入1-3:" num
case $num in
    1) echo "一"
    ;;
    2) echo "二"
    ;;
    3) echo "三"
    ;;
    *) echo "您输入错误，$num"
    ;;
esac

# 示例2：判断参数
case $1 in
    -a)
    echo "参数-a 处理 $2"
    ;;
    -b)
    echo "参数-b 处理 $2"
    ;;
    *)
    echo "usage: -[a|b] param2"
    ;;
esac

# ./test.sh -a 3
# 参数-a 处理 3
# ./test.sh -b 3
# 参数-b 处理 3
# ./test.sh -c 3
# usage: -[a|b] param2
```

### until

```bash
until [ condition ]
do
done
```



## 函数

### 定义函数

* 定义函数必须在使用函数前

```bash
# 定义函数方法1，无关键字
funname() {
	command1
	command2
}

# 定义函数方法2，使用关键字function
function funcname() {
	command1
	command2
}
```

### 调用函数及获取函数返回值
* 1、shell函数返回值只能是整数，一般0表示成功
* 2、使用return语句显式返回值
* 3、没有显式返回值，则最后一条命令的返回值就是返回值

```bash
# 示例1
say_hello () {
    echo "Hello world!"
}

say_hello # 调用函数


# 示例2
sum () {
    read -p "Input number1:" number1
    read -p "Input number2:" number2
    return $(( $number1 + $number2 ))
}

sum
echo "Sum is $?" # 使用$? 获取返回值
```

### 删除函数

* 与删除变量同样使用unset，但需要加上.f参数
```bash
unset .f funcname
```

### 函数的参数

* 函数获取参数与使用脚本参数相同
* 函数输入参数与调用脚本参数相同
* 第十个参数开始不能使用$1获取，只能使用${10}获取

```bash
#!/bin/bash

sum () {
    echo "param1 $1"
    echo "param2 $2"
    echo "param all $*"
    echo "param all $@"
    echo "param count $#"

    tmp=0
    for param in $@
    do
        tmp=$(expr $tmp + $param)
    done

    return $tmp
}

sum `seq 5`
# sum $(seq 1000)，会引起错误

echo "sum is $?"
```



快速入门
==========

uname
----------
<https://www.kernel.org>
```
uname    # uname -s
uname -s # --kernel-name    # Linux  
uname -n # --nodename       # hostname
uname -r # --kernel-release # 2.6.32-573.7.1.el6.x86_64
uname -v # --kernel-version # #1 SMP Tue Sep 22 22:00:00 UTC 2015
uname -m # --machine        # x86_64
uname -p # --processor      # x86_64
uname -i # --hardware-platform # x86_64
uname -o # --operating-system  # GNU/Linux
uname -a # --all
# Linux yx-centos 2.6.32-573.7.1.el6.x86_64 
# #1 SMP Tue Sep 22 22:00:00 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

引导修复
----------
CentOS启动盘 -> Troubleshooting -> Rescue a CentOS system
```
chroot /mnt/sysimage    
grub2-install /dev/vda    
exit    
reboot    
```

Grub引导Win7
----------
vim /etc/grub.d/40_custom添加：    
```
menuentry "Windows 7"  
{  
    set root='(hd0,3)'  
    chainloader +1  
}  
grub2-mkconfig -o /boot/grub2/grub.cfg  
```

修改Grub参数启动等待时间
----------
/etc/default/grub  

CentOS7网卡命名
----------
<https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Understanding_the_Predictable_Network_Interface_Device_Names.html>  

重新启动X Window
----------
alt + ctrl + backspace  


系统命名大小写
----------
Linux下文件名、命令区分大小写

语言语系
----------
```
locale  # 显示语言语系  
```

### 修改语言语系
语系配置文件: /etc/locale.conf
```
LANG=en_US.utf8 # 输出信息语言语系
export LC_ALL=en_US.utf8  
export LC_ALL=zh_CN.UTF8 # 中文
```

date # 日期时间
----------
```
# 设置日期
date MMDDhhmm[[CC]YY][.ss]

# 显示时间
date -u # --utc --universal UTC时间  
date +%s # UTC秒  
date +%Y%m%d # 20200628
date +'%Y %m %d' # 2020 06 28
```

### format格式化显示日期
```
date +FORMAT  
%Y 长年份  
%y 短年份   
%m 数字月   
%b 缩写月字  
%B 完整月字  
%d 日期  
%H 数字小时  
%M 数字分钟  
%S 分钟内秒数  
%s 1970-01-01 00：00：00 起的秒数  
%a 缩写星期字  
%A 完整星期字  
%w 一周中的第几天  
%W 一年中的第几周  
%z 时区 +0800  
%：z 时区 +08：00  
%Z 时区 字符缩写  
```

cal # calendar 月历
----------
```
cal  # 本月月历  
cal [year] # 全年月历  
cal 2020
cal [month] [year]  # 指定月历  
cal 06 2020
```

bc # 交互式计算器
----------
bc  
```
> quit # 退出  
> scale=3 # 小数点设置，默认整数计算  
```

### bc运算符号
+ - * / ^ % 

info
----------
### info文件路径
/usr/share/info  

### 顶部提示
File # 归属文件  
Node # 节点，TOP为顶层  
Next # 下一章节   
Up # 上一层  
Prev # 前一章节  

### 操作
N # 下一节  
P # 上一节  
U # 上一层  
h # 帮助  
Enter # 进入带 * 超链接    
Tab # 在带*超链接中移动  
Space # 下一节  



shutdown
----------
shutdown [-option] [time] [message]  
time type 1 : now  
time type 2 : 20:25  
time type 3 : +10 (minute)  

* shutdown centos6
```
shutdown -k # 发送警告
shutdown -r # reboot  
shutdown -h # halt  
shutdown -c # cancel  
shutdown -P # poweroff # init 0  
```

* shutdown centos7
```
systemctl halt # halt  
systemctl reboot # reboot  
systemctl poweroff # poweroff  
systemctl suspend  # 睡眠 
systemctl hibernate # 休眠
```

CentOS(GDM)开机自动登录
----------
* 配置文件 `/etc/gdm/custom.conf`
``` 
[daemon]  
AutomaticLoginEnable=true  
AutomaticLogin=jiangwx  
TimedLoginEnable=true  
TimedLogin=jiangwx  
TimedLoginDelay=7  
[security]  
AllowRoot=false  
[xdmcp]  
[greeter]  
DefalutWelcome=false  
Welcome=Wait seconds...  
[chooser]  
```

*  

配置login欢迎信息
----------
/etc/issue  
自定义说明：man agetty 搜索关键词 escape  




文件与目录管理
==========

当前工作目录
----------
pwd  
pwd -P 显示链接真实路径  

建立目录
----------
mkdir -m 775 dir #建立目录时设置权限  
mkdir -p /dir/dir/dir/dir #建立多级目录  

删除目录
----------
rmdir -p #连同上层空目录一起删除  

查看环境变量
----------
env  
echo $PATH  

添加PATH变量
----------
PATH="${PATH}:/ADDPATH"  

ls 默认按文件名排序
----------
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

cp # 复制
----------
### 权限
cp默认使用执行者的权限

### 参数 
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

rm
----------
rm -f #强制  
rm -i #删除前询问  
rm -r #递归删除  

mv
----------
mv -f #强制  
mv -i #询问是否覆盖  
mv -u #update  

rename
----------
用于批量改名  
rename .htm .html *.htm  

临时取消alias
----------
\alias 反斜杠  

取得文件名
----------
basename path  

取得目录名
----------
dirname path  

cat(Concatenate)
----------
cat -v #显示不可见字符  
cat -E #显示结尾换行字符$  
cat -T #显示TAB为^|  
cat -A # = cat -vET  
cat -n #显示行号，空白行有行号  
cat -b #显示行号，空白行无行号  

tac
----------
逆向cat  

nl(numbering line)
----------
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

more
----------
不能向前翻页  

less
----------
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

head
----------
default 10行  
head -n num file #显示前n行  
head -n -num file #显示直到倒数n行  

tail
----------
default 10行  
tail -n num file #显示后n行  
tail -n +num file #显示直到正数n行  
tail -f #持续检测  

od(octal dump)
----------
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

touch
----------
默认修改modify和access  
touch -a #修改accesstime  
touch -m #修改modifytime  
touch -c #只修改时间，不创建  
touch -d #--date="时间或者日期"  
touch -t YYYYMMDDhhmm #修改为指定时间  

chattr
----------
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

lsattr
----------
-a #显示隐藏文件的属性  
-d #显示目录的属性，默认显示目录内的文件  
-R #递归  

file
----------
检测文件类型  
检测压缩类型  
解析mbr、gpt分区备份  

which
----------
which command #查找命令位置  
which -a command #查找所有命令位置  
type command #查找bash指令  

whereis # 命令搜索
----------
只查找bin man src  
whereis -l #列出whereis查找的目录  
whereis -b #只查找binary格式文件  
whereis -m #只查找manual文件  
whereis -s #只查找srouce文件  
whereis -u #搜索除bin man src之外的其他特殊文件  

locate # 索引搜索
----------
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

find # 全盘搜索
----------
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

### 多个后缀名查询
find /mnt/f \( -name *.mp4 -o -name \) -exec ./ffprobe {} \;

磁盘与文件系统
==========
MBR与GPT
----------
### MBR
MBR（Master Boot Record)是MSDOS的分区格式，早期Linux兼容磁盘分区方式也使用MBR  
MBR区域为512Bytes，前446Bytes为MBR主开机记录区，后64Byte为分割表（partition table）区，64Byte记录4个主分区（Primary)或延展分区（Extended）或逻辑分区logical partition，每个分区16字节，限制磁盘容量大小为2.2T。延展分区分区表记录在延展分区之前（或主分区之后）。  
/dev/sda[1-4]为主分区设备名，若有逻辑分区则越过。  
/dev/sda[5-n]为逻辑分区设备名。  

### GPT(GUID partition table)


dumpe2fs 显示分区格式信息
----------
dump ext2/3/4 filesystem information  
dumpe2fs /dev/sda1 #列出文件系统信息  
dumpe2fs -h /dev/sda1 #只列出superblock  

blkid
----------
blkid #列出目前挂载的已经格式化的设备的UUID  

系统支持的文件系统
----------
cat /proc/filesystems  
ls -l /lib/modules/$(uname -r)/kernel/fs  

df
----------
df -option path  
df -a #列出全部文件系统，包括虚拟文件系统  
df -{k|m} # == df -B{K|M|G|T|P|E|Z|Y} --block-size=SIZE  
df -h #humanreadable  
df -T #列出文件系统格式  
df -i #列出inode数量信息，默认列出容量信息  

du
----------
du -option path  
du -a #列出目录与文件大小，默认不显示文件  
du -h #humanreadabe  
du -s --summarize  
du -S --separate-dirs #不统计子目录  

lsblk # list block device  
----------
lsblk -option path  
lsblk -d #disk only  
lsblk -f #列出文件系统类型及UUID  
lsblk -i #使用ASCII输出关系符号  
lsblk -m #输出设备权限信息  
lsblk -p #列出设备完整名称  
lsblk -t #列出设备详细资料  

parted 命令式分区工具
----------
parted -l #显示所有分区  
mkpart [primary|logical|extended] [ext4|vfat|xfs] startMB endMB  
parted rm [partition] #删除  
parted device print #显示  
parted device mklabel gpt|msdos|loop #修改分区类型  

gpt交互式分区工具
----------
gdisk /dev/device  
? #help  
d #delete  
n #new  
p #print  
q #quit  
o #重建GPT分区表  
w #write  

mbr交互式分区工具
----------
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

立即更新分区表
----------
partprobe  
partprobe -s #打印信息  

查看分区信息
----------
cat /proc/partitions  

备份还原MBR
----------
### 备份硬盘主引导记录   
``` sh
dd if=/dev/hda of=/disk.mbr bs=512 count=1  
```
### 还原硬盘主引导记录  
``` sh
dd if=/disk.mbr of=/dev/hda bs=512 count=1  
```

mkfs.xfs
----------
mkfs.xfs -option /dev/device  
mkfs.xfs -b blocksize #512 - 4K  
mkfs.xfs -d parms #data section高级参数  
mkfs.xfs -f #强制格式化已经被格式化的分区  
mkfs.xfs -i parms #inode相关高级参数  
mkfs.xfs -L labelname  
mkfs.xfs -r parms #realtime section相关参数   

mke2fs
----------
创建分区系统/格式化  
mke2fs -t ext4 /dev/sdb1  
-b #blocksize 块大小  
-c #check badblocks 坏块  
-L #label 指定卷标  
-j #journal 建立文件系统日志  

### 格式化相关默认值
/etc/mke2fs.conf   

### 其他
mkfs.ext3 /dev/sdb1  
mkfs.ext4 -b size -L label /dev/sdb1  
mkfs.vfat /dev/sdb1  

### 显示/修改lable标签
e2label /dev/sdb1 NEWNAME  

xfs_repair
----------
xfs_repair /dev/device  
xfs_repair -f file #检查文件   
xfs_repair -n #只检查而不修复   
xfs_repair -d #在单人维护模式下修复根目录   

xfs_growfs
----------
expands an existing xfs filesystem  

fsck.ext4
----------
fsck.ext4 -pf -b superblock /dev/device  
fsck.ext4 -p /dev/device #自动确认修复  
fsck.ext4 -f /dev/device #强制检查  

fsck
----------
fsck -t ext4 /dev/sdb1 #检查分区 -t指定文件系统格式，损坏严重时使用  
fsck -y /dev/sdb1 #自动修复  

mount
----------
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

### mount替硬连接
硬链接不能连接目录，不能跨文件系统  
mount --bind oldfile new file  
mount --bind olddir newdir  
umount卸载时必须使用挂载点  

### mount samba
mount.cifs //192.168.1.5/d /mnt/d -o username=yx,password=H  
mount -t cifs //192.168.1.5/e /mnt/e -o username=yx,password=H,iocharset=utf8  

umount
----------
umount -f #强制卸载  
umount -l #立即卸载  
unmount device  
unmount dir  

fuser
----------
yum install psmisc   
identify processes using files or sockets   
fuser -m /dev/sdb1 #列出文件打开的用户   

lsof
----------
yum install lsof  
lsof /mnt/testsdb #list open file 列出打开文件  

mknod
----------
mknod device [bcp] [Major] [Minor]  
修改设备类型、设备代码  
b block  
c char  
p fifo  

xfs_admin
----------
xfs_admin [-lu] [-L label] [-U uuid] device  
修改xfs系列文件系统信息  
修改xfs Label UUID等配置  
xfs_admin -l #显示设备Label  
xfs_admin -u #显示设备uuid  

uuidgen
----------
生成uuid  

tune2fs
----------
tune2fs [-l] [-L label] [-U uuid] device    
修改ext系列文件系统信息  
tune2fs -l device#dumpe2fs -h 显示superblock信息  
tune2fs -L label #修改label  
tune2fs -U uuid #修改uuid  

/etc/fstab
----------
### 限制：  
1、根目录必须挂载，且必须首先挂载  
2、挂载点必须已经建立目录  
3、一个挂载点只能挂载一次  
4、一个设备只能挂载一次  

### 格式
设备/uuid/label  挂载点  文件系统类型  选项  dump是否归档  fsck  
/dev/sdb    /mnt    ext4    default    0    0  

### fstab带帐号密码格式
//server/share /mount/point smbfs username=[username],password=[password] 0 0  

重新挂载root
----------
单人模式中根目录是readonly模式使用以下命令修改文件  
mount -o remount,rw,auto /  

挂载光盘镜像
----------
mount -o loop isofile dstdir  

文件虚拟磁盘  
----------
dd if=/dev/zero of=filepath bs=1M count=512  
mkfs.xfs -f filepath  
mount -o loop filepath mountpoint  

swap
----------
分区ID设置为8200  
mkswap device  
mkswap file #文件做swap  
swapon device  
swapon -s #show  
swapon -a #加载swap  
swapoff file|device  

### /etc/fstab  
device swap swap defaults 0 0  

综合：vultr磁盘分区、格式化、挂载
----------
### 查看
``` sh
lsblk
```

### 分区
``` sh
parted -s /dev/vdb mklabel gpt
parted -s /dev/vdb unit mib mkpart primary 0% 100%
```

### 格式化
``` sh
mkfs.ext4 /dev/vdb1
```

### 挂载
``` sh
mkdir /mnt/blockstorage
echo >> /etc/fstab
echo /dev/vdb1               /mnt/blockstorage       ext4    defaults,noatime 0 0 >> /etc/fstab
mount /mnt/blockstorage
```

压缩打包备份
==================================================

后缀名
--------------------------------------------------------------------------------------
*.Z #compress（淘汰）  
*.zip #zip  
*.gz #gzip  
*.bz2 #bzip2  
*.xz #xz  
*.tar #tar  
*.tar.gz #tar gzip  
*.tar.bz2 #tar bzip2  
*.tar.xz #tar xz  

gzip
--------------------------------------------------------------------------------------
支持.Z .gz 对目录内的内容分别压缩  
gzip file  #压缩并替换原文件  
gzip -c file > file.gz #输出压缩后的数据流  
gzip -d file #decompress  
gzip -t file #测试gz文件格式是否正确  
gzip -v file #verbose  
gzip -1 file #压缩等级，-1最快，压缩比最差，-9最慢，压缩比最好，default -6  

### gzip压缩相关命令
zcat  
zmore  
zless  
zgrep  
egrep # 支持.gz  
znew # .Z 转 .gz  

bzip2
--------------------------------------------------------------------------------------
bzip2 -c file > file.bz2 #输出压缩后的数据流  
bzip2 -d file.bz2 #decompress  
bzip2 -k file #keep original file  
bzip2 -z option file #option  
bzip2 -v file #verbose  
bzip2 -1 file #-1 --fast -9 --best  

### bzip相关命令
bzcat  
bzmore  
bzless  
bzgrep  

xz
--------------------------------------------------------------------------------------
xz -c file > file.xz  
xz -d file.xz #decompress  
xz -t file.xz #test  
xz -l file.xz #list  
xz -k file #keep original file  
xz -1  

### xz相关命令
xzcat  
xzmore  
xzless  
xzgrep  

tar
--------------------------------------------------------------------------------------
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

xfsdump
--------------------------------------------------------------------------------------
1、不支持没有挂载的文件系统备份  
2、必须使用root权限  
3、只能备份xfs文件系统  
4、只能用xfsrestore还原  
5、使用UUID作为标识  
xfsdump [-L session_label] [-M media_label] [-l 0-9] [-f file] filesystem  
-l 0-9 # increamental backup level  
xfsdump -I #i info /var/lib/xfsdump/inventory  
* 注意增量备份时label务必不同 * 

xfsrestore
--------------------------------------------------------------------------------------
xfsrestore -I #i info == xfsdump -I  
xfsrestore -f dumpfile -L session_label -s spesific_file_or_dir restoredir  
依次恢复level0到level9  
xfsresotre -f dumpfile -i restoredir #互动模式  

mkisofs  
--------------------------------------------------------------------------------------
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

isoinfo
--------------------------------------------------------------------------------------
显示iso信息  
isoinfo -d -i file.iso  
-d #discription  
-i #image  

CentOS7 wodim 光盘刻录
--------------------------------------------------------------------------------------
略

CentOS6 cdrecord 光盘刻录
--------------------------------------------------------------------------------------
略

dd
--------------------------------------------------------------------------------------
dd if="input_file" of="output_file" bs="blocksize" count="number"  
bs #default 512bytes  
dd if=file1 of=file2 #复制文件  
dd if=/dev/sr0 of=file.iso #制作镜像（镜像大小等于设备容量）  
dd if=file.iso of=/dev/sda #制作usb启动盘  
dd if=/dev/urandom of=filepath bs=1M count=1 # 创建随机文件  
dd if=/dev/zero of=filepath bs=1M count=1 # 创建空文件

cpio
--------------------------------------------------------------------------------------
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













第十五章：crontab、at
==================================================
at
--------------------------------------------------------------------------------------
### 简介
需要服务atd，centos默认启动  
at产生执行文本存入/var/spool/at/等待atd调用  

### 执行优先级
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

batch # 使用方法同at
--------------------------------------------------------------------------------------
uptime load average低于1时执行  

crontab
--------------------------------------------------------------------------------------
### 简介
需要crond服务  

### 优先级
优先级1、/etc/cron.allow使用权限白名单  
优先级2、/etc/cron.deny使用权限黑名单，默认预留空cron.deny允许所有用户执行  

### 相关位置
crontab命令建立的任务存入/var/spool/cron/username  
执行日志 /var/log/cron  

### 格式、参数
crontab [-u username] [-l|-e|-r]  
-u # 给指定用户色画质  
crontab -e #vi edit  
crontab -l #list  
crontab -r #remove all, 删除其中一项用-e编辑  

### 格式
分钟    小时    日期    月份    周    command  
0-59    0-23    1-31    1-12    0-7（0和7都代表星期天，周与日月不能并存）  

### 通配符
星号* # 忽略这个参数，符合其他参数就执行  
, # 列举  
减号- # 范围  
*/n # 每隔n时间单位执行一次  

### 示例
59    23    1    5    *    command #每年五月一号23点59分执行  
*/5    *    *    *    *    command #每隔5分钟执行  
30    16    *    *    5    command #每周五16点30分执行  

### 系统crontab
/etc/crontab 
/etc/cron.d/*
* 某些系统会把crontab加载到内存，需要重启crond重新读取crontab *  
/etc/cron.d/0hourly每小时定时执行/etc/cron.hourly里的脚本  
/etc/cron.hourly/0anacron定时执行daily weekly monthly任务  
/etc/cron.hourly /etc/cron.daily /etc/cron.weekly /etc/cron.monthly 里直接放执行脚本  

### 各种cron区别
个人用户使用crontab -e其他用户看不到  
系统维护使用/etc/crontab方便管理追踪  
自己开发使用/etc/cron.d/file方便升级修改时直接替换文件  
每小时/每日/每周/每天/每月固定执行使用/etc/cron.hourly|daily|weekly|monthly，系统会从目录中随机抽取任务执行，避免同时执行  

anacron
--------------------------------------------------------------------------------------
### 简介
检查cron.daily及以上的任务是否因关机而未执行，为执行则定期执行  
/etc/cron.hourly/0anacron #每小时定时检查程序是否定时执行anacron -s  

anacron -s [job] # 根据/var/spool/anacron/*判断是否已经执行，未执行则执行  
anacron -f [job] # 不判断执行情况，强制执行   
anacron -n [job] # now，立即检查执行情况，未执行的立即执行，而不按配置延迟  
anacron -u [job] # update，只更新执行时间记录而不执行任何任务  

### 配置文件
/etc/anacrontab  
RANDOM_DELAY=45 #随机给与最大延迟时间，单位分钟  
START_HOURS_RANGE=3-12 #延迟多少个小时执行  

格式：天数    延迟时间（分钟）    工作名称定义    实际要执行的command（利用cron的run-parts)  
天数：与/var/spool/anacron/*记录的时间差，超过则执行  
延迟时间：按天数判断超时后，延迟START_HOURS_RANGE+延迟时间 执行，避免同时执行  

程序管理与SELinux
==========
基础知识
----------
子程序会继承父程序权限  
程序 program  
进程 process  
fork and exec # Linux进程会先fork复制一份进程作为临时程序，然后再exec执行子程序。  

工作管理
----------
### 限制登录个数
/etc/security/limits.conf  

### 在后台执行任务
job control只能管理一个bash下的工作  
工作状态有stop和running  
使用&符号启动后台任务  
立即返回值为[JobID]ProcessID，JobID只跟当前bash有关  
执行完成后返回在bash中返回执行结果  
执行过程中会将信息打印在bash，可以使用 > output.txt 2>&1 &，将输出、错误导出到文件  

### 在后台暂停任务
[ctrl]+[z]  

### 恢复后台暂停任务
bg  
bg %jobid  

### 查看后台任务状态
jobs [-lrs]  
-l # 列出所有后台任务的JobID Job状态 命令 PID  
-r # running 列出正在运行状态的工作  
-s # stop 列出正在暂停的工作  
加号+代表最近的一个任务，使用fg即可恢复  
减号-代表倒数第二个任务   

### 恢复后台任务
fg  
fg %jobid  

### 停止后台任务
kill -signalid %jobid # 对job发出信号，使用man 7 signal查询信号  
kill -l # 列出所有signal信号  

信号：  
-1 # 重新读取一次参数设定，类似reload  
-2 # 与[ctrl]+[c]相同  
-9 # 强制立即停止  
-15 # 信号默认值，以正常方式终止程序  
* 例如使用-9，vi会来不及删除swp文件，使用-15，vi会以正常流程结束  
* 使用signal名称与使用signal数字相同  

### bash后台与Linux系统后台
仅以&结尾的命令在bash后台运行，bash退出则命令停止运行  
使用at可以使命令在系统后台运行与bash无关  
使用nohup可以使命令在系统运行与bash无关  

nohup command # 在bash前台运行，会打印到前台  
nohup command & # 在bash后台运行  
输出会存储在~/nohup.out  

程序管理
----------
### ps  
参数（带-不带-有区别）：  
-A # 显示所有进程，等于-e  
-a # 显示与terminal无关的进程  
-u # 显示有效使用者（effective user）相关进程  
x # 显示完整信息  
l # 显示较长，较详细的信息  
J # 工作的格式（jobs format）  
-f # 更加完整的信息  

常用指令：  



特殊文件与权限
----------

SELinux
----------



systemd # service daemon 服务 守护进程
==========
基础知识
----------
daemon = service  
daemon进程通常以d结尾  

### init管理方式（CentOS6）
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

### systemd管理方式（CentOS7）
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

systemctl
----------
### 控制命令
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

### 服务状态
active(running) # 正在活动、正在执行  
active(exited) # 正在活动，之行结束  
active(waiting) # 正在活动，等待触发  
inactive # 服务当前没有活动  
enabled # 开机时被执行  
disabled # 开机时不执行  
static # 只能被其他服务唤醒，不能开机执行  
mask # 设置为无法被启动，注销。  

### 查询服务
参数：  
--all # 列出未启动的  
--type=service,socket,target # 列出指定类型  

命令：  
systemctl == systemctl list-units # 列出所有启动（loaded）的unit  
systemctl list-unit-files # 列出/usr/lib/systemd/system/ 下所有服务  
systemctl list-unit-files | grep enabled # 查看已启动的服务列表  

### target管理
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

### target切换原理
systemd使用链接来指向默认的运行级别。在创建新的链接前，可以通过下面命令删除存在的链接  
rm /etc/systemd/system/default.target  
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target # 默认启动运行级别3  
ln -sf /lib/systemd/system/graphical.target/etc/systemd/system/default.target # 默认启动运行级别5  

### target快捷命令
systemctl poweroff  
systemctl reboot  
systemctl suspend # 暂停模式  
systemctl hibernate # 休眠模式  
systemctl rescue # 救援模式  
systemctl emergency # 紧急救援模式  

### 依赖分析
systemctl list-dependencies x.target # 显示依赖  
systemctl list-dependencies --reverse x.target # 递归显示依赖  

配置服务
----------
### 配置目录  
/usr/lib/systemd/system/vsftpd.service # 官方配置  
/etc/systemd/system/vsftpd.service.d/custom.conf # 建立同名目录放置修改配置.conf  
/etc/systemd/system/vsftpd.service.requires/* # 启动依赖，启动前调用  
/etc/systemd/system/vsftpd.service.wants/* # 建议依赖，启动后调用  

### 配置说明
`After=xxx` # 赋值  
`1, yes, true, on` # 真  
`0, no, false, off` # 假  
`#, ;` # 注释  

### 配置项
[Unit] # 设置unit说明  
[Services], [Socket], [Timer], [Mount], [Path] # 设置类型相关配置  
[Install] # 设置归属target  

### Unit配置项
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

### Service配置项
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

### Install配置项
WantedBy # 归属target，如multi-user.target  
Also # 开启依赖服务，开启时自动enable的服务  
Alias # 别名，例如default.target是multi-user.target  

### 自定义服务配置示例
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

getty及相同服务多实例通配
----------
（使用@通配文件中数字，使用%I在脚本中获取通配）  
<http://linux.vbird.org/linux_basic/0560daemons.php#systemd_cfg_repeat>  

timer相关服务配置
----------
<http://linux.vbird.org/linux_basic/0560daemons.php#systemctl_timer>

CentOS7预设服务配置
----------
### 预设服务
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

### 常用server服务
dovecot # POP3/IMAP服务  
httpd # web server  
named # 域名服务  
nfs / nfs-server # Network Filesystem，NFS网络文件服务  
smb / nmb # Windows文件共享，网络邻居服务  
vsftpd # FTP服务  
sshd # ssh服务  
rpcbind # RPC服务，NFS、NIS依赖RPC  
postfix # 邮件服务  


第十八章：日志分析
==================================================
重要  


第十九章：开机流程、模块管理、Loader
==================================================
重要  


第二十章：系统基本设置与备份
==================================================
nmcli
--------------------------------------------------------------------------------------
### 列出所有连接/连接配置
nmcli connection show  
nmcli connection enp0s3  

### 配置固定文件
nmcli connection modify eth0 connection.autoconnect yes  
nmcli connection modify eth0 ipv4.method manual  
nmcli connection modify eth0 ipv4.addresses 192.168.1.33/24  
nmcli connection modify eth0 ipv4.gateway 192.168.1.1  
nmcli connection modify eth0 ipv4.dns 192.168.1.1  


timedatectl
--------------------------------------------------------------------------------------
timedatectl # 列出时间日期信息  

### 修改时区
timedatectl list-timezones  
timedatectl set-timezone "Asia/Shanghai"  

### 修改时间
timedatectl set-time "2012-10-30 18:17:16"  
date/hwclock #timedatectl包含修改主板时间动作  

### 自动ntp对时
timedatectl set-ntp true/false  
systemctl status chronyd.service  

### 手动ntp对时
ntpdate tock.stdtime.gov.tw  
hwclock -w  

localectl
--------------------------------------------------------------------------------------
localectl # 显示系统语系设定  
localectl set-locale LANG=en_US.utf8  
local #显示bash语系设定  
export LC_ALL=en_US.utf8 #设置bash语系设定  

dmidecode
--------------------------------------------------------------------------------------
dmidecode -t system  
dmidecode -t processor  
dmidecode -t memory  

lspci
--------------------------------------------------------------------------------------
lspci  
lspci -v # 详细  
lspci -vv # 细节  
lspci -n # 直接显示ID不解析厂商名称  
lspci -s 00:00.0 [-vvn] # 显示某一设备的信息  

/usr/share/hwdata/pci.ids # pci信息对照表  
update-pciids # 更新pci信息对照表  

lsusb
--------------------------------------------------------------------------------------
lsusb  
lsusb -t # tree显示  
/usr/share/hwdata/pci.ids  

iostat
--------------------------------------------------------------------------------------
### 安装  
yum install sysstat  

### 参数
iostat # 显示所有信息  
iostat -c # 仅显示cpu状态  
iostat -d # 仅显示存储设备状态  
iostat -k #按K bytes显示存储信息  
iostat -m #按M bytes显示存储信息  
iostat -t #显示时间日期  
iostat -d 2 3 vda #间隔2秒 检测3次 vda  

硬盘SMART
--------------------------------------------------------------------------------------
smartctl -a /dev/sda  
smartctl -t testtype /dev/sda  

备份
--------------------------------------------------------------------------------------
/etc #配置  
/home #用户文件  
/var/spool/mail #邮件  
/var/spool/at #定时任务  
/var/spool/cron #周期任务  
/boot #内核更新  
/root #管理员文件  
/usr/local #第三方软件及配置  
/opt #第三方软件及配置  
/var/www #httpd  
/srv/www #httpd  
/var/lib/mysql #db  

### dd备份整盘
dd if=/dev/sda of=/dev/sdb  

### cpio备份整盘
find / -print | cpio -covB > /dev/st0  
cpio -iduv < /dev/st0  

### xfsdump差异备份
xfsdump -l 0 -L 'full -M 'full' -f /backupdata/home.dump /home  
xfsdump -l 1 -L 'full-1' -M 'full-1' -f /backupdata/home.dump /home  

### tar排除备份
tar --exclude /proc --exclude /mnt --exclude /tmp --exclude /backupdata -jvcp -f /backupdata/system.tar.bz2 /  

### tar差异备份
tar -N '2015-09-01' -jpcv -f /backupdata/home.tar.bz2 /home  

### rsync本地同步备份
rsync -av srcdir dstdir  

### rsync网络同步文件夹
rsync *.* root@192.168.1.1:/root/  
rsync -av -e ssh srcdir root@192.168.1.1:/dstdir  



CentOS源 # rpmfusion  EPEL  Remi RPMForge
--------------------------------------------------------------------------------------

### centos6
sudo yum install epel-release  
sudo yum install http://rpms.famillecollet.com/enterprise/remi-release-6.rpm  
sudo yum localinstall --nogpgcheck http://download1.rpmfusion.org/free/el/updates/6/i386/rpmfusion-free-release-6-1.noarch.rpm  

### centos7
sudo yum install epel-release  
sudo yum install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm  
sudo yum install http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el7.rf.x86_64.rpm  

srpm
--------------------------------------------------------------------------------------
### 不修改任何设置直接对srpm包操作
rpmbuild --rebuild # 编译、打包、生成rpm包、不安装，生成的安装包存放路径有提示  
rpmbuild --recompile # 编译、打包、并且安装  

### 目录
./rpmbuild/SPECS # 软件的设置文件  
./rpmbuild/SOURCES # 源码以及config文件  
./rpmbuild/BUILD # 编译过程文件  
./rpmbuild/RPMS # rpm包  
./rpmbuild/SRPMS # SRPM解压  

### 设置文件
./rpmbuild/SPECS/packagename.spce  

### 在srpm解压的包中编译 
rpmbuild -ba package.spes # 编译成rpm和srpm  
rpmbuild -bb package.spes # 编译成rpm  

### 将自己的程序打包成rpm和srpm
建立spec文件  
http://linux.vbird.org/linux_basic/0520rpm_and_srpm.php  




第二十三章：xwindow
==================================================
略

第二十四章：内核编译
==================================================
略

服务器四：Internet连接
==================================================
查看内核网卡加载
--------------------------------------------------------------------------------------
dmesg | grep -in eth
eth0: e1000_probe: Intel(R) PRO/1000 Network Connection

查看pci网卡加载
--------------------------------------------------------------------------------------
lspci | grep -i ethernet

查看内核模块加载
--------------------------------------------------------------------------------------
lsmod | grep e1000_probe

查看模块信息
--------------------------------------------------------------------------------------
modinfo e1000

手动加载模块
--------------------------------------------------------------------------------------
cp e1000.ko /lib/modules/$(uname-r)/kernel/drivers/net/e1000/e1000.ko  
rmmod e1000  
modprobe e1000  
modinfo e1000  

手动设置开机加载模块
--------------------------------------------------------------------------------------
echo "alias eth0 e1000" > /etc/modprobe.d/ether.conf  
 sync; reboot;  

手动临时设置ip
--------------------------------------------------------------------------------------
ifconfig eth0 192.168.1.1  

配置文件
--------------------------------------------------------------------------------------
### /etc/sysconfig/network-scripts/ifcfg-eth0
```
DEVICE="eth0"  
BOOTPROTO="dhcp/none" #是否使用dhcp  
HWADDR="MAC" #用于区分相同驱动网卡  
IPADDR=ip  
NETMASK=ip  
ONBOOT="yes"  
GATEWAY=ip
NM_CONTROLLED="no" #网管软件
NETWORK=ip #略
BROADCAST=ip #略
MTU=1500 #略
```

### /etc/sysconfig/network
```
NETWORKING=是否支持网络
NETWORKING_IPV6=是否支持IPV6
HOSTNAME=主机名
```

### /etc/resolv.conf
nameserver dnsip  
dig www.baidu.com #检查是否设置成功  

### /etc/hosts
ip    主机名称    别名  

### /etc/service
tcp/udp服务端口号定义  

### /etc/protocols
ip承载协议定义  

### /etc/init.d/network restart
systemctl restart network.service  
网络启动脚本  

### ADSL
yum install rp-pppoe 
pppoe-setup  
pppoe-start  
pppoe-stop  
chkconfig pppoe-server off  

### GATEWAY
gateway 只能在一个网卡的配置文件中设置，系统共用一个gateway  
dhcp时不能设置gateway  

服务器五：网络操作
==================================================



dhcclient # 手动获取ip
--------------------------------------------------------------------------------------
dhclient eht0  

### 检测网络内所有ip
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


telnet
--------------------------------------------------------------------------------------
telnet host  
telnet ip  
telnet ip port # 检测端口是否启动  

ftp
--------------------------------------------------------------------------------------
ftp host/ip port  
```
>anonymous #匿名登录  
>help  
```

lftp # 命令式ftp
--------------------------------------------------------------------------------------
lftp -p port -u user,pwd host/ip #交互模式  
lftp -f ftpscriptfile #ftp脚本模式  
lftp -c "command" #命令模式  

links # 文字浏览器
--------------------------------------------------------------------------------------
略

wget # 下载
--------------------------------------------------------------------------------------
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

tcpdump
--------------------------------------------------------------------------------------
tcpdump [-AennqX] [-i interface] [-w writefile] [-c count] [-r readfile] [filter]  
-A #截取内容以ASCII显示  
-e #使用mac层信息显示  
nn #不解析，使用ip/port显示  
-q #仅列出简短信息  
-X #十六进制 ASCII对照显示  

### filter
'host baidu.com'  
'host ip'  
'net 192.168'  
'src host 127.0.0.1'  
'dst net 192.168'  
'tcp/udp/arp/ether port 22'  
and #且  
or #或  

nc/netcat # 端口连接器
--------------------------------------------------------------------------------------
nc ip port  
nc -u ip/host port #udp  
nc -l ip/host port #listen  
nc -l ip port -e /bin/bash #将本地bash暴露到端口，需要编译参数GAPING_SECURITY_HOLE  

服务器七：网络安全
==================================================
防火墙分类
--------------------------------------------------------------------------------------
网络层防火墙：IP Filtering Net Filter  
传输层防火墙：/etc/hosts.allow /etc/hosts/hosts.allow  
应用层防火墙：应用程序权限  
系统级防火墙：SELinux，系统控制的应用程序权限  
文件级防火墙：文件权限  

nmap
--------------------------------------------------------------------------------------
nmap [type] [option] [hosts or nets]  

### type
-sT #已连接的tcp  
-sS #带有SYN的tcp  
-sP #以ping方式进行扫描  
-sU #以UDP进行扫描  
-sO #以IP进行扫描  

### option
-PT #使用TCP的ping扫描  
-PI #使用ICMP的ping扫描  
-p portrange #使用特定端口范围扫描  

### hosts or nets
192.168.1.100  
192.168.1.0/24  
192.168.*.*  
192.168.1.0-50, 60-100, 103, 200  

### 示例
nmap ip #默认，扫描tcp端口  
nmap -sTU ip #扫描tcp和udp端口  
nmap -sP net #扫描网络内所有开机的机器  

selinux
--------------------------------------------------------------------------------------
略

服务器八：路由
==================================================
查看是否开启转发功能
--------------------------------------------------------------------------------------
cat /proc/sys/net/ipv4/ip_forward

临时开启数据转发功能
--------------------------------------------------------------------------------------
echo 1 > /proc/sys/net/ipv4/ip_forward

永久开启数据转发功能
--------------------------------------------------------------------------------------
/etc/sysctl.conf  
net.ipv4.ip_forward = 1  
sysctl -p # 立即生效  

使用linux做路由器
--------------------------------------------------------------------------------------
### 自动路由  
### arp转发  

服务器九：防火墙、NAT转发
==================================================

服务器十：域名
==================================================

服务器十一：远程连接
==================================================
ssh
--------------------------------------------------------------------------------------
ssh ip #使用当前用户登录默认端口ip的ssh    
ssh -p port user@ip #指定端口用户登录   
ssh -f user@ip command #远程执行command不登录   
ssh -o option ip #ConnectTimeout=second, StrictHostKeyChecking=[yes|no|ask]   

### ssh证书登录
ssh -i x.pem ec2-user@ip  

### 创建ssh登录密钥
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"  

### server端密钥存储位置
cat 公钥 >> /user/.ssh/authorized_key or authorized_key2  

### 密钥格式转换
ssh-keygen -e -f ~/.ssh/id_dsa > ~/.ssh/id_dsa_com.pub   
ssh-keygen -i -f ~/.ssh/id_dsa_com.pub > ~/.ssh/id_dsa.pub  

### client端密钥存储位置
cat 私钥 > /user/.ssh/id_rsa  

### 多个client私钥存储
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

### 本机公私钥匙存储路径
/etc/ssh/ssh_host_*  
/etc/ssh/ssh_host_*.pub  

### 本用户已知公钥存储路径
~/.ssh/known_hosts  

### 本用户登录公钥存储路径
~/.ssh/authorized_key2  

### sshd配置
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

### 安全配置
/etc/hosts.allow  
sshd: ip1 ip2/255.255.255.0  
/etc/hosts.deny  
sshd:ALL  

sftp
--------------------------------------------------------------------------------------
sftp user@ip  

scp
--------------------------------------------------------------------------------------
scp [-pr] [-l limitspeed] file user@ip:/dir/file  
scp [-pr] [-l limitspeed] user@ip:/dir/file file  
-p # 传输时保留文件权限及时间戳  
-r # 递归复制目录  
-l # 限制速率为kbps  
-C # 传输时数据压缩  

xdmcp
--------------------------------------------------------------------------------------
### 组件
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

linux下远程连接桌面
--------------------------------------------------------------------------------------
runlevel 5 启动图形界面， :0 在 tty1  
runlevel 3 启动图形界面， :0 在 tty7，tty8  

xhost + 192.168.1.2  
iptales -A INPUT -i $EXTIF -s 192.168.1.2/24 -p tcp --dport 6001 -j ACCEPT  
iptables-save  
X -query 192.168.1.2 :1  

xnet
--------------------------------------------------------------------------------------
在图形界面中远程x window

xming
--------------------------------------------------------------------------------------
windows远程桌面

vnc
--------------------------------------------------------------------------------------
不加密

xrdp
--------------------------------------------------------------------------------------
加密
windows 远程连接协议

rsync
--------------------------------------------------------------------------------------
加密远程同步

服务器十二：DHCP Server
==================================================

服务器十三：NFS Server 文件服务器
==================================================

服务器十四：NIS Server 账户管理服务器
==================================================

服务器十五：NTP Server
==================================================

服务器十六：SAMBA Server 文件服务器
==================================================

服务器十七：Proxy Server 代理服务器
==================================================

服务器十八：iSCSI Server
==================================================

服务器十九：DNS Server
==================================================

服务器二十：WWW Server
==================================================

服务器二十一：vsFTPD Server
==================================================

服务器二十二：Mail Server
==================================================

其他
==================================================
Linux 驱动 硬件 
--------------------------------------------------------------------------------------
lspci 是显示硬件的。  
lsmod 是显示加载内核模块的（内核模块就是驱动）。  
用硬件检测程序kuduz探测新硬件：service kudzu start ( or restart)  
查看CPU信息：cat /proc/cpuinfo  
查看板卡信息：cat /proc/pci  
查看PCI信息：lspci (相比cat /proc/pci更直观）  
查看内存信息：cat /proc/meminfo  
查看USB设备：cat /proc/bus/usb/devices  
查看键盘和鼠标:cat /proc/bus/input/devices  
查看系统硬盘信息和使用情况：fdisk & disk - l & df  
查看各设备的中断请求(IRQ):cat /proc/interrupts  
dmidecode查看硬件信息，包括bios、cpu、内存等信息  
dmesg | more 查看硬件信息  


pandoc # markdown转html、pdf
----------
### pandoc
pandoc --ascii -f markdown -t html -o demo.html demo.md  
pandoc demo.md -o demo.doc  

### html2markdown # html转markdown

shadowsocks
----------
### 安装
```
sudo pip install shadowsocks
sudo yum install m2crypto #加速加解密  
```

### 运行
```
# 添加到 /etc/rc.local
sudo /usr/bin/python2.7 /usr/local/bin/ssserver -c  /etc/shadowssocks.json -d start  
```

shadowsocks-libev
----------
### epel 安装
repo:<https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/>
``` sh
sudo yum install yum-utils
sudo yum-config-manager --add-repo https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
sudo yum install shadowsocks-libev
```

### 运行
``` sh
# 用户配置
sudo vim /etc/shadowsocks-libev/config.json
# 系统配置
sudo vim /etc/default/shadowsocks-libev
# system v 配置
sudo /etc/init.d/shadowsocks-libev start
# systemd 配置
sudo systemctl start shadowsocks-libev # for systemd
```

### centos7 注意
默认配置中group默认为nogroup，在debian系统中有nogroup，需要修改/usr/lib/systemd/system/shadowsocks-libev.service为nobody。
``` sh
# 重新加载systemd配置
sudo systemctl daemon-reload
sudo systemctl start shadowsocks-libev
sudo systemctl status shadowsocks-libev
```

### python版本注意
ss-server不支持port_password和fast_open选项。
多个端口密码（port_password）可以使用多个配置文件代替。


tcp-bbr设置
----------
### 下载更换内核
官方网站：<http://elrepo.org/linux/kernel/el7/x86_64/RPMS/>
``` sh
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml -y
```

``` sh
# 查看内核是否安装成功
rpm -qa | grep kernel
# 删除旧内核(可选)
rpm -ev 旧内核  

# 更新 grub 系统引导文件并重启

# 查看内核列表
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
# default 0表示第一个内核设置为默认运行, 选择最新内核就对了
grub2-set-default 0  
# 重启
reboot
```

### 开启bbr
开机后 uname -r 看看是不是内核 >= 4.9
执行 lsmod | grep bbr，如果结果中没有 tcp_bbr 的话就先执行
``` sh
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
# 注意，某些服务商（如Digital Ocean）可能需要首先将VPS配置为可自定义内核，然后grub2的配置才会生效。
# 重新启动后，如果会出现“read-only file system” 的错误，root账户下执行mount -o remount rw / 即可
```

### 检查bbr开启
``` sh
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
# 如果结果都有bbr, 则证明你的内核已开启bbr
# 看到有 tcp_bbr 模块即说明bbr已启动
```

ssh代理
----------
### socks5代理
``` sh
ssh -D local-ip:local-port user@ssh-ip -p ssh-port
```
通过ssh-ip建立socks5代理，监听local-ip:local-port。
local-ip可以省略，默认为127.0.0.1。
windows下使用plink -D。
可以使用privoxy软件将socks5代理转为http代理。

``` sh
ssh -N -C yingxuanxuan@yingxuanxuan.com -p 10086 -i D:\sync\__important\openssh_keys\vultr_private -D 127.0.0.1:10085
```
-N 不执行远程命令
-C 压缩数据
-i 调用private_key
-p 指定远程接口

### 正向映射
``` sh
ssh -L local-ip:local-port:remote-ip:remote-port user@ssh-ip -p ssh-port
ssh -L :local-port:remote-ip:remote-port user@ssh-ip -p ssh-port
```
将remote-ip通过ssh-server映射到local-ip。
local-ip可以省略，默认为本机。
remote-ip为ssh-server可访问的ip。
通过http://local-ip:local-port访问remote-ip:remote-port。

### 反向映射
``` sh
ssh -R remote-ip:remote-port:local-ip:local-port user@ssh-ip -p ssh-port
```
将local-ip通过ssh-ip映射到remote-ip。
remote-ip不能省略，若省略则为127.0.0.1，只能从本机访问。
通过http://remote-ip:remote-port访问local-ip:local-port。

ss5（socks5代理）
----------

sslocal（socks5代理）
----------
### 运行
``` sh
sudo sslocal -s ss.yingxuanxuan.com -p 10085 -b 0.0.0.0 -l 10080 -k pwd -m method
```
### 开机启动
``` /etc/rc.local
/usr/bin/python /usr/bin/sslocal -s ss.yingxuanxuan.com -p 10085 -b 0.0.0.0 -l 10080 -k pwd -m aes-256-cfb --user nobody -d start --pid-file /run/sslocal.pid --log-file /var/log/sslocal.log

# 与ssserver冲突，需要修改--pid-file和--log-file
```
### 添加端口
sudo firewall-cmd --zone=public --add-port=10080/tcp --permanent

Polipo # socks5 转 http proxy
----------
### 安装
git clone https://github.com/jech/polipo.git  
make  
sudo make install  

### 配置
``` config
proxyAddress = "0.0.0.0"  
proxyPort = 10080  
socksParentProxy = "127.0.0.1:10085"  
socksProxyType = socks5  
daemonise = true  
```

### 运行
polipo  
firewall-cmd --zone=public --add-port=10080/tcp --permanent  
firewall-cmd --reload  

screen
----------
screen #进入screen  
screen -ls #列出screen  
screen -r 17793 #恢复screen  
exit  

常用
-----------
sync #同步内存中的文件  
watch -n 1 command#每1秒刷新命令  
Ctrl + r #搜索历史命令  
id username#获取用户信息  
jobs #正在后台运行的命令  
Ctrl + z #挂起  
bg #继续后台运行  
fg #前台运行  
hwclock/clock #硬件时钟  
lspci #列出pic设备 -v详细显示  
lsusb #列出usb设备 -v详细显示  
lsmod #查看加载的模块/驱动  
zip/gzip xxx.zip xxx #打包为zip  
unzip xxx.zip #解压到当前文件夹  

flash
----------
### 自动安装 flash rpm
sudo yum install epel-release  
sudo yum install flash-plugin  

### 手动安装firefox flash module
tar -xvfz *.tar.gz  
mv *.so /usr/lib64/mozilla/plugins/.  

网速监控
----------
iptraf, iftop, slurm

磁盘监控
----------
htop, iotop

系统资源监控 网速 磁盘IO CPU 系统
----------
dstat

文字图片
----------
toilet, figlet


Linux启动流程
----------
单用户模式下，不需要密码登录root，可以修改密码  
BIOS:检查硬件并查找可启动设备  
MBR:512字节，最后两个字节为0x550xaa，前446字节为引导代码  
引导程序GRUB # 配置文件/boot/grub/grub.conf  
加载内核  
执行init # /etc/initt  
ab  
runlevel # 3:多用户模式 5:带图形化的多用户 0:关机 1:单用户 2:不带网络的多用户 4:未使用 6:重新启动  

日志/rSyslog服务
----------
syslog # CentOS 5  
rsyslog # CentOS 6 
/var/log/secure # 安全相关  
/var/log/boot # 启动  
/var/log/dmesg # 内核  
/var/log/message # 正常日志  

### /etc/rsyslog.conf配置
Facility # 消息来源设备 kern # 内核消息 user # 用户级     
Priority/Severity Level # 优先级  
facility.prority    location # 格式  
-location # 无需等待同步  
*.*    @192.168.1.1    # udp发送到日志服务器  
*.*    @@192.168.1.1    # tcp发送到日志服务器  

DNS
----------
/etc/hosts #主机列表  
/etc/nsswitch.conf #DNS查询顺序  
/etc/resolv.conf #DNS服务器配置  
host www.baidu.com #查询  
dig www.baidu.com #详细查询  
dig +trace www.baidu.com #详细流程  
dig -t mx qq.com #查询邮件服务器  
dig -x 8.8.8.8 #逆向解析  
dig -t soa #start of authrity  

BIND服务器
----------
yum install -y bind bind-chroot bind-utils  
端口53#服务 953#远程控制  
/etc/named.conf #BIND服务主配置文件  
/var/named #zone文件  
若安装bind-chroot，封装到伪根目录下，配置文件变为  
/var/named/chroot/etc/named.conf  
/var/named/chroot/var/named/  
BIND安装后没有预制配置文件，从配置文档中copy  
cp -rv /usr/share/doc/bind*/sample/* /var/named/chroot/. #copy模板  
在/var/named/chroot/etc/named.conf添加  
zone "yingxuan.com"  
{  
    type master;  
    file "yingxuan.com.zone";  
};  
cp named.localhost yingxuan.com.zone #copy模板  
named-checkconf /etc/named.conf  
/etc/resolv.conf #修改DNS服务器  
systemctl reload named  

劫持DNS域名
----------
zone "." {  
    type master;  
    file "/etc/bind/db.fakeroot";  
};  
Then, in that db.fakeroot file, you will need something like the following:  
@ IN SOA ns.domain.com. hostmaster.domain.com. ( 1 3h 1h 1w 1d )  
  IN NS <ip>  
* IN A <ip>  
NFS|SMB|CIFS 文件共享服务
----------
yum install samba  
/etc/samba/smb.conf  
yum install samba-client  
smbd #文件及打印共享，使用139，445端口  
nmbd #提供NetBIOS支持，使用137端口，被DNS替代  
yum install samba-winbind  
winbindd #提供对Windows Server用户及组信息的解析功能  

添加sudo用户
----------
方法一：将用户添加为sudoer  
/etc/sudoer  
user ALL=(ALL) ALL  
方法二：将用户添加到wheel用户组  
adduser yingxuanxuan  
passwd yingxuanxuan #用户必须有密码才能进行sudo操作  
gpasswd -a yingxuanxuan wheel #usermod -G wheel yingxuanxuan  

LVM逻辑卷管理
----------
说明：
LVM（Logical volume Manager）通过将底层物理硬盘抽象封装起来，以逻辑卷的形式表现给上层习哦他能够  

功能：  
扩展硬盘空间  
合并多硬盘空间  
动态调整大小，不会丢失现有数据  

基本概念：  
PE(physical Extend)逻辑卷空间管理最基本单位，默认4M  
PV(physical volue)  
VG(volume group)  
LV(logical volume)  
PE->PV->VG->LV  

命令：  
查看  
pvs #列出所有pv  
pvdisplay  
vgs #列出所有vg  
vgdisplay  
lvs #列出所有lv  
lvdisplay  
删除  
umount /mnt/sdnew  
lvremove /dev/newvg/sdnew  
vgremove newvg  
pvremove /dev/sda /dev/sdc  

创建LVM  
1将物理磁盘设别初始化为物理卷PV  
pvcreate /dev/sdb /dev/sdc  
2创建卷组，并将PV加入到VG卷组中  
vgcreate newvg /dev/sdb /dev/sdc  
3基于卷组创建逻辑卷  
lvcreate -n newlv -L 2g newdisk #-L large -n name   
生成设备/dev/newvg/sdnew->/dev/dm-0  
4为创建好的逻辑卷创建文件系统  
mkfs.ext4 /dev/newvg/sdnew   
5将格式化好的逻辑卷挂载使用  
mkdir /mnt/sdnew; mount /dev/newvg/sdnew /mnt/sdnew  

拉伸逻辑卷LV  
1保证VG中有未使用空间  
vgdisplay  
2扩充逻辑卷  
lvextend -L + 100m /dev/vgnew/lv1  
3查看扩充后逻辑卷大小  
lvdisplay  
4更新文件系统  
resize2fs /dev/vgnew/lv1  
5查看更新后文件系统  
df -h  

拉伸卷组PV  
1将要添加到VG中的硬盘格式化为PV  
pvcreate /dev/sdd  
2将新的PV添加到指定卷组VG中  
vgextend vgnew /dev/sdd  
3查看扩充后大小  
vgs/vgdisplay  

缩小逻辑卷LV（离线）  
1卸载已经挂载的逻辑卷  
umount /dev/vgnew/lv1  
2缩小文件系统  
resize2fs /dev/vgnew/lv1 2G ##2G为新大小  
3缩小LV  
lvreduce -L -2G /dev/vgnew/lv1 ##2G为减小的大小  
4缩小VG  
vgreduce vgnew /dev/sdd  
5挂载  
mount /dev/vgnew/lv1 /mnt/sdnew  

Linux高级权限管理ACL
----------
传统权限模型的缺点：  
UGO无法满足复杂权限设置需要，只能设置一个文件组  

ACL：  
ACL(Access Control List)是一种高级权限机制，允许对文件或文件夹进行灵活复杂的权限设置  
ACL需要在挂在文件的时候打开#mount -o acl /dev/sda5 /mnt  
根分区默认打开ACL功能  

命令：  
查看一个文件/文件加的ACL设置  
getfacl /  
针对一个用户对文件进行ACL设置  
setfacl -m u:root:rwx / #-m modify  u:username:rwx path  
正对一个组对文件进行ACL设置  
setfacl -m g:root:rw / #-m modify g group  
删除 一个acl设置  
setfacl -x u:root / #-x delete   

高级网卡命令
----------
ifconfig  
mii-tool eth0 #查看网卡底层状态  
ethtool eth0 #查看网卡物理特征  
ethtool -i eth0 #查看网卡驱动信息  
ethtool -S eth0 #查看网卡统计状态  
nmtui  
setup  
yum -y install NetworkManager-tui  
nmtui-edit eno16777736  #修改网卡配置  
nmtui-connect eno16777736  
systemctl  restart network  
systemctl  status network  
nmcli con reload #重新读取配置  
nmcli dev con enp0s3  
dhclient wlan0 #DHCP  

IP别名 # 一个物理网卡上配置多个IP地址
----------
NetworkManager服务功能简单，需要禁用  
systemctl disable NetworkManager.service  
systemctl stop NetworkManager.service  
service stop NetworkManager  
chkconfig NetworkManager off  
添加IP地址  
ip addr add 192.168.1.77/24 dev eth0  label eth0:0  
ip addr add 192.168.1.77/24 dev enp0s3 label enp0s3:0  
/etc/sysconfig/network-scripts/ifcfg-enp0s3:0  
DEVICE=enp0s3:0  
IPADDR=192.168.1.77  
PREFIX=24  
ONPARENT=yes  

多网卡绑定
----------
模式0：平衡轮训 #提高带宽  
模式1：主动备份 #热备功能，不能提高带宽  
模式3：广播 #发送相同数据  
绑定后的逻辑网卡命名为bondn，n为编号，如/dev/bond0 /dev/bond1  
/etc/sysconfig/network-scripts/ifcfg-bond0  
DEVICE=bond0  
IPADDR=192.168.1.66  
PREFIX=24  
ONBOOT=yes  
BOOTPROTO=none  
USERCTL=no  
BONDING_OPTS="mode=1 mlimon=50"  

SELinux # Secure Enhanced Linux
----------
2.6内核后继承，继承在内核中，默认开启  

域和上下文：  
所有安全机制都是对 进程 和 系统资源（文件，网络套接字，系统调用等）进行限制。  
domain #域用来对进程进行限制  
content #上下文用来对系统资源进行限制  
ps -Z #查看domain信息  
ls -Z #查看content信息  

策略：  
SELinux通过定义策略来控制域可以访问哪些上下文  
SELinux有预置策略  
CentOS/RHEL使用预知的目标（target）策略  
目标（Target）策略定义只有目标进程收到SELinux限制  
目标策略只影响网络应用程序  

模式：  
强制(enforcing) #所有违反策略的行动都被禁止，并作为内核信息记录  
允许(permissive) #违反策略的行动都不会被精致，但是会产生报警信息  
禁用(disable) #禁用SELinux  
/etc/sysconfig/selinux->/etc/selinux/config  

命令：  
getenforece #查看SELinux工作状态  
setenforce 0 #临时关闭SELinux  
restorecon -R -v /var/www #恢复文件上下文 con context  
chcon --reference=/etc/named.conf.orig /etc/named.conf #改变文件上下文  

信息格式：  
用户(user)：角色(role)：类型(type)：MLS、MCS  
system_u:object_r:httpd_exec-t:s0  

IPTables基础 # 网络访问控制
---------
iptables -I INPUT -p tcp --dport 3306 -j ACCEPT  
service iptables save  

控制内核netfilter模块  

数据包控制：  
允许  
丢弃  
修改  

数据包分类：  
源IP地址  
目标IP地址  
使用接口  
使用协议（TCP,UDP,ICMP...)  
端口号  
连接状态（new,ESTABLISHED,RELATED,INVALID)  

Filtering Point    Table（功能划分）  
chain #过滤点    filter    nat    mangle  
INPUT                 x                        x  
FORWARD          x                        x  
OUTPUT              x        x              x  
PREROUTING               x               x  
POSTROUTING             x              x  

filter#过滤  
nat#对源目的进行修改  
mangle#高级修改  

规则：  
一个规则使用一行配置  
规则按顺序排列，规则逐条匹配  
只按照第一个匹配的规则执行相关动作：丢弃，放行，修改  

iptables [table]    [chain]        [condition]        [action]  
iptables -t filter    -A INPUT    -s 192.168.1.1    -j DROP  

[table]:filter nat mangle  
[chain]:INPUT OUTPUT FORWARD PREROUTING POSTROUTING  
[action]:ACCEPT DROP REJECT  

命令：  
service iptables status  
systemctl status iptables  
iptables   
-L #list  
-I #插入  
-s #源ip  
-d #目标ip  
-p #packet  
--dport  
-j #action  
-D # delete  
-i #指定接收接口  
-o #指定输出接口  
iptalbe -D INPUT 3 #删除INPUT表第三条  
iptable -F #删除全部  
-s '!' 192.168.1.0/24 #取反  

### 通过http流量
iptables -t filter -A INPUT -p tcp -dport http -j ACCEPT 

### 使用NAT进行跳转
iptables-t nat -A PREOUTING -p tcp -dport 80 -j DNAT --to-dest 192.168.1.10

### 使用NAT对出向数据进行跳转
iptables -t nat -A OUTPUT -p tcp --dport 80 -j DNAT --to-dest 192.168.1.100:8080

### 通过NAT对数据进行伪装(内网地址伪装为一个外部公网IP地址）
iptalbse -t nat -A POSTROUTING -o eth0 -j MASQUERADE

### 通过NAT隐藏源IP地址
iptables -t nat -A POSTROUTING -j SNAT --to-sourse 1.2.3.4

service iptables save #保存配置  
/etc/sysconfig/iptables #配置文件地址  
iptables-save > iptables-script  
iptables-restore iptables-script  

配置策略：  
iptables -A INPUT -p tcp --dport 22 -j ACCEPT  
iptables -I INPUT 1 -p tcp --sport 53 -j ACCEPT  
iptables -I INPUT 1 -p udp --sport 53 -j ACCEPT  
iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT  
iptables -A INPUT -j DROP  

firewall-cmd # CentOS7防火墙
----------
firewall-cmd --zone=public --add-port=80/tcp --permanent  
firewall-cmd --zone=public --remove-port=80/tcp --permanent  
firewall-cmd --zone=public --add-service=mysql --permanent  
firewall-cmd --zone=public--remove-service=ssh --permanent  
firewall-cmd --reload  
firewall-cmd --list-all  
firewall-cmd --get-service  

hdparm
----------
hdparm -Y /dev/hd*：使硬盘进入睡眠模式；  
hdparm -y /dev/hd*：使硬盘进入省电模式；   
hdparm -S[num] /dev/hd*：设置超时值使硬盘进入睡眠模式；  

VNC Server
-----------
安装：  
yum install  tigervnc-server  
配置：  
/etc/sysconfg/vncservers  
VNCSERCES="1:root 2:yingxuanxuan"  
创建密码：  
vncpasswd  
启动服务：  
service vncserver start  
开放防火墙：  
iptablse -F  

TFTP
----------
安装：  
yum install tftp-server.x86_64  
配置：  
/etc/xinetd.conf  
/etc/xinetd.d/service  
服务：  
chkconfig tftp on    
service xinetd start  
chkconfig xinetd on  
帮助：  
man xinetd.conf  
man in.tftpd  

PXE
----------
Pre-boot Execution Environment

系统监控
----------
net-snmp  
net-snmp-devel  
net-snmp-libs  
net-snmp-utils  
服务启动：  
service snmpd start  
配置:  
snmpd.conf  
1、设置security name和community  
com2sec [notConfigUser] default [public]  
2、将设置的security name绑定到一个组中  
group [notConfigGroup] [v2c] [notConfigUser]  
3、指定组的权限到一个权限示图  
access notConfigGroup '' any noauth exact [all] none none  
4、指定权限示图权限  
view all included [.1]    80  

获取命令：  
snmpget  
snmpwalk -v1 -c public 192.168.1.8 .1  
snmpwalk -v 2c -c public 192.168.1.8 .1  

Cacti系统监控
----------
1、搭建php环境  
yum install -y httpd php php-mysql net-snmp net-snmp-devel net-snmp-libs net-snmp-utils mysql mysql-server mysql-devel  
2、启动httpd  
service httpd configtest  
service httpd graceful  
3、启动mysql  
4、设置snmp权限  
5、设置php支持snmp  
wget .../php-snmp-5.3.3-40.el6_6.x86_64.rpm  

webmin网页管理界面  
----------
linux web 管理界面  

Linux安全加固
----------
1、删除不用的账号：lp sync shutdown halt news uucp operator games gopher  
2、删除不用的组：lp sync shutdown halt news uucp operator games gopher  
3、设置最小密码长度：sed -i '/PASS_MIN_LEN/s/5/8/g' /etc/login.defs  
4、设置密码有效期：sed -i '/PASS_MAX_DAYS/s/99999/90/g' /etc/login.defs  
5、检查是否存在空口令：awk -F: '($2==""){print $1}' /etc/shadow  
6、检查除root外是否有其他用户id为0  
```
mesg=`awk -F: '($3 == 0) { print $1 }' /etc/passwd|grep -v root`  
if [ -z $mesg ]  
then   
echo "There don't have other user uid=0"  
```
```
#5、---------------------------------------------------------------------
echo "#确保root用户的系统路径中不包含父目录，在非必要的情况下，不应包含组权限为777的目录"
echo "check the Path set for root,make sure the path for root dont have father directory and 777 rights"
echo "#-------------------------------------"
echo $PATH | egrep '(^|:)(\.|:|$)'
find `echo $PATH | tr ':' ' '` -type d \( -perm -002 -o -perm -020 \) -ls
#6、---------------------------------------------------------------------
echo "#检查操作系统Linux远程连接"
echo "Check if system have remote connection seting"
echo "#-------------------------------------"
find  / -name  .netrc
find  / -name  .rhosts 
echo "检查操作系统Linux用户umask设置"
echo "Check the system users umask setting"
echo "#-------------------------------------"
for i in /etc/profile /etc/csh.login /etc/csh.cshrc /etc/bashrc
do
grep -H umask $i|grep -v "#"
done
###################设置umask为027
#7、---------------------------------------------------------------------
#echo "#检查重要目录和文件的权限"
##echo "Check the important files and directory rights"
echo "#-------------------------------------"
for i in /etc /etc/rc.d/init.d /tmp /etc/inetd.conf /etc/passwd /etc/shadow /etc/group /etc/security /etc/services /etc/rc*.d
do
ls  -ld $i
done
echo -n "Please check if the output is ok ? yes or no :"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "Please recheck the output!"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac
#8、---------------------------------------------------------------------
#echo "#配置rc.d下脚本的权限"
echo "Configure the scripts right(750) in rc.d directory"
echo "#-------------------------------------"
chmod -R 750 /etc/rc.d/init.d/*
chmod 755 /bin/su 改了之后只能root su，没有了s位别的用户无法成功su
chmod 664 /var/log/wtmp
#chattr +a /var/log/messages
#9、---------------------------------------------------------------------
echo "#查找系统中存在的SUID和SGID程序"
echo "Find the files have suid or Sgid"
echo "#-------------------------------------"
for PART in `grep -v ^# /etc/fstab | awk '($6 != "0") {print $2 }'`; do
find $PART \( -perm -04000 -o -perm -02000 \) -type f -xdev -print |xargs ls  -ld
done
echo -n "Please check if the output is ok ? yes or no :"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "Please recheck the output!"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac
#10、---------------------------------------------------------------------  
echo "#查找系统中任何人都有写权限的目录"
echo "Find the directory everyone have the write right"
echo "#-------------------------------------"
for PART in `awk '($3 == "ext2" || $3 == "ext3") \
{ print $2 }' /etc/fstab`; do
find $PART -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print |xargs ls  -ld
done
echo -n "Please check if the output is ok ? yes or no :"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "Please recheck the output!"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac
#11、---------------------------------------------------------------------
#echo "#查找系统中任何人都有写权限的文件"
echo "Find the files everyone have write right"
echo "#-------------------------------------"
for PART in `grep -v ^# /etc/fstab | awk '($6 != "0") {print $2 }'`; do
find $PART -xdev -type f \( -perm -0002 -a ! -perm -1000 \) -print |xargs ls -ld
done
echo -n "Please check if the output is ok ? yes or no :"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "Please recheck the output!"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac#12、---------------------------------------------------------------------  
echo "#查找系统中没有属主的文件"
echo "Find no owner or no group files in system"
echo "#-------------------------------------"
for PART in `grep -v ^# /etc/fstab |grep -v swap| awk '($6 != "0") {print $2 }'`; do
find $PART -nouser -o -nogroup |grep  -v "vmware"|grep -v "dev"|xargs ls  -ld
done
echo -n "Please check if the output is ok ? yes or no :"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "Please recheck the output!"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac
#13、---------------------------------------------------------------------  
###echo "#查找系统中的隐藏文件"
##echo " Find the hiding file in system"
##echo "#-------------------------------------"
###linux执行报错\排除/dev”目录下的那些文件
####find  / -name \(".. *"  -o "…*"  -o ".xx" -o ".mail" \) -print -xdev
## #find  / -name "…*" -print -xdev | cat -v
##find  /  \( -name ".*"  -o -name  "…*"  -o -name ".xx" -o -name ".mail" \) -xdev
##echo -n "If you have check all the output files if correct yes or no ? :"
##read i
## case $i in 
## y|yes)
## break
## ;;
## n|no)
## echo "Please recheck the output!"
## echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
## continue
## ;;
## *)
## echo "please input yes or no"
## ;;
## esac
##
#14、---------------------------------------------------------------------    
echo "#判断日志与审计是否合规"
echo "Judge if the syslog audition if follow the rules"
echo "#-------------------------------------"
autmesg=`cat /etc/syslog.conf |egrep ^authpriv`
if [ ! -n "$autmesg" ]
then
echo "there don't have authpriv set in /etc/syslog.conf"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
echo -n "If you have know this y or n ?"
read i
case $i in 
y|yes)
break
;;
n|no)
echo "there don't have authpriv set in /etc/syslog.conf"
echo "!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!"
continue
;;
*)
echo "please input yes or no"
;;
esac
else
# echo "日志与审计合规"
echo "syslog audition follow the rules"
fi
#15、---------------------------------------------------------------------    
echo "#关闭linux core dump"
echo "Turn off the system core dump"
echo "#-------------------------------------"
mesg1=`grep "* soft core 0" /etc/security/limits.conf`
mesg2=`grep "* hard core 0" /etc/security/limits.conf`
if [ ! -n "$mesg1" -o ! -n "$mesg2" ]
then
cp /etc/security/limits.conf /etc/security/limits.conf_$date
if [ ! -n "$mesg1" ]
then
echo "* soft core 0" >> /etc/security/limits.conf
fi
if [ ! -n "$mesg2" ]
then
echo "* hard core 0" >> /etc/security/limits.conf
fi
fi
#修改login文件使limits限制生效
cp /etc/pam.d/login /etc/pam.d/login_$date
echo "session required /lib/security/pam_limits.so" >> /etc/pam.d/login
#16、---------------------------------------------------------------------   
#登录超时设置
#检查/etc/pam.d/system-auth文件是否存在account required /lib/security/pam_tally.so deny=的相关设置
#建议设置为auth required pam_tally.so onerr=fail deny=6 unlock_time=300
#17、---------------------------------------------------------------------   
#su命令使用,对su命令使用进行限制设置
#检查/etc/pam.d/su文件设置
#文件中包含
#auth sufficient /lib/security/pam_rootok.so debug
#auth required /lib/security/pam_wheel.so group=isd
#20、---------------------------------------------------------------------  
echo "#登录超时自动退出"
echo "set session time out terminal "
echo "#-------------------------------------"
tmout=`grep -i TMOUT /etc/profile`
if [ ! -n "$tmout" ]
then 
echo
echo -n "do you want to set login timeout to 300s? [yes]:"
read i
case $i in 
y|yes)
cp /etc/profile /etc/profile_$date
echo "export TMOUT=300" >> /etc/profile
. /etc/profile
;;
n|no)
break
;;
*)
echo "please input yes or no"
;;
esac
else 
mesg=`echo $tmout |awk -F"=" '{print $2}'`
if [ "$mesg" -ne 300 ]
then
echo "The login session timeout is $mesg now will change to 300 seconds"
cp /etc/profile /etc/profile_$date
echo "export TMOUT=300" >> /etc/profile
. /etc/profile
fi
fi
sed  -i 's/HISTSIZE=1000/HISTSIZE=100/g' /etc/profile
#21、---------------------------------------------------------------------   
echo "#禁用telnet启用ssh"
echo "Stop telnet and start up sshd"
echo "#-------------------------------------"
mesg1=`lsof -i:23`
mesg2=`lsof -i:22`
if [ ! -n "$mesg2" ]
then
service start sshd 
chkconfig sshd on
mesg2=`lsof -i:22`
fi
if [ ! -n "$mesg1" -a ! -n "$mesg2" ]
then 
echo 
echo "Will Deactive telnet"
    chkconfig krb5-telnet off
chkconfig ekrb5-telnet off
fi
#22、---------------------------------------------------------------------   
#echo "#设置终端超时，使系统10分钟后自动退出不活动的Shell"
#echo "#-------------------------------------"
#mesg=`grep "export TMOUT=600" /etc/profile`
#if [ -z $mesg ]
#then
#echo "export TMOUT=600" >>/etc/profile
#. /etc/profile
#fi
#23、---------------------------------------------------------------------  
echo "#禁用不必要的服务"
echo "Stop unuseing services"
echo "#-------------------------------------"
list="avahi-daemon bluetooth cups firstboot hplip ip6tables iptables iscsi iscsid isdn kudzu pcscd rhnsd rhsmcertd rpcgssd rpcidmapd sendmail smartd  yum-updatesd netfs portmap autofs nfslock nfs"
for i in $list
do
chkconfig $i off
service $i stop
done
echo "change kernel parameter for network secure"
cp  /etc/sysctl.conf /etc/sysctl.conf.$date
#echo "net.ipv4.icmp_echo_ignore_all = 1">>/etc/sysctl.conf
sysctl -a |grep arp_filter|sed -e 's/\=\ 0/\=\ 1/g' >>/etc/sysctl.conf
sysctl -a |grep accept_redirects|sed -e 's/\=\ 1/\=\ 0/g' >>/etc/sysctl.conf
sysctl -a |grep send_redirects|sed -e 's/\=\ 1/\=\ 0/g' >>/etc/sysctl.conf
sysctl -a |grep log_martians |sed -e 's/\=\ 0/\=\ 1/g'>>/etc/sysctl.conf
sysctl -p 
#24、---------------------------------------------------------------------  
echo "设置热键"
#ctrl+alt+del
if [ -d  /etc/init ]
then
sed  -i 's/^[^#]/#&/g' /etc/control-alt-delete.conf
else
sed -i 's/^ca::/#&/g' /etc/inittab
fi
#25、---------------------------------------------------------------------  
echo "demo:禁止除了db2inst1的用户su到root"
usermod -G wheel db2inst1
sed -i '/pam_wheel.so use_uid/s/^#//g' /etc/pam.d/su
echo "SU_WHEEL_ONLY yes">>/etc/login.defs  
```



常用命令
----------
```
sync # 将数据同步到硬盘中
shutdown -h now # 现在halt
shutdown -r now # 现在重启
shutdown -r 00:00 # 定时重启
shutdown -r +10 # 10分钟后重启
shutdown -c # 取消关机
reboot / halt / poweroff / init 0

sudo # 用户临时提升权限执行管理命令
/etc/sudoers # 设置sudo用户或用户组，默认仅root用户及wheel用户组可以使用sudo

alias ll='ls -lrt --color=auto' # 设置别名
unalias # 取消别名

uname # 系统相关信息，unix name

ps -ef # 显示进程
pstree # 显示进程树

top # 实时显示进程
h # 交互式帮助

lsof # 显示打开的文件
kill pid # 杀死进程
kill -9 pid
kill %jobid # 杀死后台job
pmap pid # 分析进程内存状况

sar # yum install -y sysstat
sar -u # cpu使用率
sar -q # 历史cpu平均负载
sar -r # 内存
sar -W # 页面交换
sar -n DEV # 查看历史网口流量
sar -n DEV 1 5 # 

free # 内存使用量

vmstat # 查看整体系统负载
vmstat 1 5 # 每隔1秒打印5次

w # 查看当前登录用户及系统负载

```

### 性能优化
```
top
free
vmstat
pstack

pstrace

gprof # 性能分析工具

valgrind # 内存泄漏分析工具

OProfile # 性能分析工具

```







### 程序调试
```
https://wiki.jikexueyuan.com/project/linux-tools/gdb.html

gdb program # gdb 启动程序
gdb program corefile # gdb 分析coredump
gdb program pid # gdb挂载到正在执行的程序

# 运行
run：简记为 r ，其作用是运行程序，当遇到断点后，程序会在断点处停止运行，等待用户输入下一步的命令。
continue （简写c ）：继续执行，到下一个断点处（或运行结束）
next：（简写 n），单步跟踪程序，当遇到函数调用时，也不进入此函数体；此命令同 step 的主要区别是，step 遇到用户自定义的函数，将步进到函数中去运行，而 next 则直接调用函数，不会进入到函数体内。
step （简写s）：单步调试如果有函数调用，则进入函数；与命令n不同，n是不进入调用的函数的
until：当你厌倦了在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体。
until+行号： 运行至某行，不仅仅用来跳出循环
finish： 运行程序，直到当前函数完成返回，并打印函数返回时的堆栈地址和返回值及参数值等信息。
call 函数(参数)：调用程序中可见的函数，并传递“参数”，如：call gdb_test(55)
quit：简记为 q ，退出gdb

# 设置断点
break n （简写b n）:在第n行处设置断点 （可以带上代码路径和代码名称： b OAGUPDATE.cpp:578）
b fn1 if a＞b：条件断点设置
break func（break缩写为b）：在函数func()的入口处设置断点，如：break cb_button
delete 断点号n：删除第n个断点
disable 断点号n：暂停第n个断点
enable 断点号n：开启第n个断点
clear 行号n：清除第n行的断点
info b （info breakpoints） ：显示当前程序的断点设置情况
delete breakpoints：清除所有断点：

# 查看源代码
list ：简记为 l ，其作用就是列出程序的源代码，默认每次显示10行。
list 行号：将显示当前文件以“行号”为中心的前后10行代码，如：list 12
list 函数名：将显示“函数名”所在函数的源代码，如：list main
list ：不带参数，将接着上一次 list 命令的，输出下边的内容。

# 打印表达式
print 表达式：简记为 p ，其中“表达式”可以是任何当前正在被测试程序的有效表达式，比如当前正在调试C语言的程序，那么“表达式”可以是任何C语言的有效表达式，包括数字，变量甚至是函数调用。
print a：将显示整数 a 的值
print ++a：将把 a 中的值加1,并显示出来
print name：将显示字符串 name 的值
print gdb_test(22)：将以整数22作为参数调用 gdb_test() 函数
print gdb_test(a)：将以变量 a 作为参数调用 gdb_test() 函数
display 表达式：在单步运行时将非常有用，使用display命令设置一个表达式后，它将在每次单步进行指令后，紧接着输出被设置的表达式及值。如： display a
watch 表达式：设置一个监视点，一旦被监视的“表达式”的值改变，gdb将强行终止正在被调试的程序。如： watch a
whatis ：查询变量或函数
info function： 查询函数
扩展info locals： 显示当前堆栈页的所有变量


# 查询运行信息
where/bt ：当前运行的堆栈列表；
bt backtrace 显示当前调用堆栈
up/down 改变堆栈显示的深度
set args 参数:指定运行时的参数
show args：查看设置好的参数
info program： 来查看程序的是否在运行，进程号，被暂停的原因。

# 分割窗口
layout：用于分割窗口，可以一边查看代码，一边测试：
layout src：显示源代码窗口
layout asm：显示反汇编窗口
layout regs：显示源代码/反汇编和CPU寄存器窗口
layout split：显示源代码和反汇编窗口
Ctrl + L：刷新窗口

cgdb # gdb -tui, 调试时同步显示代码

pstrack pid # 查看挡墙调用栈

strace # 查看进程运行时的 系统调用 和 信号

nm # 列出目标文件的符号清单

objdump # 显示二进制文件信息

readelf # 显示二进制文件信息


size # 查看二进制文件内存占用

file # 查看文件类型

strings bin # 查看二进制文件中的文本信息

fuser # 显示使用文件的用户

xdd # 十六禁止显示文件

ldd # 查看程序依赖库
```

```

```

### 文件操作、文件权限、权限系统
```
[root@www /]# ls -l
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot

d # 目录          ugo # user group other
- # 文件          rwx # read write execute
l # link          rwx # 4    2     1
b # block
c # char字符设备
s # socket套接字，进程间通信

chown -R [user] [path]
chown -R [user:group] [path]

chgrp -R [group] [path]

chmod -R 777 [path]
chmod -R u=777,g=777,o=777 [path]
chmod -R u+x,g-r,o-r [path]
chmod -R u=rwx,go=rx [path]
chmod -R a=rwx [path] # all


cd - # 上一个目录
cd ~ # 家目录
cd . # 当前目录
cd .. # 上一层目录

pwd # 当前路径 
pwd -P # 显示实际路径（非link路径）如 /var/mail -> /var/spool/mail

umask 0002 # 普通用户默认属性模
umask 0022 # root用户默认属性模
755 # 默认目录权限，目录用777减
644 # 默认文件权限，文件用666减

mkdir -p dir/dir # Parent递归创建
mkdir -m 700 # 创建时设置属性

rmdir -p dir # Parent递归删除目录（只能删除目录）
rm -f # 强制删除
rm -r # 删除目录
rm -i # 互动删除，注意某些操作系统会默认alias rm='rm -i'


cp -a src dst # 等于-pdr，常用
cp -d # 源文件为link则任然复制为link，默认会复制文件
cp -p # 使用原属性复制，默认为umask属性
cp -r # 递归复制目录

cp -s # 软连接，symbolic link
cp -i # 硬链接，hard link
ln -s # 软连接

cp -f # 若文件已经存在则强制删除再复制
cp -i # 若目标存在，则询问
cp -u # 目标文件存在时且旧才复制，update


mv # 移动或者重命名
mv -i # 若存在则询问


ls -l # 详细信息
ls -a # 显示全部文件
ls -d # 显示目录本身，而非目录内容
ls --color=auto # 显示颜色

chattr / lsattr # 特殊属性

find ./ | wc -l # 当前目录下所有文件个数
find ./ -name "core*" | xargs file # 结果输出执行命令
find ./ -name '*.o'
find ./ -name "*.o" -exec rm {} \; # 内置执行命令
find -type [fbcdls] # 文件类型

locate file # 索引快速查找
update db # 更新索引

tar -cvf tarname files # 创建tar文件
tar -xvf tarname files # 解压tar文件
-c # create
-x # ex
-v # visual
-f # filename，必须在最后，紧跟tar文件名称
-z # gzip
-j # bzip
--exclude # 过滤文件
```

### 用户管理
```
useradd -option username
-c # 加注释comment
-d HOME # 指定用户主目录，不存在不创建，加-m选项不存在则创建
-M # 不创建HOME目录
-g GID # 用户组
-G # 附加用户组
-s SHELL # shell
-u UID # 指定ID

userdel user # 删除用户
-r # 删除主目录

usermod -option username # 修改用户属性
-l # 新用户名
# 其他选项与useradd相同

passwd username # 修改密码
-l # lock禁用账号
-u # unlock解锁
-d # delete无口令
-f # 强制用户下次登录修改口令

groupadd -option groupname # 添加用户组
groupadd groupname
groupadd [-g gid] groupname # 指定groupid

groupdel groupname # 删除用户组

groupmod -g id # 修改gid
groupmod -n new old # 修改用户组名

newgrp # 切换用户组

# /etc/passwd
user:x:userid:groupid:comment:home:shell:pseudo伪用户

# 伪用户：
bin # 可执行的用户命令文件
sys # 拥有系统文件
adm # 拥有账户文件
uucp # UUCP
lp # lp lpd
nobody # NFS使用
audit
cron
mail
usenet

# /etc/shadow
# 使用pwconv根据/etc/passwd自动产生/etc/shadow
登录名:加密口令:最后修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
加密口令 # 长度为13，为空不许要密码，有哈希有效字符之外字符则不能登录
最后修改时间 # 单位utc天

# /etc/group
组名:口令:组识别号:组内用户列表


```

常用路径
----------
```
/bin -> /usr/bin/ # 常用Binary
/sbin -> /usr/sbin/ # 系统管理Binary
/lib -> /usr/lib/ # 动态链接库
/usr/src # 内核源代码默认的放置目录
/boot # 启动文件，linux内核
/dev # Device设备
/etc # 配置文件
/home # 非root用户工作目录
/lost+found # 非法关机未存盘文件
/media # 自动挂载目录，u盘，光驱
/mnt # 手动挂载
/opt # 第三方软件安装目录
/proc # 系统内存映射，操作系统信息接口，可直接读取或修改
/root # root用户主目录
/srv # 服务相关数据
/sys
/tmp # 临时文件
/var # 日志等不断扩充的文件
/run # 启动后临时文件，重启后删掉
```

### 文本相关
```
cat -A # -vET显示所有特殊字符
cat -E # 显示换行为$
cat -T # 显示Tab为^|
cat -v # 显示其他空白字符
cat -b # 非空行行号
cat -n # 显示行号

tac # 反向显示

nl

more # 显示多页，只能向后，显示完则退出，空格后翻一页
less # 可以向前翻页，使用pageup、pagedown，使用`/?`搜索，n调到下一个搜索结果

head file # 默认显示前10行
head -n file # 显示前n行
tail -n # 显示后n行
tail -f # 动态更新

diff file1 file2 # 文本比较

egrep 'expression' file # 文件过滤

grep  # 过滤内容

cut -d "sep" -f n # 按分割字符分割列，并取第n个值

wc # 统计文档行数、字符数、词数
-l # 统计行数
-m # 统计字符数
-w # 统计词数

uniq # 去掉重复行
uniq -c # 统计并合并重复行数，并把行数写在合并行前面
sort # 去重前需要先排序

tee # 分流器
echo "xxx" | tee x.txt # 将内容输出到x.txt并显示在终端上

tr # translate

split / sed / awk
```

shell脚本
----------
### bash基础知识
```
home/.bash_history # 保存历史命令，仅当正常退出时保存


env # 环境变量
set # 系统预设变量
export var # 导出环境变量，所有子shell都可以使用
export # 导出所有变量
/etc/profile # 所有用户都是用的变量
/home/.bashrc # 某个用户使用的变量
a='b c' # 声明变量a，等号右侧遇见空白字符则终端，需要使用引号包括
a="b'c" # 使用双引号包含单引号内容
a=`pwd` # 反引号中的内容将会执行，a的值等于执行后的结果输出 
a="b"c # 只有双引号可以累加内容
"$a" # 双引号使用$符号展开变量内容
a='$a'c # 单引号中特殊字符$失效，不会展开内容
unset var # 取消变量

# 特殊字符
* # 匹配0-n个字符
? # 匹配1个字符
[] # 中括号中任意一个值 [123abc] [a-zA-Z] [0-9]
# 注释
\ # 转义字符
$ # 取变量 or 代表上条命令输出最后的内容

; # 顺序执行多条命令
cmd1&&cmd2 # cmd1执行成功后执行cmd2
cmd1||cmd2 # cmd1执行成功后cmd2不执行
```

### 脚本
```
# 声明变量
a=b # 等号左变是变量，等号两边不能加空格，等号右值有特殊字符时用引号包括

# 使用变量
$var
${var}

# echo
echo string string
\n # 换行，echo结束会自动换行
\r # return
\t # table
\v # 垂直制表符
\\ # 转译斜杠

# printf 
printf "fmt" var1 var2 var... # 复制c语言

# I/O重定向
< # 输入重定向
> # 输出重定向
2> # 错误重定向
>> # 追加重定向
| # 管道
/dev/null # 输出到此直接丢弃，作为输入文件结束
/dev/tty # 终端

```







## shell基础

### .bash_history

记录这次登录之前的command  

wildcard 通配符
--------------------------------------------------------------------------------------

略

type
--------------------------------------------------------------------------------------

type command #显示是内建指令or外部指令  
type -t command #测试命令类型是file alias builtin？  
type -p command #显示外部指令path  
type -a command #显示命令搜索顺序，显示所有command命令和alias  

命令换行，转译Enter
--------------------------------------------------------------------------------------

\Enter


声明变量
--------------------------------------------------------------------------------------

=  
等号两边不能有空格  
等号右边内容有空格时，需要使用' or "  
""内的$var会被调用  
''内的$var会保留$  
可以使用\转译Enter,$,\,space,\,'等  

调用变量
--------------------------------------------------------------------------------------

$var  
${var}  

获取命令执行结果
--------------------------------------------------------------------------------------

`command`  
$(command)  

变量内容累加
--------------------------------------------------------------------------------------

"$var"append  
${var}append  

unset
--------------------------------------------------------------------------------------

取消变量  
unset var  

变量跨进程引用  
--------------------------------------------------------------------------------------

export #显示所有环境变量  

env 环境变量
--------------------------------------------------------------------------------------

HOME #家目录~  
SHELL #shell类型  
HISTSIZE #history size  
MAIL # /var/spool/mail/`whoami`  
PATH  
LANG  
RANDOM #随机数 declare -i number=$RANDOM*10/32768 ; echo $number  

set
--------------------------------------------------------------------------------------

显示所有变量包括env  
BASH #当前使用bash路径  
COLUMNS #列数  
HISTFILE #历史文件  
HISTFILESIZE #文件中最大记录数  
HISTSIZE #内存中最大记录数  
IFS=$'\t\n' #预设分隔符号  
LINES #目前终端最大行数  
PS1 #命令提示字符格式  
PS2 #续行命令提示符  
$ #当前shell所有的PID   
? #上一条命令的返回值  

locale
--------------------------------------------------------------------------------------

locale -a #显示所有支持语言  
locale #显示当前语言配置  
*LANG与LC_ALL两个变量会被其他locale变量使用*   

read
--------------------------------------------------------------------------------------

read var #读取输入到变量  
read -p "xx" var #提示字符  
read -t second var #等待n秒  

declare / typeset # 默认bash变量都是字符串，不会进行计算
--------------------------------------------------------------------------------------

declare #读取所有变量和值 ==set  
declare -a var #array ${var[n]}  
declare -i #integer #只能进行整数运算  
declare -x # ==export  
declare -r #readonly #必须重新进入bash才能改变  

ulimit # user limit 用户限制
--------------------------------------------------------------------------------------

ulimit -a #显示所有限制值

```
core file size                      (blocks, -c)                       0 #core dump file size, 0 is unlimit  
data seg size                     (kbytes, -d)         unlimited #最大内存分段  
scheduling priority            (-e)                                  0  
file size                               (blocks, -f)         unlimited #单一文件大小限制，一次登录中只能减小不能增加  
pending signals                 (-i)                             3911  
max locked memory         (kbytes, -l)                      64 #最大锁定内存量  
max memory size              (kbytes, -m)       unlimited  
open files                           (-n)                             1024 #开启文件数量限制  
pipe size                             (512 bytes, -p)                8  
POSIX message queues     (bytes, -q)             819200  
real-time priority                (-r)                                  0  
stack size                            (kbytes, -s)                8192  
cpu time                             (seconds, -t)      unlimited #最大CPU时间  
max user processes            (-u)                            3911 #最大进程数量  
virtual memory                   (kbytes, -v)        unlimited  
file locks                              (-x)                    unlimited  
```

ulimit -H #hard limit限制值  
ulimit -S #soft limit警告值  

变量内容删除
--------------------------------------------------------------------------------------

${var#key} #从左向右删除，删除最短匹配  
${var##key} #从左向右删除，删除最长匹配  
${var%key} #从右向左删除，删除最短匹配  
${var%%key} #从右向左删除，删除最长匹配  
${var/key1/key2｝ #将首个匹配的key1替换为key2  
${var//key1/key2｝ #将所有匹配的key1替换为key2  
表达式 str没有定义 str="" str已经定义且不等于""
var=${str-expr} var=expr var= var=$str
var=${str:-expr} var=expr var=expr var=$str
var=${str+expr} var= var=expr var=expr
var=${str:+expr} var= var= var=expr
var=${str=expr} str=exprvar=expr str不变var= str不变var=$str
var=${str:=expr} str=exprvar=expr str=exprvar=expr str不变var=$str
var=${str?expr} expr输出至stderr var= var=$str
var=${str:?expr} expr输出至stderr expr输出至stderr var=$str

alias unalias
--------------------------------------------------------------------------------------

使用\转译还原alias  

history
--------------------------------------------------------------------------------------

history n # 列出最近n条命令  
history -c # clean  
history -r # read histfile  
history -w # write histfile  
history -a # append write  
!n # 执行第n条指令  
!key # 搜索开头为key的指令执行  
!! # 执行上一条指令  
ctrl + r  

bash欢迎信息
--------------------------------------------------------------------------------------

/etc/issue #bash欢迎信息  
/etc/issue.net #telnet欢迎信息  
/etc/motd #登录后显示的欢迎信息  

/etc/profile # login shell 配置文件
--------------------------------------------------------------------------------------

### 执行过程  

决定PATH MAIL USER HOSTNAME HISTSIZE umask  
执行/etc/profile.d*.sh  
执行/etc/profile.d/lang.sh -> /etc/locale.conf  
执行/usr/share/bash-completion/completions/*  

### 执行顺序

* login shell按顺序读取第一个存在的配置文件 *  
  ~/.bash_profile # 读取~/.bashrc/bashrc->读取/etc/bashrc  
  ~/.bash_login  
  ~/.profile  

source # 读取环境设置文件
--------------------------------------------------------------------------------------

source ~/.bashrc  
. ~/.bashrc #小数点读取环境设置文件  

~/.bashrc # non-login shell读取配置文件
--------------------------------------------------------------------------------------

读取/etc/bashrc 为CentOS特有，目的是执行login shell的profile和profild.d  

~/.bash_logout # 退出后执行的命令
--------------------------------------------------------------------------------------

略  

stty #s etting tty 快捷键配置
--------------------------------------------------------------------------------------

stty -a # list all  
stty keyword keyboard #^是ctrl  

intr # interrupt  
quit # quit signal  
erase # 向后删除  
kill # 删除在目前指令上的所有文字  
eof # End of file  
start # 重新启动输出  
stop # 停止输出  
susp # terminal stop  

set # bash tty设置
--------------------------------------------------------------------------------------

set -u #default no #未定义的变量显示错误信息  
set -v #default no #信息输出前，会先显示信息的原始内容  
set -x #default no #指令执行前，会显示指令内容，++指令内容  
set -h #default yes #history  
set -H #default yes #history  
set -m #default yes #manage  
set -B #default yes #设置[]  
set -C #default no #使用>输出到文件时，文件存在则不覆盖  
echo $- #显示所有set设定  

其他terminal设置
--------------------------------------------------------------------------------------

/etc/inputrc  
/etc/DIR_COLORS*  
/usr/share/terminfo/*   

通配符
--------------------------------------------------------------------------------------

星号* #任意n个字符  
? #任意1个字符  
[a,b] #括号内某个字符  
[a-b] #编码顺序内任意字符  
[^a] #排除字符  

特殊符号
--------------------------------------------------------------------------------------

'#' # 注释  
\ # 转义字符  
| # pipo管道  
; # 命令分隔符  
~ # 家目录  
$ # 取变量  
& # 后台运行  
! # not  
/ # 路径分割  
大于号> # 替换  
两个大于号>> # 累加  
< << #  
'' # 不能置换变量  
"" # 置换变量  
`` # 先执行指令，再显示 ==$(cmd)  
() # 子shell起止  
{} # 命令区块  

标准输入输出
--------------------------------------------------------------------------------------

stdin 0 < << #<<设置结束字符  
stdout 1 > >>  
stderr 2 2> 2>>  
两条流不能同时写入一个文件，所以 cmd >file 2>file是错误的  
正确的写法是 cmd >file 2>&1 或者 &>file  
2>/dev/null #丢弃错误输出  

组合命令
--------------------------------------------------------------------------------------

cmd;cmd #顺序执行  
cmd1&&cmd2 #cmd1返回0则执行cmd2  
cmd1||cmd2 #cmd1返回1则执行cmd2  

cut
--------------------------------------------------------------------------------------

cut -d 'deviceby' -f fieldnum #取分段，从1开始计数  
cut -c n1-n2 #截取字符n1到n2  
cut -c n1- #截取字符n1到最末尾  
cut无法处理多个空白字符，使用awk  

sort
--------------------------------------------------------------------------------------

sort [-fbMnrtuk] file  
-f #忽略大小写  
-b #忽略行首空白  
-M #以英文月份排序  
-n #以数字排序  
-r #reverse  
-u #uniq 合并相同行  
-t #分隔符，default=tab  
-k #key field，以特定列排序  

uniq # 合并重复
--------------------------------------------------------------------------------------

-i # ignore case  
-c # count  

wc
--------------------------------------------------------------------------------------

-l # line count  
-w # English word count  
-m # 字符个数  

tee # 带显示的重定向 >  
-a # append >>  

tr
--------------------------------------------------------------------------------------

tr [a-z] [A-Z] # 按字符替换  
tr key1 key2 # 按词组替换  
tr -d key # delete  

col
--------------------------------------------------------------------------------------

col -x # 将tab替换为对等的space  
col -A # 显示所有空白字符  

join # 按行拼接两个文本。默认以空白分隔，比较每行首列，一样则合并
--------------------------------------------------------------------------------------

-t # 设置分隔符号  
-i # ignore case  
-1 # 设置第一个文件用第几列比较  
-2 # 设置第二个文件用第几列比较  

paste
--------------------------------------------------------------------------------------

paste -d ' ' file1 file2 #按行拼接，默认两行以tab分割，-d设置devide  

expand
--------------------------------------------------------------------------------------

expand -t n file #将一个tab以n个空格替换  

split
--------------------------------------------------------------------------------------

split -b [b|k|m] file newname #按大小分割文件  
split -l num file newname #按行分割文件  

xargs # 给不支持管道的命令提供参数
--------------------------------------------------------------------------------------

cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -n 1 id #-n 设置每次读取n个参数  
cut -d ':' -f 1 /etc/passwd | head -n 3 | xargs -p -n 1 id #-p 互动模式，每次询问是否使用参数  
cut -d ':' -f 1 /etc/passwd | xargs -e'sync' -n 1 id #-eEOF 设置结束字符  
find /usr/sbin -perm /7000 | xargs ls -l == ls -l $(find /usr/sbin -perm /7000)   

减号- # stdout stdin替代符
--------------------------------------------------------------------------------------

tar -cvf - /home | tar -xvf - -C /tmp/homeback #-f - 将打包结果输出到stdout #-f - 从stdin输入tarball  



## Shell 脚本基础

 



整数运算
--------------------------------------------------------------------------------------

declare -i total=${num1}*${num2}  
total=$((${num1}*${num2}))  

+  

-  

*  /  
   %  

浮点数运算
--------------------------------------------------------------------------------------

echo "1.1*2.2" | bc  


test
--------------------------------------------------------------------------------------

test -e file && echo "exist" || echo "not exist"  

### 文件测试  

-e #exist  
-f #file  
-d #directory  
-b #block device  
-c #character device  
-S #Socket  
-p #pipe fifo  
-L #link  

### 权限测试  

-r #read  
-w #write  
-x #execute  
-u #SUID  
-g #SGID  
-k #Sticky bit  
-s #非空白文档  

### 文件比较

-nt #newer than  
-ot #older than  
-ef #equal(hard link)  

### 数值比较

-eq #equal  
-ne #not equal  
-gt #greater than  
-lt #less than  
-ge #greater than or equal  
-le #less than or equal  

### 字符串

test -z string #zero 空字符串  
test -n string #not 非空字符串 #可以省略 test string  
test str1 == str2  
test str1 != str2  

### 多重条件

-a #and  
-o #or  
test ! option object #非  

判断符号[]
--------------------------------------------------------------------------------------

[ -z "${HOME}" ]  
[ "$HOME" == "$MAIL" ] #注意全部加空格以和正则表达式区分  
变量还原注意两边加双引号"$HOME"避免还原后有空格  

与或非
--------------------------------------------------------------------------------------

[] && [] #AND  
[] || [] #or  
[ -o ]  
[ -a ]  

参数偏移
--------------------------------------------------------------------------------------

shift #偏移一个参数，删除第一个参数  
shift n #再偏移n个参数，删除前n个参数  



### 函数参数输入

fname var1 var2  



随机数
--------------------------------------------------------------------------------------

check=$((${RANDOM}*${varnum}/32767+1))  

sh
--------------------------------------------------------------------------------------

-n #不执行，仅检查语法  
-v #执行前将script打印到屏幕上  
-x #将使用到的script内容显示出来  

判断命令是否存在
--------------------------------------------------------------------------------------

command -v foo >/dev/null 2>&1 #POSIX script  
type foo > /dev/null 2>&1     #consider built-ins & keywords  
hash foo 2 > /dev/null    #for bash  
which不是bash自带，不一定存在  
