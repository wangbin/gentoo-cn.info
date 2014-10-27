+++
date = "2014-10-21T12:39:33+08:00"
draft = false
title = "Porticron —— 自动同步 Portage 的脚本"
tags = ["portage", "shell"]
author = "Admin"
+++

[Porticron](https://github.com/hollow/porticron) 是一个自动同步 portage 的 shell 脚本，类似 [Debian](https://www.debian.org/) 下的 [Apticron](http://terminal28.com/apticron-automatic-email-notification-when-updates-available-debian/)，它可以通过 Cron Job 自动同步 Portage tree，并且发邮件通知你需要更新的软件，此外它还可以通过自动调用 glsa-check 工具检查安全方面的更新。 
<!--more-->
Porticron 已经在官方的 Portage 中，可以直接使用 emerge 安装：

``` bash
# emerge porticron
```

安装完后，配置文件在 /etc/porticron.conf，porticron 的配置非常简单：

 - **SYNC_CMD**
 
    配置 porticron 用于同步 Portage tree 的命令，默认是 */usr/bin/emerge --sync*，如果你安装了 *app-portage/eix*，可以配置使用 */usr/bin/eix-sync* 来进行同步。
 
 - **SYNC_OVERLAYS_CMD**
 
    配置 portage 用于同步 Overlay 的命令，默认是不同步的，如果过你安装了 *app-portage/layman* 并且使用了 Overlay， 可以将此项设置为 */usr/bin/layman --sync-all*。
    
 - **UPGRADE_OPTS**
 
    配置 emerge 选项，用于检查可以更新的软件。
    
 - **DIFF_CMD**
 
    配置 ebuild 更新列表，需要安装 *app-portage/eix*。注意默认的配置*/usr/bin/eix-sync -d* 已经过时了，可以修改成 */usr/bin/eix-diff*。
    
 - **RCPT**
 
    配置收件人邮箱。
    
 - **SUBJECT**
 
    配置邮件主题。
    
 - **SUBJECT_WARN**
 
    配置警告邮件的主题，比如发现有安全更新的时候。
    
 - **SENDMAIL**
 
    配置发送邮件的命令。

## 注意事项

 - 如果你希望 porticron 能够发送提醒邮件，需要事先配置好发送邮件的服务。如果你不需要发送邮件的服务，可以使用 **-n** 参数来禁止 porticron 发送邮件。  
 
 - 在配置好之后，需要手动将 porticron 加入到 Cron Job 中。
   
下面是一个 Porticron 发送的提醒邮件的例子：

>>>  porticron report [Tue, 09 Dec 2008 05:07:06 +0100]
>>>  ========================================================================
>>>
>>>  porticron has detected that some packages need upgrading:
>>>
>>>      [ebuild     U ] sys-libs/timezone-data-2008i [2008g-r1]
>>>      [ebuild     U ] sys-apps/man-pages-3.14 [3.12]
>>>      [ebuild     U ] sys-process/htop-0.8.1-r1 [0.8.1]
>>>      [ebuild     U ] sys-apps/util-linux-2.14.1 [2.13.1.1]
>>>      [ebuild     U ] app-portage/elogv-0.7.2 [0.7.1]
>>>      [ebuild     U ] sys-apps/busybox-1.11.3 [1.11.1]
>>>      [ebuild     U ] app-admin/eselect-1.0.11-r1 [1.0.10]
>>>
>>>  ========================================================================
>>>
>>>  You can perform the upgrade by issuing the command:
>>>
>>>      emerge --deep --update world
>>>
>>>  as root on foo.example.com

>>>  It is recommended that you pretend the upgrade first to confirm that
>>>  the actions that would be taken are reasonable. The upgrade may be
>>>  pretended by issuing the command:
>>>
>>>      emerge --deep --update --pretend world