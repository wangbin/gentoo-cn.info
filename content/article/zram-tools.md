+++
date = "2015-02-16T12:27:38+08:00"
draft = false
title = "内存不够用？使用 zRam 来压缩内存数据"
author = "admin"
tags = ["zram"]
+++

本文编译自 [zram-tools: zRAM made easy for Gentoo and Sabayon Linux](http://www.danilopianini.org/oldsite/index.php?option=com_content&view=article&id=66:zram-tools-zram-made-easy-for-gentoo-and-sabayon-linux&catid=6:articles&Itemid=23)，原作者是 Danilo Pianini，部分内容有所改动。

如果你的电脑内存比较小，在运行一些比较吃内存的程序，或者编译一些比较大的包的时候，有可能因为内存吃紧，造成大量的系统页面交换（page swap），造成系统响应变慢，严重的时候甚至会很长时间没有响应。遇到这种情况，最好的解决方法就是给电脑升级，加更多的内存。但是如果因为某些情况无法添加内存的时候，你可以试试zRam。
<!--more-->

下面是维基百科上对 [zRam](http://zh.wikipedia.org/wiki/Zram) 的介绍：

> zram（也称为zRAM，先前称为compcache）是Linux内核的一项功能，可提供虚拟内存压缩。zram通过在RAM内的压缩块设备上分页，直到必须使用硬盘上的交换空间，以避免在磁盘上进行分页，从而提高性能。由于zram可以用内存替代硬盘为系统提供交换空间的功能，zram可以在需要交换/分页时让Linux更好利用RAM，在物理内存较少的旧电脑上尤其如此。

Gentoo Wiki 上对 [zRam 配置和使用的介绍](https://wiki.gentoo.org/wiki/Zram)，这里就不赘述了。这篇文章介绍的是另一个工具 [zram-tools](https://bitbucket.org/licho/zram-utils)。这个工具目前还不在官方的 Portage 中，不过最新的0.3版已经在 [nirvana](https://bitbucket.org/danysk/nirvana-overlay) 这个 Overlay 中，你可以添加这个 Overlay， 然后安装 zram-tools：

```
# layman -a nirvana
# emerge zram-utils
```

zram-tools 自带了 systemd 的配置文件，如果你使用 systemd 的话，可以使用下面的命令进行启动和关闭：

```
# systemctl start zswap@zram0.service
# systemctl start zswap@zram0.service
```

如果你不使用 systemd 的话，也可以手动的进行启动和关闭：

```
# /usr/sbin/zswap.sh start zram0
# /usr/sbin/zswap.sh stop zram0
```

需要注意的是，在使用之前，你需要在内核中开启了相应的模块：

```
Device Drivers  --->
    [*] Block devices --->
        <M> Compressed RAM block device support
```

Gentoo Wiki 上建议编译成模块，更多信息请参看 [Gentoo Wiki](https://wiki.gentoo.org/wiki/Zram#Enabling_zram)。

