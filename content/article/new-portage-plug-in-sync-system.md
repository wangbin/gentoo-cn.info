+++
date = "2015-02-10T18:10:54+08:00"
draft = false
title = "sys-apps/portage-2.2.16 发布，支持多种同步方式"
author = "admin"
tags = ["Portage",]

+++

**2015年2月11日更新，由于 [Bug 539746](https://bugs.gentoo.org/show_bug.cgi?id=539746)，=sys-apps/portage-2.2.16 已经被 Mask 了，建议用户先降级到 2.2.15，以免造成系统损坏。**

2015年1月的[Gentoo 月报](/article/gentoo-monthly-newsletter-2015-01/)中提到了新版本的 Portage 将支持基于 Git 的同步系统。2月2日发布的 Gentoo News 也对将要到来的新功能和改变做了详细的说明，下面是原文：
<!--more-->

> Title: New portage plug-in sync system
 
> Author: Brian Dolbec <dolsen@gentoo.org>
 
> Content-Type: text/plain
 
> Posted: 2015-02-02
 
> Revision: 1
 
> News-Item-Format: 1.0

> Display-If-Installed: sys-apps/portage

> There is a new plug-in sync system in >=sys-apps/portage-2.2.16.
This system will allow third party modules to be easily installed.  Look
for a new layman plug-in sync module in layman's next release.  Next is
a brief look at the changes.  See the url [1] listed below for detailed
descriptions and usage.

> Changes:  /etc/portage/repos.conf/*
 
> New setting for all repository types (needed):
    
>     auto-sync = yes/no, true/false  # default if absent: yes/true

>    New for git sync-type: (applies to clone only)
>    
>     sync-depth = n  where n = {0,1,2,3,...} (optional, default = 1)
>         0 -- full history
>         1 -- shallow clone, only current state (default)
>         2,3,... number of history changes to download

>    New sync-type modules:
>    
>     sync-type = svn  # sync a subversion repository
>     sync-type = websync # Perform an emerge-webrsync operation
>     sync-type = laymanator  # (if installed) runs a layman -s action

>    New native portage postsync hooks
>    
>     /etc/portage/postsync.d/*
>         Runs hooks once, only after all repos have been synced.
>     /etc/portage/repo.postsync.d/*
>         Runs each script with three arguments:
>             repo name, sync-uri, location
>         Each script is run at the completion of every repo synced.

> Migration:

>    Edit /etc/portage/repos.conf/*.conf files, add the auto-sync option
    to each repository definition.  Edit sync-type option to one of the
    supported types {rsync, git, cvs, svn, websync, laymanator}.
    
>     [some-repo]
>     ...
>     sync-type = rsync
>     auto-sync = yes

>    For an existing /etc/portage/repos.conf/layman.conf file:
>    
>     1) change/add the sync-type
>         sync-type = laymanator
>         
>     2) Ensure you have the correct layman version installed with

>    Alternate method:
>    
>        Please see the wiki page url [1] for detailed instructions.

> Primary control of all sync operations has been moved from emerge to
emaint.  "emerge --sync" now just calls the emaint sync module with the
--auto option.  The --auto option performs a sync on only those
repositories with the auto-sync setting not set to 'no' or 'false'. If
it is absent, then it will default to yes and "emerge --sync" will sync
the repository.

> NOTE: As a result of the default auto-sync = True/Yes setting, commands
    like "eix-sync", "esync -l", "emerge --sync && layman -S" will cause
    many repositories to be synced multiple times in a row.  Please edit
    your configs or scripts to adjust for the new operation.

> WARNING:
>    Due to the above default. For any repos that you EXPLICITLY do not
    want to be synced. You MUST set "auto-sync = no"

> The 'emaint sync' module operates similar to layman.  It can sync
single or multiple repos.  See "emaint --help" or for more details and
examples see the wiki page listed below [1].

> Additional help and project API documentation can be found at:

> [1] https://wiki.gentoo.org/wiki/Project:Portage/Sync

[Gentoo Wiki](https://wiki.gentoo.org/wiki/Project:Portage/Sync) 上有更详尽的说明，包括如何手动更新，以及 API 使用的介绍。

其实对于普通用户来说，升级并不麻烦，**emerge =sys-apps/portage-2.2.16** 之后，会在 */etc/portage/repos.conf/* 目录下面自动生成一个 gentoo.conf 的文件，在我的机器上内如如下：

```
[DEFAULT]
main-repo = gentoo

[gentoo]
location = /usr/portage
sync-type = rsync
sync-uri = rsync://rsync.cn.gentoo.org/gentoo-portage
```

注意，因为我使用的是中国的官方 SYNC 镜像， **sync-uri** 跟我在 */etc/portage/make.conf* 中设置的 **SYNC** 变量是一致的。

根据提示，**SYNC** 变量不再需要了，你可以在 */etc/portage/make.conf* 中去掉。

如果你使用 [Overlay](https://wiki.gentoo.org/wiki/Overlay)，好消息是 **app-portage/layman-2.3.0** 已经支持新的 Portage 同步插件机制了，新版的 layman 多了一个 **sync-plugin-portage** USE Flag，你需要开启这个 USE Flag 之后更新 layman，然后还有几处需要手动修改的敌方：

1. 打开 */etc/layman/layman.cfg* 文件，确保包含了下面这行：

    ```
    conf_type : repos.conf
    ```

2. 执行 **layman-updater -R** 命令，会在 */etc/portage/repos.conf/* 下面自动生成 **layman.conf** 文件。

    ``` !bash
    $ layman-updater -R
    ```
    
    该文件包含了你安装的所有 Overlays，例如我安装了 **gentoo-zh** 这个 Overlay，在我的layman.conf 中就有下面几行：
    
    ```
    [gentoo-zh]
    priority = 50
    location = /var/lib/layman/gentoo-zh
    layman-type = git
    sync-type = laymansync
    sync-uri = git://github.com/microcai/gentoo-zh.git
    auto-sync = Yes
    ```
    
3. 删除旧版 layman 的 *make.conf* 文件：

    ```
    $ rm /var/lib/layman/make.conf
    ```
    
4. 编辑 */etc/portage/make.conf* 文件， 删除 **source /var/lib/layman/make.conf** 这行。

执行完上面4步，下次你再执行 **emerge --sync** 的时候，就会自动同步所有的 Overlay 了，前提是你的 Overlay 设置了 **auto-sync = Yes**。

如果你跟我一样使用 **app-portage/eix** 来同步 Portage 和 Overlay 的话，你会注意到，执行 **eix-sync** 的时候，Overlay 同步了两次，因为新的 **emerge --sync** 会也会自动同步 Overlay 了，你并不需要再对 eix 进行额外的配置，你可以删除 */etc/eix-sync.conf* 文件就可以了。

还有一点值得一提的是， **sys-apps/portage-2.2.16** 引入了一个新的命令 **emaint**， 专门用于同步 Portage 和 Overlay，用法跟以前的 **emerge sync** 命令一致：

```
$ emaint sync
```

更多关于 **emaint** 的用法，可以查看 manpage。
