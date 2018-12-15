title: 'Keras Intro : 基本模型保存'
author: Kitekii
date: 2018-10-21 22:06:16
tags:
---
## Sequential-Module模型
### 常用层
#### Dense全连接
```python
keras.layers.core.Dense(units,
						activation = None,
						use_bias = True,
						kernel_initializer = 'glorot_uniform', 
						bias_initializer = 'zeros',
						kernel_regularizer = None,
						bias_regularizer = None,
						activity_regularizer = None,
						kernel_constraint = None,
						bias_constraint = None)
```
Dense就是常用的全连接层，所实现的运算是`output = activation(dot(input, kernel)+bias)`。其中`activation`是逐元素计算的激活函数，`kernel`是本层的权值矩阵，`bias`为偏置向量，只有当`use_bias=True`才会添加。
参数说明
- units：大于0的整数，代表该层的输出**维度**。
- activation：激活函数，为预定义的激活函数名（参考激活函数），或逐元素（element-wise）的Theano函数。如果不指定该参数，将不会使用任何激活函数（即使用线性激活函数：a(x)=x）
- use_bias: 布尔值，是否使用**偏置项**
- kernel_initializer：权值初始化方法，为预定义初始化方法名的字符串，或用于初始化权重的初始化器。参考initializers
- bias_initializer：偏置向量初始化方法，为预定义初始化方法名的字符串，或用于初始化偏置向量的初始化器。参考initializers
- kernel_regularizer：施加在权重上的正则项，为Regularizer对象
- bias_regularizer：施加在偏置向量上的正则项，为Regularizer对象
- activity_regularizer：施加在输出上的正则项，为Regularizer对象
- kernel_constraints：施加在权重上的约束项，为Constraints对象
- bias_constraints：施加在偏置上的约束项，为Constraints对象