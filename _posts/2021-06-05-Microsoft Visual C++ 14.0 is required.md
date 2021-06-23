---
layout: post
title: Microsoft Visual C++ 14.0 is required
categories: tips
description: 安装部分 python 库时遇到 Microsoft Visual C++ 14.0 is required 反馈
keywords: Microsoft Visual C++ 14.0 is required
---

## 问题描述
+ 安装 torch-scatter 等 python 库时出现 `Microsoft Visual C++ 14.0 is required` 的内容。


## 主要原因

部分库的安装需要 C++ 的编译功能

## 解决方法

+ 部分可以直接去[网站](https://www.lfd.uci.edu/~gohlke/pythonlibs/)下载对应版本的.whl文件，然后运行pip install xxx.whl进行安装
+ 和`pytorch-geometric`相关的库可以从[链接](https://pytorch-geometric.com/whl/)中下载对应的whl文件
+ 不能找到的需要安装 `Microsoft Visual C++ Build Tools` 
  1. 下载[Microsoft Visual C++ Build Tools 2015](http://go.microsoft.com/fwlink/?LinkId=691126)得到`visualcppbuildtools_full.exe`文件，安装。
  2. 如果安装`visualcppbuildtools_full.exe`文件时提示 .Net Framework 版本过低，则需要安装 .Net Framework 4.6.2
  3. 如果安装`.Net Framework 4.6.2`时出现“无法建立到信任根颁发机构的证书链”，实际上是要安装一个根证书 MicrosoftRootCertificateAuthority2011.cer [下载地址](http://download.microsoft.com/download/2/4/8/248D8A62-FCCD-475C-85E7-6ED59520FC0F/MicrosoftRootCertificateAuthority2011.cer)，点击证书文件，选择`受信任的根证书颁发机构`存储，完成后重新运行`.Net Framework 4.6.2`安装程序即可。但是此时还可能出现`时间戳签名和或证书无法验证或已损坏`的问题。可以尝试安装`KB2813430`补丁；[32位系统补丁下载地址](https://www.microsoft.com/zh-CN/download/details.aspx?id=39110)，[64位系统补丁下载地址](https://www.microsoft.com/zh-CN/download/details.aspx?id=39115)。


