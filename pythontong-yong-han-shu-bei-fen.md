### Python通用函数备份

* ###### 通用备份

```
def zip_folder(folder_path, zip_file_path_name=None):
    if zip_file_path_name is None:
        now_time_str = datetime.datetime.now().strftime('%Y%m%d_%H%M%S')
        zip_file_path_name = os.path.join(os.path.curdir,
                                          '{}_{}.zip'.format(os.path.split(folder_path)[1], now_time_str))

    tc_zip_file = zipfile.ZipFile(zip_file_path_name, 'w', compression=zipfile.ZIP_DEFLATED)

    for folder, sub_folders, files in os.walk(folder_path):
        for file_name in files:
            while True:
                try:
                    tc_zip_file.write(os.path.join(folder, file_name),
                                      arcname=os.path.relpath(os.path.join(folder, file_name),
                                                              os.path.dirname(folder_path)))
                except Exception as err:
                    logging.critical('Zip handle error happened, the error information: {}'.format(err))
                else:
                    break

    tc_zip_file.close()


def get_all_files_with_suffix(dir_path, suffix=None):
    target_file_path_list = []

    if os.path.exists(dir_path) and os.path.isdir(dir_path):
        file_list = os.listdir(dir_path)

        for file in file_list:
            file_path = os.path.join(dir_path, file)

            if os.path.isdir(file_path):
                continue
                # do not support recursion getting target file, recommend to use get_all_files_with_suffix_by_os_walk
                # get_all_files_with_suffix(file_path, suffix)
            else:
                if suffix is None:
                    target_file_path_list.append(file_path)
                else:
                    if re.search(r'{}$'.format(re.escape(suffix)), os.path.split(file_path)[1]):
                        target_file_path_list.append(file_path)
                    """
                     if -1 != os.path.split(file_path)[1].find(r'{}'.format(suffix)):
                        target_file_path_list.append(file_path)
                   """
                """
                if suffix is None:
                    target_file_path_list.append(file_path)
                else:
                    # suffix = ['log', 'xml']
                    if isinstance(suffix, list) or isinstance(suffix, tuple):
                        if os.path.splitext(os.path.split(file_path)[1])[1][1:] in suffix:
                            target_file_path_list.append(file_path)
                    elif isinstance(suffix, str):
                        # suffix = '*.log'
                        # if '*.' in suffix:  # abc*.log will match this condition
                        if 0 == suffix.find(r'*.'):
                            if fnmatch.fnmatch(file_path, suffix):
                                target_file_path_list.append(file_path)
                        else:
                            # abc*d.log ==> log
                            suffix = re.sub(r'^.*\.', '', suffix)
                            if os.path.splitext(os.path.split(file_path)[1])[1][1:] == suffix:
                                target_file_path_list.append(file_path)
                """

    logging.debug('File list with suffix({}): {}'.format(suffix, target_file_path_list))
    return target_file_path_list


def get_all_files_with_suffix_by_os_walk(dir_path, suffix=None):
    target_file_path_list = []

    if os.path.exists(dir_path) and os.path.isdir(dir_path):
        for folder, sub_folder, file_list in os.walk(dir_path):
            # logging.debug('folder: {}, sub_folder: {}, file_list: {}'.format(folder, sub_folder, file_list))

            for file_name in file_list:
                file_path = os.path.join(folder, file_name)

                if suffix is None:
                    target_file_path_list.append(file_path)
                else:
                    # if re.search(r'{}$'.format(suffix), os.path.split(file_path)[1]):
                    if re.search(r'{}$'.format(re.escape(suffix)), os.path.split(file_path)[1]):
                        target_file_path_list.append(file_path)

    logging.debug('File list with suffix({}): {}'.format(suffix, target_file_path_list))

    return target_file_path_list


def kill_process_by_pid(pid):
    subprocess.run('taskkill /F /T /PID {}'.format(pid), stdin=None, stdout=None, stderr=None)


class RunCommand(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout):
        logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

        '''
        A Popen creationflags parameter to specify that a new process group will be created.
        This flag is necessary for using os.kill() on the subprocess.
        '''
        # self.process = subprocess.Popen(self.cmd, shell=False, creationflags=subprocess.CREATE_NEW_PROCESS_GROUP)
        self.process = subprocess.Popen(self.cmd, shell=False)

        try:
            self.process.communicate(timeout=timeout)
        except subprocess.TimeoutExpired:
            logging.info('The command execute TimeoutExpired, start to terminating process with pid {}...'.
                         format(self.process.pid))

            kill_process_by_pid(self.process.pid)
            self.process.kill()


def remove_file(file_name):
    if os.path.exists(file_name):
        if os.path.isdir(file_name):
            shutil.rmtree(file_name)
        else:
            os.remove(file_name)


def copy_file(src_file, dst_file):
    if os.path.exists(src_file):
        if os.path.exists(dst_file):
            remove_file(dst_file)

        if os.path.isdir(src_file):
            shutil.copytree(src_file, dst_file)
        else:
            shutil.copy(src_file, dst_file)


def move_file(src_file, dst_file):
    if os.path.exists(src_file):
        if os.path.exists(dst_file):
            remove_file(dst_file)

        shutil.move(src_file, dst_file)
```

* ###### 命令行参数读取

```
def get_command_line_parameter():
    parser = argparse.ArgumentParser()

    parser.add_argument('-t', '--target', action='store', dest='target_name', default='Dev_01',
                        help='The target board')
    parser.add_argument('-v', '--version', nargs='+', action='store', dest='version_list', default=['alpha'],
                        help='The test version list')


    args_param = parser.parse_args()

    return args_param

# usage    
python -u main.py -v alpha beta -t Dev_99
```

* ###### subprocess获取返回数据

```
build_info_str = subprocess.run(['findstr', 'Error(s)', '{}'.format(build_log)], check=True,
                                stdout=subprocess.PIPE, stderr=subprocess.PIPE).stdout.decode('utf-8')
```

* ###### remove, copy, move的优化

```
import os
import shutil


def remove_file(file_name):
    if os.path.exists(file_name):
        if os.path.isdir(file_name):
            shutil.rmtree(file_name)
        else:
            os.remove(file_name)


def copy_file(src_file, dst_file):
    if os.path.exists(src_file):
        if os.path.isdir(src_file) and os.path.exists(dst_file):
            remove_file(dst_file)

        if os.path.isdir(src_file)：
            shutil.copytree(src_file, dst_file)
        else:
            shutil.copy(src_file, dst_file)


def move_file(src_file, dst_file):
    if os.path.exists(src_file):
        if os.path.exists(dst_file) and os.path.isfile(dst_file):
            remove_file(dst_file)

        shutil.move(src_file, dst_file)
```

* ###### zip压缩文件夹

```
def zip_folder(folder_path, zip_file_path_name=None, change_dir=False):
    cur_dir = os.path.curdir

    if change_dir:
        os.chdir(os.path.split(folder_path)[0])

    if zip_file_path_name is None:
        # now_time_str = datetime.datetime.now().strftime('%Y%m%d_%H%M%S')
        # zip_file_path_name = os.path.join(os.path.curdir,
        #                                   '{}_{}.zip'.format(os.path.split(folder_path)[1], now_time_str))

        zip_file_path_name = os.path.join(os.path.curdir, '{}.zip'.format(os.path.split(folder_path)[1]))

    tc_zip_file = zipfile.ZipFile(zip_file_path_name, 'w', compression=zipfile.ZIP_DEFLATED)

    for folder, sub_folders, files in os.walk(folder_path):
        for file_name in files:
            while True:
                try:
                    tc_zip_file.write(os.path.join(folder, file_name),
                                      arcname=os.path.relpath(os.path.join(folder, file_name),
                                                              os.path.dirname(folder_path)))
                except Exception as err:
                    logging.critical('Zip handle error happened, the error information: {}'.format(err))
                else:
                    break

    tc_zip_file.close()

    os.chdir(cur_dir)
```

* ###### 清空文件夹

```
def clean_folder(folder_path):
    if os.path.exists(folder_path):
        if not os.path.isdir(folder_path):
            try:
                os.remove(folder_path)
                os.makedirs(folder_path)
            finally:
                pass
        else:
            try:
                shutil.rmtree(folder_path)
                os.makedirs(folder_path)
            finally:
                pass
    else:
        try:
            os.makedirs(folder_path)
        finally:
            pass


def clean_folder(folder_path, file_list=None):
    if os.path.exists(folder_path):
        if not os.path.isdir(folder_path):
            try:
                os.remove(folder_path)
                os.makedirs(folder_path)
            finally:
                pass
        else:
            if file_list is None:
                clean_file_list = os.listdir(folder_path)
            else:
                clean_file_list = file_list

            for file_name in clean_file_list:
                file_path = os.path.join(folder_path, file_name)

                try:
                    if os.path.isdir(file_path):
                        shutil.rmtree(file_path)
                    else:
                        os.remove(file_path)
                finally:
                    pass
    else:
        try:
            os.makedirs(folder_path)
        finally:
            pass
```

* ###### 



