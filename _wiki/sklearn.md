---
layout: wiki
title: sklearn 基本使用
categories: wiki sklearn
description: sklearn 基本使用 错误汇总
keywords: sklearn
---

## 归一化

> 归一化后加快了梯度下降求最优解的速度；归一化有可能提高精度。

+ min-max 标准化 (Min-max normalization) 
    * x ＝ (x - min)/(max - min)
    ```python
    from sklearn import preprocessing
    scaler = preprocessing.MinMaxScaler()
    x_train_minmax  = scaler.fit_transform(x_train)
    ```
+ z-score 0均值标准化(zero-mean normalization)
    * x = (x - u)/σ ，其中 u 为所有样本数据的均值，σ: 为所有样本数据的标准差。
    ```python
    from sklearn import preprocessing
    scaler = preprocessing.scale(X)
    x_scaler = scaler.fit_transform(x)
    ```
    除此之外还有另一个函数 StandardScaler 。使用该类的好处在于可以保存训练集中的参数（均值、方差）直接使用其对象转换测试集数据。
    ```python
    from sklearn import preprocessing
    scaler = preprocessing.StandardScaler()
    x_train_scaled  = scaler.fit_transform(x_train) # fit_transform 等同于 fit + transform 两个函数的结合
    x_test_scaled = scaler.transform(x_test)
    ```
+ 