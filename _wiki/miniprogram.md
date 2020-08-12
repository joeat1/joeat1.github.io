---
layout: wiki
title:微信小程序
categories: 微信小程序
description: 微信小程序使用
keywords: 微信小程序
---

> [开放文档](https://developers.weixin.qq.com/miniprogram/dev/framework/) 

## 基本框架

一个小程序主体部分由app.js app.json app.wxss三个文件组成，必须放在项目的根目录。每个页面page包含js、json、wxml、wxss四个文件。

+ 渲染层：[WXML](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/)+WXSS（类似于HTML+CSS）

  + text组件 展示文字

  + image组件 展示图片

  + view组件  元素容器，利用flex设计弹性盒子

  + 利用响应式长度单位rpx来适配不同屏幕的宽度（所有设备的屏幕宽高都为750rpx）

  + navigator 组件实现页面跳转

+ 逻辑层：JS

+ 配置信息：Json



## 基本规范

+ 友好：重点突出，流程明确
+ 清晰：导航明确，提供等待提示
+ 便捷：减少输入
+ 统一：风格配色
+ 平台规范：禁止流量分发诱导分享等



## 基本流程

+ 小程序注册（邮箱，邮箱激活，信息登记）
+ 微信开发者工具（小程序APPID）下载
+ 小程序开发
+ 小程序上传
+ 官方审核



## 开发实践

+ 小程序配置
  + 根目录下的 `app.json` 文件用来对微信小程序进行全局配置，决定页面文件的路径pages（所有页面都需要放在此列表中）、窗口表现、设置网络超时时间、设置多 tab ，底部 `tab` 栏的表现等。
  + `app.json` 的 `pages` 字段的第一个页面就是这个小程序的首页
  + 小程序启动之后，在 `app.js` 定义的 `App` 实例的 `onLaunch` 回调会被执行

+ 页面配置

### 错误

+ navigateTo:fail can not navigateTo a tabbar page：标签不能用于导航，**只能配置最少 2 个、最多 5 个 tab**
+ 