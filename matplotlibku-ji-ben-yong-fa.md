### matplotlib库基本用法

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


# http://codingpy.com/article/a-quick-intro-to-matplotlib/
def matplotlib_init():
    """
下面的代码将画出一个简单的正弦曲线。np.linspace(0, 2 * np.pi, 50) 这段代码将会生成一个包含 50 个元素的数组，
这 50 个元素均匀的分布在 [0, 2pi] 的区间上。

plot 命令以一种简洁优雅的方式创建了图形。提醒一下，如果没有第一个参数 x，图形的 x 轴坐标将不再是 0 到 2pi，而应该是数组的索引范围。

最后一行代码 plt.show() 将图形显示出来，如果没有这行代码图像就不会显示。
    """
    # 简单的绘图
    x = np.linspace(0, 2 * np.pi, 50)
    plt.plot(x, np.sin(x))  # 如果没有第一个参数 x，图形的 x 坐标默认为数组的索引
    plt.show()  # 显示图形

    """
下面的代码同时绘制了表示函数 sin(x) 和 sin(2x) 的图形。这段代码和前面绘制一个数据集的代码几乎完全相同，
只有一点例外，这段代码在调用 plt.plot() 的时候多传入了一个数据集，并用逗号与第一个数据集分隔开。
    """
    x = np.linspace(0, 2 * np.pi, 50)
    plt.plot(x, np.sin(x),
             x, np.sin(2 * x))
    plt.show()

    """
上述代码展示了两种不同的曲线样式：'r-o' 和 'g--'。字母 'r' 和 'g' 代表线条的颜色，后面的符号代表线和点标记的类型。
例如 '-o' 代表包含实心点标记的实线，'--' 代表虚线。其他的参数需要读者自己去尝试，这也是学习 Matplotlib 最好的方式。

颜色： 蓝色 - 'b' 绿色 - 'g' 红色 - 'r' 青色 - 'c' 品红 - 'm' 黄色 - 'y' 黑色 - 'k'（'b'代表蓝色，所以这里用黑色的最后一个字母） 白色 - 'w'
线： 直线 - '-' 虚线 - '--' 点线 - ':' 点划线 - '-.'
常用点标记 点 - '.' 像素 - ',' 圆 - 'o' 方形 - 's' 三角形 - '^' 更多点标记样式 http://matplotlib.org/api/markers_api.html
    """
    # 自定义曲线的外观
    x = np.linspace(0, 2 * np.pi, 50)
    plt.plot(x, np.sin(x), 'r-o',
             x, np.cos(x), 'g--')
    plt.show()

    """
使用子图只需要一个额外的步骤，就可以像前面的例子一样绘制数据集。即在调用 plot() 函数之前需要先调用 subplot() 函数。
该函数的第一个参数代表子图的总行数，第二个参数代表子图的总列数，第三个参数代表活跃区域。

活跃区域代表当前子图所在绘图区域，绘图区域是按从左至右，从上至下的顺序编号。例如在 4×4 的方格上，活跃区域 6 在方格上的坐标为 (2, 2)。
    """
    # 使用子图
    x = np.linspace(0, 2 * np.pi, 50)
    plt.subplot(2, 1, 1)  # （行，列，活跃区）
    plt.plot(x, np.sin(x), 'r')
    plt.subplot(2, 1, 2)
    plt.plot(x, np.cos(x), 'g')
    plt.show()

    """
正如上面代码所示，你只需要调用 scatter() 函数并传入两个分别代表 x 坐标和 y 坐标的数组。
注意，我们通过 plot 命令并将线的样式设置为 'bo' 也可以实现同样的效果。

这里的figure()命令是可选的,因为默认情况下将创建figure(1), 如果不手动指定任何轴域,则默认创建subplot(111)。
subplot()命令指定numrows, numcols	, fignum, 其中fignum的范围是从1到numrows *	 numcols。	
如果numrows * numcols < 10, 则subplot命令中的逗号是可选的。	因此,子图subplot(211)	与subplot(2,	1,1)相同。	
你可以创建任意数量的子图和轴域。	如果要手动放置轴域,即不在矩形网格上,请使用axes()命令,
该命令允许你将axes([left, bottom, width, height])指定为位置,其中所有值都使用小数(0到1)坐标。
    """
    # 简单的散点图
    x = np.linspace(0, 2 * np.pi, 50)
    y = np.sin(x)
    plt.subplot(3, 1, 1)
    plt.scatter(x, y)
    plt.subplot(3, 1, 2)
    plt.plot(x, y, 'bo')
    plt.subplot(3, 1, 3)
    plt.plot(x, y, 'ro')
    plt.show()

    """
上面的代码大量的用到了 np.random.rand(1000)，原因是我们绘图的数据都是随机产生的。

同前面一样我们用到了 scatter() 函数，但是这次我们传入了另外的两个参数，分别为所绘点的大小和颜色。
通过这种方式使得图上点的大小和颜色根据数据的大小产生变化。

然后我们用 colorbar() 函数添加了一个颜色栏。
    """
    # 彩色映射散点图
    x = np.random.rand(1000)
    y = np.random.rand(1000)
    size = np.random.rand(1000) * 50
    colour = np.random.rand(1000)
    plt.scatter(x, y, size, colour)
    plt.colorbar()
    plt.show()

    """
直方图是 Matplotlib 中最简单的图形之一。你只需要给 hist() 函数传入一个包含数据的数组。第二个参数代表数据容器的个数。
数据容器代表不同的值的间隔，并用来包含我们的数据。数据容器越多，图形上的数据条就越多。
    """
    # 直方图
    x = np.random.randn(1000)
    plt.hist(x, 50)
    plt.show()

    """
当需要快速创建图形时，你可能不需要为图形添加标签。但是当构建需要展示的图形时，你就需要添加标题，标签和图例。

为了给图形添加图例，我们需要在 plot() 函数中添加命名参数 'label' 并赋予该参数相应的标签。
然后调用 legend() 函数就会在我们的图形中添加图例。

接下来我们只需要调用函数 title()，xlabel() 和 ylabel() 就可以为图形添加标题和标签。
    """
    # 添加标题，坐标轴标记和图例
    x = np.linspace(0, 2 * np.pi, 50)
    plt.plot(x, np.sin(x), 'r-x', label='Sin(x)')
    plt.plot(x, np.cos(x), 'g-^', label='Cos(x)')
    plt.legend()  # 展示图例
    plt.xlabel('Rads')  # 给 x 轴添加标签
    plt.ylabel('Amplitude')  # 给 y 轴添加标签
    plt.title('Sin and Cos Waves')  # 添加图形标题
    plt.show()

    """
    https://liam0205.me/2014/09/11/matplotlib-tutorial-zh-cn/
    
    # 创建一个 8 * 6 点（point）的图，并设置分辨率为 80
    figure(figsize=(8,6), dpi=80)
    
    参数      默认值                 描述
    num         1	                图像的数量
    figsize     figure.figsize	    图像的长和宽（英寸）
    dpi	        figure.dpi	        分辨率（点/英寸）
    facecolor	figure.facecolor	绘图区域的背景颜色
    edgecolor	figure.edgecolor	绘图区域边缘的颜色
    frameon	    True	            是否绘制图像边缘
    """
    x = [random.randint(1, 100) for i in range(100)]
    print(x)
    plt.figure(num=0, figsize=(16, 10), dpi=100)
    plt.plot(x)
    plt.show()


def main(argv):
    logging_config(LOGGING_LEVEL)
    matplotlib_init()


if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()

    main(sys.argv)

    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))

```



