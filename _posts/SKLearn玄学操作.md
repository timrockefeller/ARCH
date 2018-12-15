title: SKLearn玄学操作
author: Kitekii
tags:
  - machine learning
  - coding
categories: []
date: 2018-10-20 16:54:00
---




### 分离数据集
[**train_test_split**](https://blog.csdn.net/kyriehe/article/details/77507473)是交叉验证中常用的函数，功能是从样本中随机的按比例选取train data和test data，形式为：
```python
X_train,X_test, y_train, y_test = cross_validation.train_test_split(train_data, train_target, test_size=0.4, random_state=0)
```
- train_data：所要划分的样本特征集 
- train_target：所要划分的样本结果 
- test_size：样本占比，如果是整数的话就是样本的数量 
- random_state：是随机数的种子。




### 引用
[ML神器：sklearn的快速使用](https://www.cnblogs.com/lianyingteng/p/7811126.html)

[OneHot编码知识点](https://blog.csdn.net/tengyuan93/article/details/78930285)

[机器学习零基础？手把手教你用TensorFlow搭建图像分类器](https://blog.csdn.net/o4dc8ojo7zl6/article/details/78431750)