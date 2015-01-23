+++
date = "2015-01-23T15:29:56+08:00"
draft = false
title = "系统太旧更新出现问题？这都不是事儿"
author = "admin"
tags = ["portage",]

+++

如果你的 Gentoo 系统太久没更新过，你想按照正常的步骤进行更新的时候，也许不幸会遇到一些奇怪的问题而导致升级失败，这时候要怎么办呢？Gentoo 开发者 [Sven Vermeulen](http://blog.siphos.be/) 在这篇[博文](http://blog.siphos.be/2015/01/old-gentoo-system-not-a-problem/)中提到，可以通过使用 Portage 快照，渐进式的升级系统（例如以6个月的时间为间隔，逐步更新）。

<!--more-->

[Portage snapshot historical archives](http://dev.gentoo.org/~swift/snapshots/) 里面介绍了更新的步骤：

1. 下载 一个比你的系统 Portage 新一点的镜像，例如你上次同步 Portage 是在2009年1月，那么你可以下载一个2009年7月份的镜像（比你的Portage新6个月）：

``` bash
# wget http://dev.gentoo.org/~swift/snapshots/portage-20090720.tar.bz2{,.gpgsig,.md5sum,.umd5sum}
# gpg --verify portage-20090720.tar.bz2.gpgsig portage-2090720.tar.bz2
```

2. 备份新的 Portage，解压缩镜像：

``` bash
# mv /usr/portage /usr/portage.latest
# tar xjpf portage-20090720.tar.bz2 -C /usr
```

3. 升级 System

``` bash
# emerge -u system
```

4. 升级 Portage

``` bash
# emerge -u portage
```

5. 切换回最新的 Portage，尝试是否可以更新到最新的系统：

``` bash
# mv /usr/portage /usr/portage.old
# mv /usr/portage.latest /usr/portage
```

如果更新成功，恭喜你，如果还是不行的话，那么重复上面的步骤，只是这次下载一个比2009年7月的镜像更新一点的，再次尝试更新系统。
