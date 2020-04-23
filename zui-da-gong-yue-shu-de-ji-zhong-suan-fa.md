### 最大公约数的几种算法

```
# -*- coding:utf-8 -*-

"""
@Author:    clhon@qq.com
@Date:      2018-06-07
"""

import time
import random
import logging
import datetime


# log level
LOGGING_LEVEL = logging.DEBUG


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


# http://www.runoob.com/python3/python3-hcf.html
def regular_algorithm_01(a, b):
    hcf = 1

    if a > b:
        smaller = b
    else:
        smaller = a

    for i in range(1, smaller + 1):
        if 0 == a % i and 0 == b % i:
            hcf = i

    return hcf


def regular_algorithm_02(a, b):
    hcf = 1

    if a == b:
        return a
    elif a > b:
        smaller = b
    else:
        smaller = a

    for i in range(smaller, 1, -1):
        if 0 == a % i and 0 == b % i:
            hcf = i
            break

    return hcf


'''
https://zh.wikipedia.org/zh-hans/%E8%BC%BE%E8%BD%89%E7%9B%B8%E9%99%A4%E6%B3%95
https://blog.csdn.net/linj_m/article/details/19167147

在数学中，辗转相除法，又称欧几里得算法（英语：Euclidean algorithm），是求最大公约数的算法。
辗转相除法首次出现于欧几里得的《几何原本》（第VII卷，命题i和ii）中，而在中国则可以追溯至东汉出现的《九章算术》。

两个整数的最大公约数是能够同时整除它们的最大的正整数。辗转相除法基于如下原理：两个整数的最大公约数等于其中较小的数和两数的差的最大公约数。
例如，252和105的最大公约数是21（252 = 21 × 12 ； 105 = 21 × 5 ）；因为252 − 105 = 21 × (12 − 5) = 147 ，
所以147和105的最大公约数也是21。在这个过程中，较大的数缩小了，所以继续进行同样的计算可以不断缩小这两个数直至其中一个变成零。
这时，所剩下的还没有变成零的数就是两数的最大公约数。由辗转相除法也可以推出，两数的最大公约数可以用两数的整数倍相加来表示，
如21 = 5 × 105 + (−2) × 252 。这个重要的结论叫做贝祖定理。
'''
def euclidean_algorithm_01(a, b):
    if 0 == b:
        return a
    else:
        return euclidean_algorithm_01(b, a % b)


def euclidean_algorithm_02(a, b):
    while 0 != b:
        a, b = b, a % b

    return a


'''
https://zh.wikipedia.org/zh-hans/%E6%89%A9%E5%B1%95%E6%AC%A7%E5%87%A0%E9%87%8C%E5%BE%97%E7%AE%97%E6%B3%95

扩展欧几里得算法（英语：Extended Euclidean algorithm）是欧几里得算法（又叫辗转相除法）的扩展。已知整数a、b，
扩展欧几里得算法可以在求得a、b的最大公约数的同时，能找到整数x、y（其中一个很可能是负数），使它们满足贝祖等式ax + by = gcd(a, b).
'''
def ext_euclidean_algorithm(a, b):
    if 0 == b:
        return 1, 0, a
    else:
        x, y, q = ext_euclidean_algorithm(b, a % b)  # q = gcd(a, b) = gcd(b, a%b)
        x, y = y, (x - (a // b) * y)
        return x, y, q


'''
https://xuanwo.org/2015/03/11/number-theory-gcd/
https://www.cnblogs.com/drizzlecrj/archive/2007/09/14/892340.html

欧几里德算法是计算两个数最大公约数的传统算法，他无论从理论还是从效率上都是很好的。但是他有一个致命的缺陷，这个缺陷只有在大素数时才会显现出来。

考虑现在的硬件平台，一般整数最多也就是64位，对于这样的整数，计算两个数之间的模是很简单的。
对于字长为32位的平台，计算两个不超过32位的整数的模，只需要一个指令周期，而计算64位以下的整数模，也不过几个周期而已。
但是对于更大的素数，这样的计算过程就不得不由用户来设计，为了计算两个超过64位的整数的模，
用户也许不得不采用类似于多位数除法手算过程中的试商法，这个过程不但复杂，而且消耗了很多CPU时间。对于现代密码算法，
要求计算128位以上的素数的情况比比皆是，设计这样的程序迫切希望能够抛弃除法和取模。 （注：说到抛弃除法和取模，其实辗转相除法可以写成减法的形式)

Stein算法由J. Stein 1961年提出，这个方法也是计算两个数的最大公约数。和欧几里德算法 算法不同的是，Stein算法只有整数的移位和加减法，
这对于程序设计者是一个福音。 
'''
def stein_algorithm(a, b):
    if 0 == a:
        return b

    if 0 == b:
        return a

    if 0 == a % 2 and 0 == b % 2:
        return 2 * stein_algorithm(a >> 1, b >> 1)
    elif 0 == a % 2:
        return stein_algorithm(a >> 1, b)
    elif 0 == b % 2:
        return stein_algorithm(a, b >> 1)
    else:
        return stein_algorithm(abs(a - b), min(a, b))


'''
https://www.idomaths.com/zh-Hans/hcflcm.php

如何求最大公约数
求最大公约数其实有很多方法，以下将介绍比较常见的三种：
    分解质因数法
    短除法
    辗转相除法（欧几里德算法）
'''
def main():
    logging_config(LOGGING_LEVEL)

    # gcd_a = int(input('Please Input the first number:'))
    # gcd_b = int(input('Please Input the second number:'))

    # gcd_a = random.randint(0, 0xFFFFFF)
    # gcd_b = random.randint(0, 0xFFFFFF)

    # gcd_a = random.randrange(0, 0xFFFFFF, 2)
    # gcd_b = random.randrange(0, 0xFFFFFF, 2)

    gcd_a = random.choice(range(0, 0xFFFFFFFFFFFF, 2))
    gcd_b = random.choice(range(0, 0xFFFFFFFFFFFF, 2))

    print('gcd_a: {}, gcd_b: {}'.format(gcd_a, gcd_b))

    '''
    start_time = time.time()
    print('gcd: {}'.format(regular_algorithm_01(gcd_a, gcd_b)))
    logging.debug('For regular_algorithm_01 algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))

    start_time = time.time()
    print('gcd: {}'.format(regular_algorithm_02(gcd_a, gcd_b)))
    logging.debug('For regular_algorithm_02 algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))
    '''

    start_time = time.time()
    print('gcd: {}'.format(euclidean_algorithm_01(gcd_a, gcd_b)))
    print('For euclidean_algorithm_01 algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))

    start_time = time.time()
    print('gcd: {}'.format(euclidean_algorithm_02(gcd_a, gcd_b)))
    print('For euclidean_algorithm_02 algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))

    start_time = time.time()
    print('gcd: {}'.format(ext_euclidean_algorithm(gcd_a, gcd_b)[2]))
    print('For ext_euclidean_algorithm algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))

    start_time = time.time()
    print('gcd: {}'.format(stein_algorithm(gcd_a, gcd_b)))
    print('For stein_algorithm algorithm, total elapsed time {} seconds.'.format(time.time() - start_time))


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal Elapsed Time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))

```



