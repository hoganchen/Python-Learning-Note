### 正则表达式使用技巧

* ##### 替换大括号

```
>>> '{{}}'.format('0x11')
'{}'
>>> '\{{}\}'.format('0x11')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Single '}' encountered in format string
>>> '\{{}}'.format('0x11')
'\\{}'
>>> '{{}}'.format('0x11')
'{}'
>>> '{}'.format('0x11')
'0x11'
>>> '{{}\}'.format('0x11')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Single '}' encountered in format string
>>> '{{{}}}'.format('0x11')
'{0x11}'
```

* ##### 每隔N个字符分组

```
>>> re.findall(r'.{2,2}', 'FB89674523F1')
['FB', '89', '67', '45', '23', 'F1']
>>> ' '.join(re.findall(r'.{2,2}', 'FB89674523F1'))
'FB 89 67 45 23 F1'
>>> ' '.join(re.findall(r'.{2,2}', 'FB89674523F12'))
'FB 89 67 45 23 F1'
>>> ' '.join(re.findall(r'.{2,1}', 'FB89674523F12'))
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/chenlianghong/anaconda3/lib/python3.6/re.py", line 222, in findall
    return _compile(pattern, flags).findall(string)
  File "/home/chenlianghong/anaconda3/lib/python3.6/re.py", line 301, in _compile
    p = sre_compile.compile(pattern, flags)
  File "/home/chenlianghong/anaconda3/lib/python3.6/sre_compile.py", line 562, in compile
    p = sre_parse.parse(p, flags)
  File "/home/chenlianghong/anaconda3/lib/python3.6/sre_parse.py", line 855, in parse
    p = _parse_sub(source, pattern, flags & SRE_FLAG_VERBOSE, 0)
  File "/home/chenlianghong/anaconda3/lib/python3.6/sre_parse.py", line 416, in _parse_sub
    not nested and not items))
  File "/home/chenlianghong/anaconda3/lib/python3.6/sre_parse.py", line 606, in _parse
    source.tell() - here)
sre_constants.error: min repeat greater than max repeat at position 2
>>> ' '.join(re.findall(r'.{1,2}', 'FB89674523F12'))
'FB 89 67 45 23 F1 2'
>>> ' '.join(re.findall(r'.{1,2}', 'FB89674523F1'))
'FB 89 67 45 23 F1'
>>> ' '.join(re.findall(r'.{2,2}', 'FB89674523F1'))
'FB 89 67 45 23 F1'
>>> ' '.join(re.findall(r'.{2}', 'FB89674523F1'))
'FB 89 67 45 23 F1'
>>> ' '.join(re.findall(r'.{2, 3}', 'FB89674523F1'))
''
>>> ' '.join(re.findall(r'.{2,3}', 'FB89674523F1'))
'FB8 967 452 3F1'
>>> ' '.join(re.findall(r'.{1,3}', 'FB89674523F1'))
'FB8 967 452 3F1'
>>> ' '.join(re.findall(r'.{1,}', 'FB89674523F1'))
'FB89674523F1'
```

* ##### 字符串按单词翻转

```
>>> re.sub(r'[0x,]', '', '0xf1, 0x23, 0x45, 0x67, 0x89, 0xfb')
'f1 23 45 67 89 fb'
>>> re.sub(r'[0x,]', '', '0xf1, 0x23, 0x45, 0x67, 0x89, 0xfb').split()[::-1]
['fb', '89', '67', '45', '23', 'f1']
>>> ''.join(re.sub(r'[0x,]', '', '0xf1, 0x23, 0x45, 0x67, 0x89, 0xfb').split()[::-1])
'fb89674523f1'
>>> ''.join(re.sub(r'[0x,]', '', '0xf1, 0x23, 0x45, 0x67, 0x89, 0xfb').split()[::-1]).upper()
'FB89674523F1'
>>> ''.join(re.sub(r'[0x,]', '', '0xf1, 0x23, 0x45, 0x67, 0x89, 0xfb').split()[::-1]).upper()
'FB89674523F1'

>>> x
'0xc6, 0x50, 0xb4, 0x25'
>>> '0x{}'.format(''.join(x.replace('0x', '').replace(',', '').split()[::-1]).upper().lower())
'0x25b450c6'
>>>
```

* ##### re.escape的作用

```
import re

path_name = '..\\..\\hello\\from\\testtxt'
path = '..\\..\\hello\\from\\test.txt'
suffix='.txt'

match_obj = re.search(r'{}$'.format(suffix), path)
if match_obj:
    print(match_obj.group(0))
else:
    print('not match')

match_obj = re.search(r'{}$'.format(re.escape(suffix)), path)
if match_obj:
    print(match_obj.group(0))
else:
    print('not match')

match_obj = re.search(r'{}$'.format(suffix), path_name)
if match_obj:
    print(match_obj.group(0))
else:
    print('not match')

match_obj = re.search(r'{}$'.format(re.escape(suffix)), path_name)
if match_obj:
    print(match_obj.group(0))
else:
    print('not match')


output:
.txt
.txt
ttxt
not match
```

* ##### 



