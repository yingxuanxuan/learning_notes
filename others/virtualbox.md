# virtualbox

# 目录
[toc]

## virtualbox 虚拟机中操作系统缓慢
* 解决方法:关闭swap交换空间

```bash
# 一次性关闭交换空间
swapoff /mnt/swap

# 修改/etc/fstab，禁止加载交换空间分区
删除 /mnt/swap swap swap defaults 0 0 这一行或者注释掉这一行

# 查看是否有交换空间
free -m

# 临时设置交换控件使用率为0
echo 0 > /proc/sys/vm/swappiness   # 临时生效

# 永久降低交换空间使用率为0
# 修改/etc/sysctl.conf
# 添加 vm.swappiness=0

# 加载配置使配置生效
sysctl -p                              
```

* 解决方法：打开virtualbox磁盘缓存
```
存储 -> 控制器 -> SATA -> 使用主机输入输出（I/O）缓存
```

## virtualbox minimal 挂载增强安装盘
```python
mkdir -p /mnt/cdrom
mount /dev/sr0 /mnt/cdrom
cd /mnt/cdrom
./VBoxLinuxAddtion.run
```

## virtualbox 安装增强功能
```bash
# 升级内核版本
yum update kernel -y

# 下载内核头文件
sudo yum install kernel kernel-devel kernel-headers

# 下载编译工具
sudo yum install gcc make

# 重启系统切换内核
sudo reboot

# 调用增强包安装脚本
./VBoxLinuxAddtion.run
```

## win10配置网卡路由转发
1. 在跳板机上开启路由转发
2. 在访问机上添加路由

### windows上添加网卡转发
```powershell
# 执行命令修改注册表，打开路由转发
reg add HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v IPEnableRouter /D 1 /f

# 启动Routing及其依赖服务
sc config RemoteAccess start= auto
sc start RemoteAccess
```

### Linux下设置网卡转发
```
# 临时开启网卡转发
echo 1 > /proc/sys/net/ipv4/ip_forward

# 永久开启网卡转发
# /etc/sysctl.conf
# net.ipv4.ip_forward = 1

# 添加转发路由
route add -net 192.168.1.0 mask 255.255.255.0 dev eth0
route add -net 192.168.2.0 mask 255.255.255.0 dev eth1

# 添加防火墙规则
/sbin/iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

### windows上设置路由
```powershell
# 设置192.168.56.1 为网关
route add * mask 0.0.0.0 192.168.56.1
```
