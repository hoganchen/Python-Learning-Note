### 正则表达式分组引用示例01

[http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html](http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html)

[https://www.crifan.com/files/doc/docbook/python\_topic\_re/release/html/python\_topic\_re.html](https://www.crifan.com/files/doc/docbook/python_topic_re/release/html/python_topic_re.html)

[https://www.crifan.com/python\_re\_sub\_detailed\_introduction/](https://www.crifan.com/python_re_sub_detailed_introduction/)

[https://blog.csdn.net/SeeTheWorld518/article/details/49302829](https://blog.csdn.net/SeeTheWorld518/article/details/49302829)

[https://www.cnblogs.com/jiayongji/p/7118967.html](https://www.cnblogs.com/jiayongji/p/7118967.html)

[https://www.cnblogs.com/jiayongji/p/7586906.html](https://www.cnblogs.com/jiayongji/p/7586906.html)

[https://blog.csdn.net/dnxbjyj/article/details/70837505](https://blog.csdn.net/dnxbjyj/article/details/70837505)

[https://blog.csdn.net/caianye/article/details/6901289](https://blog.csdn.net/caianye/article/details/6901289)

[http://www.cnblogs.com/lovychen/p/5167261.html](http://www.cnblogs.com/lovychen/p/5167261.html)

```
'''
Return the string obtained by replacing the leftmost non-overlapping occurrences of pattern in string by the replacement repl. 
If the pattern isn’t found, string is returned unchanged. repl can be a string or a function; 
if it is a string, any backslash escapes in it are processed. 
That is, \n is converted to a single newline character, \r is converted to a carriage return, and so forth. 
Unknown escapes such as \& are left alone. Backreferences, such as \6, 
are replaced with the substring matched by group 6 in the pattern. For example:

>>> re.sub(r'def\s+([a-zA-Z_][a-zA-Z_0-9]*)\s*\(\s*\):',
...        r'static PyObject*\npy_\1(void)\n{',
...        'def myfunc():')
'static PyObject*\npy_myfunc(void)\n{'

If repl is a function, it is called for every non-overlapping occurrence of pattern. 
The function takes a single match object argument, and returns the replacement string. For example:

>>> def dashrepl(matchobj):
...     if matchobj.group(0) == '-': return ' '
...     else: return '-'
>>> re.sub('-{1,2}', dashrepl, 'pro----gram-files')
'pro--gram files'
>>> re.sub(r'\sAND\s', ' & ', 'Baked Beans And Spam', flags=re.IGNORECASE)
'Baked Beans & Spam'

The pattern may be a string or an RE object.

The optional argument count is the maximum number of pattern occurrences to be replaced; 
count must be a non-negative integer. If omitted or zero, all occurrences will be replaced. 
Empty matches for the pattern are replaced only when not adjacent to a previous match, 
so sub('x*', '-', 'abc') returns '-a-b-c-'.

In string-type repl arguments, in addition to the character escapes and backreferences described above, 
\g<name> will use the substring matched by the group named name, as defined by the (?P<name>...) syntax. 
\g<number> uses the corresponding group number; \g<2> is therefore equivalent to \2, 
but isn’t ambiguous in a replacement such as \g<2>0. \20 would be interpreted as a reference to group 20, 
not a reference to group 2 followed by the literal character '0'. 
The backreference \g<0> substitutes in the entire substring matched by the RE.

Changed in version 3.1: Added the optional flags argument.
Changed in version 3.5: Unmatched groups are replaced with an empty string.
Changed in version 3.6: Unknown escapes in pattern consisting of '\' and an ASCII letter now are errors.
Deprecated since version 3.5, will be removed in version 3.7: Unknown escapes in repl consisting of '\' and an ASCII letter now raise a deprecation warning and will be forbidden in Python 3.7.
'''

'''
http://www.cnblogs.com/huxi/archive/2010/07/04/1771073.html
sub(repl, string[, count]) | re.sub(pattern, repl, string[, count]):
使用repl替换string中每一个匹配的子串后返回替换后的字符串。
当repl是一个字符串时，可以使用\id或\g<id>、\g<name>引用分组，但不能使用编号0。
当repl是一个方法时，这个方法应当只接受一个参数（Match对象），并返回一个字符串用于替换（返回的字符串中不能再引用分组）。
count用于指定最多替换次数，不指定时全部替换。 
'''

import re
line_data = '#define APP_CODE_RUN_ADDR       0x10010000    /**<Code run address. */'
match_obj = re.search(r'#define APP_CODE_RUN_ADDR(\s+)\w+', line_data)
new_line_data = re.sub(r'APP_CODE_RUN_ADDR(\s+)\w+', r'APP_CODE_RUN_ADDR\g<1>0x30010000',line_data)
print(new_line_data)

inputStr = "hello crifan, nihao crifan";
replacedStr = re.sub(r"hello (\w+), nihao \1", "\g<1>", inputStr);
print("replacedStr=",replacedStr); #crifan

inputStr = "hello crifan, nihao crifan";
replacedStr = re.sub(r"hello (?P<name>\w+), nihao (?P=name)", "\g<name>", inputStr);
print("replacedStr=",replacedStr); #crifan
```



