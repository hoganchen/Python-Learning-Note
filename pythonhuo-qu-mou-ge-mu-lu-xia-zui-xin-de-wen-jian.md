### Python获取某个目录下最新的文件

https://tangx.in/2016/11/24/python-libaray-shutil-shell-command-for-python/

https://blog.csdn.net/liudinglong1989/article/details/78731754

http://www.cnblogs.com/gongxr/p/7351858.html

https://www.cnblogs.com/pipihaoke/p/8034395.html

```
def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


def get_mtime_of_latest_file(file_dir=None):
    file_name = None

    if file_dir is None:
        file_dir = '.\\Logs'

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



