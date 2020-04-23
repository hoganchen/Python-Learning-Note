### Python SubProcess超时处理

* ##### 方法一

```
class Command(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout):
        def target():
            logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

            # 当使能shell=True，subprocess会新建一个cmd的进程，而执行的命令作为该进程的子进程，当使用process.terminate()时，
            # 这时terminate的是新创建cmd的进程，而不会kill掉命令的进程。
            # process.terminate() doesn't work when using shell=True. This answer will help you
            # https://stackoverflow.com/questions/4084322/killing-a-process-created-with-pythons-subprocess-popen
            # self.process = subprocess.Popen(self.cmd, shell=False, creationflags=subprocess.CREATE_NEW_PROCESS_GROUP)
            self.process = subprocess.Popen(self.cmd, shell=False)
            # self.process = subprocess.Popen(self.cmd, shell=False, stdout=subprocess.PIPE, stdin=subprocess.PIPE,
            #                                 stderr=subprocess.PIPE)
            self.process.communicate()
            # self.process.wait()

            logging.info('The new thread finished')

        abort_flag = False
        thread_join_time = 60
        max_file_write_interval = 60 * 5
        thread_time_start = datetime.datetime.now()

        thread = threading.Thread(target=target)
        thread.start()
        thread.join(max_file_write_interval)

        while True:
            file_name, file_a_time = get_mtime_of_latest_file()
            time_current = datetime.datetime.now()
            running_time = time_current - file_a_time

            if file_name is None:
                abort_flag = True
                logging.info('The test hang up while do not execute any test case')
                break

            if running_time.seconds > max_file_write_interval:
                abort_flag = True
                break
            else:
                # time.sleep(thread_join_time)
                logging.debug('start to join test threading with {} seconds...'.format(thread_join_time))
                thread.join(thread_join_time)

            if not thread.is_alive():
                logging.info('The test is finished')
                break

            thread_running_time = datetime.datetime.now() - thread_time_start
            if thread_running_time.seconds >= timeout:
                logging.info('The test force exit due to running time exceeded the default value {} seconds'.
                             format(timeout))
                break

        # thread.join(timeout)

        if thread.is_alive():
            logging.info('The command execute abnormal, start to terminating process with pid {}...'.
                         format(self.process.pid))

            if self.process is not None:
                # self.process.terminate()
                # self.process.kill()
                # os.kill(self.process.pid, signal.SIGINT)

                # 如果在使用subprocess在新建进程中执行命令，但是新建的进程会调用别的命令作为子进程，
                # 在Linux环境下，可以在popen的时候指定参数creationflags=subprocess.CREATE_NEW_PROCESS_GROUP，
                # 并在异常结束时调用os.killpg()函数kill掉整个进程group，而在windows环境下不支持os.killpg()，
                # 即windows没有process group的概念，则可以通过使用psutil模块解决该问题，
                # 如psutil不能解决的，统一可通过subprocess.run(('taskkill /F /T /IM xxx.exe')解决

                # parent_process = psutil.Process(self.process.pid)
                # logging.debug('parent_process: {}'.format(parent_process))
                # children_process = parent_process.children(recursive=True)
                # for child in children_process:
                #     logging.debug('child: {}'.format(child))
                #     child.kill()
                # psutil.wait_procs(children_process, timeout=5)

                # parent_process.kill()
                # parent_process.wait(5)
                # os.kill(self.process.pid, signal.SIGINT)

                subprocess.run('taskkill /F /T /IM xxx.exe', stdin=None, stdout=None, stderr=None)
                self.process.terminate()

            thread.join()

        # add appended information to the abort test case
        if abort_flag and file_name is not None:
            with open(file_name, 'a') as fd:
                fd.write('The test hang up while execute this case!\n')

        logging.debug('return code: {}'.format(self.process.returncode))


def run_command_line():
    cli_command = Command('ping -n 100 127.0.0.1')
    cli_command.run(timeout=10)
```

* ##### 方法二

```
import os
import time
import signal
import logging
import datetime
import threading
import subprocess

# batch file name
BATCH_FILE_NAME = 'open_notepad.bat'

# log level
LOGGING_LEVEL = logging.DEBUG


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


class RunCommand(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def thread_run(self, timeout, kill_flag=True):
        def target():
            logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

            # process.terminate() doesn't work when using shell=True. This answer will help you
            # https://stackoverflow.com/questions/4084322/killing-a-process-created-with-pythons-subprocess-popen
            self.process = subprocess.Popen(self.cmd, shell=False)
            self.process.communicate()

            logging.info('The new thread finished')

        thread = threading.Thread(target=target)
        thread.start()
        thread.join(timeout)

        if thread.is_alive() and kill_flag:
            logging.info('The command execute abnormal, start to terminating process with pid {}...'.
                         format(self.process.pid))

            if self.process is not None:
                kill_uv4_process()
                self.process.terminate()
                # self.process.kill()

            thread.join()

        logging.debug('return code: {}'.format(self.process.returncode))

    def run(self, timeout):
        logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

        '''
        A Popen creationflags parameter to specify that a new process group will be created. 
        This flag is necessary for using os.kill() on the subprocess.
        '''
        # self.process = subprocess.Popen(self.cmd, shell=False, creationflags=subprocess.CREATE_NEW_PROCESS_GROUP)
        self.process = subprocess.Popen(self.cmd, shell=False)

        try:
            # outs, errs = self.process.communicate(timeout=timeout)
            # logging.debug('outs:\n{}\n\nerrs:\n{}\n'.format(outs, errs))
            self.process.communicate(timeout=timeout)
        except subprocess.TimeoutExpired:
            logging.info('The command execute abnormal, start to terminating process with pid {}...'.
                         format(self.process.pid))

            # os.kill(self.process.pid, signal.SIGTERM)
            logging.debug('signal.SIGTERM is {} on windows'.format(signal.SIGTERM))

            kill_process_by_pid(self.process.pid)
            self.process.kill()

            # outs, errs = self.process.communicate()
            # logging.debug('outs:\n{}\n\nerrs:\n{}\n'.format(outs, errs))
            # self.process.communicate()


def run_command_line():
    if os.path.exists(os.path.join(os.path.curdir, BATCH_FILE_NAME)):
        os.remove(os.path.join(os.path.curdir, BATCH_FILE_NAME))

    with open(os.path.join(os.path.curdir, BATCH_FILE_NAME), 'w') as fd:
        fd.write('{}\n\n'.format('notepad.exe'))

    build_cmd = RunCommand(BATCH_FILE_NAME)
    build_cmd.run(5)


def kill_uv4_process():
    subprocess.run('taskkill /F /T /IM uv4.exe', stdin=None, stdout=None, stderr=None)


def kill_process_by_name(process_name):
    subprocess.run('taskkill /F /T /IM {}'.format(process_name), stdin=None, stdout=None, stderr=None)


def kill_process_by_pid(pid):
    subprocess.run('taskkill /F /T /PID {}'.format(pid), stdin=None, stdout=None, stderr=None)


def main():
    logging_config(LOGGING_LEVEL)
    run_command_line()


if __name__ == "__main__":
    print("Script start execution at %s\n\n" % str(datetime.datetime.now()))
    time_start = time.time()
    main()
    print("\n\nScript end execution at %s" % str(datetime.datetime.now()))
    print("Total Elapsed Time: %s seconds\n" % (time.time() - time_start))方法二
```

* ##### 方法三

```
import os
import re
import time
import logging
import datetime
import threading
import subprocess

TEST_ENV = {
    'category_name': '',
    'version': '',
    'test_type': '',
    'rerun_time': 0,
}

# log config
TEST_LOG_DIR = '.\\logs'
TEST_LOG_RESULT_SUFFIX = '(log|result)_\[.*\].txt'
TEST_LOG_FORMAT = 'log_[{}].txt'.format
TEST_RESULT_LOG_FORMAT = 'result_[{}].txt'.format
TEST_RESULT_LOG_SUFFIX = 'log_main.txt'

TEST_RUN_TEST_CHECK_TIME = 5  # seconds, check the file information
TEST_RUN_TEST_MAX_EXEC_TIME = 30 * 60  # seconds
TEST_RUN_TEST_MAX_LOG_SIZE = 50  # M bytes


def kill_uv4_process():
    # subprocess.run('taskkill /F /T /IM uv4.exe', stdin=None, stdout=None, stderr=None)
    pass


def kill_process_by_pid(pid):
    subprocess.run('taskkill /F /T /PID {}'.format(pid), stdin=None, stdout=None, stderr=None)


def get_datetime_now():
    now_time_str = datetime.datetime.now().strftime('%Y%m%d_%H%M%S')

    return now_time_str


def get_all_log_files_with_suffix_by_os_walk(dir_path, suffix=None):
    log_file_path_list = []

    if os.path.exists(dir_path) and os.path.isdir(dir_path):
        for folder, sub_folder, file_list in os.walk(dir_path):
            # logging.debug('folder: {}, sub_folder: {}, file_list: {}'.format(folder, sub_folder, file_list))

            for file_name in file_list:
                file_path = os.path.join(folder, file_name)

                if suffix is None:
                    log_file_path_list.append(file_path)
                else:
                    # if re.search(r'{}$'.format(re.escape(suffix)), os.path.split(file_path)[1]):
                    if re.search(r'{}$'.format(suffix), os.path.split(file_path)[1]):
                        log_file_path_list.append(file_path)

    logging.debug('Log File list: {}'.format(log_file_path_list))
    return log_file_path_list


def get_latest_log_file_info(file_dir=None):
    latest_file_info = {'file': None, 'file_path': None, 'size': None, 'ctime': None, 'mtime': None, 'atime': None}

    if file_dir is None:
        file_dir = TEST_LOG_DIR

    # file_list = os.listdir(file_dir)
    file_list = get_all_log_files_with_suffix_by_os_walk(file_dir, suffix=TEST_LOG_RESULT_SUFFIX)

    if len(file_list):
        file_list.sort(key=lambda fn: os.path.getmtime(fn)
                       if not os.path.isdir(fn) else 0)

        # get the mtime of latest file
        file_name = file_list[-1]
        latest_file_info['file'] = os.path.basename(file_name)
        latest_file_info['file_path'] = file_name
        latest_file_info['size'] = os.path.getsize(file_name) / 1024 / 1024
        latest_file_info['ctime'] = datetime.datetime.fromtimestamp(os.path.getctime(file_name))
        latest_file_info['mtime'] = datetime.datetime.fromtimestamp(os.path.getmtime(file_name))
        latest_file_info['atime'] = datetime.datetime.fromtimestamp(os.path.getatime(file_name))

        logging.debug('file name: {}, file path: {}, file size: {}, file ctime: {}, file mtime: {}, file atime: {}\n'.
                      format(latest_file_info['file'], latest_file_info['file_path'], latest_file_info['size'],
                             latest_file_info['ctime'], latest_file_info['mtime'], latest_file_info['atime']))

    time.sleep(1)
    return latest_file_info


class RunCommand(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def thread_run(self, timeout, kill_flag=True):
        def target():
            logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

            # process.terminate() doesn't work when using shell=True. This answer will help you
            # https://stackoverflow.com/questions/4084322/killing-a-process-created-with-pythons-subprocess-popen
            self.process = subprocess.Popen(self.cmd, shell=False)
            self.process.communicate()

            logging.info('The new thread finished')

        thread = threading.Thread(target=target)
        thread.start()
        thread.join(timeout)

        if thread.is_alive() and kill_flag:
            logging.info('The command execute abnormal, start to terminating process with pid {}...'.
                         format(self.process.pid))

            if self.process is not None:
                kill_uv4_process()
                self.process.terminate()
                # self.process.kill()

            thread.join()

        logging.debug('return code: {}'.format(self.process.returncode))

    def run(self, timeout):
        logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

        '''
        A Popen creationflags parameter to specify that a new process group will be created. 
        This flag is necessary for using os.kill() on the subprocess.
        '''
        # self.process = subprocess.Popen(self.cmd, shell=False, creationflags=subprocess.CREATE_NEW_PROCESS_GROUP)
        self.process = subprocess.Popen(self.cmd, shell=False)
        spent_timeout = 0
        test_env = TEST_ENV

        while True:
            time.sleep(TEST_RUN_TEST_CHECK_TIME)
            spent_timeout += TEST_RUN_TEST_CHECK_TIME

            file_info = get_latest_log_file_info()

            if file_info.get('ctime') is not None:
                time_current = datetime.datetime.now()
                running_time = time_current - file_info['ctime']

                logging.debug('running_time: {} self.process.poll(): {}\n'.format(running_time, self.process.poll()))

                if 'functional' == test_env['test_type']:
                    if (file_info.get('size') is not None and file_info['size'] > TEST_RUN_TEST_MAX_LOG_SIZE) \
                            or self.process.poll() is not None or spent_timeout > timeout \
                            or running_time.seconds > TEST_RUN_TEST_MAX_EXEC_TIME:
                        break
                    else:
                        pass
                        # time.sleep(TEST_RUN_TEST_CHECK_TIME)
                elif 'stress' == test_env['test_type']:
                    if (file_info.get('size') is not None and file_info['size'] > TEST_RUN_TEST_MAX_LOG_SIZE) \
                            or self.process.poll() is not None or spent_timeout > timeout:
                        break
                    else:
                        pass
                        # time.sleep(TEST_RUN_TEST_CHECK_TIME)
                else:
                    pass
            else:
                break

        try:
            self.process.communicate(timeout=TEST_RUN_TEST_CHECK_TIME)
        except subprocess.TimeoutExpired:
            logging.info('The command execute abnormal, start to terminating process with pid {}...'.
                         format(self.process.pid))

            kill_process_by_pid(self.process.pid)
            self.process.kill()
```



