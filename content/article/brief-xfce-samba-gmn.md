+++
date = "2015-03-13T15:14:02+08:00"
draft = false
title = "短消息：Xfce Samba 二月份月报"
author = "admin"
tags = ["Samba", "Xfce", "Newsletter", "GMN"]
+++

短消息三则，Xfce 4.12版，Samba 4.2.0 版，Gentoo 月报2015年2月期发布。
<!--more-->

距上个版本时隔2年10个月，轻量级桌面环境 Xfce 4.12版于2底发布，最新版本已经在官方 Gentoo Portage 中，有兴趣的同学可以直接进行升级了。如果你想了解新版本有什么变化，请移步 [Xfce 4.12 Changelog](http://xfce.org/download/changelogs/4.12)，如果你不了解 Xfce，请阅读 [Xfce 指南](https://wiki.gentoo.org/wiki/Xfce/HOWTO/zh-cn)。

Samba 最新稳定版的4.2.0版于3月初发布，开发组同时宣布将停止对 Samba 3及更早版本的维护，建议用户尽快升级到4.0版本以上。目前4.2.0版本在 Portage 中还是被 Mask 的，最新的不稳定版是4.1.17，但在更新的时候会发生几个软件包互相 block 的情况：

```
[blocks B      ] app-crypt/mit-krb5 ("app-crypt/mit-krb5" is hard blocking app-crypt/heimdal-1.5.3-r2)
[blocks B      ] <net-fs/samba-4.1.7 ("<net-fs/samba-4.1.7" is hard blocking sys-libs/ntdb-1.0-r1)
[blocks B      ] app-crypt/heimdal ("app-crypt/heimdal" is hard blocking app-crypt/mit-krb5-1.13.1)
```

已经有人提了Bug，详情参见 [Bug 490872](https://bugs.gentoo.org/show_bug.cgi?id=490872)。如果你不想升级 Samba，建议先手工屏蔽 *>=samba-4.0*。

如果你想尝鲜，可以参考 [Gentoo Forum](http://forums.gentoo.org/viewtopic-t-1012280.html)上提供的一个简单粗暴的方案解决冲突：

```
# emerge -C mit-krb5 samba
# emerge --update world
```

关于如何从 Samba 3迁移到4，可以参考 Wiki 上的 [Samba4 Migrating HOWTO](https://wiki.gentoo.org/wiki/Samba4_Migrating/HOWTO)。

[Gentoo 月报2015年2月期](http://blogs.gentoo.org/news/2015/03/07/gentoo-monthly-newsletter-february-2015/)上周发布，其中提到了 [Gentoo 邮件列表归档网站 archives.gentoo.org](http://archives.gentoo.org/) 进行了重大升级并重新上线。