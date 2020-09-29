---
layout: wiki
title: pytorch 基本使用
categories: wiki pytorch
description: pytorch 基本使用 错误汇总
keywords: pytorch
---

## 优势特性

+ 类似 numpy 的多种张量计算函数，并能使用强大的 GPU 加速
+ 构建在 Python 之上，基于 autograd 系统的深度神经网络，并且采用动态图框架，比较方便 debug 和编写。

+ Variable 是对 tensor 的封装，操作和 tensor 一样，但包含了三个属性用于构建神经网络。 Variable 中的 tensor本身.data，对应 tensor 的梯度.grad以及这个 Variable 是通过什么方式得到的.grad_fn。这些属性可以在 Variable 发现传播（`backward()` 时自动求导。如何对一个向量或者矩阵自动求导，需要往backward()中传入一个参数和反向传播的数值维度一样大，其中的数值用于标记各个元素分量求导后的权重。多次自动求导需要手动设置 `retain_graph=True` 保留计算图。

```python
from torch.autograd import Variable
x_tensor = torch.randn(10, 5)
x = Variable(x_tensor, requires_grad=True) # 默认 Variable 是不需要求梯度的，所以我们用这个方式申明需要对其进行求梯度
y = x + x
y.backward()
print(x.grad)
```

+ Parameter 这个本质上和 Variable 是一样的，只不过其默认是要求梯度的，而 Variable 默认是不求梯度的。 PyTorch 中的优化器 torch.optim 就是 Parameter 。
```python
optimizer = torch.optim.SGD( lr=1.)
optimizer.zero_grad() # 使用优化器将梯度归 0
loss.backward()
optimizer.step() # 使用优化器来更新参数
```

## 安装使用

相比 tensorflow 而言，其安装比较方便，参考[官方链接](https://pytorch.org/get-started/locally/)可以快速了解。原始版本也可以在[链接](https://pytorch.org/get-started/previous-versions/)中找到内容。
使用 conda 进行安装： `conda install pytorch torchvision cudatoolkit=10.2 -c pytorch`  此处 cudatoolkit 需要根据系统的版本特点进行设定，在此之前需要 **安装好 GPU 相关驱动程序** （CUDA 与 CUDNN）。

验证安装的 pytorch 是否支持 CUDA ，以及对应的 CUDA 和 cuDNN 版本
```python
import torch
print(torch.__version__)
print(torch.cuda.is_available())
print(torch.version.cuda)
print(torch.backends.cudnn.version())
```

## 数据读取

+ Dataset ： 非常方便的定义一个数据读入，同时也能够方便的定义数据预处理
```python
from torch.utils.data import Dataset
# 定义一个子类叫 custom_dataset，继承与 Dataset
class custom_dataset(Dataset):
    def __init__(self, txt_path, transform=None):
        self.transform = transform # 传入数据预处理
        with open(txt_path, 'r') as f:
            lines = f.readlines()
        
        self.img_list = [i.split()[0] for i in lines] # 得到所有的图像名字
        self.label_list = [i.split()[1] for i in lines] # 得到所有的 label 

    def __getitem__(self, idx): # 根据 idx 取出其中一个
        img = self.img_list[idx]
        label = self.label_list[idx]
        if self.transform is not None:
            img = self.transform(img)
        return img, label

    def __len__(self): # 总数据的多少
        return len(self.label_list)
```
+ DataLoader ： 加载数据
```python
from torch.utils.data import DataLoader
train_data1 = DataLoader(folder_set, batch_size=2, shuffle=True) # 将 2 个数据作为一个 batch
im, label = next(iter(train_data1)) # 使用这种方式访问迭代器中第一个 batch 的数据
```


## 常用函数

> 大量函数包含两种方式，一种为 x_tensor.func(args) ，一种是 torch.func(x_tensor, args)

### 数据处理

数据生成

+ x = torch.randn(3, 2) 随机生成 
+ x = torch.ones(2, 2)  这是一个 全是 1 的 float tensor 。
+ mask = y_pred.ge(0.5).float() 大于 0.5 的为 1，小于的设为 0 。

数据类型转换

+ torch.from_numpy(numpy_tensor) 或者 torch.Tensor(numpy_tensor) 可以将 Numpy 转为 Tensor 
+ x_tensor.numpy()   将 Tensor 转为 Numpy ，如果 tensor 在 gpu 上，需要先复制到cpu上，之后再转换： x.cpu().numpy() 
+ x.cuda() 将 tensor 放到 GPU 上，如果有多个 GPU ，则添加参数，如 cuda(0) 表示 加到第一个 GPU 。
+ x_tensor.type() 得到 tensor 的数据类型，如果添加参数，则可以设定张量为对应类型。
+ x = x.float() 类型转为 float 

数据形态特点

+ x_tensor.shape() 通过 shape 和 size 方法都可以获取 tensor 的大小
+ x_tensor.dim() 得到 tensor 的维度，即 shape 的 len 。
+ x_tensor.numel() # 得到 tensor 的所有元素个数 

+ x = x.squeeze() # 将 tensor 中所有的一维全部都去掉，如 [1,4,4] -> [4,4]
+ x = x.unsqueeze(1) # 在第二维增加一个维度且值为 1 ，如 [4,4] -> [4,1,4] ，在 numpy 格式的一维数据转为 tensor 之后一般需要使用 `.unsqueeze(1)` 使得可以被模型接受。
+ x = x.permute(1, 0, 2) # permute 可以重新排列 tensor 的维度，和 transpose 类似。
+ torch.transpose(input, dim0, dim1) 交换矩阵的两个维度
+ x = x.view(-1, 5) # -1 表示任意的大小，5 表示第二维变成 5，相当于将所有元素按照一定的规则重新排列。
+ torch.cat([x1, x2], dim=1)  dim=1 为水平拼接(每一个样本数据的特征维度增加)，dim=0 为竖直拼接（样本数据量增加）

### 数据运算

+ max_value, max_idx = torch.max(x, dim=1) 沿着行取最大值和对应的下标
+ sum_x = torch.sum(x, dim=1)  # 沿着行对 x 求和
+ z = x + y 或 z = torch.add(x, y) # 两个 tensor 求和。 pytorch中大多数的操作都支持 inplace 操作，也就是可以直接对 tensor 进行操作而不需要另外开辟内存空间，一般都是在操作的符号后面加 `_` ，比如 `transpose_` 。


+ torch.dot(x, y) 或 x.dot(y) 与 torch.mul(x, y) 或 x.mul(y)  点乘
+ torch.mm(x, y)  x.mm(y)   叉乘，如果input为（n x m）张量，则mat2为（m x p）张量，out将为（n x p）张量，但此功能不广播
+ torch.matmul(input, other, out=None) **广播的矩阵乘法**，如果两个张量都是一维的，则返回点积（标量），如果两个参数都是二维的，则返回矩阵矩阵乘积。如果两个自变量至少为一维且至少一个自变量为N维（其中N> 2），则返回批处理矩阵乘法。如果第一个参数是一维的，则在其维数之前添加一个1，以实现批量矩阵乘法并在其后删除。如果第二个参数为一维，则将1附加到其维上，以实现成批矩阵倍数的目的，然后将其删除。非矩阵（即批量）维度可以被广播（因此必须是可广播的）。例如，如果input为（jx1xnxm）张量，而other为（k×m×p）张量，out将是（j×k×n×p）张量。


```python
import torch.nn.functional as F
print(F.sigmoid(torch.mm(x, w) + b))
```

## 损失函数与评价指标
> https://pytorch.org/docs/1.5.0/nn.html#loss-functions

回归损失函数

+ 平均绝对误差 MAE (L1损失) `nn.L1Loss` 。 平均绝对误差（Mean Absolute Error,MAE) 是目标变量和预测变量之间绝对差值之和，其函数大部分情况下梯度都是相等的，求解效率低，这并不利于收敛，但是数值稳定，不会出现梯度爆炸的问题，并且对离群点不那么敏感，较为稳健。
+ 均方误差 MSE (L2 Loss) `nn.MSELoss` 。均方误差（Mean Square Error,MSE）是模型预测值与真实样本值之间差值平方的平均值。其函数曲线连续且处处可导，便于梯度下降，并且随着差值的减小，梯度也在减小，这有利于收敛。但平方运算会将大于 1 的差值放大，也就使得损失函数更关注偏离过大的值，对离群点反应敏感，从而可能为了拟合离群点而降低了整体的模型的性能。
+ Smooth L1 Loss  `nn.SmoothL1Loss` 
。结合 MAE 与 MSE 的优点，形成一个分段函数，但是为了函数处处可导，设置当 x<1 时， loss=0.5*x^2，其他时候， loss=|x|-0.5 。
    ```python
    def _smooth_l1_loss(input, target, reduction='none'):
        # type: (Tensor, Tensor) -> Tensor
        t = torch.abs(input - target)
        ret = torch.where(t < 1, 0.5 * t ** 2, t - 0.5)
        if reduction != 'none':
            ret = torch.mean(ret) if reduction == 'mean' else torch.sum(ret)
        return ret
    ```

分类损失函数


+ 交叉熵损失函数 `nn.CrossEntropyLoss` 。在 torch 中 CrossEntropyLoss 会做 softmax 操作，相当于 `log_softmax() + NLLLoss()` 

+ 负对数似然损失函数 `nn.NLLLoss` 

统计准确数量
num_correct = (pred == label).sum().item()


## 优化器

+ Adam
optimizer = torch.optim.Adam(net.parameters(), lr=1e-3)
+ SGD
optimzier = torch.optim.SGD(net.parameters(), 1e-2)



## 模型与网络

+ torch.nn.Softmax(dim=-1) 将最后一个维度的数据进行 softmax 操作
+ torch.sigmoid() 与 torch.nn.Sigmoid() 元素操作
+ torch.nn.Linear(in_features, out_features, bias=True)

+ embed = torch.nn.Embedding(n_vocabulary,embedding_size)  嵌入层
+ gru = torch.nn.GRU(input_size,hidden_size,n_layers)  GRU
+ torch.nn.Conv1d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True) in_channels 输入信号的通道

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


```python
from torch import nn
import torch.nn.functional as F
class net_name(nn.Module):
    def __init__(self):
        super(net_name, self).__init__()
        # 可以添加各种网络层
        self.conv1 = nn.Conv2d(3, 10, 3)
        # 具体每种层的参数可以去查看文档
        
    def forward(self, x):
        # 定义向前传播
        out = self.conv1(x)
        return out
```


## 参考

+ [深度学习入门之PyTorch]
(https://github.com/L1aoXingyu/code-of-learn-deep-learning-with-pytorch)
+ [pytorch 项目模板](https://github.com/victoresque/pytorch-template)