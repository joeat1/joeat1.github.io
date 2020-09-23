---
layout: wiki
title: numpy 基本使用
categories: wiki numpy
description: numpy 基本使用 错误汇总
keywords: numpy
---


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