### Python类的静态函数，类函数和成员函数示例

```
import inspect


class Book(object):

    def __init__(self, title):
        self.title = title

    @classmethod
    def create(cls, title):
        book = cls(title=title)
        print(book)
        print(title)
        return book

book1 = Book("python")
book2 = Book.create("python and django")
print(book1.title)
print(book2.title)


def print_line_file_func():
    caller_frame_record = inspect.stack()[1]
    print(caller_frame_record)
    frame = caller_frame_record[0]
    print(frame)
    info = inspect.getframeinfo(frame)
    print(info.filename)                        # __FILE__
    print(info.function)                        # __FUNCTION__
    print(info.lineno)                          # __LINE__


def logger(msg):
    frame = inspect.currentframe()



IND = 'ON'


class Kls(object):
    # this global variable also can be accessed in instance object
    g_data = 10

    def __init__(self, data):
        self.db = None
        self.data = data
        # modified the global variable g_data to 20
        # Kls.g_data = 20

    def self_func(self):
        print("call private_func", self.data)
        print("Kls.g_data = ", Kls.g_data)

    @staticmethod
    def check_ind():
        print("IND = %s\n" % IND)
        print("Kls.g_data = %s\n" % Kls.g_data)

        Kls.g_data += 1
        print_line_file_func()
        return IND == 'ON'

    @classmethod
    def do_reset(cls):
        cls_inst = cls(10)
        cls_inst.self_func()
        cls.check_ind()
        if cls_inst.check_ind():
            print('Reset done for:', cls_inst.g_data)
            print('Reset done for:', cls.g_data)

        cls.g_data += 1

    def set_db(self):
        Kls.do_reset()
        if self.check_ind():
            self.db = 'New db connection'
        print('DB connection made for: ', self.data)
        print(Kls.g_data)


Kls.check_ind()
Kls.do_reset()
ik1 = Kls(12)
ik1.check_ind()
ik1.do_reset()
ik1.set_db()
Kls.check_ind()
Kls.do_reset()
ik1.do_reset()
ik1.set_db()

```



