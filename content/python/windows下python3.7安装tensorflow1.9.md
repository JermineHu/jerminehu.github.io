---
title: "Windows下python3.7安装tensorflow1"
date: 2018-07-29T17:49:08+08:00
categories: ["All","python","tensorflow","DeepLearn"]
tags: ["All","python","tensorflow","DeepLearn"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["All","python","tensorflow","DeepLearn" ]
description: "Windows下python3.7安装tensorflow1"
---

# 准备文件：

从[这里下载windows的python3.7的tensorflow1.9安装包。](https://download.lfd.uci.edu/pythonlibs/o4uhg4xd/tensorflow-1.9.0-cp37-cp37m-win_amd64.whl)

*注: [这个网站](https://www.lfd.uci.edu/~gohlke/pythonlibs/)的whl文件是非python官方的windows二进制扩展包。

# 执行安装命令

```
PS C:\Users\Husee\Desktop> pip install .\tensorflow-1.9.0-cp37-cp37m-win_amd64.whl
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Processing c:\users\husee\desktop\tensorflow-1.9.0-cp37-cp37m-win_amd64.whl
Collecting termcolor>=1.1.0 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/8a/48/a76be51647d0eb9f10e2a4511bf3ffb8cc1e6b14e9e4fab46173aa79f981/termcolor-1.1.0.tar.gz
Requirement already satisfied: setuptools<=39.1.0 in c:\users\husee\appdata\local\programs\python\python37\lib\site-packages (from tensorflow==1.9.0) (39.0.1)
Collecting astor>=0.6.0 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/35/6b/11530768cac581a12952a2aad00e1526b89d242d0b9f59534ef6e6a1752f/astor-0.7.1-py2.py3-none-any.whl
Collecting protobuf>=3.4.0 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/77/78/a7f1ce761e2c738e209857175cd4f90a8562d1bde32868a8cd5290d58926/protobuf-3.6.1-py2.py3-none-any.whl (390kB)
    100% |████████████████████████████████| 399kB 12.8MB/s
Collecting grpcio>=1.8.6 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/b1/0b/f46b579e65e11a1ddb0d2a19f458cac91ed71841fed39322e980fd58be44/grpcio-1.14.1-cp37-cp37m-win_amd64.whl (1.5MB)
    100% |████████████████████████████████| 1.5MB 13.7MB/s
Collecting absl-py>=0.1.6 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/cc/e6/6cc5c834023685dd83a28bdb5c1826d9340111493a447e9a9230269defa8/absl-py-0.4.0.tar.gz (88kB)
    100% |████████████████████████████████| 92kB 13.1MB/s
Collecting tensorboard<1.10.0,>=1.9.0 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/9e/1f/3da43860db614e294a034e42d4be5c8f7f0d2c75dc1c428c541116d8cdab/tensorboard-1.9.0-py3-none-any.whl (3.3MB)
    100% |████████████████████████████████| 3.3MB 25.7MB/s
Requirement already satisfied: wheel>=0.26 in c:\users\husee\appdata\local\programs\python\python37\lib\site-packages (from tensorflow==1.9.0) (0.31.1)
Collecting numpy>=1.13.3 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/90/ca/fac7871a7c7d78beb78d7d9562b8d5bfce9ff316dc6c2a7ac34927895609/numpy-1.15.1-cp37-none-win_amd64.whl (13.5MB)
    100% |████████████████████████████████| 13.5MB 12.6MB/s
Requirement already satisfied: six>=1.10.0 in c:\users\husee\appdata\local\programs\python\python37\lib\site-packages (from tensorflow==1.9.0) (1.11.0)
Collecting gast>=0.2.0 (from tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/5c/78/ff794fcae2ce8aa6323e789d1f8b3b7765f601e7702726f430e814822b96/gast-0.2.0.tar.gz
Collecting werkzeug>=0.11.10 (from tensorboard<1.10.0,>=1.9.0->tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/20/c4/12e3e56473e52375aa29c4764e70d1b8f3efa6682bef8d0aae04fe335243/Werkzeug-0.14.1-py2.py3-none-any.whl (322kB)
    100% |████████████████████████████████| 327kB 34.2MB/s
Collecting markdown>=2.6.8 (from tensorboard<1.10.0,>=1.9.0->tensorflow==1.9.0)
  Downloading http://mirrors.aliyun.com/pypi/packages/6d/7d/488b90f470b96531a3f5788cf12a93332f543dbab13c423a5e7ce96a0493/Markdown-2.6.11-py2.py3-none-any.whl (78kB)
    100% |████████████████████████████████| 81kB 4.8MB/s
Building wheels for collected packages: termcolor, absl-py, gast
  Running setup.py bdist_wheel for termcolor ... done
  Stored in directory: C:\Users\Husee\AppData\Local\pip\Cache\wheels\65\c8\98\8361afe9bafba434b7acf14c08627560d63018272226ff3e10
  Running setup.py bdist_wheel for absl-py ... done
  Stored in directory: C:\Users\Husee\AppData\Local\pip\Cache\wheels\50\6f\41\ae7dbb65f38f6e607b399117e4cb959977e016d12b6166a67f
  Running setup.py bdist_wheel for gast ... done
  Stored in directory: C:\Users\Husee\AppData\Local\pip\Cache\wheels\17\0a\dc\bb6d7b129029482a8d55901d66b65e756a681f6a1da7297a9b
Successfully built termcolor absl-py gast
Installing collected packages: termcolor, astor, protobuf, grpcio, absl-py, numpy, werkzeug, markdown, tensorboard, gast, tensorflow
Successfully installed absl-py-0.4.0 astor-0.7.1 gast-0.2.0 grpcio-1.14.1 markdown-2.6.11 numpy-1.15.1 protobuf-3.6.1 tensorboard-1.9.0 tensorflow-1.9.0 termcolor-1.1.0 werkzeug-0.14.1
```

# 查看安装列表

```
PS C:\Users\Husee\Desktop> pip list
Package           Version
----------------- -------
absl-py           0.4.0
astor             0.7.1
astroid           1.6.5
colorama          0.3.9
gast              0.2.0
grpcio            1.14.1
isort             4.3.4
lazy-object-proxy 1.3.1
Markdown          2.6.11
mccabe            0.6.1
numpy             1.15.1
pip               18.0
protobuf          3.6.1
pylint            1.9.2
setuptools        39.0.1
six               1.11.0
tensorboard       1.9.0
tensorflow        1.9.0
termcolor         1.1.0
torch             0.4.1
Werkzeug          0.14.1
wheel             0.31.1
wrapt             1.10.11
PS C:\Users\Husee\Desktop> pip -V
pip 18.0 from c:\users\husee\appdata\local\programs\python\python37\lib\site-packages\pip (python 3.7)
PS C:\Users\Husee\Desktop>
```

# 通过python调用tensorflow查看版本

```
python
Python 3.7.0b5 (v3.7.0b5:abb8802389, May 31 2018, 01:54:01) [MSC v.1913 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
Traceback (most recent call last):
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 35, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 30, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: DLL load failed: 找不到指定的模块。

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\__init__.py", line 22, in <module>
    from tensorflow.python import pywrap_tensorflow  # pylint: disable=unused-import
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 74, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 35, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 30, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "C:\Users\Husee\AppData\Local\Programs\Python\Python37\lib\imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: DLL load failed: 找不到指定的模块。


Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/install_sources#common_installation_problems

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.

```

### ImportError： DLL load failed：找不到指定的模块 解决方案（Python）：

首先检查numpy、scipy、matplotlib、scikit-learn的版本是否更新到最新且符合当前Python版本： 如果出现不是最新的版本，先卸载该版本：（windows+".\"）pip uninstall numpy
再去http://www.lfd.uci.edu/~gohlke/pythonlibs/ 安装最新版本：（windows+".\"）pip install numpy

优先更新numpy，通常更新完numpy即可解决问题！如果`pip install numpy` 不能解决问题，记得上 https://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy 去找`whl`文件，然后离线安装，解决dll依赖的问题。

### 如果还未解决，采用如下方式：

解决：下载了一个depends，查看caffe.dll，发现cudnn64_5.dll为黄色叹号，说是找不到这个文件，用ererything查找，拷贝到python执行文件(xxx.py)文件同目录，就可以了。
depends下载地址：
http://www.dependencywalker.com/

下面是另一个案例：


  python3，win7  64位，在import cv2时，显示"ImportError: DLL load failed: 找不到指定的模块"，后来用用depends.exe软件查看（下载地址http://www.dependencywalker.com/），发现缺少concrt140.dll 等几个文件，

解决方法：主要是因为没有安装Visual C++ Redistributable for Visual Studio 2015软件导致的，安完此软件后就可以正常使用cv2了。以下是安装步骤

        （1）下载opencv_python-3.2.0-cp35-cp35m-win_amd64.whl（下载地址http://www.lfd.uci.edu/~gohlke/pythonlibs/），在命令窗口中用pip install 的方式安装。

        （2）下载 Visual C++ Redistributable for Visual Studio 2015 (下载地址https://www.microsoft.com/zh-cn/download/details.aspx?id=48145&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)，下载完直接安装即可。

      （3）import cv2就可以正常使用了

win7安装python出问题主要是缺少系统文件，win7系统会不断的更新补丁，如果有哪个补丁未安装可能就会出问题，尤其是纯净版的win7系统，在安装其它软件之前一定要把win7的补丁补全，虽然一大堆。

### 解决以后再查看：

结果如下：

```
PS C:\Users\Husee\Desktop> pip install .\numpy-1.15.1+mkl-cp37-cp37m-win_amd64.whl
Looking in indexes: http://mirrors.aliyun.com/pypi/simple
Processing c:\users\husee\desktop\numpy-1.15.1+mkl-cp37-cp37m-win_amd64.whl
Installing collected packages: numpy
Successfully installed numpy-1.15.1+mkl
PS C:\Users\Husee\Desktop>
PS C:\Users\Husee\Desktop> python
Python 3.7.0b5 (v3.7.0b5:abb8802389, May 31 2018, 01:54:01) [MSC v.1913 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow
>>> tensorflow.__version__
'1.9.0'
>>>
```