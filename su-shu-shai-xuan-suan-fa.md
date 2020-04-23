### 素数筛选算法

```
# -*- coding:utf-8 -*-
"""
@Author:    clhon@qq.com
@Date:      2017-10-24

1、输出所有小于等于max的质数，这里提供两种方法:
    produce_prime1使用质数判断方法，一直遍历到某一个数的平方根值
    produce_prime2使用筛法，筛选剔除小于等于max的所有非质数，剩下的就是质数了
2、从小到大，输出前m个质数。  筛法的速度远超普通方法，针对这个需求，普通方法很慢，筛法又不适用，因为不知道前m个质数对应的max是多少，
  这怎么办呢？质数越往后越稀疏，有个素数定理就是用来预估max以内有多少质数，最简洁的公式有x/ln(x)，会有一定的误差，但是不超过百分之十五
 那么我们可以根据m反推出max的大小
"""
import math
import time
import datetime


# 最朴素的判断质数的方法， 即根据质数的定义，一直从2到该数的平方根，判断是否能整除
def produce_prime1(max_value):
    for i in range(2, max_value + 1):
        if __is_prime(i):
            print(i)
            pass


# 筛选法找质数，即“埃拉托色尼筛法”，挖掉2的倍数、3的倍数、一直到max的平方根的倍数，剩下的都是质数了
def produce_prime2(max_value):
    li = []
    for i in range(2, max_value + 1):
        if i > 2 and i % 2 == 0:
            li.append(0)
        else:
            li.append(i)

    for i in range(3, int(math.sqrt(max_value)) + 1, 2):
        if li[i - 2] != 0:
            for j in range(i + i, max_value + 1, i):
                li[j - 2] = 0

    for i in li:
        if i != 0:
            print(i)
            pass


# 从小到大，输出前count(count > 10)个质数
# 这里先使用素数定理求出count个素数分布的范围，再使用筛选法筛除所有素数，最后输出前count个素数
def produce_pre_prime(count):
    max_value = int(__find_max(count) * 1.15)
    li = []

    for i in range(2, max_value + 1):
        if i > 2 and i % 2 == 0:
            li.append(0)
        else:
            li.append(i)

    for i in range(3, int(math.sqrt(max_value)) + 1, 2):
        if li[i - 2] != 0:
            for j in range(i + i, max_value + 1, i):
                li[j - 2] = 0

    j = 0
    for i in li:
        if i != 0:
            print(i)
            j += 1
            if j >= count:
                break


# 判断number是否是质数
def __is_prime(number):
    if number <= 1:
        return False

    for i in range(2, int(math.sqrt(number)) + 1):
        if number % i == 0:
            return False

    return True


# 根据素数定理，x/ln(x) >= m 找到最小的一个整数x解，m > 10
def __find_max(m):
    if m <= 10:
        raise NameError('input illegal m <= 10!')

    start = m
    end = m * 2
    while True:
        if end / math.log(end, math.e) >= m:
            break
        start = end
        end = end * 2
    index = int((start + end) / 2)
    while True:
        m1 = index / math.log(index, math.e)
        m2 = (index - 1) / math.log(index - 1, math.e)
        if m1 >= m > m2:
            break
        if m1 >= m:
            end = index
        else:
            start = index
        index = int((start + end) / 2)

    return index


def p10(n):
    r = int(n**0.5)
    # assert r*r <= n and (r+1)**2 > n
    v_list = [n//i for i in range(1, r+1)]
    # print(v_list)
    v_list += list(range(v_list[-1]-1, 0, -1))
    # print(v_list)
    s_list = {i: i*(i+1)//2-1 for i in v_list}
    # print(s_list)
    for p in range(2, r+1):
        if s_list[p] > s_list[p-1]:  # p is prime
            sp = s_list[p-1]  # sum of primes smaller than p
            p2 = p*p
            for v in v_list:
                if v < p2:
                    break
                s_list[v] -= p*(s_list[v//p] - sp)
    return s_list[n]


# https://www.zhihu.com/question/29580448
def p10_zhihu(n):
    r = int(n**0.5)
    # assert r*r <= n and (r+1)**2 > n
    v_list = [n//i for i in range(1, r+1)]
    print(v_list)
    v_list += list(range(v_list[-1]-1, 0, -1))
    print(v_list)
    s_list = {i: i*(i+1)//2-1 for i in v_list}
    print(s_list)
    for p in range(2, r+1):
        if s_list[p] > s_list[p-1]:  # p is prime
            sp = s_list[p-1]  # sum of primes smaller than p
            p2 = p*p
            for v in v_list:
                if v < p2:
                    break
                s_list[v] -= p*(s_list[v//p] - sp)
    return s_list[n]


def main():
    # max_value = 1000000
    max_value = 10

    start = time.time() * 1000
    produce_prime1(max_value)
    end1 = time.time() * 1000
    produce_prime2(max_value)
    end2 = time.time() * 1000

    n_value = 1000000000
    print(p10_zhihu(n_value))
    end3 = time.time() * 1000

    produce_pre_prime(10000)

    print("使用质数定义法找出所有小于等于" + str(max_value) + "质数并输出，总共耗时" + str(end1 - start) + "毫秒")
    print("使用筛选法找出所有小于等于" + str(max_value) + "质数并输出，总共耗时" + str(end2 - end1) + "毫秒")
    print("计算" + str(n_value) + "内质数相加的和并输出，总共耗时" + str(end3 - end2) + "毫秒")


if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()

    main()

    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))
```



