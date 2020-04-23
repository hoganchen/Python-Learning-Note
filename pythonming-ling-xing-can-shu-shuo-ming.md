### Python命令行参数说明

http://chenjiee815.github.io/pythonming-ling-xing-can-shu.html

```
Contents

    接口参数
    通用参数
    各式各样的参数
    弃用参数

Note

此处只列出了 CPython 实现版本支持的命令行参数，其它实现版本请查看其官方文档。

Python 命令行调用的基本格式如下：

$ python [-BdEiOQsRStuUvVWxX3?] [-c command | -m module-name | script | - ] [args]

接口参数

用来指定不同的方式调用 Python 代码。

-c <command>

    直接将 Python 代码作为 <command> 来执行。

-m <module-name>

    查找 sys.path 中指定的模块名并执行其 __main__ 模块中的代码。

    Note

    无法用来指定 Python 内置模块和其它语言编写的 Python 扩展模块。

-

    从标准输入读入 Python 代码。

<script>

    指定执行某个含有 Python 代码的文件或者包含 __main__.py 文件的目录 /Zip 包。

通用参数

-? -h --help

    打印帮助信息 

-V --version

    打印版本信息 

各式各样的参数

-B

    在导入源码模块时不生成 .pyc 或 .pyo 文件。

-d

    打开调试信息输出。

-E

    忽略所有的 Python 环境变量。

-i

    强制 Python 以交互式模式启动，即使以 -c 或者 <script> 模式启动。

-O

    打开最基本的编译优化。-OO: 进一步优化，忽略 docstrings 。

-Q <arg>

    控制 Python 的除法行为。<arg> 为下面四个值之一：

        old, 默认值，int/int == int, long/long == log
        new, int/int == float, long/long == flat
        warn, 和 old 一样，但是有 warning 提示。
        warnall, 对所有的除法行为都有 warning 提示。

    $ python -Q new
    Python 2.7.5+ (default, Feb 27 2014, 19:39:55)
    [GCC 4.8.1] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 1 / 1
    1.0
    >>> 2334534543534534234234234234/ 134323435345456456576575654534
    0.01737994965309306
    >>>

    $ python -Q warnall
    Python 2.7.5+ (default, Feb 27 2014, 19:39:55)
    [GCC 4.8.1] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 1. 1
      File "<stdin>", line 1
        1. 1
           ^
    SyntaxError: invalid syntax
    >>> 1 /1
    __main__:1: DeprecationWarning: classic int division
    1
    >>> 1.0 /1
    __main__:1: DeprecationWarning: classic float division
    1.0
      File "<stdin>", line 1
        1. 1
           ^
    SyntaxError: invalid syntax

-r

    使用随机值在生成不可以变对象的 Hash。

-s

    不会将用户的 site-packages 添加到 sys.path 中。

-S

    忽略导入 site 模块和操作 sys.path 所依赖的模块。多用于受限的环境。

-t

    如果 Python 代码中混用了 TAB 和空格来进行缩进，则进行 warning 提示。

    -tt: 进行 error 提示。

-u

    强制使 stdin/stdout/stderr 不进行缓存操作。

-v

    显示调试信息。-vv 显示更多的调试信息。

-W arg

    告警控制。Python 默认的告警提示输出到 stderr，其一般格式为：

        file:line: category: message

    arg 的值为以下值其中之一：

        ignore, 忽略所有告警提示
        default, 每一行源码打印一个告警
        all, 每次出现告警都打印
        module, 每种告警在某模块中第一次出现时打印
        once, 每种告警仅打印一次
        error, 抛出异常，而不是打印出 warning 提示

    Tip

    也可以通过 warings 模块来进行告警控制。

-x

    忽略源码的第一行。

    Note

    一般第一行为 #!/user/bin/env python， 多用于 DOS 系统。

-3

    当在 Python2 中使用 Python3 中移除或者显著改变的特性时，进行 DeprecationWarning 提示。

弃用参数

-J

    用来调用 Jython

-U

    将所有字符串都当作 unicode 字符串来处理。但不建议这么用。

    最好在代码中使用 from __future__ import unicode_literals

-X

    调用其它的 Python 实现版本。
```





