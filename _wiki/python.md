---
layout: wiki
title: python 使用技巧
categories: wiki python
description: python 使用技巧汇总
keywords: python
---


### 离线安装 python 拓展库

> 拓展库的路径一般在 /usr/lib/python3.6/site-packages 下。
> 按照安装包的方式可以分为， 源码包，wheel 包， egg包，相对来说，wheel 格式是 egg 格式的升级版本。
> wheel 包的命名格式为 {distribution}-{version}(-{build tag})?-{python tag}-{abi tag}-{platform tag}.whl 。其中 platform 包含 win32 , linux_i386 , linux_x86_64, 而 any 表示跨平台； abi 表示应用程序二进制接口（Application Binary Interface）。

根据需要逐项安装拓展库，也可以将依赖库写在一个文件 requirement.txt 中，每一行包含`包名==版本号`，版本号可不写。下载拓展库 download 相关的基本参数 -d :指定存放下载包路径 、 -r：指定请求文件 、 --python-version 36 指定解释器版本。
如果需要选择安装环境可以使用参数 `--platform` 其默认值为当前运行系统所属平台，但是一般不推荐使用，最终生成的都是源码包形式，跨平台编译的时候可能存在问题。设置平台类型和解释器版本都需要添加 `--no-deps` 才能执行，
```bash
pip download -d mypkgs jieba
pip download -d mypkgs --platform linux_x86_64 --no-deps  jieba 
pip download -d mypkgs -r requirement.txt

#在对应的服务器上根据 requirement.txt 安装拓展库，注意: --no-index 必须搭配 --find-links 使用
pip install --no-index --find-links=mypkgs -r requirement.txt 
```
部分时候需要更新 pip 版本：
`pip install --upgrade pip` 

