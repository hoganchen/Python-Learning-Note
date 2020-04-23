### Python奇淫技巧

* 逗号用法

```python
>>> a = 1
>>> a
1
>>> a = 1,
>>> a
(1,)
```

* list和set混用

```
>>> client_set
['a']
>>> client_set.append('a')
>>> client_set
['a', 'a']
>>> client_set = list(set(client_set))
>>> client_set
['a']
>>> client_set = list(set(client_set.append('a')))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'NoneType' object is not iterable
>>> 

* 错误的原因是list的append方法没有返回值，或者说返回值为None
```

* 
* 


