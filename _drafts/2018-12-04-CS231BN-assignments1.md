---
layout: post
title:  CS231N assignments1
categories: CS231N
tags:  CS231N
author: ccpocker
---

* content
{:toc}

## 综述
这一节的作业，主要是在于介绍几个基本的算法:kNN、SVM、SoftMax、two-layer network(fully-connected network)。

### kNN
kNN是一个相对比较简单的算法。是从最近邻法的思想得出来的，最近邻法是判断testcase与样本之间谁的距离（范数）最近，就将testcase判成最近的样本的类别。而KNN，它的思路是：如果一个样本在特征空间中的k个最相似(即特征空间中最邻近)的样本中的大多数属于某一个类别，则该样本也属于这个类别，其中K通常是不大于20的整数。

如何计算距离，实际上就是定义好矩阵的范数:如L1范数(曼哈顿距离)，L2范数(欧氏距离)等等。

其算法的描述为：
1）计算测试数据与各个训练数据之间的距离；

2）按照距离的递增关系进行排序；

3）选取距离最小的K个点；

4）确定前K个点所在类别的出现频率；

5）返回前K个点中出现频率最高的类别作为测试数据的预测分类。

### kNN代码
CS231N的kNN代码是定义了个类：
```python
class KNearestNeighbor
def __init__(self):
    pass
def train(self, X, y):#训练就是存储所有的训练数据样本
    self.X_train = X  #训练样本
    self.y_train = y  #训练样本类别
```

第一个要完成的代码就是实现计算距离、分别使用两重循环、一重循环、以及不用循环来实现距离计算。

使用两重循环相对比较简单
```python
def compute_distances_two_loops(self, X):
    num_test = X.shape[0]#测试样本数量
    num_train = self.X_train.shape[0]#训练样本数量
    #dists[i,j]表示X[j]与X_train[j]之间的L2距离
    dists = np.zeros((num_test, num_train))
    for i in range(num_test):
      for j in range(num_train):
        #计算L2距离
        dists[i,j]=np.sqrt(np.sum((X[i,:]-self.X_train[j,:])**2))
    return dists
```
得到测试样本与训练样本距离的numpy数组后，然后就可以进行预测测试样本的标签:
```python
def predict_labels(self, dists, k=1):
    num_test = dists.shape[0]
    y_pred = np.zeros(num_test)
    for i in range(num_test):
        closest_y=[]
        #前k个对应的self.X_train[]
        index=np.argsort(dists[i])[:k] 
        #对应的标签
        closest_y=self.y_train[index]
        #使用np.bincount记录对应的次数
        count=np.bincount(closest_y)
        #返回最大次数对应的标签
        y_pred[i]=np.argmax(count)
    return y_pred
```








```python
#使用一重循环的话
def compute_distances_one_loop(self, X):
    num_test = X.shape[0]
    num_train = self.X_train.shape[0]
    dists = np.zeros((num_test, num_train))
    for i in range(num_test):
        dists[i,:]=np.sqrt(np.sum((X[i,:]-self.X_train)**2,axis=1))
    return dists

#不使用循环，全部由向量
# 根据公式:(x-y)**2=x**2-2*x*y+y**2
def compute_distances_no_loops(self, X):
    num_test = X.shape[0]
    num_train = self.X_train.shape[0]
    dists = np.zeros((num_test, num_train)) 

    #计算X^2
    X_square_sum=np.sum(X**2,axis=1)
    #计算self.X_train^2
    Xtrain_square_sum=np.sum(self.X_train**2,axis=1)
    #计算2*X*X_train
    X_AND_Xtrain=np.dot(X,self.X_train.T)
    #距离dists
    dists=np.sqrt(X_square_sum[:,np.newaxis]+Xtrain_square_sum-2*X_AND_Xtrain)
    return dists
```
三种方式用时如图所示：


#### kNN的优点与缺点
Pros：
* kNN不用训练，容易实现并且

Cons：
* kNN由于不要训练，每次测试都需要与所有训练数据进行计算，所以耗费时间相当巨大。
* The classifier must remember all of the training data and store it for future comparisons with the test data. This is space inefficient because datasets may easily be gigabytes in size.
* Classifying a test image is expensive since it requires a comparison to all training images.
### SVM




### Appendix

* [numpy.argsort](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.argsort.html#numpy.argsort)
```python
numpy.argsort(a, axis=-1, kind='quicksort', order=None)
#example
>>> x = np.array([3, 1, 2])
>>> np.argsort(x)
array([1, 2, 0])
```

* [numpy.bincount](https://docs.scipy.org/doc/numpy/reference/generated/numpy.bincount.html)
```python
numpy.bincount(x, weights=None, minlength=0)
#The input array needs to be of integer dtype, otherwise a TypeError is raised:
>>> np.bincount(np.arange(5))
array([1, 1, 1, 1, 1])
>>> np.bincount(np.array([0, 1, 1, 3, 2, 1, 7]))
array([1, 3, 1, 1, 0, 0, 0, 1])
```

