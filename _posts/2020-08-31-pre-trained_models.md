---
layout: post
title: 论文笔记：Pre-trained Models for Natural Language Processing
categories: paper-notes nlp
description: Pre-trained Models for Natural Language Processing
keywords: paper-notes nlp PTMs
---

> [Pre-trained Models for Natural Language Processing](http://arxiv.org/abs/2003.08271)
> 参考： https://zhuanlan.zhihu.com/p/144927093

## 动机


## 主要贡献
+ 对 NLP 的预训练模型的背景知识、模型架构、预训练任务、扩展方式、迁移方法和实际应用进行了全面的介绍。
+ 对预训练模型从语言表示方法，模型架构，预训练任务和拓展方式四个不同的角度进行了系统的分类。
+ 包含了 PTMs 相关的丰富资源和研究方向的分析，读者可以快速找到相关领域所需要的信息。

## 主要内容

+ CNN 、 RNN 与 transformer的对比：[参考](https://zhuanlan.zhihu.com/p/54743941)
    + RNN 本身结构就是可以接纳 NLP 领域不定长输入的由前向后进行信息线性传导的网络结构，虽然存在线性序列结构在反向传播的时候存在优化困难问题（梯度消失或梯度爆炸），但 LSTM 和 GRU 模型，通过增加中间状态信息直接向后传播，以此缓解梯度消失问题，获得了很好的效果对于捕获长距离特征也是非常有效的。 RNN 需要按照输入序列逐步计算，在结构上是无法并行计算的。
    + CNN 通过卷积核和滑动窗口覆盖句子片段，对于长距离特征较难获取。现有的部分技巧如 跳跃覆盖（Dilated CNN），并且为了避免 pooling 层丢失位置信息，目前 NLP 领域的 CNN 的一个发展趋势是抛弃Pooling层。另外带有门限机制的 CNN 也有一定的性能提升，如 Gated CNN ，设计了 Gated Linear Units (GLU) 对输入实现门控功能，同时也能较好地减轻梯度弥散。
    + Transformer 每个 Block 里包括 多头（Multi-head）、 自注意力机制（self attention）、 跳跃连接（Skip connection）、归一化层（LayerNorm），前馈神经网络（Feed Forward） 几个基本结构
    + 能力对比：
        + 语义特征提取能力； Transformer >> CNN >> RNN 
        + 长距离特征捕获能力； Transformer >> RNN >> CNN 
        + 任务综合特征抽取能力； Transformer >> CNN >> RNN 
        + 并行计算能力及运行效率. Transformer和CNN差不多，都远强于RNN。
        + 单层的计算量： Transformer Block > CNN > RNN 
    + 一种改造 RNN 与 CNN 的方式就是将其变体替换 transformer 中的 self attention 结构，与原始的 RNN 与 CNN 性能有所提高。

+ 知识拓展
    + 把“知识图谱”加入Bert的模型中 
    + 百度 2019 年提出的 ERNIE 模型，将知识图谱信息在第一阶段直接编码进了预训练模型。考虑到 bert 对中文是基于单字处理，并不会学到短语或实体的语义；但是相比字输入，使用词输入对分词质量要求高，并且存在OOV问题和过大词规模的问题。百度在其模型中采取的是字输入，但是 Mask 使用单词的方式。
    + 