### Python类变量与实例变量的访问

    '''
    http://kuanghy.github.io/2015/12/19/python-variable

    访问方式

    类变量：

    在类内部和外部，类变量都用 ` 类名.类变量 的形式访问。在类内部，也可以用 self.类变量 来访问类变量，但此时它的含义已经变了，实际上它已经成了一个实例变量。在实例变量没有被重新赋值时，用 self.类变量` 才能访问到正确的值。简单的说就是实例变量会屏蔽掉类变量的值，就像局部变量屏蔽掉全局变量的值一样。所以一般情况下是不将类变量作为实例变量使用的。

    实例变量：

    在类内部，实例变量用 self.实例变量 的形式访问；在类外部，用 实例名.实例变量 的形式访问。实例变量是绑定到一个实例上的变量，它只属于这个实例。这一点区别于类变量，类变量是所有来自于同一个类的实例所共享的。
    '''


    '''
    https://www.jianshu.com/p/9fefb52ca91b

    1、类变量可以使用className.类变量和self.类变量两种方式访问。
    2、如果使用self.类变量的方式访问并重新赋值后，这个变量就会成为实例变量和self绑定，实际上就变成了一个实例变量，实例变量会屏蔽掉类变量的值。
    3、类变量是共享的，最好使用类名的方式来访问类变量。
    4、类变量通过self访问时，就会被转化成实例变量，被绑定到特定的实例上。
    5、实例变量(self)的形式对类变量重新赋值后，类变量的值不会随之变化。
    6、实例变量对每一个对象是不可见的，每一个对象拥有着可能不同的值。
    '''


    class ClassTest(object):
        class_status = 1

        def __init__(self, param = 2):
            self.param = param
            self.inst_count = 0
            self.class_count = 0

            self._protected_param = 99
            self.__private_param = 88

        def print_param(self):
            print('class_status: {}, param: {}'.format(self.class_status, self.param))

        def instance_param_change(self):
            # self.class_status = 10 + self.inst_count * 10
            self.param = 20 + self.inst_count * 10

            self.inst_count += 1

        def class_param_change(self):
            ClassTest.class_status = 10 + self.class_count * 10
            self.class_count += 1


    class ClassTestB(ClassTest):
        def new_print_param(self):
            print('ClassTestB: class_status: {}, param: {}'.format(self.class_status, self.param))
            print('ClassTestB: _protected_param: {}'.format(self._protected_param))
            print('ClassTestB: __private_param: {}'.format(self.__private_param))


    if __name__ == "__main__":
        class_test = ClassTest()

        class_test.print_param()
        class_test.class_param_change()
        class_test.print_param()
        print('ClassTest.class_status: {}'.format(ClassTest.class_status))

        class_test.print_param()
        class_test.instance_param_change()
        class_test.print_param()
        print('ClassTest.class_status: {}'.format(ClassTest.class_status))

        ClassTest.class_status = 100

        class_test.print_param()
        class_test.instance_param_change()
        class_test.print_param()
        print('ClassTest.class_status: {}'.format(ClassTest.class_status))

        class_test_b = ClassTestB()
        class_test_b.new_print_param()

```
Output:

hogan@hogan:~$ condapython class_test.py 
class_status: 1, param: 2
class_status: 10, param: 2
ClassTest.class_status: 10
class_status: 10, param: 2
class_status: 10, param: 20
ClassTest.class_status: 10
class_status: 100, param: 20
class_status: 100, param: 30
ClassTest.class_status: 100
ClassTestB: class_status: 100, param: 2
ClassTestB: _protected_param: 99
Traceback (most recent call last):
  File "class_test.py", line 81, in <module>
    class_test_b.new_print_param()
  File "class_test.py", line 57, in new_print_param
    print('ClassTestB: __private_param: {}'.format(self.__private_param))
AttributeError: 'ClassTestB' object has no attribute '_ClassTestB__private_param'

```



