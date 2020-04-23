### Python中继承object和不继承object的区别

[https://www.cnblogs.com/keke-xiaoxiami/p/8459386.html](https://www.cnblogs.com/keke-xiaoxiami/p/8459386.html)

[http://www.runoob.com/note/28629](http://www.runoob.com/note/28629)

[https://www.zhihu.com/question/19754936](https://www.zhihu.com/question/19754936)

[https://blog.csdn.net/qq\_27828675/article/details/79358893](https://blog.csdn.net/qq_27828675/article/details/79358893)

[http://gohom.win/2015/10/20/pyObject/](http://gohom.win/2015/10/20/pyObject/)

[https://www.cnblogs.com/bigberg/p/7182741.html](https://www.cnblogs.com/bigberg/p/7182741.html)

* ##### 继承object

```
class A(object):
    def foo(self):
        print('called A.foo()')

class B(A):
    pass

class C(A):
    def foo(self):
        print('called C.foo()')

class D(B, C):
    pass

if __name__ == '__main__':
    d = D()
    d.foo()
```

```
Output:

hogan@hogan:~$ python --version
Python 2.7.6
hogan@hogan:~$ condapython --version
Python 3.6.5 :: Anaconda, Inc.

hogan@hogan:~$ python object_class.py
called C.foo()

hogan@hogan:~$ condapython object_class.py
called C.foo()
```

* ##### 不继承object

```
class A:
    def foo(self):
        print('called A.foo()')

class B(A):
    pass

class C(A):
    def foo(self):
        print('called C.foo()')

class D(B, C):
    pass

if __name__ == '__main__':
    d = D()
    d.foo()
```

```
Output:

hogan@hogan:~$ python --version
Python 2.7.6
hogan@hogan:~$ condapython --version
Python 3.6.5 :: Anaconda, Inc.

hogan@hogan:~$ python object_class.py
called A.foo()

hogan@hogan:~$ condapython object_class.py
called C.foo()
```

* ##### 继承object和不继承object

```
class ObjectA(object):
    def foo(self):
        print('called ObjectA.foo()')


class ObjectB(ObjectA):
    pass


class ObjectC(ObjectA):
    def foo(self):
        print('called ObjectC.foo()')


class ObjectD(ObjectB, ObjectC):
    pass


class A:
    def foo(self):
        print('called A.foo()')


class B(A):
    pass


class C(A):
    def foo(self):
        print('called C.foo()')


class D(B, C):
    pass


if __name__ == '__main__':
    object_d = ObjectD()
    object_d.foo()

    d = D()
    d.foo()
```

```
Output:

hogan@hogan:~$ python --version
Python 2.7.6
hogan@hogan:~$ condapython --version
Python 3.6.5 :: Anaconda, Inc.

hogan@hogan:~$ python object_class.py
called ObjectC.foo()
called A.foo()

hogan@hogan:~$ condapython object_class.py
called ObjectC.foo()
called C.foo()
```

**这俩的区别在于——————**

**在python2.x中，通过分别继承自object和不继承object定义不同的类，之后通过dir\(\)和type分别查看该类的所有方法和类型**

**在3.x中：两者是一致的，因为在3.x中，默认继承就是object了**

