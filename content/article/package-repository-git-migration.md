+++
date = "2015-08-13T11:34:42+08:00"
draft = false
title = "Gentoo Portage tree 正式从 CVS 迁移到了 Git"
author = "admin"
tags = ["git", "portage"]
+++

8月12日 Gentoo 官网上正式宣布将 package repository （即 Portage tree，这两个术语都不知道怎么翻译，就用原文了）从 CVS 迁移到了 Git，官方的消息原文在[这里](https://www.gentoo.org/news/2015/08/12/git-migration.html)。
<!--more-->

如果你有兴趣，可以使用 git 从 git.gentoo.org checkout 出来整个 Portage tree，或者访问[Git web 界面](https://gitweb.gentoo.org/repo/gentoo.git/)。

这一变化主要是面向 Gentoo 开发者的，对于普通用户来说，并没有什么影响，之前的更新方法（rsync，webrsync，snapshot）依然可以正常使用，官方随后会发布如何配置使用 git 更新的方法。
