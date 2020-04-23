### 正则表达式替换大括号转义

[http://blog.xiayf.cn/2013/01/26/python-string-format/](http://blog.xiayf.cn/2013/01/26/python-string-format/)

[https://www.bufeihua.cn/p/57373c42b98b3512bf9fd042](https://www.bufeihua.cn/p/57373c42b98b3512bf9fd042)

```
'''
'hello {name}.format(name='world')'的时候大括号是特殊转义字符，如果需要原始的大括号，用{{代替{, 用}}代替}， 如下:

>>> 'hello {{worlds in braces!}}, {name}'.format(name='zhangsan')
'hello {worlds in braces!}, zhangsan'

受以上启发，所以当替换的内容包含大括号，大括号就需要转义，用{{表示一个{， 用}}表示一个}
'''

import re

class ArgsParameter(object):
    def __init__(self):
        self.module_name = 'all'
        self.target_name = 'full'
        self.flash_mode = 'xip'
        self.svn_revision = '4173'
        # self.svn_revision = None


html_header_template = """<!DOCTYPE html>
<html>
<head>
    <title>{0} Test Summary Report</title>
</head>
<body>
<center>
    <font color="red" size="5">{0} Test Summary Report</font>
</center>
<br>
<br>
<table border="1" bgcolor="Lavender" color="" align="center">
<tr><th>Module Name</th><th>Case Number</th><th>Passed Number</th><th>Failed Number</th><th>Timeout Number</th><th>Skipped Number</th></tr>
"""

args_param = ArgsParameter()

if args_param is not None:
    target_name = args_param.target_name
    svn_revision = args_param.svn_revision

    if svn_revision is None:
        html_header = html_header_template.format(target_name.capitalize())
    else:
        fpga_version = 'R07_15a_B0718a_b2_k7'
        html_header_template = re.sub(r'</center>\n<br>\n<br>',
                                      r'</center>\n<br><style>p.indent{{ padding-left: 19.6em; line-height: 0.0}}</style>\n<p class="indent"><b>SVN Revision: {1}</b></p>\n<p class="indent"><b>FPGA Version: {2}</b></p>',
                                      html_header_template)
        html_header = html_header_template.format(target_name.capitalize(), svn_revision, fpga_version)
else:
    html_header = html_header_template.format('Automation')

print(html_header)
```



