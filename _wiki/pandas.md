---
layout: wiki
title: pandas 基本使用
categories: wiki pandas
description: pandas 基本使用 错误汇总
keywords: pandas
---


## 数据处理

+ pandas.DataFrame.sample 随机选取若干行。  df.sample(n=3,random_state=1) 提取3行数据列，使用random_state，以确保可重复性使用。

+ [Pandas中DataFrame基本函数整理（全）](https://blog.csdn.net/brucewong0516/article/details/81782312
)

+ 同时处理不同列的数据
    ```python
    def my_test(a, b):
        return a + b
    
    df['label'] = df.apply(lambda row: my_test(row['a'], row['b']), axis=1)
    print(df)
    ```