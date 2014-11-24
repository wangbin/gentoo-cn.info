+++
date = "2014-11-24T11:24:28+08:00"
draft = false
title = "python 3.4 成为默认的 python 3 解释器"
author = "admin"
+++

11月23日的 Gentoo News 中提到，Python 3.4 现在成为默认的 Python 3 解释器，默认的 Python 2 的解释器版本是 Python 2.7。
<!--more-->

News 中还提到， Python 3.2 的支持将从 python-r1 eclass 中移除，因为这个版本将不再有 Bug 修复，只会有安全方面的更新，如果你的 Python 3 的默认解释器的版本还是3.2的话，建议进行更新，更新方法如下：

``` shell
eselect python set --python3 python3.4

emerge -uDv --changed-use @world
```

下面是 News 的原文：

>>>2014-11-23-python-targets

>>>Title                     Python 3.4 enabled by default

>>>Author                    Mike Gilbert <floppym@gentoo.org>

>>>Posted                    2014-11-23

>>>Revision                  1

>>>Python 3.4 is now enabled by default, replacing Python 3.3 as the default Python 3 interpreter.

>>>PYTHON_TARGETS will be adjusted to contain python2_7 and python3_4 by default via your profile.

>>>PYTHON_SINGLE_TARGET will remain set to python2_7 by default.

>>>If you have PYTHON_TARGETS set in make.conf, that setting will still be respected. You may want to adjust this setting manually.

>>>At the same time, support for Python 3.2 will be removed from the python-r1 family of eclasses. This version no longer receives regular bug fixes, and is currently only receiving security updates.

>>>Once the changes have taken place, a world update should take care of reinstalling any python libraries you have installed. You should also switch your default python3 interpreter using eselect python.

>>>For example:

>>>eselect python set --python3 python3.4

>>>emerge -uDv --changed-use @world
