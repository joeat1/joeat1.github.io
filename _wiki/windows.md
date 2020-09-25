---
layout: wiki
title: winodws 使用技巧
categories: wiki winodws
description: winodws 使用技巧汇总
keywords: winodws
---

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
+ (install the windows performance toolkit)[https://msdn.microsoft.com/en-gb/windows/hardware/commercialize/test/wpt/index?f=255&MSPPError=-2147217396]。`xperf -on latency -stackwalk profile -buffersize 1024 -MaxFile 256 -FileMode Circular && timeout -1 && xperf -d cpuusage.etl` 生成 The log will be stored in C:\Windows\system32 with the file name as cpuusage.etl.