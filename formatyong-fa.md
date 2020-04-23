### Format用法

* ##### 位运算

```
>>> print('0x%08x' % a)
0x25b210c6
>>> print('0x{:04x}'.format((a & 0x0007fc00) >> 10))
0x0084
>>> print('0x{:>04x}'.format((a & 0x0007fc00) >> 10))
0x0084
>>> print('0x%04x' % ((a & 0x0007fc00) >> 10))
0x0084

>>> '{:b}'.format(11)
'1011'
>>> '{:d}'.format(11)
'11'
>>> '{:o}'.format(11)
'13'
>>> '{:x}'.format(11)
'b'
>>> '{:X}'.format(11)
'B'
>>> '0x{:x}'.format(11)
'0xb'
>>> '0x{:X}'.format(11)
'0xB'
>>> '{:#x}'.format(11)
'0xb'
>>> '{:#X}'.format(11)
'0XB'
>>> '{:#04X}'.format(11)
'0X0B'
>>> '{:#08X}'.format(11)
'0X00000B'
>>> '{:#04x}'.format(11)
'0x0b'
>>> '{:#08x}'.format(11)
'0x00000b'


gcd_a = random.randrange(0x0FFFFFFF)
gcd_b = random.randrange(0x0FFFFFFF)

# https://www.runoob.com/python/att-string-format.html
# https://docs.python.org/zh-cn/3/library/string.html
'''
>>> a = '100000000'

>>> print('0x{:>08x}'.format(int(a)))
0x05f5e100
>>> print('{:>#08x}'.format(int(a)))
0x5f5e100
>>> print('{:#>08x}'.format(int(a)))
#5f5e100
>>> print('{:#x}'.format(int(a)))
0x5f5e100
>>> print('{:#08x}'.format(int(a)))
0x5f5e100
'''
print('gcd_a: 0x%08x, gcd_b: 0x%08x' % (gcd_a, gcd_b))
print('gcd_a: {}, gcd_b: {}'.format(gcd_a, gcd_b))
print('gcd_a: {:#x}, gcd_b: {:#x}'.format(gcd_a, gcd_b))
print('gcd_a: {:#>08x}, gcd_b: {:#>08x}'.format(gcd_a, gcd_b))
print('gcd_a: {:>08x}, gcd_b: {:>08x}'.format(gcd_a, gcd_b))
print('gcd_a: {:>#08x}, gcd_b: {:>#08x}'.format(gcd_a, gcd_b))
print('gcd_a: 0x{:>08x}, gcd_b: 0x{:>08x}'.format(gcd_a, gcd_b))
```

* ##### 



