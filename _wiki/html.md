---
layout: wiki
title: HTML 基础
categories: wiki HTML
description: HTML 基础
keywords: HTML
---

## 快速开发思路
+ 从代码网址下载类似模板
+ 使用模板作为基础，根据需求添加对应代码
+ 将特定功能的JS、CSS代码以插件的模式组合起来

### 常用功能组件
+ jquery
+ Bootstrap
+ 弹框：[Layer.js](http://lib.h-ui.net/layer/layer_v3.1.1.zip)（web弹层组件）参考[网页](https://blog.csdn.net/meixu568/article/details/81207340)使用
+ 数据库表格插件：[datatables.js](http://lib.h-ui.net/datatables/datatables_v1.10.15.zip)
+ 弹幕 [danmuplayer](https://github.com/chiruom/DanmuPlayer/)
+ 离线存储 localForage
+ 压缩 jszip
+ 图片处理 [WASM-imageMagick](https://github.com/KnicKnic/WASM-ImageMagick) 
+ 图标字体 [fontawesome](https://fontawesome.com/)

### 保护代码
+ 代码混淆
+ 禁用F12，右键等
+ 检查域名
+ a标签target为_blank时，最好加上rel="noopener noreferrer"防止新打开的页面中可以通过 window.opener 获取到源页面的部分控制权

## 基础知识

### 常用简单标签
+ html 限定了文档的开始点和结束点
+ head 标签用于定义文档的头部，描述了文档的各种属性和信息
  + title 定义文档的标题
  + meta 提供有关页面的元信息
  + base 
    + 为页面上所有相对 URL 规定基准 URL（href），如定义基准链接为加速用的CDN链接
    + 为页面上所有链接规定默认目标（target），如定义在新窗口访问链接`<base target="_blank">`，或原页面`_self`
  + link 
    + 链接外部样式表 `<link rel="stylesheet" type="text/css" href="theme.css" />`
    + 链接图标 `<link rel="shortcut icon" href="res/favicon.ico">`
  + script 插入一段 JavaScript
  + style 定义样式信息
+ body 定义文档的主体。


### meta 标签
> meta 是 html 语言 head 区的一个辅助性标签。主要作用有：定义页面使用语言、自动刷新并指向新的页面、控制页面缓冲、控制网页显示的窗口、搜索引擎优化（SEO）等。

#### name属性 
> 便于搜索引擎机器人查找信息和分类信息
+ keywords ：网页关键字 
+ description ：网页内容简述
+ author ：网页作者 
+ viewport ：移动设备适配显示 `<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">` 让当前viewport的宽度等于设备的宽度，同时不允许用户手动缩放
+ referrer ：定义请求头部 Referrer
+ theme-color ：网页主题颜色
+ baidu-site-verification ：百度站长平台验证网站归属权的验证代码，可以不写

#### http-equiv属性 
> 模拟 http 响应文件头的属性

+ refresh ：刷新间隔，如`<meta http-equiv="refresh" content="300">`
+ content-Type ：规定网页字符编码，如`<meta http-equiv="content-type" content="text/html; charset=UTF-8">`，在HTML5中可以直接使用charset属性`<meta charset="UTF-8">`
+ X-UA-Compatible ：定义浏览器渲染方式，主要用于IE兼容性模式，如`<meta http-equiv="X-UA-Compatible" content="IE=edge" />` 

### form
+ action 中的参数，如果是post则可以使用，如果是get则无效