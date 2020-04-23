### Python元类学习

https://www.cnblogs.com/huchong/p/8260151.html

http://blog.jobbole.com/21351/

http://www.dongwm.com/archives/%E8%AF%A6%E8%A7%A3Python/

https://blog.csdn.net/gzlaiyonghao/article/details/3048947

https://www.cnblogs.com/tkqasn/p/6524879.html

https://zhuanlan.zhihu.com/p/23887627

https://github.com/qiwsir/StackOverFlowCn/blob/master/302.md

http://python3-cookbook-personal.readthedocs.io/zh\_CN/latest/c09/p13\_using\_mataclass\_to\_control\_instance\_creation.html

https://juejin.im/entry/57e1490ba22b9d0061280410



除了使用`type()`动态创建类以外，要控制类的创建行为，还可以使用metaclass。

metaclass，直译为元类，简单的解释就是：

当我们定义了类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例。

但是如果我们想创建出类呢？那就必须根据metaclass创建出类，所以：先定义元类（不自定义时，默认用type），然后创建类。

连接起来就是：先定义metaclass，就可以创建类，最后创建实例。

所以，metaclass允许你创建类或者修改类。换句话说，你可以把类看成是元类创建出来的“实例”。

默认情况下，类是使用type\(\)构造的。类主体在一个新的名称空间中执行，类名在本地绑定到类型的结果\(名称、基、名称空间\)。

可以通过在类定义行中传递元类关键字参数来定制类创建过程，或者从包含此类参数的现有类继承。在下面的示例中，MyClass和MySubclass都是Meta的实例:

```
'''
https://docs.python.org/3.7/library/functions.html#super
For both use cases, a typical superclass call looks like this:

class C(B):
    def method(self, arg):
        super().method(arg)    # This does the same thing as:
                               # super(C, self).method(arg)


https://taizilongxu.gitbooks.io/stackoverflow-about-python/content/19/README.html

super()的好处就是可以避免直接使用父类的名字.但是它主要用于多重继承,这里面有很多好玩的东西.如果还不了解的话可以看看官方文档

注意在Python3.0里语法有所改变:你可以用super().__init__()替换super(ChildB, self).__init__().(在我看来非常nice)


https://foofish.net/magic-method.html
从输出结果来看，__new__ 方法的返回值就是类的实例对象，这个实例对象会传递给 __init__ 方法中定义的 self 参数，以便实例对象可以被正确地初始化。

如果 __new__ 方法不返回值（或者说返回 None）那么 __init__ 将不会得到调用，这个也说得通，因为实例对象都没创建出来，调用 init 也没什么意义，此外，Python 还规定，__init__ 只能返回 None 值，否则报错，这个留给大家去试。
'''

# The python language reference -- APPENDIX D
class MyClass(object):
    def __init__(self):
        print('call "__init__" function')
        super(MyClass, self).__init__()
        
    def __new__(cls):
        print('call "__new__" function')
        return super(MyClass, cls).__new__(cls)
        
    def func(self,name='world'):
        print('Hello, %s.' % name)
        
    # http://pycoders-weekly-chinese.readthedocs.io/en/latest/issue6/a-guide-to-pythons-magic-methods.html
    def __str__(self):
        return 'call "__str__" function'
    
    def __call__(self):
        print('call "__call__" function')

myc = MyClass()

print(MyClass, type(MyClass))
print(myc, type(myc))
myc.func()
myc()

def fn(self, name='world'): # 先定义函数
    print('Hello, %s.' % name)
    
def print_str(self):
    return('print_str')

def class_call(self):
    fn(self)

MyClass = type('MyClass', (object,), {'func':fn, '_str_': print_str, '__call__':class_call}) # 创建MyClass类,得到一个type的类对象
# MyClass = type('MyClass', (object,), {'func':lambda self,name:name}) # 创建MyClass类

myc=MyClass()

print(MyClass, type(MyClass))
print(myc, type(myc))
myc.func()
myc()
```

Output:

```
call "__new__" function
call "__init__" function
<class '__main__.MyClass'> <class 'type'>
call "__str__" function <class '__main__.MyClass'>
Hello, world.
call "__call__" function
<class '__main__.MyClass'> <class 'type'>
<__main__.MyClass object at 0x7fd4d4023b00> <class '__main__.MyClass'>
Hello, world.
Hello, world.
```



