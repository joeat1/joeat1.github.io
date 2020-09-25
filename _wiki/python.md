---
layout: wiki
title: python 使用技巧
categories: wiki python
description: python 使用技巧汇总
keywords: python
---

### jupyter

"jupyter_contrib_nbextensions" 模块包含了一系列共同开发的非正式插件

## 安装拓展包 ##
> 一旦需要下载国外的内容的，一般考虑国内是否有镜像网站，例如 pip install 的软件源
>
> 建议在独立的虚拟环境中安装拓展包，保证一个相对纯净的开发环境 `python3 -m venv XXX`
>  
> 直接使用 pip install 可能出现无法编译安装的情况，此时可能需要去官网下载包装好的文件，如 exe （Windows） 和 wheel 

+ 拓展库的路径一般在 /usr/lib/python3.6/site-packages 下。
+ 按照安装包的方式可以分为， 源码包，wheel 包， egg包，相对来说，wheel 格式是 egg 格式的升级版本。
+ wheel 包的命名格式为 {distribution}-{version}(-{build tag})?-{python tag}-{abi tag}-{platform tag}.whl 。其中 platform 包含 win32 , linux_i386 , linux_x86_64, 而 any 表示跨平台； abi 表示应用程序二进制接口（Application Binary Interface）。

### 几种安装方法 ###
+ pip install XXX
  + pip install tensorflow==1.9.0 keras==2.2.0 -i https://pypi.tuna.tsinghua.edu.cn/simple   使用清华的镜像，可以加快某些包的下载
  + 包更新
    pip install matplotlib --upgrade
  + 安装特定版本的包：pip install XXX==X.X.X.X 如果安装A时需要的B也要限定版本， 需要将B的版本写在前面。一般会出现版本的相互依赖，需要不断调整顺序，特别是python3的一些包是不能用于python2的
  + `pip`出现现有安装包干扰，可以增加 --ignore-installed 
+ easy_install XXX
+ 到模块对应官网或github下载源码文件或whl文件或release版本可运行的exe文件


### 离线安装 python 拓展库

根据需要逐项安装拓展库，也可以将依赖库写在一个文件 requirement.txt 中，每一行包含`包名==版本号`，版本号可不写。

下载拓展库 download 相关的基本参数：
  ```
  -d :指定存放下载包路径
  -r：指定请求文件
  -i https://pypi.tuna.tsinghua.edu.cn/simple ：指定镜像网站
  --python-version 36 指定解释器版本。
  ```
如果需要选择安装环境可以使用参数 `--platform` 其默认值为当前运行系统所属平台，但是一般不推荐使用，最终生成的都是源码包形式，跨平台编译的时候可能存在问题。设置平台类型和解释器版本都需要添加 `--no-deps` 才能执行。

```bash
pip download -d mypkgs jieba
pip download -d mypkgs --platform linux_x86_64 --no-deps  jieba 
pip download -d mypkgs -r requirement.txt

#在对应的服务器上根据 requirement.txt 安装拓展库，注意: --no-index 必须搭配 --find-links 使用
pip install --no-index --find-links=mypkgs -r requirement.txt 
```
部分时候需要更新 pip 版本：
`pip install --upgrade pip` 


## python 基本语法

+ 常用模块：提供文件和进程处理功能的 `os` 模块；时间日期处理相关的 `time/datetime` 模块；常用的字符串处理的`string` 模块；数学计算操作和常量的`math`模块；字符串匹配的`re`模块；访问解释器相关参数的`sys`模块

+ `dir` 返回由给定模块, 类, 实例, 或其他类型的所有成员组成的列表； `type` 函数检查一个变量的类型；`isinstance` 函数检查一个对象是不是给定类(或其子类)的实例

+ 加载与重载：`import`  `__import__`(内置导入) `reload` （重载）

  ```python
  module = __import__(module_name)
  func = getattr(module, func_name)
  reload(module_name)
  ```

+ 常用内置函数（并不需要导入模块即可执行使用）
  ```
  abs(x)  返回数字 x 的绝对值
  chr(x)  返回 unicode 编码 x 的字符
  ord(x)  返回 x 的 Unicode 编码
  bin(x)  把数字 x 转换为二进制串
  hex(x)  把数字 x 转换成 16 进制串
  hasattr(obj，nam)   测试对象是否有该属性
  id(obj)     返回元素的内存地址 type
  type(obj)   返回对象类型
  help(obj)   查看函数使用说明
  dir(obj)    查看模块常量和内容
  map(func，seq)    对序列都映射到 func 上
  ```

+ 字符串常用方法
  ```
  find     #返回第一次出现位置  find(str, num1，num2）从num1到num2中查找
  rfind    #从尾部向前查找
  index  rindex
  count    #计出现次数
  split    #从左侧分割  rsplit从右侧
  join     #把多个字符串连接
  lower  upper  #切换大小写
  repace        #替换字符
  strip         #去除空格
  startswith  endswith  #判断是否开头或者结尾有某些字符子串
  in                    #判断某字符或子串存在于字符串或列表中
  ```

+ 自定义函数： lamba 类似定义公式，定义了一个参数为x的函数g，返回值是一个lambda函数，在lambda函数里，对x和y的大小进行比较并返回一个布尔值。在批量处理时，常和 apply 函数一起使用。

+ `eval` 函数将一个字符串（ 简单的表达式, 或者内建 Python 函数）作为 Python 表达式求值. 

+ compile 函数会返回一个代码对象, 你可以使用 exec 语句执行它；`execfile` 函数可以从文件加载代码, 编译代码, 执行代码


+ 异常处理
  ```python
  try:
  except Exception as e:
      print (e)
  ```
  常见的异常类型： KeyError（键值隐射异常）  ValueError（参数传入无效） IOError（输入输出操作异常） TypeError（类型无效操作）

+ python 的魔法方法 
  + 如 `__init__` ， `__cmp__` ， `__index__` ， `__getattr__` ， `__setattr__` ， `__getattribute__` 等

+ 类
  + 类似__xxx__这样的变量是特殊变量，可以被直接引用，但是有特殊用途
  + 类似_xxx和__xxx这样的函数或变量就是非公开的（private），不应该被直接引用，外部不需要引用的函数全部定义成private，只有外部需要引用的函数才定义为public
  + 类变量是唯一的，只要一处更改在其他地方立即生效，可以利用类变量实现全局的config设置
  + @property广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性。

+ 包和模块
  + if __name__ == '__main__':     #直接运行的时候运行时判断成立，也可以让一个模块通过命令行运行时执行一些额外的代码，最常见的就是运行测试。
  + if __name__ == 'XXXX'   #如果XXXX为文件模块名则是被调用时判断成立
  + 文件夹下包含一个__init__.py文件表示该文件夹是一个python的包，在包的每个目录下都必须有一个，可以是空文件，主要用于设置__all__变量以及执行初始化包所需的代码
  + 如果__init__文件中有__all__ = ['XXXX','XXXX','XXXXX']，就可以直接使用from XXX import *的方式导入函数
  + 默认情况下，Python解释器会搜索当前目录、所有已安装的内置模块和第三方模块，搜索路径存放在sys模块的path变量中，直接修改sys.path，添加要搜索的目录 sys.path.append('/Users/my_py_scripts')

## 虚拟开发环境

开辟每一个虚拟开发环境的配置状态，安装 virtualenv 和 virtualenvwrapper 实现不同项目之间的频繁切换。

```bash
virtualenv venv                     # 初始化虚拟环境venv
source venv/bin/activate            # 进入virtualenv venv
pip install -r requirement.txt      # 将需要安装的内容复制在TXT中
cp /usr/lib/python2.7/dist-packages/libvirt* /homevenv/lib/python2.7/site-packages
                                    #将python中的一些文件复制到虚拟机中的文件中
deactivate                          #关闭虚拟环境
```

## python与ipython程序的常用参数 ##

python -i xxxx.py   在运行完成后将打开python终端，终端保留着主程序的变量信息
python3 -m cProfile XXX.py > XXX.log  分析XXX.py的函数耗时和调用次数

ipython -pylab 可以直接导入scipy, numpy等常用包
ipython notebook 打开网页版ipython编辑场所

ipython中使用
问号和help查询指令用法
%run -t XXX.py  获取脚本运行时间
%run -p XXX.py  获取函数调用，运行时间，代码对应位置灯运行信息 

%debug调试程序脚本  list看源码，bt看调用栈
%hist查看历史输入的指令
使用 %logstart 与 %logoff 保存 运行会话文件
使用！后连接shell语言，可以执行shell指令

在python终端运行时，常需要更改文件，在保持现有变量情况下继续检验函数，需要重新加载修改后的文件，从而重新测试，如：python 中的import importlib中又一个函数importlib.reload(plugins.Moods_crawler)可以将文件重新加载



```bash
# Run a cell via a shell command
%%script

# Run cells with bash in a subprocess
# This is a shortcut for %%script bash
%%bash

# Run cells with python2 in a subprocess
%%python2

# Run cells with python3 in a subprocess
%%python3
```

## pdb调试

python -m pdb myscript.py

- 输入 next 运行到下一行 
- 输入 list 显示下面 11 行代码 
- 输入 p 打印变量 - 输入 quit 结束调试

## 协程

### async/await

+ asyncio.wait(task)
+ asyncio.sleep(2)
  ```python
  loop = asyncio.get_event_loop()
  loop.run_until_complete(task) 
  loop.close()
  ```


**程序内部插入断点**：pdb.set_trace()

## 其他机制

### 单元测试

+ 工程项目中一定需要做，使用unittest模块，比较预期与实际输出。

### python 中编码与格式

+ 在 unix 操作系统中的换行是\n，而在windows中的换行是\r\n，在python进行文件识别处理的时候需要注意。
+ bytes与 str 的转换 encode(encoding='utf-8') 与 decode() , 注：存在转化无效的状态时，可以考虑 decode('utf8','ignore')
+ bytes与 hex 的转换 bytes.hex() 与 bytes.fromhex()
+ 字典不支持如list，set等不能hash化的变量作为键值
+ binhex 模块用于到 Macintosh BinHex 格式的相互转化.

### python 与其他语言的结合
+ jpython 程序用于 python 解释 Java
+ 利用 ctype 实现底层 c 的函数调用

### 文件的版本迁移
+ 2to3.exe   在 python3 的 script 目录下能够将 python2 迁移到 python3

## 常见错误 ##

+ 异常状态提示
```
AttributeError： 调用不存在的方法
EOFError： 遇到文件末尾引发的异常
ImportError： 导入模块包出错
IndexError： 列表越界
IOError： IO操作出现异常，如打开文件
KeyError： 使用字典中不存在的关键字
NameError： 使用不存在的变量名
TabError： 语句缩进不正确
ValueError： 搜索列表不存在对应值
ZeroDivisionError： 除数为零
```

+ python 递归深度报错（maximum recursion depth exceeded）
    `sys.setrecursionlimit(1000000)`设置递归深度达到 1000000 （根据实际需求修改）

+ 在 spyder 中自动补全需要修改：D:\Anaconda3\Lib\site-packages\spyderlib\utils\introspection下的module_completion.py
+ 缩进问题，建议直接使用空格而不是 tab 。

+ 命令行调用 python 出现编码错误： UnicodeDecodeError: ‘gbk‘ codec can‘t decode byte 0x9a in position
    找到安装路径 anaconda3\Lib\site-packages\pyreadline\lineeditor 中的 history.py 文件的第 82 行，将 `for line in open(filename, 'r'):` 替换成 `for line in open(filename, 'r', encoding='utf-8'):` 。

