+++
date = "2015-02-04T10:49:17+08:00"
draft = false
title = "Gentoo 月报2015年1月期发布"
author = "admin"
tags = ["Newsletter", "GMN", "Portage", "Mirror", "Git"]
+++

[Gentoo 月报2015年1月期](http://blogs.gentoo.org/news/2015/02/03/gentoo-monthly-newsletter-january-2015/)发布了，有兴趣的同学可以去看一下。其中比较让人感兴趣的是 Gentoo Portage 的非官方 Git 镜像的建立。
<!--more-->

由 Sven Wegener 和 Michał Górny 两位开发者在 Github 创建了一个 Gentoo Portage 的非官方的 rsync2git 镜像，每半小时与 Gentoo Portage 仓库同步一次。

因为这一镜像是对于 rsync 的全部数据的镜像，对于普通用户来说，可以用来作为同步本地 Portage 的源。**sys-app/portage-9999** 已经支持了基于 Git 的同步，这一特性将会在2.2.16版本中正式引入。如果你想尝鲜试用一下，需要修改 **/etc/portage/repos.conf/gentoo.conf** 文件：

```
[gentoo]
location = /var/db/repos/gentoo
sync-type = git
sync-uri = https://github.com/gentoo/gentoo-portage-rsync-mirror
auto-sync = true
```

需要注意的是，如果你试用当前的 Portage 存放目录（默认是 /usr/portage），在使用 git 同步之前你需要先删除该目录。

另一个需要注意的是，如果你在本地修改了某些文件，下次更新的时候可能会失败，使用过 git 的人都会明白，解决的办法要么 *git commit* 到本地的 git repository， 或者 *git reset --hard* 放弃修改。

使用 Git 镜像的另一个好处就是利于更多开发者的参与合作，如果你修正了某个 ebuild 的 bug，或者写了一个新的软件的 ebuild，你可以先 fork 这个项目，添加自己的修改，然后向开发者提交 pull request，相对于在 Bugzilla 上以附件的方式提交修改，git 的 pull request 无疑更加友好。注意，如果在你提交 pull request 之前，请先阅读项目的 [REAME](https://github.com/gentoo/gentoo-portage-rsync-mirror#README)，以免给自己和开发者造成不必要的麻烦。

考虑到不是所有的 Gentoo 开发者都会喜欢这种方式，所以 Git 镜像和现有的工作流将并存。

如果你想更多了解 Portage 的 Git 镜像项目，请阅读项目的 [REAME](https://github.com/gentoo/gentoo-portage-rsync-mirror#README)。 



