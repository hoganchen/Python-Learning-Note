### Zip和Map用法示例01

```
test1 = [1, 2, 3, 4, 5]
test2 = [1, 2, 3, 4, 5]
zip1 = zip(test1, test2)
list(zip1)
list(zip1)
zip1 = zip(test1, test2)
mapobj = map(lambda args: args[0] * args[1], zip1)
list(mapobj)
list(mapobj)
list(zip1)
zip1 = zip(test1, test2)
mapobj = map(lambda args: args[0] * args[1], zip1)
list(zip1)
list(mapobj)

output:
Python 3.6.1 |Anaconda custom (64-bit)| (default, May 11 2017, 13:09:58) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> test1 = [1, 2, 3, 4, 5]
>>> test2 = [1, 2, 3, 4, 5]
>>> zip1 = zip(test1, test2)
>>> list(zip1)
[(1, 1), (2, 2), (3, 3), (4, 4), (5, 5)]
>>> list(zip1)
[]
>>> zip1 = zip(test1, test2)
>>> mapobj = map(lambda args: args[0] * args[1], zip1)
>>> list(mapobj)
[1, 4, 9, 16, 25]
>>> list(mapobj)
[]
>>> list(zip1)
[]
>>> zip1 = zip(test1, test2)
>>> mapobj = map(lambda args: args[0] * args[1], zip1)
>>> list(zip1)
[(1, 1), (2, 2), (3, 3), (4, 4), (5, 5)]
>>> list(mapobj)
[]
>>> 

```



