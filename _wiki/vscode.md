---
layout: wiki
title: VSCODE 的使用技巧
categories: wiki VSCODE
description: VSCODE使用
keywords: VSCODE
---

## 常用的插件

+ Code Runner：可以运行C，C++，Java，JS，PHP，Python等代码
+ Markdown All in One ： 便于Markdown的编译和预览
+ TODO Highlight： 高亮`TODO:`的位置
+ Chinese (Simplified) Language Pack：适用于 VS Code 的中文（简体）语言包


## 快速的Git提交

VSCODE 提供了Git项目的内容变化检测，可以在源代码代理模块（快捷键: `Ctrl + Shift +G` ）查看变化信息。

在进行Git拉取，推送之类工作前，需要安装Git程序，并设置好程序的环境变量。之后设置Git的全局信息：
```bash
git config --global user.name "XXXXX" 
git config --global user.email "XXXXXX@qq.com"
```
可以在空文件夹内通过 `git init` 新建项目后连接远端仓库地址。
```bash
touch README.md 
git add README.md 
git commit -m "first commit" 
git remote add origin XXXXXXX  // 远程仓库地址
git push -u origin master
```

或者通过 `git clone` 从github等代码托管平台拉取项目信息，此处一般自动绑定了remote信息。

在代码管理界面，对文件夹右键，选择全部提交后选择推送，即可实现。