---
layout: wiki
title: winodws 使用技巧
categories: wiki winodws
description: winodws 使用技巧汇总
keywords: winodws
---

## 镜像下载

> 镜像可以用于系统重装和虚拟机创建

访问官方地址：https://www.microsoft.com/zh-cn/software-download/windows10ISO/ 根据提示进行系统工具下载，通过工具制作系统盘或更新系统
如果需要下载镜像，可以通过谷歌浏览器，切换 use-agent 为手机模式，即可发现下载直链。

## 默认设置更换

> 部分 Windows 的默认设置并不符合个人需求

### 更换默认下载目录
+ 在快速访问下找到 `下载` ，右击鼠标点击 `属性` 后找到 `位置` 这个导航栏，从中点击 `移动` 然后选择需要更改的文件夹

### 更换新文档保存路径
+ 打开的设置窗口，我们依次点击 系统-存储 找到 `更多存储设置` 的 更改新内容的保存位置，即可全部切换到非系统盘。此项操作之后系统会自动在对应盘创建一个用户文件夹来保存上述内容。

### 更换文件夹权限
> 参考[链接](https://www.zhihu.com/question/31001796/answer/1099015956) 或者 [链接](https://blog.csdn.net/wpwalter/article/details/79394709)
+ 第一步：文件夹右键——属性——“安全”选项卡——“高级”选项卡——更改“所有者”——输入“你当前用户的用户名”，再勾选上【替换子容器和对象的所有者】，点确定退出“高级”选项卡。然后再点确定退出“属性”选项卡。第二步：依照第一步再次打开“高级”选项卡。点下面“禁用继承”按钮，全部转为“显式权限”。然后把所有包含“拒绝”的权限条目删除。再然后点“添加”，添加“Users”的“完全控制”权限(如果已经有了就不用加了)。第三步：勾选“使用可从此对象继承的权限替换所有子对象的权限”，确定。

### 重装 office 

+ 建议新电脑激活之后，更换默认程序路径之后重装。可以参考[链接](https://support.microsoft.com/zh-cn/office/%e5%9c%a8%e7%94%b5%e8%84%91%e6%88%96-mac-%e4%b8%8a%e4%b8%8b%e8%bd%bd%e5%b9%b6%e5%ae%89%e8%a3%85%e6%88%96%e9%87%8d%e6%96%b0%e5%ae%89%e8%a3%85-microsoft-365-%e6%88%96-office-2019-4414eaaf-0478-48be-9c42-23adc4716658?ui=zh-cn&rs=zh-cn&ad=cn)，在已激活的电脑或 Mac 上下载并安装或重新安装 Office 365 或 Office 2019： 访问 https://account.microsoft.com/services/ 即可看到软件安装下载的按钮。

### 删除重装系统后产生的windows.old文件夹
+ 尝试使用磁盘清理功能来删除Windows.old文件夹
+ 右键点击C盘，点击属性，点击“磁盘清理”，勾选“以前的Windows安装”，点击确认后“删除文件”按钮即可。

## 常用快捷键

### 截图

+ window自带的 **截图工具**
+ PrtSc 全屏截图
+ Win + Shift + S 可选范围式截图
+ Alt + A 使用 QQ 、微信等软件的截图快捷键


### 输入法
+ win10 自带微软拼音现在输入 emoji 的快捷键是 Ctrl + Shift + B 

### 浏览器

+ CTRL+T 开启新标签页   Ctrl+N 打开新窗口  Ctrl+Tab 切换窗口


## 自动更新

+ 与Windows更新相关的服务：
    
    > net用于打开没有被禁用的服务或关闭服务     net stop wuausery
    >
    > 使用sc命令实现关闭服务  sc config wuausery start=disabled
    > taskkill /F /FI "SERVICES eq UsoSvc"
    
    1. Windows 10 Update Facilitation Service     osrss
    2. Windows Update   wuauserv
    3. Windows Update Medic Service   WaaSMedicSvc
    4. Update Orchestrator Service   UsoSvc


## 错误

### ntoskrnl.exe

> 参考：https://www.thewindowsclub.com/ntoskrnl-exe-high-cpu-disksage

+ ntoskrnl.exe（Windows NT 操作系统 内核的简称），也称为内核映像，提供Windows NT内核空间的内核和执行层，负责各种系统服务，如硬件抽象，进程和内存管理，因此使其成为系统的基本组成部分。它包含缓存管理器，执行程序，内核，安全性参考监视器，内存管理器和调度程序（Dispatcher）。


### 解决方案

+ `Dism /Online /Cleanup-Image /RestoreHealth`
+ `msdt.exe /id PerformanceDiagnostic` check the performance 性能故障排除程序
+ 关闭`Windows Search`服务
+ 如果Runtime Broker进程占用的内存超过15％，则可能是PC上的应用程序存在问题。在这种情况下，您需要停止Runtime Broker进程。
+ chkdsk c: /f
+ [install the windows performance toolkit](https://msdn.microsoft.com/en-gb/windows/hardware/commercialize/test/wpt/index?f=255&MSPPError=-2147217396)。`xperf -on latency -stackwalk profile -buffersize 1024 -MaxFile 256 -FileMode Circular && timeout -1 && xperf -d cpuusage.etl` 生成 The log will be stored in C:\Windows\system32 with the file name as cpuusage.etl.
