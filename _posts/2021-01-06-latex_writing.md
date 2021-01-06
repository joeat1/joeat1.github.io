---
layout: post
title: 论文 latex 要求
categories: paper-notes standard
description: 论文 latex 要求
keywords: latex 论文
---

> 以 IEEE 系列会议或期刊为例

## 基本原则

+ 在利用latex排版之前，先准备好论文主体内容和图片数据。
+ 根据 `IEEEtran.cls`（论文风格的规范文件）和会议或期刊发布的示例 `tex` 来进行写作，并且需要关注会议对论文的具体要求（如页数限制）。
+ 不要在标题或者摘要中使用符号，特殊字符，脚注或数学公式等形式的内容。
+ 即使是在摘要中已经定义的缩写和首字母简称，在文本中第一次使用时还要对它们进行定义。
+ 方程中出现的符号需要在方程之前已经定义好，或者紧接着会进行定义。

## 具体要求
> 此处只是示例，需要根据期刊或者会议的具体说明来进行

+ 字体设置：默认字体大小为 `10pt`（`10 point Times Roman font`），如需修改为`12pt`，可以为 `\documentclass[12pt]`。
+ 模板样式：默认是期刊 `journal` ，会议投稿需要设置为 `\documentclass[conference]`
+ 单双栏设置：默认为双栏 `twocolumn` ，单栏一般用于草稿，可以用 `\documentclass[onecolumn]` 表示。
+ 页面类型设置：类型默认为美国通用的 US letter (`letterpaper`)， 也是IEEE通用的，也可以改为A4（`a4paper`），即`\documentclass[a4paper]`。
+ 行距设置：大部分模板默认为单倍行距（single-spaced）， `\linespread{1.5}` 可以将行间距设置 1.5 。
+ 页边距设置：可以通过 `\usepackage[bottom=2.0cm, top=2.0cm,left=1.7cm,right=1.7cm]{geometry}` 来调整页边距。

## 其他

+ 为了使你的方程更紧凑，你可以使用斜杠表示除法，使用exp表示自然指数。并且使用罗马符号来表示参数和变量。
+ 脚注（footnotes）用上标编号，把脚注放在引用它的那一栏的底部。不要在摘要或参考书目中加上脚注。用字母作为表格脚注。
+ 将图、表放置在列的顶部和底部。较大的图形和表可以跨越这两个列。图片描述应在图片下面；表头应出现在表的上方。