---
layout: wiki
title: numpy 基本使用
categories: wiki numpy
description: numpy 基本使用 错误汇总
keywords: numpy
---


## 数据处理

+ 数组拼接： np.concatenate((a,b,c),axis=0) 


## 数据生成

生成仿真数据： (x,y) 的点遵循均值为[1.4, 0]、协方差矩阵为[[0.3, 0], [0, 0.3]]的二维高斯分布。
```python
import numpy as np
np.random.seed(2)   # 设置种子参数后，随机得到的数值每次都是相同的
mean = [0, 0]
cov = [[0.5, 0], [0.5, 1]] 
mean2 = [1.4, 0]
cov2 = [[0.3, 0], [0, 0.3]] 
x, y = np.random.multivariate_normal(mean, cov, 100).T
x2, y2 = np.random.multivariate_normal(mean2, cov2, 20).T
```


## 问题

+ `ModuleNotFoundError: No module named numpy.random._pickle`
  + 使用的 numpy 版本较老或者版本不匹配，使得在其他包调用时存在问题。通过 `pip install numpy --upgrade` 来实现版本更新，或者在安装时直接指定 numpy 的版本号。
