### Python环境设置

* ##### uninstall anaconda

[https://stackoverflow.com/questions/22585235/python-anaconda-how-to-safely-uninstall](https://stackoverflow.com/questions/22585235/python-anaconda-how-to-safely-uninstall)

```
rm -rf ~/anaconda
rm -rf ~/.anaconda/navigator
rm -rf ~/.condarc ~/.conda ~/.continuum
```

* ##### install anaconda

```
bash Anaconda3-5.2.0-Linux-x86_64.sh
```

* ##### multiple python version

[https://www.cnblogs.com/dong-c/p/python.html](https://www.cnblogs.com/dong-c/p/python.html)

[https://www.jb51.net/article/114311.htm](https://www.jb51.net/article/114311.htm)

[https://www.jb51.net/article/123427.htm](https://www.jb51.net/article/123427.htm)

[http://www.cnblogs.com/deeper/p/7429084.html](http://www.cnblogs.com/deeper/p/7429084.html)

[https://www.cnblogs.com/zelos/p/7222921.html](https://www.cnblogs.com/zelos/p/7222921.html)

安装第三方模块

```
1. 给python2安装第三方模块
打开命令行工具，执行如下的命令进行安装python2需要的模块
python2 -m pip install 模块名称 / pip2 install 模块名称

2. 给python3安装第三方模块
打开命令行工具，执行如下的命令进行安装python3需要的模块
python3 -m pip install 模块名称 / pip3 install 模块名称
```

查看pip版本

```
python2 -m pip -V
python3 -m pip -V
```

安装第三方模块

```
python2下pip安装命令如下：
py -2 -m pip install xxxxxx

python3下pip安装命令如下：
py -3 -m pip install xxxxxx
```

升级安装包

    pip当前内建命令并不支持升级所有已安装的Python模块。
    列出当前安装的包：
    pip list

    列出可升级的包：
    pip list --outdate

    升级一个包：
    pip install --upgrade requests  // mac,linux,unix 在命令前加 sudo -H

    升级所有可升级的包：

    pip freeze --local | grep -v '^-e' | cut -d = -f 1  | xargs -n1 pip install -U
    for i in `pip list -o --format legacy|awk '{print $1}'` ; do pip install --upgrade $i; done

* ##### add anaconda path variable

```
# added by Anaconda3 4.4.0 installer
#export PATH="~/anaconda3/bin:$PATH"
alias condapython='~/anaconda3/bin/python'
alias condajupyter='~/anaconda3/bin/jupyter'
alias condapip='~/anaconda3/bin/pip'
alias conda='~/anaconda3/bin/conda'
alias pyinstaller='~/anaconda3/bin/pyinstaller'
```

* ##### python virtualenv

[https://www.cnblogs.com/jasmine-Jobs/p/7045016.html](https://www.cnblogs.com/jasmine-Jobs/p/7045016.html)

* ##### install download package

```
python setup.py install
```

* ##### install anaconda package

[https://stackoverflow.com/questions/36164986/how-to-install-package-in-anaconda](https://stackoverflow.com/questions/36164986/how-to-install-package-in-anaconda)

```
condapip install pyserial
condapip install tushare
condapip install tensorflow
condapython -m pip install paramiko

mysql-connector-python
https://dev.mysql.com/doc/connector-python/en/connector-python-installation-binary.html
https://anaconda.org/anaconda/mysql-connector-python
condapip install mysql-connector-python
conda install -c anaconda mysql-connector-python
```



