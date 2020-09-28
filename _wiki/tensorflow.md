---
layout: wiki
title: tensorflow 基本使用
categories: wiki tensorflow
description: tensorflow 基本使用 错误汇总
keywords: tensorflow
---

## 安装使用
+ TensorFlow 2 软件包 tensorflow：支持 CPU 和 GPU 的最新稳定版（适用于 Ubuntu 和 Windows）
+ 对于 1.15 （TensorFlow 1.x 的最终版本）及更早版本，CPU 和 GPU 软件包是分开的
    ```bash
    pip install tensorflow==1.15      # CPU
    pip install tensorflow-gpu==1.15  # GPU
    ```
+ 系统要求：Python 3.5-3.7 、 pip 19.0 或更高版本（需要 manylinux2010 支持）、 Ubuntu 16.04 或更高版本（64 位） 、 Windows 7 或更高版本（64 位）（仅支持 Python 3）、 GPU 支持需要使用支持 CUDA® 的显卡（适用于 Ubuntu 和 Windows）
+ 验证安装效果： `python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"` 

## 数据读取

### 常见数据格式

+ tf.int 、 tf.int1 、 tf.int32 、 tf.int64   整数系列
+ tf.float16 、 tf.float32 、 tf.float64 浮点数
+ tf.uint8 、 tf.uint16  无符号整数
+ tf.string 字符串
+ tf.bool   布尔型

tfrecord 格式数据

> 参考：https://www.cnblogs.com/king-lps/p/12736544.html
+ `tf.data.Dataset.from_tensor_slices` 可以将 numpy 、 list 、 pandas 、 dict 等数据类型封装转换为 Tensor 格式，生成的 `tf.data.Dataset` 还可以通过 `map`
 函数实现预处理，如图片加载、数据增强、标签one hot化等。
+ `tf.cast(labels, tf.int64)` 用于修改数据类型
  + 还可以将一些条件转换为数值形式 `tf.cast(a<2,dtype=tf.float32)` 
+ `tf.data.Dataset.zip` 用于合并数据
+ `print(dataset.__iter__())` 查看每一个迭代下的数据
+ 通过 `prefetch` 方法让模型的训练和每个batch数据集的加载并行
+ `tf.data.Dataset.from_generator(lambda: data_generator(arg1, arg2), output_types=((tf.int32, tf.int32)` 通过函数生成数据集，适用于数据规模较大的情况。但是这个数据生成方法常常容易出现问题，在解析数据的类型和投喂给模型时常常出现数据格式识别不了等情况。

+ 切分数据集
    ```python
    sample_num = x.shape[0]
    train_sample = int(sample_num * (1 - test_size - valid_size))  # 训练集的份数
    test_sample = int(sample_num * test_size)                      # 测试集测份数
    valid_train = int(sample_num * valid_size)    
    x_train, x_test, x_valid = tf.split(  
            x, # 准备切分的张量，不是数据集
            num_or_size_splits=[train_sample, test_sample, valid_train],
            axis=0
        )
    ```
+ 打乱并划分 batch 
    ```python
    def add_noise(x, y):
        x += np.random.uniform(0.0, 0.01)
        return x, y
    data = tf.data.Dataset.from_tensor_slices((x, y))    # 封装 dataset数据集格式
    data = data.map(add_noise)
    data_ = data.shuffle(        # 乱序
        buffer_size=x.shape[0],  # 官方文档说明 shuffle的buffer_size 必须大于或等于样本数量，过大会浪费内存空间，过小会导致打乱不充分。
    )
    data_ = data_.batch(batch_size)
    ```

## 模型训练

+ 冻结所有层
    ```python
    for layer in model.layers:
        layer.trainable = False
    ```
+ 利用 tensorboard 查看训练效果
    ```python
    log_dir = "logs/" + datatime.datatime.now().strftime("%Y%m%d-%H%M%S")
    tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=log_dir, histogram_freq=1)
    model.fit(dataset, callbacks=[tensorboard_callback])
    ```
    tensorboard --logdir=log_dir



## 错误汇总

+ 运行报错 ValueError: Graph disconnected: cannot obtain value for tensor Tensor

    + 检查是否所有的输入层输出层都写入到了 model 的 inputs 与 outputs 中，缺失某层信息也无法成功连接模型。
    + 可能由于不同层使用相同的名称而导致模型无法连接

+ Could not create cudnn handle: CUDNN_STATUS_INTERNAL_ERROR
    * 检查是否存在 cuda 和 cudnn 版本与 TF 不匹配，需要根据需求进行调整；
    * 如果仍然不正常，尝试在任意终端执行 `sudo rm -rf ~/.nv/` 后重启电脑；
    * 如果显存不够或者本来是够的但是使用不当而导致的问题，尝试降低程序的 batch_size 。
    * 也有可能是 RTX 显卡分配策略的问题，尝试在程序前添加以下代码，使 Tensorflow 2.x 可以实现 GPU 按需分配方式。 [参考](https://blog.csdn.net/ghy_111/article/details/86672450)
        ```python
        gpus = tf.config.experimental.list_physical_devices('GPU')
        for gpu in gpus:
            tf.config.experimental.set_memory_growth(gpu, True)
        ```
+ out of memory
    * 问题为 GPU 内存不够，使用 nvidia-smi 查看当前 GPU 使用情况
    * 如果此时 GPU 利用率不高，可能是刚运行的模型过于复杂而本地机器的显卡本身的内存不大，此时需要简化模型，或者替换 GPU 设备。
    * 如果此时 GPU 利用率很高，可能是其他脚本暂用了或者未成功释放大量的 GPU ，找出不需要再运行，或者未释放资源的程序对应的编号，利用 `kill -9 PID` 的方法强制结束进程。

+ ValueError: Failed to convert a NumPy array to a Tensor (Unsupported object type float)
    * 没有把训练样本和测试样本转化成 TF 可接受的数据类型。一般需要转换为浮点类型，如 `trainX = trainX.astype('float64')` 如果还是无法执行可能存在 object 等不能被 TF 所支持的数据类型，可以通过 `trainX.dtypes` 查看

+ Anaconda3\lib\site-packages\tensorflow\python\keras\engine\data_adapter.py:1356 unpack_x_y_sample_weight  raise ValueError("Data not understood.")
    * 源于 `x, y, sample_weight = data_adapter.unpack_x_y_sample_weight(data)` 无法正确地将 x 与 y 分开，导致数据无法被程序理解
    * 在利用 `tf.data.Dataset` 时，最后提供给 model 的数据需要为 x, y 的形式， x 可以为 list 或者 tuple 。

