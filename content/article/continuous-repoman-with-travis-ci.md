+++
date = "2015-04-15T21:55:58+08:00"
draft = false
title = "使用Travis CI 配合 Repoman 对 Overlay 进行持续的 QA 测试"
author = "Admin"
tags = ["Overlay", "Travis CI", "Repoman"]
+++

[Travis CI](https://travis-ci.org/) 是一个在线的持续集成服务，用来构建及测试在 GitHub 托管的代码。如果你有维护自己的 Overlay，并且托管在 Github 上的话，你可以很方便的使用 Travis CI 的服务对自己的 Overlay 进行持续的测试。
<!--more-->

[Repoman](https://dev.gentoo.org/~zmedico/portage/doc/man/repoman.1.html) 是一个针对 Gentoo ebuild 做 QA 测试的工具，属于 Portage 的一部分，Gentoo 开发者在提交 ebuild 之前，都要确保通过 Repoman 的检测。

Gentoo 开发者 Manuel Rüger 在 Github 上发布了一个叫 [repoman-travis](https://github.com/mrueg/repoman-travis) 的项目，将这二者结合起来，对于维护自己 Overlay 的 Gentoo 用户来说，可以对自己的 ebuild 做 持续的 QA 测试了。

repoman-travis 的使用非常简单：

 1. 把 .travis.yml 文件拷贝到你的 Overlay 根目录。
 2. 使用你的 Github 帐号登录到 [Travis CI](https://travis-ci.org/)。
 3. 针对你的 Overlay 的 repository 开启 travis 集成。
 
之后你每次 git push 到 Github 之后，登录 Travis CI 网站，就能看到你的 Overlay 的测试结果了。