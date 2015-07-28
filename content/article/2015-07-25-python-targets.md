+++
date = "2015-07-25T11:27:40+08:00"
draft = false
title = "Python 3.4 成为 Python 3 默认版本"
author = "admin"
tags = ["Python"]
+++

7月25日的 Gentoo News 中再次提到了 Python 3.4 取代 3.3 成为默认的 Python 3 解释器版本。
<!--more-->

我记得去年11月23日有一则 [News](/article/python-3.4-as-default-python-3-interpreter/) 说过这件事，但是我去 Portage 中验证，却发现该消息已经不存在了，不知何故。

下面是 News 的原文与之前的报道并没有多少变化：

>>>Title: Python 3.4 enabled by default

>>>Author: Mike Gilbert <floppym@gentoo.org>

>>>Content-Type: text/plain

>>>Posted: 2015-07-25

>>>Revision: 1

>>>News-Item-Format: 1.0

>>>Python 3.4 is now enabled by default, replacing Python 3.3 as the default Python 3 interpreter.

>>>PYTHON_TARGETS will be adjusted to contain python2_7 and python3_4 by default via your profile.

>>>PYTHON_SINGLE_TARGET will remain set to python2_7 by default.

>>>If you have PYTHON_TARGETS set in make.conf, that setting will still be respected. You may want to adjust this setting manually.

>>>Once the changes have taken place, a world update should take care of reinstalling any python libraries you have installed. You should also switch your default python3 interpreter using eselect python.

>>>For example:

>>>eselect python set --python3 python3.4

>>>emerge -uDv --changed-use @world
