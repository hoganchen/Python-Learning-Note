### 排序算法

```
# -*- coding:utf-8 -*-

"""
@Author:    hogan.chen@ymail.com
@Date:      2017-12-05
@Version:   V0.1.20171205
"""

import copy
import time
import logging
import datetime


LOGGING_LEVEL = logging.DEBUG


def logging_config():
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[line: %(lineno)d] - %(levelname)s - %(message)s"
    logging.basicConfig(level=LOGGING_LEVEL, format=log_format)


def bubble_sort(items_list):
    time_start_loc = time.time()

    if 1 >= len(items_list):
        return items_list

    """
    >>> range(10, 0, -1)
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    >>> range(10)
    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    """
    for index in range(len(items_list) - 1, 0, -1):
        flag = False

        for sub_index in range(index):
            if items_list[sub_index] > items_list[sub_index + 1]:
                items_list[sub_index], items_list[sub_index + 1] = items_list[sub_index + 1], items_list[sub_index]
                flag = True

        if not flag:
            break

    print('For bubble_sort, total elapsed time: %s seconds\n' % (time.time() - time_start_loc))
    return items_list


def copy_list(items_list, new_list):
    for item in items_list:
        if isinstance(item, list) or isinstance(item, tuple):
            list_temp = []
            new_list.append(list_temp)
            copy_list(item, list_temp)
        elif isinstance(item, dict):
            dict_temp = {}
            new_list.append(dict_temp)
            copy
        else:
            new_list.append(item)


def copy_list_verify():
    a_list = [{'a': 100}, 1, 2, [3, 4, [8, 9, (10, 11)]], [5]]
    b_list = []
    logging.debug(a_list)
    copy_list(a_list, b_list)
    a_list[4].append(6)
    a_list.append(7)
    a_list[0]['a'] = 200
    logging.debug(a_list)
    logging.debug(b_list)


def main():
    copy_list_verify()
    pass


if __name__ == "__main__":
    print('Script start execution at %s\n\n' % str(datetime.datetime.now()))
    time_start = time.time()

    logging_config()
    main()

    print('\n\nScript end execution at %s' % str(datetime.datetime.now()))
    print('Total Elapsed Time: %s seconds\n' % (time.time() - time_start))

```



