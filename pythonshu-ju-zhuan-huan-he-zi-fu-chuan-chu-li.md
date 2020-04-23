### Python数据转换和字符串编码处理

* ###### 数据处理

```
import math

x = '12.345'
y = '12345'
z = '0x12345'
i = '123456789098765432101234567890987654321'
xx = 12345
yy = 'A'
zz = 'This is a string, and include some chinese characters, include “你好”，"再见"！！！'
zz_str = 'This is a string\0x65, and include some chinese characters, include “你好”，"再见"！！！'

print('type(float(x)): {}, value: {}'.format(type(float(x)), float(x)))
print('type(int(y)): {}, value: {}, hex value: {}'.format(type(int(y)), int(y), int(y, 16)))
print('type(int(z, 16)): {}, hex value: {}'.format(type(int(z, 16)), int(z, 16)))
print('type(int(i, 16)): {}, value: {}, hex value: {}'.format(type(int(i, 16)), int(i), int(i, 16)))
print('xx = {}, hex(xx) = {}, oct(xx): {}, bin(xx): {}'.format(xx, hex(xx), oct(xx), bin(xx)))
print('yy = {}, ord(yy) = {}, ord(yy.lower()) = {}, chr(65): {}, chr(97): {}'.format(yy, ord(yy), ord(yy.lower()), chr(65), chr(97)))
print('\n')

# all是所有的值为True，则结果为True，其中一个为False，结果为False，all的参数为列表或者元组
print('all(ord(c) for c in zz): {}, [ord(c) for c in zz]: {}'.format(all(ord(c) for c in zz), [ord(c) for c in zz]))
print('all(ord(c) < 128 for c in zz): {}, [ord(c) < 128 for c in zz]: {}'.format(all(ord(c) < 128 for c in zz), [ord(c) < 128 for c in zz]))
print('all([1, 2, 3, 4, 5]): {}, all([True, False, True]): {}'.format(all([1, 2, 3, 4, 5]), all([True, False, True])))
print('\n')

xxx = 12.543
yyy = 12.345

# int是取整，round是四舍五入，ceil是向上取整，floor是向下取整，modf是把小数部分和整数部分以元组类型返回
print('int(xxx): {}, round(xxx) = {}, math.ceil(xxx): {}, math.floor(xxx): {}, math.modf(xxx): {}'.format(int(xxx), round(xxx), math.ceil(xxx), math.floor(xxx), math.modf(xxx)))
print('int(yyy): {}, round(yyy) = {}, math.ceil(yyy): {}, math.floor(yyy): {}, math.modf(yyy): {}'.format(int(yyy), round(yyy), math.ceil(yyy), math.floor(yyy), math.modf(yyy)))
```

Output:

```
type(float(x)): <class 'float'>, value: 12.345
type(int(y)): <class 'int'>, value: 12345, hex value: 74565
type(int(z, 16)): <class 'int'>, hex value: 74565
type(int(i, 16)): <class 'int'>, value: 123456789098765432101234567890987654321, hex value: 6495562831739207890243838329755629378328675105
xx = 12345, hex(xx) = 0x3039, oct(xx): 0o30071, bin(xx): 0b11000000111001
yy = A, ord(yy) = 65, ord(yy.lower()) = 97, chr(65): A, chr(97): a


all(ord(c) for c in zz): True, [ord(c) for c in zz]: [84, 104, 105, 115, 32, 105, 115, 32, 97, 32, 115, 116, 114, 105, 110, 103, 44, 32, 97, 110, 100, 32, 105, 110, 99, 108, 117, 100, 101, 32, 115, 111, 109, 101, 32, 99, 104, 105, 110, 101, 115, 101, 32, 99, 104, 97, 114, 97, 99, 116, 101, 114, 115, 44, 32, 105, 110, 99, 108, 117, 100, 101, 32, 8220, 20320, 22909, 8221, 65292, 34, 20877, 35265, 34, 65281, 65281, 65281]
all(ord(c) < 128 for c in zz): False, [ord(c) < 128 for c in zz]: [True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, True, False, False, False, False, False, True, False, False, True, False, False, False]
all([1, 2, 3, 4, 5]): True, all([True, False, True]): False


int(xxx): 12, round(xxx) = 13, math.ceil(xxx): 13, math.floor(xxx): 12, math.modf(xxx): (0.5429999999999993, 12.0)
int(yyy): 12, round(yyy) = 12, math.ceil(yyy): 13, math.floor(yyy): 12, math.modf(yyy): (0.34500000000000064, 12.0)
```

* ###### 字符串编码

```
import binascii
import base64

# https://www.cnblogs.com/xywq/p/7594280.html
xx = 'AABBCCDDEEFF'
yy = 'hello from Hogan'
zz = '11223344556677889900aa'
kk = 'hello from 陈良洪'

# python3不太一样：因为3.x中字符都为unicode编码，而b64encode函数的参数为byte类型，所以必须先转码。
print('base64.b64encode(xx.encode()): {}'.format(base64.b64encode(xx.encode())))
print('base64.b64encode(yy.encode()): {}'.format(base64.b64encode(yy.encode())))
print('base64.b64encode(zz.encode()): {}'.format(base64.b64encode(zz.encode())))
print('base64.b64encode(kk.encode()): {}'.format(base64.b64encode(kk.encode())))
print('\n')

print('base64.b64decode(base64.b64encode(xx.encode())): {}'.format(base64.b64decode(base64.b64encode(xx.encode()))))
print('base64.b64decode(base64.b64encode(yy.encode())): {}'.format(base64.b64decode(base64.b64encode(yy.encode()))))
print('base64.b64decode(base64.b64encode(zz.encode())): {}'.format(base64.b64decode(base64.b64encode(zz.encode()))))
print('base64.b64decode(base64.b64encode(kk.encode())): {}'.format(base64.b64decode(base64.b64encode(kk.encode()))))
print('\n')

print('base64.b64encode(xx.encode()): {}'.format(str(base64.b64encode(xx.encode()), 'utf-8')))
print('base64.b64encode(yy.encode()): {}'.format(str(base64.b64encode(yy.encode()), 'utf-8')))
print('base64.b64encode(zz.encode()): {}'.format(str(base64.b64encode(zz.encode()), 'utf-8')))
print('base64.b64encode(kk.encode()): {}'.format(str(base64.b64encode(kk.encode()), 'utf-8')))
print('\n')

print('base64.b64encode(xx.encode()): {}'.format(base64.b64encode(xx.encode()).decode()))
print('base64.b64encode(yy.encode()): {}'.format(base64.b64encode(yy.encode()).decode()))
print('base64.b64encode(zz.encode()): {}'.format(base64.b64encode(zz.encode()).decode()))
print('base64.b64encode(kk.encode()): {}'.format(base64.b64encode(kk.encode()).decode()))
print('\n')

print('binascii.b2a_base64(xx.encode()): {}'.format(binascii.b2a_base64(xx.encode())))
print('binascii.b2a_base64(yy.encode()): {}'.format(binascii.b2a_base64(yy.encode())))
print('binascii.b2a_base64(zz.encode()): {}'.format(binascii.b2a_base64(zz.encode())))
print('binascii.b2a_base64(kk.encode()): {}'.format(binascii.b2a_base64(kk.encode())))
print('\n')

print('binascii.a2b_base64(binascii.b2a_base64(xx.encode())): {}'.format(binascii.a2b_base64(binascii.b2a_base64(xx.encode()))))
print('binascii.a2b_base64(binascii.b2a_base64(yy.encode())): {}'.format(binascii.a2b_base64(binascii.b2a_base64(yy.encode()))))
print('binascii.a2b_base64(binascii.b2a_base64(zz.encode())): {}'.format(binascii.a2b_base64(binascii.b2a_base64(zz.encode()))))
print('binascii.a2b_base64(binascii.b2a_base64(kk.encode())): {}'.format(binascii.a2b_base64(binascii.b2a_base64(kk.encode()))))
print('\n')

print('xx.encode(): {}, xx.encode("gb2312"): {}, xx.encode("GBK"): {}'.format(xx.encode(), xx.encode('gb2312'), xx.encode('GBK')))
print('yy.encode(): {}, yy.encode("gb2312"): {}, yy.encode("GBK"): {}'.format(yy.encode(), yy.encode('gb2312'), yy.encode('GBK')))
print('zz.encode(): {}, zz.encode("gb2312"): {}, zz.encode("GBK"): {}'.format(zz.encode(), zz.encode('gb2312'), zz.encode('GBK')))
print('kk.encode(): {}, kk.encode("gb2312"): {}, kk.encode("GBK"): {}'.format(kk.encode(), kk.encode('gb2312'), kk.encode('GBK')))
print('\n')

print('bytearray(xx.encode()): {}'.format(bytearray(xx.encode())))
print('bytearray(yy.encode()): {}'.format(bytearray(yy.encode())))
print('bytearray(zz.encode()): {}'.format(bytearray(zz.encode())))
print('bytearray(kk.encode()): {}'.format(bytearray(kk.encode())))
print('\n')

print('xx.encode(): {}'.format(xx.encode()))
print('binascii.a2b_hex(xx): {}, binascii.unhexlify(xx): {}'.format(binascii.a2b_hex(xx), binascii.unhexlify(xx)))
print('binascii.b2a_hex(xx): {}, binascii.hexlify(xx): {}'.format(binascii.b2a_hex(xx.encode()), binascii.hexlify(xx.encode())))
print('\n')

print('yy.encode(): {}'.format(yy.encode()))
# print('binascii.a2b_hex(yy): {}, binascii.unhexlify(yy): {}'.format(binascii.a2b_hex(yy), binascii.unhexlify(yy)))
print('binascii.b2a_hex(yy): {}, binascii.hexlify(yy): {}'.format(binascii.b2a_hex(yy.encode()), binascii.hexlify(yy.encode())))
print('\n')

print('zz.encode(): {}'.format(zz.encode()))
print('binascii.a2b_hex(zz): {}, binascii.unhexlify(zz): {}'.format(binascii.a2b_hex(zz), binascii.unhexlify(zz)))
print('binascii.b2a_hex(zz): {}, binascii.hexlify(zz): {}'.format(binascii.b2a_hex(zz.encode()), binascii.hexlify(zz.encode())))
print('\n')

print('binascii.a2b_uu(xx): {}, binascii.b2a_uu(xx): {}'.format(binascii.a2b_uu(xx), binascii.b2a_uu(xx.encode())))
```

Output:

```
base64.b64encode(xx.encode()): b'QUFCQkNDRERFRUZG'
base64.b64encode(yy.encode()): b'aGVsbG8gZnJvbSBIb2dhbg=='
base64.b64encode(zz.encode()): b'MTEyMjMzNDQ1NTY2Nzc4ODk5MDBhYQ=='
base64.b64encode(kk.encode()): b'aGVsbG8gZnJvbSDpmYjoia/mtKo='


base64.b64decode(base64.b64encode(xx.encode())): b'AABBCCDDEEFF'
base64.b64decode(base64.b64encode(yy.encode())): b'hello from Hogan'
base64.b64decode(base64.b64encode(zz.encode())): b'11223344556677889900aa'
base64.b64decode(base64.b64encode(kk.encode())): b'hello from \xe9\x99\x88\xe8\x89\xaf\xe6\xb4\xaa'


base64.b64encode(xx.encode()): QUFCQkNDRERFRUZG
base64.b64encode(yy.encode()): aGVsbG8gZnJvbSBIb2dhbg==
base64.b64encode(zz.encode()): MTEyMjMzNDQ1NTY2Nzc4ODk5MDBhYQ==
base64.b64encode(kk.encode()): aGVsbG8gZnJvbSDpmYjoia/mtKo=


base64.b64encode(xx.encode()): QUFCQkNDRERFRUZG
base64.b64encode(yy.encode()): aGVsbG8gZnJvbSBIb2dhbg==
base64.b64encode(zz.encode()): MTEyMjMzNDQ1NTY2Nzc4ODk5MDBhYQ==
base64.b64encode(kk.encode()): aGVsbG8gZnJvbSDpmYjoia/mtKo=


binascii.b2a_base64(xx.encode()): b'QUFCQkNDRERFRUZG\n'
binascii.b2a_base64(yy.encode()): b'aGVsbG8gZnJvbSBIb2dhbg==\n'
binascii.b2a_base64(zz.encode()): b'MTEyMjMzNDQ1NTY2Nzc4ODk5MDBhYQ==\n'
binascii.b2a_base64(kk.encode()): b'aGVsbG8gZnJvbSDpmYjoia/mtKo=\n'


binascii.a2b_base64(binascii.b2a_base64(xx.encode())): b'AABBCCDDEEFF'
binascii.a2b_base64(binascii.b2a_base64(yy.encode())): b'hello from Hogan'
binascii.a2b_base64(binascii.b2a_base64(zz.encode())): b'11223344556677889900aa'
binascii.a2b_base64(binascii.b2a_base64(kk.encode())): b'hello from \xe9\x99\x88\xe8\x89\xaf\xe6\xb4\xaa'


xx.encode(): b'AABBCCDDEEFF', xx.encode("gb2312"): b'AABBCCDDEEFF', xx.encode("GBK"): b'AABBCCDDEEFF'
yy.encode(): b'hello from Hogan', yy.encode("gb2312"): b'hello from Hogan', yy.encode("GBK"): b'hello from Hogan'
zz.encode(): b'11223344556677889900aa', zz.encode("gb2312"): b'11223344556677889900aa', zz.encode("GBK"): b'11223344556677889900aa'
kk.encode(): b'hello from \xe9\x99\x88\xe8\x89\xaf\xe6\xb4\xaa', kk.encode("gb2312"): b'hello from \xb3\xc2\xc1\xbc\xba\xe9', kk.encode("GBK"): b'hello from \xb3\xc2\xc1\xbc\xba\xe9'


bytearray(xx.encode()): bytearray(b'AABBCCDDEEFF')
bytearray(yy.encode()): bytearray(b'hello from Hogan')
bytearray(zz.encode()): bytearray(b'11223344556677889900aa')
bytearray(kk.encode()): bytearray(b'hello from \xe9\x99\x88\xe8\x89\xaf\xe6\xb4\xaa')


xx.encode(): b'AABBCCDDEEFF'
binascii.a2b_hex(xx): b'\xaa\xbb\xcc\xdd\xee\xff', binascii.unhexlify(xx): b'\xaa\xbb\xcc\xdd\xee\xff'
binascii.b2a_hex(xx): b'414142424343444445454646', binascii.hexlify(xx): b'414142424343444445454646'


yy.encode(): b'hello from Hogan'
binascii.b2a_hex(yy): b'68656c6c6f2066726f6d20486f67616e', binascii.hexlify(yy): b'68656c6c6f2066726f6d20486f67616e'


zz.encode(): b'11223344556677889900aa'
binascii.a2b_hex(zz): b'\x11"3DUfw\x88\x99\x00\xaa', binascii.unhexlify(zz): b'\x11"3DUfw\x88\x99\x00\xaa'
binascii.b2a_hex(zz): b'31313232333334343535363637373838393930306161', binascii.hexlify(zz): b'31313232333334343535363637373838393930306161'


binascii.a2b_uu(xx): b'\x86(\xa3\x8eI%\x96i\x80\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00', binascii.b2a_uu(xx): b',04%"0D-#1$1%149&\n'
```



