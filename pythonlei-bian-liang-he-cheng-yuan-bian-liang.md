### Python类变量和成员变量

```
class TestClass:
    g_list = [1, 2, 3, 4]
    
    def __init__(self):
        self.list = [5, 6, 7, 8]
        
    def pop_list(self):
        self.list.pop()
        
    def pop_g_list(self):
        self.g_list.pop()
        
t = TestClass()
t2 = TestClass()
print('TestClass.g_list: {}'.format(TestClass.g_list))
t.pop_list()
print('t.g_list: {}'.format(t.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t.list: {}'.format(t.list))

print('t2.g_list: {}'.format(t2.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t2.list: {}'.format(t2.list))

t.pop_g_list()
print('t.g_list: {}'.format(t.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t.list: {}'.format(t.list))

print('t2.g_list: {}'.format(t2.g_list))
print('TestClass.g_list: {}'.format(TestClass.g_list))
print('t2.list: {}'.format(t2.list))

# output
'''
TestClass.g_list: [1, 2, 3, 4]
t.g_list: [1, 2, 3, 4]
TestClass.g_list: [1, 2, 3, 4]
t.list: [5, 6, 7]
t2.g_list: [1, 2, 3, 4]
TestClass.g_list: [1, 2, 3, 4]
t2.list: [5, 6, 7, 8]
t.g_list: [1, 2, 3]
TestClass.g_list: [1, 2, 3]
t.list: [5, 6, 7]
t2.g_list: [1, 2, 3]
TestClass.g_list: [1, 2, 3]
t2.list: [5, 6, 7, 8]
'''
```



