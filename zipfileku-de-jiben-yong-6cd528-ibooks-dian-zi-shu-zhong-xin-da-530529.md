### zipfile库的基本用法\(ibooks电子书重新打包\)

```
# -*- coding:utf-8 -*-

"""
@Author:    clhon@qq.com
@Date:      2018-06-07
"""

import os
import re
import time
import zipfile
import logging
import datetime

# log level
LOGGING_LEVEL = logging.DEBUG
TODO_FOLDER = '..\\ibooks'
EPUB_FOLDER = '.\\'
PARSE_FILE_NAME = 'iTunesMetadata.plist'


def logging_config(logging_level):
    # log_format = "%(asctime)s - %(levelname)s - %(message)s"
    # log_format = "%(asctime)s [line: %(lineno)d] - %(levelname)s - %(message)s"
    log_format = "[%(asctime)s - [File: %(filename)s line: %(lineno)d] - %(levelname)s]: %(message)s"
    logging.basicConfig(level=logging_level, format=log_format)


def zip_log_folder(folder_path, zip_file_name):
    tc_zip_file = zipfile.ZipFile(os.path.join(os.path.dirname(folder_path), '{}.epub'.format(zip_file_name)), 'w',
                                  compression=zipfile.ZIP_DEFLATED)

    for folder, sub_folders, files in os.walk(folder_path):
        # logging.debug('folder: {}'.format(folder))
        # logging.debug('sub_folders: {}'.format(sub_folders))
        # logging.debug('files: {}'.format(files))

        for file_name in files:
            while True:
                try:
                    # logging.debug('os.path.join(folder, file_name): {}'.format(os.path.join(folder, file_name)))
                    # logging.debug('arcname: {}'.format(os.path.relpath(os.path.join(folder, file_name),
                    #                                                    os.path.dirname(folder_path))))

                    # tc_zip_file.write(os.path.join(folder, file_name),
                    #                   arcname=os.path.relpath(os.path.join(folder, file_name),
                    #                                           os.path.dirname(folder_path)))

                    '''
                    z.write(filename[,arcname[,compression_type]])，
                    将zip外的文件filename写入到名为arcname的子文件中（当然arcname也是带有相对zip包的路径的），
                    compression_type指定了压缩格式，也是ZIP_STORED或ZIP_DEFLATED。
                    z的打开方式一定要是w或者a才能顺利写入文件。
                    '''
                    zip_folder_path = os.path.relpath(os.path.join(folder, file_name), os.path.dirname(folder_path))
                    related_folder_path = re.search(r'^[^\\]+\\(.*)$', zip_folder_path).group(1)

                    tc_zip_file.write(os.path.join(folder, file_name), arcname=related_folder_path)
                except Exception as err:
                    logging.critical('Zip handle error happened[{}]...'.format(err))
                else:
                    break

    tc_zip_file.close()


def get_file_name(folder_path):
    file_name = None

    file_path = os.path.join(folder_path, PARSE_FILE_NAME)

    with open(file_path, 'r', encoding='UTF-8') as fd:
        while True:
            line_str = fd.readline()
            if line_str:
                if re.search(r'<key>itemName</key>', line_str):
                    file_name_str = fd.readline()

                    match_obj = re.search(r'<string>(.*?)</string>', file_name_str)
                    if match_obj:
                        file_name = match_obj.group(1)

                    break
                else:
                    continue
            else:
                break

    return file_name


def make_epub_file():
    folder_list = os.listdir(TODO_FOLDER)

    for folder_name in folder_list:
        folder_path = os.path.join(TODO_FOLDER, folder_name)

        if re.search(r'.epub$', folder_name) and os.path.isdir(folder_path):
            file_name = get_file_name(folder_path)

            if file_name is not None:
                file_name = re.sub(r'[:?]', '-', file_name)
                logging.debug('folder_path: {}, file_name: {}'.format(folder_path, file_name))
                zip_log_folder(folder_path, file_name)
        else:
            continue


def main():
    logging_config(LOGGING_LEVEL)

    make_epub_file()


if __name__ == "__main__":
    print("Script start execution at {}".format(str(datetime.datetime.now())))

    time_start = time.time()
    main()

    print("\n\nTotal Elapsed Time: {} seconds".format(time.time() - time_start))
    print("\nScript end execution at {}".format(datetime.datetime.now()))
```



