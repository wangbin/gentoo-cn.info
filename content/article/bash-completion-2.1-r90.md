+++
date = "2014-11-28T18:28:48+08:00"
draft = false
title = "app-shells/bash-completion-2.1-r90 更新带来的一些变化"
author = "admin"
tags = ["bash",]
+++

11月25日的 Gentoo News 中提到，>=app-shells/bash-completion-2.1-r90 带来了一些新的变化，更新了得用户需要按照 News 中的提示手动更新一下，以避免出现一些问题。
<!--more-->

简单来说，从这个版本开始， bash-completion 终于和上游的设计保持一致。对用户来说，有以下几点变化：

 - 安装路径发生了变化，之前安装的补全脚本需要重新进行编译，用户可以运行下面的命令：

``` bash
$ find /usr/share/bash-completion -maxdepth 1 -type f \
        '!' -name 'bash_completion' -exec emerge -1v {} +
```

 - 上游对自动加载功能的调整，导致之前通过 *eselect bashcomp* 做的配置可能不能正常工作，用户需要手动执行下面的命令删除一些软链接：

``` bash
$ find /etc/bash_completion.d -type l -delete
```

 - 系统级的 bash 补全功能已经完善了，用户不再需要在自己的 bashrc 中执行 source 'bash_completion' 了。

 - bash-completion 这个 USE FLAG 将被移除，package 中带的补全脚本会无条件的被安装。但是这样可能会引起一些问题。想一直使用脚本补全功能的用户可以执行下面的命令：


``` bash
$ emerge -n app-shells/bash-completion
```

下面是 News 原文：

> Title: bash-completion-2.1-r90
> Author: Michał Górny <mgorny@gentoo.org>
> Content-Type: text/plain
> Posted: 2014-11-25
> Revision: 1
> News-Item-Format: 1.0
> Display-If-Installed: >=app-shells/bash-completion-2.1-r90

> Starting with app-shells/bash-completion-2.1-r90, the framework used to enable and manage completions in Gentoo is finally changing in order to properly follow upstream design. This has some important implications for our users.

> Firstly, the install location for completions changes to follow upstream default. The completions enabled before the upgrade will continue to work but you may no longer be able to enable or disable completions installed prior to the upgrade. To solve this issue, the packages installing completions need to rebuilt. The following command can be used to automatically rebuild all the relevant packages:

> $ find /usr/share/bash-completion -maxdepth 1 -type f \
>        '!' -name 'bash_completion' -exec emerge -1v {} +

> Secondly, the autoloading support introduced upstream removes the penalties involved with enabling a great number of completions. This allowed us to switch to an opt-out model where all completions installed after the upgrade are enabled by default. Specific completions can be disabled using 'eselect bashcomp disable ...'

> The model change implies that all current selections done using 'eselect bashcomp' can not be properly migrated and will be disregarded when the relevant completion files are built against the new bash-completion version. After rebuilding all the packages providing completion files, you may want to remove the symlinks that were used to configure the previous framework using the following command:

> $ find /etc/bash_completion.d -type l -delete

> Thirdly, we have solved the issue causing bash-completion support to be enabled by default on login shells only. If you needed to explicitly source 'bash_completion' script in bashrc, you can safely remove that code now since system-wide bashrc takes care of loading it.

> Lastly, we would like to explain that USE=bash-completion is being removed from packages for the completions will be installed unconditionally now. However, this will result in some implicit dependencies being removed. Most specifically, users wishing to use bash-completion will have to request app-shells/bash-completion explicitly, e.g.:

> $ emerge -n app-shells/bash-completion
