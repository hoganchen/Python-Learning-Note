### 正则表达式分组引用示例02之不分组

```
import re

match_str = '"\\\\Objects\\aon_gpio_output_input.axf" - 0 Error(s), 1 Warning(s).'
print(match_str)

match_obj = re.match(r'^"[^"]+"\s+-\s+(\d+)\s+Error\(s\),\s+(\d+)\s+Warning\(s\)\.$', match_str)

for i in range(3):
    print("match_obj.group({}): {}".format(i, match_obj.group(i)))

# 
match_obj = re.match(r'^"[^"]+"\s+-\s+(\d+)\s+Error(?:\(s\)),\s+(\d+)\s+Warning\((?:s)\)\.$', match_str)

for i in range(3):
    print("match_obj.group({}): {}".format(i, match_obj.group(i)))

match_obj = re.match(r'^"\\\\Objects[^"]+"\s+-\s+(\d+)\s+Error(?:\(s\)),\s+(\d+)\s+Warning\((?:s)\)\.$', match_str)

for i in range(3):
    print("match_obj.group({}): {}".format(i, match_obj.group(i)))
```

Output:

```
"\\Objects\aon_gpio_output_input.axf" - 0 Error(s), 1 Warning(s).
match_obj.group(0): "\\Objects\aon_gpio_output_input.axf" - 0 Error(s), 1 Warning(s).
match_obj.group(1): 0
match_obj.group(2): 1
match_obj.group(0): "\\Objects\aon_gpio_output_input.axf" - 0 Error(s), 1 Warning(s).
match_obj.group(1): 0
match_obj.group(2): 1
match_obj.group(0): "\\Objects\aon_gpio_output_input.axf" - 0 Error(s), 1 Warning(s).
match_obj.group(1): 0
match_obj.group(2): 1
```

选择匹配分组与不分组

```
import re
match_str_01 = '#define DICT_TUPLE_LIST_A0'
match_str_02 = '#define DICT_TUPLE_LIST_B0'

print(re.search(r'(_A0|_B0|_C0)$', match_str_01).group(0))
print(re.search(r'(_A0|_B0|_C0)$', match_str_01).group(1))

print(re.search(r'(_A0|_B0|_C0)$', match_str_02).group(0))
print(re.search(r'(_A0|_B0|_C0)$', match_str_02).group(1))

print(re.search(r'(?:_A0|_B0|_C0)$', match_str_01).group(0))
print(re.search(r'(?:_A0|_B0|_C0)$', match_str_02).group(0))

print(re.search(r'(?:_A0|_B0|_C0)$', match_str_01).group(1))
print(re.search(r'(?:_A0|_B0|_C0)$', match_str_02).group(1))


output:
_A0
_A0
_B0
_B0
_A0
_B0

---------------------------------------------------------------------------
IndexError                                Traceback (most recent call last)
<ipython-input-2-9a215a26e774> in <module>()
     12 print(re.search(r'(?:_A0|_B0)$', match_str_02).group(0))
     13 
---> 14 print(re.search(r'(?:_A0|_B0)$', match_str_01).group(1))
     15 print(re.search(r'(?:_A0|_B0)$', match_str_02).group(1))

IndexError: no such group
```



