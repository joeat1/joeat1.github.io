---
layout: wiki
title: pytorch 基本使用
categories: wiki pytorch
description: pytorch 基本使用 错误汇总
keywords: pytorch
---

## 安装使用


## 常用函数

### 数据处理

+ torch.from_numpy(x) 将 Numpy 转为 Tensor 
+ x.numpy()   将 Tensor 转为 Numpy 
+ torch.transpose(input, dim0, dim1) 交换矩阵的两个维度

### 数据运算

+ torch.dot(x, y) 或 x.dot(y) 与 torch.mul(x, y) 或 x.mul(y)  点乘
+ torch.mm(x, y)  x.mm(y)   叉乘，如果input为（n x m）张量，则mat2为（m x p）张量，out将为（n x p）张量，但此功能不广播
+ torch.matmul(input, other, out=None) **广播的矩阵乘法**，如果两个张量都是一维的，则返回点积（标量），如果两个参数都是二维的，则返回矩阵矩阵乘积。如果两个自变量至少为一维且至少一个自变量为N维（其中N> 2），则返回批处理矩阵乘法。如果第一个参数是一维的，则在其维数之前添加一个1，以实现批量矩阵乘法并在其后删除。如果第二个参数为一维，则将1附加到其维上，以实现成批矩阵倍数的目的，然后将其删除。非矩阵（即批量）维度可以被广播（因此必须是可广播的）。例如，如果input为（jx1xnxm）张量，而other为（k×m×p）张量，out将是（j×k×n×p）张量。


## 模型与网络

+ torch.nn.Softmax(dim=-1) 将最后一个维度的数据进行 softmax 操作
+ torch.sigmoid() 与 torch.nn.Sigmoid() 元素操作
+ torch.nn.Linear(in_features, out_features, bias=True)

+ 提取某层的参数
    ```python
    output_1=model.fc1.bias.data
    output_1=model.fc1.weight.data
    ```
+ 获取模型参数的总数
    ```python
    model_parameters = filter(lambda p: p == 0, model.parameters())
    params = sum([np.prod(p.size()) for p in model_parameters])
    print(params)
    ```
