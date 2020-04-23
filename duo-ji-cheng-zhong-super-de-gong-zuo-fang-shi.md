### 多继承中 super 的工作方式

https://mozillazg.com/2016/12/python-super-is-not-as-simple-as-you-thought.html

http://gohom.win/2016/02/23/py-super/

http://python3-cookbook.readthedocs.io/zh\_CN/latest/c08/p07\_calling\_method\_on\_parent\_class.html

```
class A:
    def __init__(self):
        self.n = 2

    def add(self, m):
        # 第四步
        # 来自 D.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @A.add, self.n = {1}'.format(self, self.n))
        self.n += m
        # d.n == 7


class B(A):
    def __init__(self):
        self.n = 3

    def add(self, m):
        # 第二步
        # 来自 D.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @B.add, self.n = {1}'.format(self, self.n))
        # 等价于 suepr(B, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 B 之后的 [C, A, object] 中查找 add 方法
        super().add(m)

        # 第六步
        # d.n = 11
        self.n += 3
        # d.n = 14

class C(A):
    def __init__(self):
        self.n = 4

    def add(self, m):
        # 第三步
        # 来自 B.add 中的 super
        # self == d, self.n == d.n == 5
        print('self is {0} @C.add, self.n = {1}'.format(self, self.n))
        # 等价于 suepr(C, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 C 之后的 [A, object] 中查找 add 方法
        super().add(m)

        # 第五步
        # d.n = 7
        self.n += 4
        # d.n = 11


class D(B, C):
    def __init__(self):
        self.n = 5

    def add(self, m):
        # 第一步
        print('self is {0} @D.add, self.n = {1}'.format(self, self.n))
        # 等价于 super(D, self).add(m)
        # self 的 MRO 是 [D, B, C, A, object]
        # 从 D 之后的 [B, C, A, object] 中查找 add 方法
        super().add(m)

        # 第七步
        # d.n = 14
        self.n += 5
        # self.n = 19

d = D()
print(D.mro())
d.add(2)
print(d.n)

```

Output:

```
[<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>]
self is <__main__.D object at 0x7fd4d40353c8> @D.add, self.n = 5
self is <__main__.D object at 0x7fd4d40353c8> @B.add, self.n = 5
self is <__main__.D object at 0x7fd4d40353c8> @C.add, self.n = 5
self is <__main__.D object at 0x7fd4d40353c8> @A.add, self.n = 5
19

```



