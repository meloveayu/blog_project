---
title: Python的numpy包学习
date: 2020-06-08 15:04:21
categories: 学习笔记
tags:
	- Python
	- Numpy
---

## 综述

Numpy是一个Python的数学库，支持很强大的数组运算，本次数据学习建模比赛里要用，记录一下学习经验。

<!--more-->

## Ndarray对象

这是Numpy包提供的一个n维数组对象，可以通过构造函数构造一个数组。

```python
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)
```

`object`部分是一个数组或者说是嵌套的数列，如[1,2,3,4,5,]，[[1,2],[3,4]]等都可以；`dtype`可选，表示数据类型；`copy`可选，表示对象是否需要复制；`order`表示创建数组的样式，C为行F为列默认为A表示任意方向；`subook`可选，返回一个与基类类型一致的数组，`ndmin`指定生成数组的最小维度。

个人感觉，`object`部分是最主要的，`dtype`指定数据类型，`ndmin`指定生成的维度。其他的相对用的不多。

## 数据类型（待补充）

## 数组属性

1. `xxx.ndim`：秩，就是数组的维度的数量，矩阵的表示用的一般都是多维数组来表示（等于ndmin）

2. `xxx.shape`：输出的是几行几列之类的

3. `xxx.size`：输出元素总个数，大概可以看成长*宽之类的

4. `xxx.dtype`：输出数据类型

5. `xxx.itemsize`：输出每个元素的大小，以字节为单位

6. `xxx.flags`：输出对象的内存信息（说实话看不懂）

   ![](https://cdn.jsdelivr.net/gh/meloveayu/blog_img@master/img/20200608222747.png)

7. `xxx.real/imag`：输出实部/虚部

## 创建数组

除了`ndarray`以外，还可以有以下的三种方法创建一个数组。

1. `numpy.empty(shape,dtype,order='C')`：建立一个用`shape`描述行列数的未初始化数组，`dtype`可选，`order`用C与F指定行还是列优先。
2. `numpy.zeros(shape,dtype,order='C')`：建立一个用0初始化的用`shape`描述行列数的数组，其他同上。
3. `numpy.ones(shape,dtype,order='C')`：建立一个用1初始化的数组，其他同上。

## 从已有数组创建数组

1. `numpy.asarray(a,dtype,order)`：和`ndarray`差不多，就是少了几个参数，可以根据a的输入完成像是根据列表或元组转换成ndarray类型等。

2. `numpy.frombuffer(buffer,dtype,count,offset)`：实现动态数组，`buffer`可任意对象以流形式读入，`count`默认为-1，表示读入全部数据，`offset`表示读取起始位置，默认为0。

   ```python
   import numpy as np
   s = b'Hello World'
   a = np.frombuffer(s, dtype='S1')
   print(a)
   ```

   结果是：

   ```python
   [b'H' b'e' b'l' b'l' b'o' b' ' b'W' b'o' b'r' b'l' b'd']
   ```

   这里string必须转成bytestring，加个b

3. `numpy.fromiter(iterable,dtype,count=-1)`：由可迭代对象建立ndarray对象，`iterable`是可迭代对象（啥？）

## 从数值范围创建数组

1. `numpy.arange(start,stop,step,dtype)`：创建一个[start,stop)的数组。`start`是开始点，默认为0；`stop`是结束点（不包括`stop`），`step`是步长，默认为1；`dtype`同上。

   ```python
   x = np.arange(5)
   print(x)
   # 结果是[0,1,2,3,4]，很明显，唯一一定要有的参数是stop
   ```

   

2. `numpy.linspace(start,stop,num=50,endpooint=True,retstep=False,dtype=None)`：生成一个知道首尾元素，知道元素个数的等差数组。`start`是起始，`stop`是终止，是否包含取决于`endpoint`，默认为True，包含，反之不包含；`num`是整个数组的元素个数，默认是50；`retstep`是输出步长，默认False。

   ```python
   import numpy as np
   a = np.linspace(1, 10, 10, retstep=True)
   print(a)
   #(array([ 1.,  2.,  3.,  4.,  5.,  6.,  7.,  8.,  9., 10.]), 1.0)
   ```

3. `numpy.logspace(start,stop,num=50,endpoint=True,base=10.0,dtype=None)`：生成一个等比数列，和生成等差数列的方法区别就在`base`上，默认为10，为对数log的底数，起始值是`base**start`，结束值是`base**stop`。 

## 切片、索引

1. 切片方法是使用`slice(start,stop,step)`来给一个量赋值，再将这个值传给ndarray对象的索引里：

   ```python
   a=slice(2,7,2)
   print(arr[a])
   # 输出的是索引2~6的步长为2的元素数组
   ```

2. 使用更多的是用冒号间隔开的一种方式，使用便利的多：

   ```python
   b=a[a:b:c]
   print(b)
   # 使用冒号隔开的三个元素分别代表起始端，结束端（不包括），步长
   # [a:]这种形式代表从索引a之后的全要，[:b]代表到索引b-1之前的全要
   # 多维数组同样适用，以数组内的元素为准
   # 还有加省略号的切片形式暂时没看懂
   ```

3. 高级索引部分：整数数组索引和花式索引。暂时有点难。参见https://www.runoob.com/numpy/numpy-advanced-indexing.html

## 广播

广播是Numpy针对形状不同的数组进行数值运算处理的方式。在形状相同（指的是内部元素类型一致而且数组的维数也一样的这种）的情况下，运算基本都是对应位置的元素进行运算即可。

但是遇到形状不同的数组，会自动触发广播机制。

草，没看懂……https://www.runoob.com/numpy/numpy-broadcast.html




