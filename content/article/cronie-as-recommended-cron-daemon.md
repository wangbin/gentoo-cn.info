+++
date = "2015-07-28T12:14:07+08:00"
draft = false
title = "cronie 取代 vixie-cron 成为 Gentoo 手册推荐的 Cron Daemon"
author = "admin"
tags = ["cron"]
+++

这条消息有些过时了，不过我都忘了自己多久没有从头安装新系统了，这次给新的服务器安装系统，查阅官方手册的时候，还是发现了不少改动的，对我而言，最大的变化当属 GRUB2 和 GPT 分区，这些手册都有很详细的说明，我就不重复了。在安装系统工具一节，手册推荐的 Cron Daemon 从 vixie-cron 换成了 cronie，这引起了我的好奇。
<!--more-->

Google 了一下，发现早在2013年底的时候，开发者就在讨论[替换的事情](http://gentoo.2317880.n4.nabble.com/Recommend-cronie-instead-of-vixie-cron-in-handbook-td272764.html)了。原因其实很简单， vixie-cron 上游开发者最后一次 release 是在2004年，居然有11年没有更新了，之后一些 bug 都是由各发行版在 patch，后来 Fedora fork 了这个项目，于是就有了 cronie。 Cronie 基本可以看作是 vixie-cron 的 drop-in replacement，在现有系统上替换也很简单：

``` bash
# rc-update del vixie-cron default
# /etc/init.d/vixie-cron stop
# emerge -C vixie-cron
# emerge cronie
# rc-update add cronie default
# /etc/init.d/cronie start 
```


