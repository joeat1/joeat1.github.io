## 说明

目标网址：https://joeat1.github.io/

模板来源：https://github.com/mzlogin/mzlogin.github.io

模板支持画流程图、时序图、mermaid 和 MathJax，因为相关的引入文件比较大可能影响加载速度，作者没有默认对所有文件开启，需要在要想开启的文件的 Front Matter （ Front-matter 是文件最上方以 — 分隔的区域，用于指定个别文件的变量，也可以说是页面属性）里加上声明：
```yaml
---
flow: true
sequence: true
mermaid: true
mathjax: true
---
```
分别对应 flowchart.js（流程图）、sequence-diagram.js（时序图）、mermaid 和 MathJax 的支持，按需开启即可。


**目录**

```
    * TOC
    {:toc}
```

## 上传同步
+ 在 windows 平台上可以利用 `push_content.bat` 进行数据的上传。

## 规范

+ 中文与英文或者数字中间需要空格
+ 注明出处和参考