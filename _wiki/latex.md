---
layout: wiki
title: Latex 使用技巧
categories: wiki Latex
description: Latex 使用技巧汇总
keywords: Latex
---

# Latex使用

## 基本信息

TeX 的源代码是后缀为 .tex 的纯文本文件。编辑器主要有专用的 `TeXstudio` 和通用的`Visual Studio Code`（添加 latex workshop 插件） 等。而编译系统主要是 `TeX Live` 和 `MiKTeX` ，它们包含 TeX 系统的执行程序以及宏包（一系列控制序列的合集)，也都带有 TeXworks 编辑器。一般编译系统内置的编译引擎包含 pdfLaTeX, XeLaTeX, LaTeX 等。 

CTeX 宏集能适配于多种编译方式，很好支持了中文和中文排版，包含若干文档类（.cls 文件）和宏包（.sty 文件）。

请注意，由于操作系统编码和 TeX 内部实现的限制，在 Windows 平台上，TeX 涉及到的文件（包括 .tex, .jpg 等）名字中都**不要包含中文**。否则，在编译时可能会因为编码问题出现报错。

## 安装

+ 下载[MikTeX](https://miktex.org/howto/install-miktex)并安装
+ 下载安装[TeXstudio](http://texstudio.sourceforge.net/)
+ 在VScode中安装Latex Workshop（可选）

在线工具
+ 在线的Latex编辑器有：http://overleaf.com/
+ Latex公式在线编辑器：http://latex.codecogs.com/eqneditor/editor.php

+ pandoc 实现 latex 转为 word ， `pandoc test.tex -o test.doc`

## 简单示例

```tex
\documentclass{ctexart}
% \documentclass{article}
% 这里是导言区。导言区出现的控制序列，往往会影响整篇文档的格式。如页面大小、页眉页脚样式、章节标题样式等等
\title{你好，world!}
\author{Liam}
\date{\today}


\begin{document}
\maketitle
    \section{你好中国}
        中国在East Asia.
        \subsection{Hello Beijing}
            北京是capital of China.
            \subsubsection{Hello Dongcheng District}
                \paragraph{Tian'anmen Square}
                is in the center of Beijing
                \subparagraph{Chairman Mao}
                is in the center of 天安门广场。
        \subsection{Hello 山东}
            \paragraph{山东大学} is one of the best university in 山东。
\end{document}
```

## 基本概念

+ 控制序列：以反斜杠 \ 开头，以第一个空格或非字母的字符结束，影响输出文档的效果，并且TeX 对控制序列的**大小写**是敏感的。一般来说控制序列的的可选参数由花括号{}包含，但也有用方括号 [] 的。以下是一些基本的控制序列：

  + `\documentclass{article}`  调用名为 article 的文档类,不同的文档类在输出效果上会有差别。`\documentclass[UTF8]{ctexart}` ctexart 就可以支持中文文档，并以 UTF-8 编码保存。
  + `begin` 控制序列总是与 `end` 成对出现，他们中间的内容被称为环境。
  + `maketitle` 能将在导言区中定义的标题、作者、日期按照预定的格式展现出来。
  + `\usepackage{}` 可以用来调用宏包。如\usepackage{amsmath} 可以使用 AMS-LaTeX 提供的数学功能。

+ 注释：TeX 以百分号 % 作为注释标记。若要输出 % 字符本身，则需要在 % 之前加上反斜杠 \ 进行转义（escape），如`\%`。


## 使用

### 数学公式

包含行内模式 (inline) 和行间模式 (display)。前者插入在正文的行文中；后者独立排列单独成行，并自动居中。在行文中，使用 $ ... $ 可以插入行内公式，使用 \[ ... \] 可以插入行间公式；如果需要对行间公式进行编号，则可以使用 equation 环境。\begin{equation} ... \end{equation}

+ 上标可以使用 ^ 来实现（下标则是 _）。它默认只作用于之后的一个字符，如果想对连续的几个字符起作用，请将这些字符用花括号 {} 括起来
+ 根式用 \sqrt{·} 来表示，分式用 \frac{·}{·} 来表示（第一个参数为分子，第二个为分母）$\sqrt{x}$ 与 $\frac{x}{y}$
+ 用 \sum, \prod, \lim, \int 生成累加、累乘、极限、积分
+ 多行公式
    ```tex
    \begin{gather}  %无需对齐的公式组可以使用 gather 环境，需要对齐的公式组可以使用 align 环境
    a = b+c+d \\
    x = y+z
    \end{gather}
    ```
+ 分段函数可以用cases次环境来实现

在线工具：[AI Powered Document Editing](https://mathpix.com/) 可以将截屏中的公式转换成 LaTeX 数学公式的代码

**图片**

利用 graphicx 宏包提供的 \includegraphics 命令插入图片，并且可以根据自己的需求调整大小，如`\includegraphics[width = .8\textwidth]{a.jpg}` 图片比例不变并且宽度会被缩放至页面宽度的百分之八十。
一般使用 figure 环境自动完成位置的调整。
```tex
\begin{figure}[htbp]
\centering
\includegraphics{a.jpg}
\caption{有图有真相}
\label{fig:myphoto}
\end{figure}
```

**表格**

tabular 环境提供了最简单的表格功能。它用 \hline 命令表示横线，在列格式中用 | 表示竖线；用 & 来分列，用 \\ 来换行。一般用table 环境完成表格位置的调整。

**字体**

在系统命令行中输入如下命令：`fc-list :lang=zh-cn > font_zh-cn.txt` 来查看系统字体。每一行的格式为：<字体文件路径>: <字体标示名1>, <字体表示名2>:Style=<字体类型>

**换行**

LaTeX 将一个换行当做是一个简单的空格来处理，如果需要换行另起一段，则需要用两个换行（一个空行）来实现。


## 参考

+ [一份其实很短的 LaTeX 入门文档](https://liam.page/2014/09/08/latex-introduction/)
+ [一份不太简短的LaTeX介绍](https://mirrors.tuna.tsinghua.edu.cn/CTAN/info/lshort/chinese/lshort-zh-cn.pdf) 即 lshort-zh-cn.pdf
+ [LaTeX (MikTeX+TeXstudio) 在win10上的配置教程](https://blog.csdn.net/weixin_39278265/article/details/81348752)
+ [各种LaTeX编辑器的比较](https://en.wikipedia.org/wiki/Comparison_of_TeX_editors)
