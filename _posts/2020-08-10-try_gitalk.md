---
layout: post
title: Try gitalk
categories: gitalk
description: have a try
keywords: gitalk
---

给 github pages 提供一个可以评论的地方，在gitalk和gitment中选择了gitalk, 但是实现中还是出现了很多问题。

## 实现

> 参考模板代码和[官方介绍](https://github.com/gitalk/gitalk)

基本流程
+ 打开 github.com/settings/applications/new 新建应用并填写信息，需要特别注意的是`Authorization callback URL`和`Homepage URL`需要由https开头的网址。

遇到问题：
> 参考
> https://iochen.com/2018/01/06/use-gitalk-in-hexo/
> https://github.com/gitalk/gitalk/issues/115
+ 未初始化
  + 解决方法：检查是否存在授权网址等参数的问题，如果还出现错误，只能每一篇博文发布后初始化一下，不过网上也有[自动初始化代码](http://edisonxu.com/2018/10/31/gitalk-auto-init.html)可以参考。
    ```js
    // Gitalk 主要参数
    enable: true #指的是是否开启Gitalk
    ClientID: xxxxxx #之前的Client ID
    ClientSecret: xxxxxxxxxxxx #之前的Client Secret
    repo: gitalk #你要存放的项目名，下文会详细再说
    owner: iosite #这个项目名的拥有者（GitHub账号或组织）
    adminUser: "['iosite']" #管理员用户，下文也会详细讲
    ID: location.pathname #页面ID，不知道就默认的就好了
    labels: "['Gitalk']" #GitHub issues的标签，下面会详细说
    perPage: 15 #每页多少个评论
    pagerDirection: last #排序方式是从旧到新（first）还是从新到旧（last）
    createIssueManually: true #如果当前页面没有相应的 isssue ，且登录的用户属于 admin，则会自动创建 issue。如果设置为 true，则显示一个初始化页面，创建 issue 需要点击 init 按钮。
    distractionFreeMode: false #是否启用快捷键(cmd|ctrl + enter) 提交评论.
    ```
+ id长度超过50
  + 由于gitalk以GitHub的issue为基础，所以在id上存在长度不超过50的限制
  + 解决方法：网上大多使用了[md5](https://github.com/blueimp/JavaScript-MD5/blob/master/js/md5.min.js)压缩的方式，也有通过截取id只留下前50个字符信息的方法，如`id: '{{ page.url | truncate: 50, "" }}',`
+ 其他原因
  + 在新建repo用于保存评论信息时，**不要画蛇添足地选择issue的模板**，否则会出现无法成功的情况