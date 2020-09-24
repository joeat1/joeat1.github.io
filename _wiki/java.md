---
layout: wiki
title: java 基础
categories: wiki java
description: java 基础
keywords: java
---

## 基本概念

+ GroupID 是项目组织唯一的标识符，实际对应 JAVA 的包的结构
+ ArtifactID 就是项目的唯一的标识符，实际对应项目的名称。
+ main 函数中包含的是运行主体


## 其他

+ 出现中文的 java 源码编译： javac -encoding utf-8 hello.java 
+ java 的 String 使用的编码是 Unicode 。
+ 类前面加 @SuppressWarnings("unchecked") 可以解决“源码中使用了不安全的操作而编译不成功”的问题