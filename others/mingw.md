# mingw

## 目录
[toc]

## 基本配置

### 配置及更新源
**同步后会被覆盖，需要再次修改**
```
修改msys64\etc\pacman.d 目录下三个文件
mirrorlist.msys
mirrorlist.mingw64
mirrorlist.mingw32
```


### 解决msys2使用缓慢问题
```bash
# 将用户名 组名 添加到文件
mkpasswd -c > /etc/passwd
mkgroup -c > /etc/group

# 注释掉file行和group行，或者只注释group行
vim /etc/nsswitch.conf
```

### msys2中导入window环境变量
```
创建一个Windows环境变量 MSYS2_PATH_TYPE=inherit
```

## 软件安装，包管理

### 安装后更新
```
pacman -Syu # 初始化msys2，运行msys2_shell.bat
pacman -Su # 关闭窗口,升级其他包，运行autorebase.bat
```

### 查找软件包
```
# 在仓库中搜索含关键字的包
pacman -Ss 关键字 # -S Sync，-s search

# 按包名过滤
pacman -Sl | grep 关键字

# 按info过滤
pacman -Si | grep 关键字

# 查找已安装的包
pacman -Qs 关键字 # -Q query, -s search

pacman -Qi 包名：查看有关包的详尽信息。
pacman -Ql 包名：列出该包的文件。
```

### 安装软件包
```
pacman -S 包名：例如，执行 pacman -S firefox 将安装 Firefox。你也可以同时安装多个包，只需以空格分隔包名即可。
pacman -Sy 包名：与上面命令不同的是，该命令将在同步包数据库后再执行安装。
pacman -Sv 包名：在显示一些操作信息后执行安装。
pacman -U：安装本地包，其扩展名为 pkg.tar.gz。
pacman -U http://www.example.com/repo/example.pkg.tar.xz 安装一个远程包（不在 pacman 配置的源里面）
```

### 使用短名称安装软件包
```
pacman -S pactoys

pacboy -S 包名:x # x86_64
pacboy -S 包名:i # i386
pacboy -S 包名:m # mingw_w64
```

### 删除包
```
pacman -R 包名：该命令将只删除包，保留其全部已经安装的依赖关系
pacman -Rs 包名：在删除包的同时，删除其所有没有被其他已安装软件包使用的依赖关系
pacman -Rsc 包名：在删除包的同时，删除所有依赖这个软件包的程序
pacman -Rd 包名：在删除包时不检查依赖。
```

### 查看包内容
```
pacman -Ql 包名
```

### 查找文件归属包
```
pacman -Qo <full file path> # 纯路径比较，需要加.exe后缀
```

### 查看包依赖
```
pactree 包名
pacman -Qi 包名
```

### 其他用法
```
pacman
pacman -S --needed filesystem msys2-runtime bash libreadline libiconv libarchive libgpgme libcurl pacman ncurses libintl


pacman -Sw 包名：只下载包，不安装。
pacman -Sc：清理未安装的包文件，包文件位于 /var/cache/pacman/pkg/ 目录。
pacman -Scc：清理所有的缓存文件
```

## 常用软件安装

### 安装git
```bash
pacman -S git

# 安装gitk
```

### 安装编译环境
```
pacboy -S gcc:m
pacboy sync make:m
pacboy sync mingw-w64-x86_64-toolchain

pacman -S  mingw-w64-i686-toolchain　　　　可以不安装
pacman -S  mingw-w64-x86_64-toolchain
```

### 常用工具
```
pacman -S base-devel
pacman -S wget
pacman -S perl
pacman -S ruby
pacman -S python2
```
