# Linux



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




