### numpy库的基本用法

```
# -*- coding:utf-8 -*-

"""
@Author:        clhon@qq.com
@Create Date:   2018-04-04
@Update Data:   2018-04-04
@Version:       V0.1.20180404
"""

import sys
import time
import random
import logging
import datetime
import numpy as np
import matplotlib.pyplot as plt


LOGGING_LEVEL = logging.DEBUG


def logging_config(logging_level):
    # log_format = "[line: %(lineno)d] - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[File: %(filename)s line: %(lineno)d] - %(levelname)s - %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


# http://codingpy.com/article/an-introduction-to-numpy/
def numpy_init():
    np_array = np.array([[11, 12, 13, 14, 15],
                         [16, 17, 18, 19, 20],
                         [21, 22, 23, 24, 25],
                         [26, 27, 28, 29, 30],
                         [31, 32, 33, 34, 35]])

    # randint include the last number, and randrange is not include the last number
    # x = random.randint(0, np_array.shape[0])
    # y = random.randint(0, np_array.shape[1])
    x = random.randrange(0, np_array.shape[0])
    y = random.randrange(0, np_array.shape[1])

    '''
np_array[4, 2]: 33
np_array: 
[[11 12 13 14 15]
 [16 17 18 19 20]
 [21 22 23 24 25]
 [26 27 28 29 30]
 [31 32 33 34 35]]
np_array.dtype = int64
np_array.shape = (5, 5)
np_array.base = None
np_array.ctypes = <numpy.core._internal._ctypes object at 0x7f4b1feed518>
np_array.data = <memory at 0x7f4b1cd56c18>
np_array.flags = 
  C_CONTIGUOUS : True
  F_CONTIGUOUS : False
  OWNDATA : True
  WRITEABLE : True
  ALIGNED : True
  UPDATEIFCOPY : False
np_array.flat = <numpy.flatiter object at 0x1875e90>
np_array.imag = 
[[0 0 0 0 0]
 [0 0 0 0 0]
 [0 0 0 0 0]
 [0 0 0 0 0]
 [0 0 0 0 0]]
np_array.itemsize = 8
np_array.nbytes = 200
np_array.ndim = 2
np_array.real = 
[[11 12 13 14 15]
 [16 17 18 19 20]
 [21 22 23 24 25]
 [26 27 28 29 30]
 [31 32 33 34 35]]
np_array.size = 25
np_array.strides = (40, 8)
np_array.T = 
[[11 16 21 26 31]
 [12 17 22 27 32]
 [13 18 23 28 33]
 [14 19 24 29 34]
 [15 20 25 30 35]]
 
如你所看，在上边的代码中 NumPy 的数组其实被称为 ndarray。我不知道为什么它被称为 ndarray，如果有人知道请在下边留言！
我猜测它是表示 n 维数组（n dimensional array）。

数组的形状（shape）是指它有多少行和列，上边的数组有五行五列，所以他的形状是（5，5）。

'itemsize' 属性是每一个条目所占的字节。这个数组的数据类型是 int64，一个 int64 的大小是 64 比特，8 比特为 1 字节，
64 除以 8 就得到了它的字节数，8 字节。

'ndim' 属性是指数组有多少维。这个数组有二维。但是，比如说向量，只有一维。

'nbytes' 属性表示这个数组中所有元素占用的字节数。你应该注意，这个数值并没有把额外的空间计算进去，
因此实际上这个数组占用的空间会比这个值大点。
    '''

    print('np_array[{}, {}]: {}'.format(x, y, np_array[x, y]))
    print('np_array: \n{}'.format(np_array))
    print('np_array.dtype = {}'.format(np_array.dtype))
    print('np_array.shape = {}'.format(np_array.shape))
    print('np_array.base = {}'.format(np_array.base))
    print('np_array.ctypes = {}'.format(np_array.ctypes))
    print('np_array.data = {}'.format(np_array.data))
    print('np_array.flags = \n{}'.format(np_array.flags))
    print('np_array.flat = {}'.format(np_array.flat))
    print('np_array.imag = \n{}'.format(np_array.imag))
    print('np_array.itemsize = {}'.format(np_array.itemsize))
    print('np_array.nbytes = {}'.format(np_array.nbytes))
    print('np_array.ndim = {}'.format(np_array.ndim))
    print('np_array.real = \n{}'.format(np_array.real))
    print('np_array.size = {}'.format(np_array.size))
    print('np_array.strides = {}'.format(np_array.strides))
    print('np_array.T = \n{}'.format(np_array.T))

    '''
a = [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24]
a = 
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]
 [15 16 17 18 19]
 [20 21 22 23 24]]
b = [10 62  1 14  2 56 79  2  1 45  4 92  5 55 63 43 35  6 53 24 56  3 56 44 78]
b = 
[[10 62  1 14  2]
 [56 79  2  1 45]
 [ 4 92  5 55 63]
 [43 35  6 53 24]
 [56  3 56 44 78]]
a + b = 
[[ 10  63   3  17   6]
 [ 61  85   9   9  54]
 [ 14 103  17  68  77]
 [ 58  51  23  71  43]
 [ 76  24  78  67 102]]
a - b = 
[[-10 -61   1 -11   2]
 [-51 -73   5   7 -36]
 [  6 -81   7 -42 -49]
 [-28 -19  11 -35  -5]
 [-36  18 -34 -21 -54]]
a * b = 
[[   0   62    2   42    8]
 [ 280  474   14    8  405]
 [  40 1012   60  715  882]
 [ 645  560  102  954  456]
 [1120   63 1232 1012 1872]]
a / b = 
[[ 0.          0.01612903  2.          0.21428571  2.        ]
 [ 0.08928571  0.07594937  3.5         8.          0.2       ]
 [ 2.5         0.11956522  2.4         0.23636364  0.22222222]
 [ 0.34883721  0.45714286  2.83333333  0.33962264  0.79166667]
 [ 0.35714286  7.          0.39285714  0.52272727  0.30769231]]
a ** 2 = 
[[  0   1   4   9  16]
 [ 25  36  49  64  81]
 [100 121 144 169 196]
 [225 256 289 324 361]
 [400 441 484 529 576]]
a < b = 
[[ True  True False  True False]
 [ True  True False False  True]
 [False  True False  True  True]
 [ True  True False  True  True]
 [ True False  True  True  True]]
a > b = 
[[False False  True False  True]
 [False False  True  True False]
 [ True False  True False False]
 [False False  True False False]
 [False  True False False False]]
a = 
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]
 [15 16 17 18 19]
 [20 21 22 23 24]]
b = 
[[10 62  1 14  2]
 [56 79  2  1 45]
 [ 4 92  5 55 63]
 [43 35  6 53 24]
 [56  3 56 44 78]]
a.dot(b) = 
[[ 417  380  254  446  555]
 [1262 1735  604 1281 1615]
 [2107 3090  954 2116 2675]
 [2952 4445 1304 2951 3735]
 [3797 5800 1654 3786 4795]]
 
除了 dot() 之外，这些操作符都是对数组进行逐元素运算。比如 (a, b, c) + (d, e, f) 的结果就是 (a+d, b+e, c+f)。
它将分别对每一个元素进行配对，然后对它们进行运算。它返回的结果是一个数组。注意，当使用逻辑运算符比如 “<” 和 “>” 的时候，
返回的将是一个布尔型数组，这点有一个很好的用处，后边我们会提到。

dot() 函数计算两个数组的点积。它返回的是一个标量（只有大小没有方向的一个值）而不是数组。

https://wizardforcel.gitbooks.io/ts-numpy-tut/content/22.html
import numpy.matlib 
import numpy as np 

a = np.array([[1,2],[3,4]]) 
b = np.array([[11,12],[13,14]]) 
np.dot(a,b)

输出如下：

[[37  40] 
 [85  92]]

要注意点积计算为：

[[1*11+2*13, 1*12+2*14],[3*11+4*13, 3*12+4*14]]
    '''
    a = np.arange(25)
    print('\n\na = {}'.format(a))
    a = a.reshape((5, 5))
    print('a = \n{}'.format(a))

    b = np.array([10, 62, 1, 14, 2, 56, 79, 2, 1, 45,
                  4, 92, 5, 55, 63, 43, 35, 6, 53, 24,
                  56, 3, 56, 44, 78])
    print('b = {}'.format(b))
    b = b.reshape((5, 5))
    print('b = \n{}'.format(b))

    print('a + b = \n{}'.format(a + b))
    print('a - b = \n{}'.format(a - b))
    print('a * b = \n{}'.format(a * b))
    print('a / b = \n{}'.format(a / b))
    print('a ** 2 = \n{}'.format(a ** 2))
    print('a < b = \n{}'.format(a < b))
    print('a > b = \n{}'.format(a > b))

    print('a = \n{}'.format(a))
    print('b = \n{}'.format(b))
    print('a.dot(b) = \n{}'.format(a.dot(b)))

    '''
a = 
[0 1 2 3 4 5 6 7 8 9]
a.sum = 45
a.min = 0
a.max = 9
a.cumsum = [ 0  1  3  6 10 15 21 28 36 45]

很明显就能看出 sum()、min() 和 max() 函数的功能：将所有元素加起来，找到最小值和最大值。

cumsum() 函数就不是那么明显了。它像 sum() 那样把所有元素加起来，但是它的实现方式是，第一个元素加到第二个元素上，把结果保存到一个列表里，
然后把结果加到第三个元素上，再保存到列表里，依次累加。当遍历完数组中所有元素则结束，返回值为运行数组的总和的列表。
    '''
    a = np.arange(10)
    print('a = \n{}'.format(a))
    print('a.sum = {}'.format(a.sum()))
    print('a.min = {}'.format(a.min()))
    print('a.max = {}'.format(a.max()))
    print('a.cumsum = {}'.format(a.cumsum()))

    '''
a = 
[ 0 10 20 30 40 50 60 70 80 90]
a = 
[ 0 10 20 30 40 50 60 70 80 90]
b = 
[10 50 90]

如你所见，上边的例子中，我们用想获取的索引的序列作为索引。它返回了我们索引的元素。
    '''
    a = np.arange(0, 100, 10)
    print('a = \n{}'.format(a))
    indices = [1, 5, -1]  # 获取第一个元素，第五个元素和倒数第一个元素
    b = a[indices]
    print('a = \n{}'.format(a))  # >>>[ 0 10 20 30 40 50 60 70 80 90]
    print('b = \n{}'.format(b))  # >>>[10 50 90]

    '''
a = 
[ 0 10 20 30 40 50 60 70 80 90]
b = 
[ 0 10 20 30 40]
c = 
[50 60 70 80 90]
d = 
[[ 0 10 20 30 40]
 [50 60 70 80 90]]
e = 
[[ 0 10 20 30 40]]

缺省索引是从多维数组的第一维获取索引和切片便捷方法。例如，你有一个数组 a = [[1, 2, 3, 4, 5], [6, 7, 8, 9, 10]]，
那么 a[3] 将会返回数组第一维中索引值为 3 的元素，这里的结果是 4。
    '''
    # Incomplete Indexing
    a = np.arange(0, 100, 10)
    b = a[:5]
    c = a[a >= 50]
    d = a.reshape((2, int(a.size / 2)))
    e = d[:1]
    print('a = \n{}'.format(a))  # >>>[ 0 10 20 30 40 50 60 70 80 90]
    print('b = \n{}'.format(b))  # >>>[ 0 10 20 30 40]
    print('c = \n{}'.format(c))  # >>>[50 60 70 80 90]
    print('d = \n{}'.format(d))
    print('e = \n{}'.format(e))  # >>>[[ 0 10 20 30 40]]

    '''
a = 
[ 0 10 20 30 40 50 60 70 80 90]
b = 
(array([0, 1, 2, 3, 4]),)
c = 
[5 6 7 8 9]

where() 函数是另外一个根据条件返回数组中的值的有效方法。只需要把条件传递给它，它就会返回一个使得条件为真的元素的列表。
    '''
    # Where
    a = np.arange(0, 100, 10)
    b = np.where(a < 50)
    c = np.where(a >= 50)[0]
    print('a = \n{}'.format(a))
    print('b = \n{}'.format(b))  # >>>(array([0, 1, 2, 3, 4]),)
    print('c = \n{}'.format(c))  # >>>[5 6 7 8 9]

    '''
我们的数组是: 
[[ 0.  1.  2.]
 [ 3.  4.  5.]
 [ 6.  7.  8.]]
大于“3”的元素的索引: (array([1, 1, 2, 2, 2]), array([1, 2, 0, 1, 2]))
使用这些索引来获取满足条件的元素: [ 4.  5.  6.  7.  8.]
    '''
    x = np.arange(9.).reshape(3, 3)
    print('我们的数组是: \n{}'.format(x))
    y = np.where(x > 3)
    print('大于“3”的元素的索引: {}'.format(y))
    print('使用这些索引来获取满足条件的元素: {}'.format(x[y]))

    '''
a = 
[ 0.          0.12822827  0.25645654  0.38468481  0.51291309  0.64114136
  0.76936963  0.8975979   1.02582617  1.15405444  1.28228272  1.41051099
  1.53873926  1.66696753  1.7951958   1.92342407  2.05165235  2.17988062
  2.30810889  2.43633716  2.56456543  2.6927937   2.82102197  2.94925025
  3.07747852  3.20570679  3.33393506  3.46216333  3.5903916   3.71861988
  3.84684815  3.97507642  4.10330469  4.23153296  4.35976123  4.48798951
  4.61621778  4.74444605  4.87267432  5.00090259  5.12913086  5.25735913
  5.38558741  5.51381568  5.64204395  5.77027222  5.89850049  6.02672876
  6.15495704  6.28318531]
b = 
[  0.00000000e+00   1.27877162e-01   2.53654584e-01   3.75267005e-01
   4.90717552e-01   5.98110530e-01   6.95682551e-01   7.81831482e-01
   8.55142763e-01   9.14412623e-01   9.58667853e-01   9.87181783e-01
   9.99486216e-01   9.95379113e-01   9.74927912e-01   9.38468422e-01
   8.86599306e-01   8.20172255e-01   7.40277997e-01   6.48228395e-01
   5.45534901e-01   4.33883739e-01   3.15108218e-01   1.91158629e-01
   6.40702200e-02  -6.40702200e-02  -1.91158629e-01  -3.15108218e-01
  -4.33883739e-01  -5.45534901e-01  -6.48228395e-01  -7.40277997e-01
  -8.20172255e-01  -8.86599306e-01  -9.38468422e-01  -9.74927912e-01
  -9.95379113e-01  -9.99486216e-01  -9.87181783e-01  -9.58667853e-01
  -9.14412623e-01  -8.55142763e-01  -7.81831482e-01  -6.95682551e-01
  -5.98110530e-01  -4.90717552e-01  -3.75267005e-01  -2.53654584e-01
  -1.27877162e-01  -2.44929360e-16]
np.pi = 3.141592653589793
s
下边的代码展示了实现布尔屏蔽。你需要做的就是传递给数组一个与它有关的条件式，然后它就会返回给定条件下为真的值。
我们用条件式选择了图中不同的点。蓝色的点（也包含图中的绿点，只是绿点覆盖了蓝点），显示的是值大于零的点。
绿点显示的是值大于 0 小于 Pi / 2 的点。
    '''
    a = np.linspace(0, 2 * np.pi, 50)
    b = np.sin(a)
    print('a = \n{}'.format(a))
    print('b = \n{}'.format(b))
    print('np.pi = {}'.format(np.pi))
    plt.plot(a, b)
    mask = b >= 0
    plt.plot(a[mask], b[mask], 'bo')
    mask = (b >= 0) & (a <= np.pi / 2)
    plt.plot(a[mask], b[mask], 'go')
    plt.show()


def main(argv):
    logging_config(LOGGING_LEVEL)
    numpy_init()


if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()

    main(sys.argv)

    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))

```



