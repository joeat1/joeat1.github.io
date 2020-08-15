---
layout: post
title: Wireshark的流量分析
categories: 流量分析
description: Wireshark的流量分析
keywords: Wireshark 流量分析
---

**目录**

* TOC
{:toc}


## 流量清洗

**Wireshark自带工具**

+ mergecap（合并流量包） mergecap -w <输出文件> <源文件1> <源文件2> …
+ tshark（流量包过滤） https://link.zhihu.com/?target=https%3A//www.wireshark.org/docs/man-pages/tshark.html
+ editcap（流量包分解） editcap.exe -c 100 D:\dump.pcap D:\test.pcap 分解为100个报文

## 基本流程

1. 将所有流量包合并：
mergecap -w total.pcap a.pcap b.pcap //（文件名太长简写成a和b） Wireshark中 File -- merge可以将两个包合并

2. 清洗：tshark -r total.pcap -Y pop||smtp||http -w result.pcap（total.pcap为上一步合并的数据包）

## 流量分层
1. 选择统计→对话，查看流量走向情况，根据IP信息，设置过滤条件，初步分析可疑点

2. 使用脚本提取报文中的重要信息，统计，分类等等


## 入侵流量追踪

根据目标IP重新筛选数据包，尝试搜索内容相关字符信息，如http matches "upload|alert|script|eval|select"



## 附录一：Tshark使用参数详解
> Tshark官方文档https://www.wireshark.org/docs/man-pages/tshark.html

捕获接口:
```
　　-i: -i <interface> 指定捕获接口，默认是第一个非本地循环接口;

　　-f: -f <capture filter> 设置抓包过滤表达式，遵循libpcap过滤语法，这个实在抓包的过程中过滤，如果是分析本地文件则用不到。

　　-s: -s <snaplen> 设置快照长度，用来读取完整的数据包，因为网络中传输有65535的限制，值0代表快照长度65535，默认也是这个值；
　　-p: 以非混合模式工作，即只关心和本机有关的流量。
　　-B: -B <buffer size> 设置缓冲区的大小，只对windows生效，默认是2M;
　　-y: -y<link type> 设置抓包的数据链路层协议，不设置则默认为-L找到的第一个协议，局域网一般是EN10MB等;
　　-D: 打印接口的列表并退出;
　　-L 列出本机支持的数据链路层协议，供-y参数使用。
```

捕获停止选项:
```
　　-c: -c <packet count> 捕获n个包之后结束，默认捕获无限个;
　　-a: -a <autostop cond.> … duration:NUM，在num秒之后停止捕获;
　　　　　　　　　　　　　　　　　　 filesize:NUM，在numKB之后停止捕获;
　　　　　　　　　　　　　　　　　 files:NUM，在捕获num个文件之后停止捕获;
```

捕获输出选项:
```
　　-b <ringbuffer opt.> … ring buffer的文件名由-w参数决定,-b参数采用test:value的形式书写;
　　　　　　　　　　　　　　　　 duration:NUM – 在NUM秒之后切换到下一个文件;
　　　　　　　　　　　　　　　　 filesize:NUM – 在NUM KB之后切换到下一个文件;
　　　　　　　　　　　　　　　　 files:NUM – 形成环形缓冲，在NUM文件达到之后;
```

RPCAP选项:
```
　　remote packet capture protocol，远程抓包协议进行抓包；
　　-A: -A <user>:<password>,使用RPCAP密码进行认证;

输入文件:
　　-r: -r <infile> 设置读取本地文件
```

处理选项:
```
　　-2: 执行两次分析
　　-R: -R <read filter>,包的读取过滤器，可以在wireshark的filter语法上查看；在wireshark的视图->过滤器视图，在这一栏点击表达式，就会列出来对所有协议的支持。
　　-Y: -Y <display filter>,使用读取过滤器的语法，在单次分析中可以代替-R选项;
　　-n: 禁止所有地址名字解析（默认为允许所有）
　　-N: 启用某一层的地址名字解析。“m”代表MAC层，“n”代表网络层，“t”代表传输层，“C”代表当前异步DNS查找。如果-n和-N参数同时存在，-n将被忽略。如果-n和-N参数都不写，则默认打开所有地址名字解析。

　　-d: 将指定的数据按有关协议解包输出,如要将tcp 8888端口的流量按http解包，应该写为“-d tcp.port==8888,http”;tshark -d. 可以列出所有支持的有效选择器。
```


输出选项:
```
　　-w: -w <outfile|-> 设置raw数据的输出文件。这个参数不设置，tshark将会把解码结果输出到stdout,“-w -”表示把raw输出到stdout。如果要把解码结果输出到文件，使用重定向“>”而不要-w参数。
　　-F: -F <output file type>,设置输出的文件格式，默认是.pcapng,使用tshark -F可列出所有支持的输出文件类型。
　　-V: 增加细节输出;
　　-O: -O <protocols>,只显示此选项指定的协议的详细信息。
　　-P: 即使将解码结果写入文件中，也打印包的概要信息；
　　-S: -S <separator> 行分割符
　　-x: 设置在解码输出结果中，每个packet后面以HEX dump的方式显示具体数据。
　　-T: -T pdml|ps|text|fields|psml,设置解码结果输出的格式，包括text,ps,psml和pdml，默认为text
　　-e: 如果-T fields选项指定，-e用来指定输出哪些字段;
　　-E: -E <fieldsoption>=<value>如果-T fields选项指定，使用-E来设置一些属性，比如
　　　　header=y|n
　　　　separator=/t|/s|<char>
　　　　occurrence=f|l|a
　　　　aggregator=,|/s|<char>
　　-t: -t a|ad|d|dd|e|r|u|ud 设置解码结果的时间格式。“ad”表示带日期的绝对时间，“a”表示不带日期的绝对时间，“r”表示从第一个包到现在的相对时间，“d”表示两个相邻包之间的增量时间（delta）。

　　-u: s|hms 格式化输出秒；
　　-l: 在输出每个包之后flush标准输出
　　-q: 结合-z选项进行使用，来进行统计分析；
　　-X: <key>:<value> 扩展项，lua_script、read_format，具体参见 man pages；
　　-z：统计选项，具体的参考文档;tshark -z help,可以列出，-z选项支持的统计方式。
```
　　
其他选项:
```
　　-h: 显示命令行帮助；
　　-v: 显示tshark 的版本信息;
```

## 附录二：Pyshark过滤参数


`a= pyshark.FileCapture(path,display_filter=’http’)`

其中a为pcap文件对象

主要过滤参数有

```
a.highest_layer	最高层的协议内容
a.http.request_method 请求方法
a.http.chat 访问方法加路径
a.http.request_uri 访问路径
a.http.host	访问host地址
a.http.field_names http包参数
a.ip.src_host	源ip地址
a.ip.dst_host	目的地址
a.http.request_full_uri url地址
str(c.http.file_data) 返回包内容
```