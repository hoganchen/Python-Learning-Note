### Python浅拷贝与深拷贝

```
import copy

# 如何拷贝一个列表对象
# 第一种方法，浅拷贝
a = [1, 2, 3]
b = {'a':1, 'b':2, 'c':3}
old_list = [a, b, 4, 5, 6]
new_list = old_list[:]
print('id(a): {}, id(b): {}'.format(id(a), id(b)))
print('id(old_list): {}, id(old_list[0]): {}, id(old_list[1]): {}'.format(id(old_list), id(old_list[0]), id(old_list[1])))
print('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))

# 第二种方法，浅拷贝
new_list = list(old_list)
print('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))

# 第三种方法：
# 浅拷贝
new_list = copy.copy(old_list)
print('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))
# 深拷贝
new_list = copy.deepcopy(old_list)
print('id(new_list): {}, id(new_list[0]): {}, id(new_list[1]): {}'.format(id(new_list), id(new_list[0]), id(new_list[1])))


test_info_dict = {
    'basic_project_list': (
        {
            'project_name': 'AA',
            'compile_flag': True,
            'download_flag': False,
            'run_flag': False,
            'project_path': '..\\..\\..\\AA.uvprojx',
        },

        {
            'project_name': 'BB',
            'compile_flag': True,
            'download_flag': False,
            'run_flag': False,
            'project_path': '..\\..\\..\\BB.uvprojx',
        },

        {
            'project_name': 'CC',
            'compile_flag': True,
            'download_flag': False,
            'run_flag': False,
            'project_path': '..\\..\\..\\CC.uvprojx',
        },

        {
            'project_name': 'DD',
            'compile_flag': True,
            'download_flag': False,
            'run_flag': False,
            'project_path': '..\\..\\..\\DD.uvprojx',
        },
    ),
}

app_project = {
    'project_name': 'app',
    'compile_flag': True,
    'download_flag': True,
    'run_flag': True,
    'project_path': '..\\..\\..\\app.uvprojx',
}


project_list = []
basic_project_list = test_info_dict['basic_project_list']

for basic_project in basic_project_list:
    project_list.append(basic_project)

project_list.append(app_project)

print('project_list: {}'.format(project_list))

app_project['run_flag'] = False

print('project_list: {}'.format(project_list))
```

Output:

```
id(a): 140303968731272, id(b): 140303968735880
id(old_list): 140303968731400, id(old_list[0]): 140303968731272, id(old_list[1]): 140303968735880
id(new_list): 140303968731336, id(new_list[0]): 140303968731272, id(new_list[1]): 140303968735880
id(new_list): 140303968731720, id(new_list[0]): 140303968731272, id(new_list[1]): 140303968735880
id(new_list): 140303968731336, id(new_list[0]): 140303968731272, id(new_list[1]): 140303968735880
id(new_list): 140303968731720, id(new_list[0]): 140303969379528, id(new_list[1]): 140303968736312
project_list: [{'project_name': 'AA', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\AA.uvprojx'}, {'project_name': 'BB', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\BB.uvprojx'}, {'project_name': 'CC', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\CC.uvprojx'}, {'project_name': 'DD', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\DD.uvprojx'}, {'project_name': 'app', 'compile_flag': True, 'download_flag': True, 'run_flag': True, 'project_path': '..\\..\\..\\app.uvprojx'}]
project_list: [{'project_name': 'AA', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\AA.uvprojx'}, {'project_name': 'BB', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\BB.uvprojx'}, {'project_name': 'CC', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\CC.uvprojx'}, {'project_name': 'DD', 'compile_flag': True, 'download_flag': False, 'run_flag': False, 'project_path': '..\\..\\..\\DD.uvprojx'}, {'project_name': 'app', 'compile_flag': True, 'download_flag': True, 'run_flag': False, 'project_path': '..\\..\\..\\app.uvprojx'}]

```



