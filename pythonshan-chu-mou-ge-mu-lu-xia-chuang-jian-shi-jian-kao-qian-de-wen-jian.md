### Python删除某个目录下创建时间靠前的文件

[https://tangx.in/2016/11/24/python-libaray-shutil-shell-command-for-python/](https://tangx.in/2016/11/24/python-libaray-shutil-shell-command-for-python/)

[https://blog.csdn.net/liudinglong1989/article/details/78731754](https://blog.csdn.net/liudinglong1989/article/details/78731754)

[http://www.cnblogs.com/gongxr/p/7351858.html](http://www.cnblogs.com/gongxr/p/7351858.html)

[https://www.cnblogs.com/pipihaoke/p/8034395.html](https://www.cnblogs.com/pipihaoke/p/8034395.html)

[http://python3-cookbook-personal.readthedocs.io/zh\_CN/stable/c05/p13\_get\_directory\_listing.html](http://python3-cookbook-personal.readthedocs.io/zh_CN/stable/c05/p13_get_directory_listing.html)

[https://blog.csdn.net/cdw\_FstLst/article/details/50009203](https://blog.csdn.net/cdw_FstLst/article/details/50009203)

[http://www.cnblogs.com/funsion/p/4017989.html](http://www.cnblogs.com/funsion/p/4017989.html)

[http://www.techug.com/post/9-ways-copy-file-in-python.html](http://www.techug.com/post/9-ways-copy-file-in-python.html)

```
def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


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


def archive_test_logs():
    logging.info('Start to backup the test log files and delete unnecessary archive files ...')

    archive_folder_path = '.\\Archive_Logs'

    if os.path.exists(archive_folder_path):
        if not os.path.isdir(archive_folder_path):
            try:
                os.remove(archive_folder_path)
                os.makedirs(archive_folder_path)
            finally:
                pass
    else:
        try:
            os.makedirs(archive_folder_path)
        finally:
            pass

    archive_file_list = get_all_files_with_suffix('.\\', suffix='.zip')

    for archive_file in archive_file_list:
        # shutil.copy2(os.path.join('.\\', archive_file), archive_folder_path)
        if False:
            shutil.copy2(archive_file, archive_folder_path)
            os.remove(archive_file)
        else:
            shutil.move(archive_file, archive_folder_path)

    archive_file_list = get_all_files_with_suffix(archive_folder_path, suffix='.zip')
    MAX_ARCHIVE_LOG_NUM = 30

    if len(archive_file_list) > MAX_ARCHIVE_LOG_NUM:
        archive_file_list.sort(key=lambda fn: os.path.getmtime(fn))

        for archive_file in archive_file_list[:0 - MAX_ARCHIVE_LOG_NUM]:
            logging.debug('delete archive file {} ...'.format(archive_file))
            os.remove(archive_file)


def get_mtime_of_latest_file(file_dir=None):
    file_name = None

    if file_dir is None:
        file_dir = pts_config.DEFAULT_PTS_LOG_PATH

    file_list = os.listdir(file_dir)
    if len(file_list):
        file_list.sort(key=lambda fn: os.path.getmtime(os.path.join(file_dir, fn))
                       if not os.path.isdir(os.path.join(file_dir, fn)) else 0)

        # get the mtime of latest file
        file_name = os.path.join(file_dir, file_list[-1])
        file_c_time = datetime.datetime.fromtimestamp(os.path.getctime(file_name))
        file_m_time = datetime.datetime.fromtimestamp(os.path.getmtime(file_name))
        file_a_time = datetime.datetime.fromtimestamp(os.path.getatime(file_name))
        logging.debug('file_name: {}, file_c_time: {}, file_m_time: {}, file_a_time: {}'.
                      format(os.path.basename(file_name), file_c_time, file_m_time, file_a_time))
    else:
        file_m_time = datetime.datetime.now()

    return file_name, file_m_time

```



