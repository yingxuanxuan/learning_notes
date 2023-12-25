# Python packaging



## 程序打包



### 打包工具推荐

* windows: PyInstaller、py2exe
* linux: Freeze - works the same way like py2exe but targets Linux platform
* mac: py2app - again, works like py2exe but targets Mac OS



### pyinstaller

* 什么是打包模式
  * 目录模式
    * 默认
    * 参数`-D`
    * 打包到目录中
  * 文件模式
    * 参数`-F`
    * 打包到自解压文件中
    * 解压到临时目录后运行
    * 注意当前文件位置会在临时目录



* 编译后获取正确运行路径的方法
  * 文件法
    * `os.path.abspath(__file__)`
  * 运行参数法
    * `sys.argv[0]`
  * 检测冻结法
    * 打包的文件`getattr(sys, 'frozen', False)`为真
    * `sys.executable`为可执行文件路径



#### 安装

```sh
pip3 install wheel
pip3 install pyinstaller
```

* 将PyQt加入PATH
* 手动安装uxp：https://upx.github.io/



#### 简单示例

```powershell
# 打包为单文件.exe
pyinstaller -F main.py

# 添加图标
pyinstaller -F main.py -i .\ico.ico

# 单文件，每次清理，名称为io，入口程序为main.py，在上一层目录
pyinstaller -F --clean -n io  ../main.py

pyinstaller.exe .\barrage_filter.py -w -F -i .\64X64.ico --noupx --clean
pyinstaller xxx.pyw -w -F -i="32X32.ico" --noupx

# bat脚本
@echo off
cd /d %~dp0 # 切换到脚本所在目录
pyinstaller -F --clean -n io  ../main.py
pause       # 按下pause再消失窗口
```



#### 参数解释

```python
# 编译参数
--distpath DIR       # 指定输出目录，默认'.\dist'
--workpath WORKPATH  # 指定临时目录，默认'.\build'
--clean              # 清空临时文件
-n --name            # 程序名称，默认为主py脚本
                     # 影响.exe名称，build临时目录名称
-D --onedir          # 生成单目录可执行程序
-F --onefile         # 生成单文件可执行程序

-c --console --nowindowed # 制作命令行程序
-w --windowed --noconsole # 制作窗口程序
-i --icon FILE.ico/FILE.exe # 指定或提取ico文件

-v --version FILE # 加入版本信息
--add-data # 添加附加文件
--add-binary # 添加二进制文件
--key="16个字符" # 依赖PyCrypto，编译好的：https://github.com/sfbahr/PyCrypto-Wheels
--upx-dir UPX_DIR # 指定UPX目录，UPX是一个可执行程序压缩软件
--noupx # 不使用UPX
```



#### 代码混淆

* pyinstaller将文件编译为pyc文件，pyc是未经过优化的字节码，很容易被反编译
* pyinstaller可以将发布文件使用AES256加密，但是很容易从exe中找到密钥并解密发布文件，
* py文件可以编译为pyo文件，pyo文件是经过优化的字节码、并且删除了文档和注释，但是pyo也很容 易被反编译，而且影响pyinstaller定位import文件
* 可以将文件编译为pyo为pyinstaller手动添加import？？？？？？？？？？？？？？？？？？？？



#### 可复用脚本、配置

```
# 生成配置文件（与pyinstaller打包时生成 app.scpc相同）
pyi-makespec options script.py [other scripts ...] 

# 使用scpc配置文件编译（只支持有限的options）
pyinstaller options script.spec  
```



#### linux下使用pyinstaller

```bash
# 0 编译python，打开共享库编译配置，生产.so文件：
# libpython3.8m.so.1.0
# libpython3.8m.so
# libpython3.8.so.1.0
# libpython3.8mu.so.1.0

make clean
./configure --enable-shared
make
make install

# 1 安装
pip3 install pyinstaller
/usr/python38/bin/pyinstaller

# 2 构建打包环境
python3 -m venv .
source ./bin/activate
# 启动需要打包的软件，pip3安装包，直到成功运行
pip freeze > requirements.txt
# 下次构建编译环境可以直接是使用，pip install -r requirements.txt
deactivate

# 3 编译
# 复制动态链接库到编译目录
cp /home/yingxuanxuan/python/Python-3.8.5/libpython3.* .
# 根据spec文件打包
/usr/python38/bin/pyinstaller --distpath linux --workpath tmp_linux --clean -F ./io_linux.spec
```



#### windows下添加version_file

- version-file是windows操作系统可以识别的版本信息文件

- 安装pyinstaller后会安装一个exe版本号提取工具`pyi-grab_version`，可以用于提取版本信息文件，作为模板

```powershell
# 获取版本信息，默认保存到当前目录
pyi-grab_version "C:\GstarSoft\浩辰CAD看图王 电脑版\gcad.exe"

# 为pyinstaller添加版本信息文件
pyinstaller --version-file .py
```

- 版本信息说明

```python
# UTF-8
#
# For more details about fixed file info 'ffi' see:
# http://msdn.microsoft.com/en-us/library/ms646997.aspx
VSVersionInfo(
  ffi=FixedFileInfo(
    # filevers and prodvers should be always a tuple with four items: (1, 2, 3, 4)
    # Set not needed items to zero 0.
    filevers=(4, 5, 0, 0),   """修改"""
    prodvers=(4, 5, 0, 0),   """修改"""
    # Contains a bitmask that specifies the valid bits 'flags'r
    mask=0x3f,
    # Contains a bitmask that specifies the Boolean attributes of the file.
    flags=0x0,
    # The operating system for which this file was designed.
    # 0x4 - NT and there is no need to change it.
    OS=0x40004,
    # The general type of file.
    # 0x1 - the file is an application.
    fileType=0x1,
    # The function of the file.
    # 0x0 - the function is not defined for this fileType
    subtype=0x0,
    # Creation date and time stamp.
    date=(0, 0)
    ),
  kids=[
    StringFileInfo(
      [
      StringTable(
        u'080404B0',
        [StringStruct(u'CompanyName', u'Gstarsoft Co.,Ltd'),
        StringStruct(u'FileDescription', u'浩辰CAD看图王 应用程序'),
        StringStruct(u'FileVersion', u'4.5.0.0'),
        StringStruct(u'InternalName', u'浩辰CAD看图王'),
        StringStruct(u'LegalCopyright', u'Gstarsoft Co.,Ltd. All Rights Reserved.'),
        StringStruct(u'OriginalFilename', u'gcad.exe'),
        StringStruct(u'ProductName', u'浩辰CAD看图王'),
        StringStruct(u'ProductVersion', u'4.5.0.0')])
      ]), 
    VarFileInfo([VarStruct(u'Translation', [2052, 1200])])
  ]
)
```



#### spec file中添加版本号及图标

- **如果使用spec file，命令行中添加版本号及图标的参数将无效，文档中没有说明**
- **图标在操作系统中有缓存，使用q-Dir等会看不到图标更改，可以使用资源管理器，修改文件名等手段更新缓存**
- 在spec file中修改如下

```
exe = EXE(
    name='version=version.txt',
    icon='128x128.ico'
)
```



#### 如何打包隐式导入的依赖

* 什么是隐式导入
  * 通过`import`语句导入的包为显式导入，pyinstaller 可以检测并自动打包依赖
  * 通过字符串模式导入的包为隐式导入，pyinstaller 不能检测打包
  * 需要在spec文件中加入`hidden import`列表中

#### 
