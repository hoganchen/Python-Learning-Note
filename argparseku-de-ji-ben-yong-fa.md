### argparse库的基本用法

```
import argparse

'''
argparse内置6种动作可以在解析到一个参数时进行触发：

store 保存参数值，可能会先将参数值转换成另一个数据类型。若没有显式指定动作，则默认为该动作。

store_const 保存一个被定义为参数规格一部分的值，而不是一个来自参数解析而来的值。这通常用于实现非布尔值的命令行标记。

store_ture/store_false 保存相应的布尔值。这两个动作被用于实现布尔开关。

append 将值保存到一个列表中。若参数重复出现，则保存多个值。

append_const 将一个定义在参数规格中的值保存到一个列表中。

version 打印关于程序的版本信息，然后退出
'''
# http://blog.xiayf.cn/2013/03/30/argparse/
# https://docs.python.org/3/library/argparse.html
def parse_usage():
    parser = argparse.ArgumentParser()

    parser.add_argument('-s', action='store', dest='simple_value',
            help='Store a simple value')

    parser.add_argument('-c', action='store_const', dest='constant_value',
            const='value-to-store',
            help='Store a constant value')

    parser.add_argument('-t', action='store_true', default=False,
            dest='boolean_switch',
            help='Set a switch to true')
    parser.add_argument('-f', action='store_false', default=False,
            dest='boolean_switch',
            help='Set a switch to false')

    parser.add_argument('-a', action='append', dest='collection',
            default=[],
            help='Add repeated values to a list')

    parser.add_argument('-A', action='append_const', dest='const_collection',
            const='value-1-to-append',
            default=[],
            help='Add different values to list')
    parser.add_argument('-B', action='append_const', dest='const_collection',
            const='value-2-to-append',
            help='Add different values to list')

    parser.add_argument('--version', action='version', version='%(prog)s 1.0')

    results = parser.parse_args()
    print('simple_value     =', results.simple_value)
    print('constant_value   =', results.constant_value)
    print('boolean_switch   =', results.boolean_switch)
    print('collection       =', results.collection)
    print('const_collection =', results.const_collection)

    return results


def simple_usage():
    parser = argparse.ArgumentParser()
    parser.add_argument('-o', '--output', dest='out')
    parser.add_argument('--address', '-a')
    parser.add_argument('-p')
    parser.add_argument('-v', dest='verbose', action='store_true')
    args = parser.parse_args()
    # print('output: {}'.format(args.output))
    print('output: {}'.format(args.out))
    print('address: {}'.format(args.address))
    print('p: {}'.format(args.p))
    print('verbose: {}'.format(args.verbose))


def pts_args_usage():
    parser = argparse.ArgumentParser()

    parser.add_argument('-m', '--module', action='store', dest='module_name', default='all',
        help='The module name["GAP", "GATTCL", "GATTSR", "L2CAP", "SM] of pts test, "all" means to run all modules')
    parser.add_argument('-t', '--type', action='store', dest='target_name', default='full',
        help='The test type of pts test, "full" means to run full test, "release" means to run release test, "A0" means to run A0 test')

    args = parser.parse_args()

    return args


def main():
    # simple_usage()

    args_result = pts_args_usage()
    print('module_name     =', args_result.module_name)
    print('target_name     =', args_result.target_name)

    # args_result = parse_usage()
    # print('simple_value     =', args_result.simple_value)
    # print('constant_value   =', args_result.constant_value)
    # print('boolean_switch   =', args_result.boolean_switch)
    # print('collection       =', args_result.collection)
    # print('const_collection =', args_result.const_collection)


if __name__ == '__main__':
    main()
```



