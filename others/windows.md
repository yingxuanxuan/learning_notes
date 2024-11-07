# windows



## 解决设置路由表造成上不了网

```
netsh int ipv4 reset
# 重启计算机
```



## 将内网映射到外网（NAT）

wsl2改进，直接使用localhost访问主机，127.0.0.1不行
所以，可以直接映射到localhost
```powershell
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8077 connectaddress=<wsl中的linux的eth0的ip> connectport=<wsl中linux映射的端口>
#例如：上面我是用wsl的ubuntu中的80端口映射到了容器的8080端口，所以我这边的connectport要填80，172.19.115.157是我ubuntu的eth0的ip
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8077 connectaddress=172.19.115.157 connectport=80
```
connectaddress在内网服务器上获取
```
ip addr | grep 127
```



### 直接映射ipv6地址，问题少，稳定

```
netsh interface portproxy add v4tov6 \
    listenport=22 listenaddress=0.0.0.0 \
    connectport=22 connectaddress=::1
```



### 查看nat映射列表

```powershell
netsh interface portproxy show v4tov4
netsh interface portproxy show v4tov6
```



### 删除nat映射

```
netsh interface portproxy delete v4tov4 listenaddress=0.0.0.0 listenport=8077 
```



## 同时上内外网，路由冲突

1.  设置两个路由，默认路由0.0.0.0走外网，专用路由走内网
2.  metric设置相同数值
3.  -p 永久生效
4.  metric相同时，内网路由比外网路由优先级高
5.  达到同时访问内外网的目的

```powershell
route add 0.0.0.0   mask 0.0.0.0     192.168.43.1  metric 1 -p
route add 10.10.0.0 mask 255.255.0.0 10.10.160.254 metric 1 -p
```



## windows server Administrator 密码不过期

```powershell
net accounts /maxpwage:unlimited
```



## windows server 2016 激活

```powershell
slmgr /upk
slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
slmgr.vbs /skms kms.lotro.cc
slmgr.vbs /ato
```



```
Windows Server 2016 数据中心
CB7KF-BWN84-R7R2Y-793K2-8XDDG

Windows Server 2016 标准版
WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY

Windows Server 2016 嵌入式版
JCKRF-N37P4-C2D82-9YXRT-4M63B
```



## VPN相关服务

Routing and Remote Access
Remote Access Connection Manager
Remote Procedure Call (RPC)

*   使用IPsec的第二层隧道协议 L2TP/IPsec
*   未加密的密码（PAP）
*   质询握手身份验证协议（CHAP）



## 虚拟桌面快捷键

1.  新建一个虚拟桌面：\[win + ctrl + D]
2.  查看所有虚拟桌面：\[win + tab]
3.  切换虚拟桌面：\[win + ctrl + 方向键]
4.  关闭当前虚拟桌面：\[win + ctrl + F4]
5.  在虚拟桌面间移动应用：\[win + tab + 鼠标操作]
6.  在所有虚拟桌面上都显示某引用：\[win + tab + 鼠标操作]



## 修改多个网络访问顺序



### 方法1

1.  "控制面板->网络连接"
2.  按住ALT调出菜单栏
3.  选择"高级->高级设置->适配器和绑定->连接"
4.  调整连接顺序



### 方法2，修改路由“跃点”（方法牛逼）

1.  跃点数值范围1 - 9999，数字越小路径越优
2.  适配器->右键->属性
3.  网络->Internet协议版本4->常规->高级->自动跃点数
4.  10 > 20 > 30 > 40 > 50



## windows端口映射，自带netsh

### 查看已配置的“端口映射”清单

    netsh interface portproxy show v4tov4

### 添加“端口映射”

*   将本机（192.168.222.145）的15001端口映射到192.168.222.63的81端口:

<!---->

    netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=2000 connectaddress=127.0.0.1 connectport=3389

### 删除“端口映射”

    netsh interface portproxy delete v4tov4 listenaddress=192.168.222.145 listenport=15001



## 关闭休眠功能、同时删除Hiberfil.sys文件

```sh
powercfg -h off
```



### 打开休眠功能

```sh
powercfg -h on
```

16g内存占用12g
8g内存占用3g



### 查看休眠功能状态

```sh
powercfg /a
```

## 查看系统是UEFI还是Legacy

以管理员身份行CMD，然后输入 bcdedit /enum {current} 执行，如下：
如果 path 中给出的路径是 winload.exe ，那么就可以说明系统是通过 legacy模式启动的，
如果 path 中给出的路径是 winload.efi ，那么就可以说明系统是通过 UEFI 模式启动的了。

## GPT or MBR

UEFI只能使用GPT
Legacy只能使用MBR

## 组策略

### 开机启动，用户启动

gpedit.msc

## host主机查询

nbtstat -a hostname
nbtstat -A ipaddr

## grub2

<http://blog.chinaunix.net/uid-15007890-id-3056369.html>



## windows 引导修复

1.用win7光盘启动，选择修复计算机，自动修复，
2.重启完之后继续用光盘引导进入继续选择修复计算机，选择命令行界面(假设系统盘在 c:)
输入命令如下
c:
cd windows/system32
bootrec.exe /FixMbr
bootrec.exe /FixBoot
bootrec.exe /REBUILDBCD
重启系统，搞定！！！！V\_\_V



## 文件、目录权限恢复

```powershell
takeown /f \* /A /R

Takeown命令用于以重新分配文件所有权的方式允许管理员重新获取先前被拒绝访问的文件访问权。
/f 参数作用在于指定文件名或目录名模式。可以用通配符 "*"，“*”在这里的作用是充当任意字符。
/a 参数用于将所有权给于管理员组，而不是当前用户
/r 参数用于递归: 指示本命令运行于指定的目录和子目录里的文件上
```



```powershell
### icacls \* /t /grant\:r everyone\:f

icacls 命令用于查看、设置、保存并恢复文件夹或文件的权限

*   表示任意字符
    /T 参数用以该命令指定的目录下的所有匹配文件/目录上执行此操作
    /grant\:r everyone参数用以授予everyone用户访问权限，并替换以前授予的所有显式权限
```



## 若要继续，请输入管理员用户名和密码

* 打开 -> windows设置 -> 更新和安全 -> 恢复 -> 立即重新启动
* 自动进入 -> 疑难解答 -> 高级选项 -> 启动设置
* 或重启电脑->按F8进入安全模式
* 在安全模式下 -> 控制面板 -> 更改账户类型 -> 将受限账户改为”管理员账户“
* 当需要Administrator密码时，未设置过则留空
* 重启后修复

