### PyInstaller打包Python程序

* ##### Ubuntu卸载软件

```
sudo apt-get purge typora
```

* ##### Ubuntu/Windows升级pip

```
condapython -m pip install --upgrade pip
```

* ##### Ubuntu/Windows安装下载的python的库

```
解压并进入目录，执行如下命令
python -m setup.py install
```

* ##### Ubuntu/Windows安装pyinstaller

```
全新安装
condapython -m pip install pyinstaller
升级安装
condapython -m pip install --upgrade pyinstaller
```

* ##### Ubuntu安装qt designer

[https://askubuntu.com/questions/763877/how-to-install-and-run-qt-designer-for-python](https://askubuntu.com/questions/763877/how-to-install-and-run-qt-designer-for-python)

[https://blog.csdn.net/wukai\_std/article/details/54728588](https://blog.csdn.net/wukai_std/article/details/54728588)

```
sudo apt-get update
sudo apt-cache search qt | grep designer
sudo apt-cache show qt4-designer
sudo apt-cache search pyqt5-dev
sudo apt-get install qt4-designer (安装qt4版本)
sudo apt-get install python-qt4 qt4-designer (同时安装python-qt4版本)
sudo apt-get install qttools5-dev-tools (安装qt5版本)

启动(任选其一命令)
$ designer
$ designer-qt4
```

* ##### Windows安装qt designer

```
1、直接在cmd中通过pip安装PyQt5
pip install pyqt5

会自动下载PyQt5以及sip并安装，因为PyQt5不再提供Qt Designer等工具，所以需要再安装pyqt5-tools，可直接在cmd中通过pip安装
pip install pyqt5-tools

因为网络等原因，可能会安装失败，可以先现在whl文件再进行安装(下载地址：https://pypi.python.org/pypi/pyqt5-tools/5.7.dev9\)

安装好之后在Python安装目录的\Lib\site-packages\pyqt5-tools\designer文件夹下面能够找到designer.exe，运行即可
```

* ##### QT Designer的UI文件转python文件

```
1. 安装pyuic5
sudo apt-get update
sudo apt-get install pyqt5-dev-tools

2. 执行命令
pyuic5 -o ui_BQBTestTool.py BQBTestTool.ui
```

* ##### Anaconda打包python with PyQt的程序

  1. 安装如下库文件  
     Ubuntu下：  
     Pyqt库: sudo apt-get install python-pyqt5  
     Qt-designer: sudo apt-get install qt5-designer  
     pyuic5: sudo apt install pyqt5-dev-tools  
     PyInstaller: pip install PyInstaller

     Windows下：  
     进入pip目录\(cd C:\Python34\Scripts\)  
     PyQt5库：  
     SIP: pip3 install SIP / python -m pip install sip  
     PyQt5: pip3 install PyQt5 / python -m pip install PyQt5  
     PyInstaller: pip3 install PyInstaller / python -m pip install pyinstaller

  2. 如果anaconda没有重新安装pyqt5的库，则可能存在使用pyinstaller生成的可执行文件存在"This application failed to start because it could not find or load the Qt platform plugin "windows" in ""."错误。\(此处存疑，也有可能是没有把PyQt5的plugins放入pyinstaller的paths参数中\)  
     实验环境: win7 64bit; Python 3.6.1 \|Anaconda 4.4.0 \(32-bit\)\| \(default, May 11 2017, 14:16:49\) \[MSC v.1900 32 bit \(Intel\)\] on win32; pyinstaller 3.3.1

     2.1 解决方法如下，重新安装pyqt5的库  
     python -m pip install pyqt5

     2.2 cmd下执行如下命令  
     set path=%path%;C:\Anaconda3\Scripts  
     :该命令没有把PyQt5的plugins放入pyinstaller的paths参数中，并打开了debug选项  
     :pyinstaller --clean -F --debug --paths C:\Anaconda3\Library\plugins UPFTestTool.py  
     :即使没有把PyQt5的plugins放入pyinstaller的paths参数中，生成的可执行文件也可以正常工作，应该是anaconda自带的PyQt5的锅，即需要重新安装最新版的PyQt5，以下3条命令生成的可执行文件都能正常工作  
     pyinstaller --clean -F --paths C:\Anaconda3\Library\plugins;C:\Anaconda3\Lib\site-packages\PyQt5\Qt\plugins UPFTestTool.py  
     pyinstaller --clean -F --paths C:\Anaconda3\Library\plugins UPFTestTool.py  
     pyinstaller --clean -F UPFTestTool.py

     :添加图标  
     pyinstaller --clean -F -i favicon.ico UPFTestTool.py

     :去除console窗口  
     pyinstaller --clean -F -w -i favicon.ico UPFTestTool.py
     
     :PyInstaller 3.4版本  
     LD_LIBRARY_PATH=~/anaconda3/lib pyinstaller --clean -w -F MiniStock3.py

    C:\projects>pyinstaller -h
    usage: pyinstaller [-h] [-v] [-D] [-F] [--specpath DIR] [-n NAME]
                       [--add-data <SRC;DEST or SRC:DEST>]
                       [--add-binary <SRC;DEST or SRC:DEST>] [-p DIR]
                       [--hidden-import MODULENAME]
                       [--additional-hooks-dir HOOKSPATH]
                       [--runtime-hook RUNTIME_HOOKS] [--exclude-module EXCLUDES]
                       [--key KEY] [-d] [-s] [--noupx] [-c] [-w]
                       [-i <FILE.ico or FILE.exe,ID or FILE.icns>]
                       [--version-file FILE] [-m <FILE or XML>] [-r RESOURCE]
                       [--uac-admin] [--uac-uiaccess] [--win-private-assemblies]
                       [--win-no-prefer-redirects]
                       [--osx-bundle-identifier BUNDLE_IDENTIFIER]
                       [--runtime-tmpdir PATH] [--distpath DIR]
                       [--workpath WORKPATH] [-y] [--upx-dir UPX_DIR] [-a]
                       [--clean] [--log-level LEVEL]
                       scriptname [scriptname ...]

    positional arguments:
      scriptname            name of scriptfiles to be processed or exactly one
                            .spec-file. If a .spec-file is specified, most options
                            are unnecessary and are ignored.

    optional arguments:
      -h, --help            show this help message and exit
      -v, --version         Show program version info and exit.
      --distpath DIR        Where to put the bundled app (default: .\dist)
      --workpath WORKPATH   Where to put all the temporary work files, .log, .pyz
                            and etc. (default: .\build)
      -y, --noconfirm       Replace output directory (default:
                            SPECPATH\dist\SPECNAME) without asking for
                            confirmation
      --upx-dir UPX_DIR     Path to UPX utility (default: search the execution
                            path)
      -a, --ascii           Do not include unicode encoding support (default:
                            included if available)
      --clean               Clean PyInstaller cache and remove temporary files
                            before building.
      --log-level LEVEL     Amount of detail in build-time console messages. LEVEL
                            may be one of TRACE, DEBUG, INFO, WARN, ERROR,
                            CRITICAL (default: INFO).

    What to generate:
      -D, --onedir          Create a one-folder bundle containing an executable
                            (default)
      -F, --onefile         Create a one-file bundled executable.
      --specpath DIR        Folder to store the generated spec file (default:
                            current directory)
      -n NAME, --name NAME  Name to assign to the bundled app and spec file
                            (default: first script's basename)

    What to bundle, where to search:
      --add-data <SRC;DEST or SRC:DEST>
                            Additional non-binary files or folders to be added to
                            the executable. The path separator is platform
                            specific, ``os.pathsep`` (which is ``;`` on Windows
                            and ``:`` on most unix systems) is used. This option
                            can be used multiple times.
      --add-binary <SRC;DEST or SRC:DEST>
                            Additional binary files to be added to the executable.
                            See the ``--add-data`` option for more details. This
                            option can be used multiple times.
      -p DIR, --paths DIR   A path to search for imports (like using PYTHONPATH).
                            Multiple paths are allowed, separated by ';', or use
                            this option multiple times
      --hidden-import MODULENAME, --hiddenimport MODULENAME
                            Name an import not visible in the code of the
                            script(s). This option can be used multiple times.
      --additional-hooks-dir HOOKSPATH
                            An additional path to search for hooks. This option
                            can be used multiple times.
      --runtime-hook RUNTIME_HOOKS
                            Path to a custom runtime hook file. A runtime hook is
                            code that is bundled with the executable and is
                            executed before any other code or module to set up
                            special features of the runtime environment. This
                            option can be used multiple times.
      --exclude-module EXCLUDES
                            Optional module or package (the Python name, not the
                            path name) that will be ignored (as though it was not
                            found). This option can be used multiple times.
      --key KEY             The key used to encrypt Python bytecode.

    How to generate:
      -d, --debug           Tell the bootloader to issue progress messages while
                            initializing and starting the bundled app. Used to
                            diagnose problems with missing imports.
      -s, --strip           Apply a symbol-table strip to the executable and
                            shared libs (not recommended for Windows)
      --noupx               Do not use UPX even if it is available (works
                            differently between Windows and *nix)

    Windows and Mac OS X specific options:
      -c, --console, --nowindowed
                            Open a console window for standard i/o (default)
      -w, --windowed, --noconsole
                            Windows and Mac OS X: do not provide a console window
                            for standard i/o. On Mac OS X this also triggers
                            building an OS X .app bundle. This option is ignored
                            in *NIX systems.
      -i <FILE.ico or FILE.exe,ID or FILE.icns>, --icon <FILE.ico or FILE.exe,ID or FILE.icns>
                            FILE.ico: apply that icon to a Windows executable.
                            FILE.exe,ID, extract the icon with ID from an exe.
                            FILE.icns: apply the icon to the .app bundle on Mac OS
                            X

    Windows specific options:
      --version-file FILE   add a version resource from FILE to the exe
      -m <FILE or XML>, --manifest <FILE or XML>
                            add manifest FILE or XML to the exe
      -r RESOURCE, --resource RESOURCE
                            Add or update a resource to a Windows executable. The
                            RESOURCE is one to four items,
                            FILE[,TYPE[,NAME[,LANGUAGE]]]. FILE can be a data file
                            or an exe/dll. For data files, at least TYPE and NAME
                            must be specified. LANGUAGE defaults to 0 or may be
                            specified as wildcard * to update all resources of the
                            given TYPE and NAME. For exe/dll files, all resources
                            from FILE will be added/updated to the final
                            executable if TYPE, NAME and LANGUAGE are omitted or
                            specified as wildcard *.This option can be used
                            multiple times.
      --uac-admin           Using this option creates a Manifest which will
                            request elevation upon application restart.
      --uac-uiaccess        Using this option allows an elevated application to
                            work with Remote Desktop.

    Windows Side-by-side Assembly searching options (advanced):
      --win-private-assemblies
                            Any Shared Assemblies bundled into the application
                            will be changed into Private Assemblies. This means
                            the exact versions of these assemblies will always be
                            used, and any newer versions installed on user
                            machines at the system level will be ignored.
      --win-no-prefer-redirects
                            While searching for Shared or Private Assemblies to
                            bundle into the application, PyInstaller will prefer
                            not to follow policies that redirect to newer
                            versions, and will try to bundle the exact versions of
                            the assembly.

    Mac OS X specific options:
      --osx-bundle-identifier BUNDLE_IDENTIFIER
                            Mac OS X .app bundle identifier is used as the default
                            unique program name for code signing purposes. The
                            usual form is a hierarchical name in reverse DNS
                            notation. For example:
                            com.mycompany.department.appname (default: first
                            script's basename)

    Rarely used special options:
      --runtime-tmpdir PATH
                            Where to extract libraries and support files in
                            `onefile`-mode. If this option is given, the
                            bootloader will ignore any temp-folder location
                            defined by the run-time OS. The ``_MEIxxxxxx``-folder
                            will be created here. Please use this option only if
                            you know what you are doing.



