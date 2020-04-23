### Python遍历查找文件夹下某类文件

```
def get_all_files_with_suffix_by_os_walk(dir_path, suffix=None):
    log_file_path_list = []

    if os.path.exists(dir_path) and os.path.isdir(dir_path):
        for folder, sub_folder, file_list in os.walk(dir_path):
            # logging.debug('folder: {}, sub_folder: {}, file_list: {}'.format(folder, sub_folder, file_list))

            for file_name in file_list:
                file_path = os.path.join(folder, file_name)

                if suffix is None:
                    log_file_path_list.append(file_path)
                else:
                    if re.search(r'{}$'.format(re.escape(suffix)), os.path.split(file_path)[1]):
                        log_file_path_list.append(file_path)

    logging.debug('Log File list: {}'.format(log_file_path_list))
    return log_file_path_list


def get_all_files_with_suffix(dir_path, suffix=None):
    log_file_path_list = []

    if os.path.exists(dir_path) and os.path.isdir(dir_path):
        file_list = os.listdir(dir_path)

        for file in file_list:
            file_path = os.path.join(dir_path, file)

            if os.path.isdir(file_path):
                continue
                # not support recursion getting
                # get_all_files_with_suffix(file_path, suffix)
            else:
                if suffix is None:
                    log_file_path_list.append(file_path)
                else:
                    if re.search(r'{}$'.format(re.escape(suffix)), os.path.split(file_path)[1]):
                        log_file_path_list.append(file_path)

                    """
                     if -1 != os.path.split(file_path)[1].find(r'{}'.format(suffix)):
                        log_file_path_list.append(file_path)
                   """
                """
                if suffix is None:
                    log_file_path_list.append(file_path)
                else:
                    # suffix = ['log', 'xml']
                    if isinstance(suffix, list) or isinstance(suffix, tuple):
                        if os.path.splitext(os.path.split(file_path)[1])[1][1:] in suffix:
                            log_file_path_list.append(file_path)
                    elif isinstance(suffix, str):
                        # suffix = '*.log'
                        # if '*.' in suffix:  # abc*.log will match this condition
                        if 0 == suffix.find(r'*.'):
                            if fnmatch.fnmatch(file_path, suffix):
                                log_file_path_list.append(file_path)
                        else:
                            # abc*d.log ==> log
                            suffix = re.sub(r'^.*\.', '', suffix)
                            if os.path.splitext(os.path.split(file_path)[1])[1][1:] == suffix:
                                log_file_path_list.append(file_path)
                """

    logging.debug('Log File list: {}'.format(log_file_path_list))
    return log_file_path_list

```



