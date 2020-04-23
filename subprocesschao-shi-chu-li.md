### subprocess超时处理

```
import os
import time
import zipfile
import datetime
import threading
import subprocess


def zip_file():
    folder_path = os.path.join(os.getcwd(), 'Documents/Docs/practice')

    tc_zip = zipfile.ZipFile(os.path.join(os.getcwd(), 'practice.zip'), 'w')

    for folder, subfolder, files in os.walk(folder_path):
        # print("folder: {}".format(folder))
        # print("subfolder: {}".format(subfolder))
        # print("files: {}".format(files))
        for file_name in files:
            tc_zip.write(os.path.join(folder, file_name),
                         os.path.relpath(os.path.join(folder, file_name), os.path.dirname(folder_path)))

    tc_zip.close()


class Command(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout):
        def target():
            print('Thread started')
            self.process = subprocess.Popen(self.cmd, shell=False)
            self.process.communicate()
            print('Thread finished')

        thread = threading.Thread(target=target)
        thread.start()

        thread.join(timeout)
        if thread.is_alive():
            print('Terminating process')
            self.process.terminate()
            # self.process.kill()
            thread.join()
        print(self.process.returncode)


class RunCommand(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout, kill_flag=True):
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
                self.process.terminate()
                # self.process.kill()

            thread.join()

        logging.debug('return code: {}'.format(self.process.returncode))


class CommandWithMultipleTimeout(object):
    def __init__(self, cmd):
        self.cmd = cmd
        self.process = None

    def run(self, timeout):
        def target():
            logging.info('Start new thread to execute command "{}"...'.format(self.cmd))

            # process.terminate() doesn't work when using shell=True. This answer will help you
            # https://stackoverflow.com/questions/4084322/killing-a-process-created-with-pythons-subprocess-popen
            # self.process = subprocess.Popen(self.cmd, shell=False, creationflags=subprocess.CREATE_NEW_PROCESS_GROUP)
            self.process = subprocess.Popen(self.cmd, shell=False)
            # self.process = subprocess.Popen(self.cmd, shell=False, stdout=subprocess.PIPE, stdin=subprocess.PIPE,
            #                                 stderr=subprocess.PIPE)
            self.process.communicate()
            # self.process.wait()

            subprocess.run('taskkill /F /T /IM pts.exe', stdin=None, stdout=None, stderr=None)

            logging.info('The new thread finished')

        abort_flag = False
        thread_join_time = 60
        max_file_write_interval = 60 * pts_config.PTS_TEST_CASE_TIMEOUT
        thread_time_start = datetime.datetime.now()

        thread = threading.Thread(target=target)
        thread.start()
        thread.join(max_file_write_interval)

        while True:
            file_name, file_m_time = get_mtime_of_latest_file()
            # sleep 1 second to avoid the file_m_time large than the current time
            time.sleep(1)
            time_current = datetime.datetime.now()
            running_time = time_current - file_m_time

            if file_name is None:
                abort_flag = True
                logging.info('The test hang up while do not execute any test case')
                break

            if running_time.seconds > max_file_write_interval:
                abort_flag = True
                case_name = re.sub(r'{}|{}'.format(pts_config.DEFAULT_PTS_LOG_SUFFIX,
                                                   pts_config.DEFAULT_TEST_LOG_SUFFIX), '', os.path.basename(file_name))
                logging.info('The test hang up while execute test case "{}", running_time.total_seconds(): {}, '
                             'running_time: {}'.format(case_name, running_time.total_seconds(), running_time))
                break
            else:
                # time.sleep(thread_join_time)
                logging.debug('start to join the pts test threading with {} seconds...'.format(thread_join_time))
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

                subprocess.run('taskkill /F /T /IM pts.exe', stdin=None, stdout=None, stderr=None)
                self.process.terminate()

            thread.join()

        # add appended information to the abort test case
        if abort_flag and file_name is not None:
            with open(file_name, 'a') as fd:
                fd.write('The test hang up while execute this case!\n')

        logging.debug('return code: {}'.format(self.process.returncode))


def run_command():
    # command = Command("echo 'Process started'; sleep 2; echo 'Process finished'")
    # command.run(timeout=30)
    # command.run(timeout=10)
    bash_command = Command("ping -n 10 127.0.0.1")
    bash_command.run(timeout=15)
    bash_command.run(timeout=5)
    bash_command.run(timeout=3)


def main():
    # zip_file()
    run_command()

if __name__ == "__main__":
    print('Script start execution at %s\n\n' % str(datetime.datetime.now()))
    time_start = time.time()

    main()

    print('\n\nScript end execution at %s' % str(datetime.datetime.now()))
    print('Total Elapsed Time: %s seconds\n' % (time.time() - time_start))
```



