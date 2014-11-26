+++
date = "2014-10-20T14:26:19+08:00"
draft = false
title = "为什么 nano 是 Gentoo 的“默认编辑器”"
author = "admin"
+++

安装过 Gentoo 的人大多都会知道 [Nano](http://www.nano-editor.org/) 这个文本编辑器。为什么 Gentoo 的开发者们选择 Nano 为“默认编辑器”，作为 Stage3 系统的一部分呢？Gentoo 开发者 [Diego Elio Pettenò](https://www.flameeyes.eu/home) 在他的这篇 Blog [《More explanations: why nano is Gentoo's “default editor”》](https://blog.flameeyes.eu/2009/10/more-explanations-why-nano-is-gentoo-s-default-editor) 中给出了详尽的解释。
<!--more-->

下面的内容编译自 Diego Elio Pettenò 的博客，英文好的朋友可以直接去看原文。

首先要说明的是，这个默认编辑器之所以加上了引号，是因为 Gentoo 系统中并没有真正的默认（Default）编辑器的，在用户未制定默认编辑器的时候（$EDITOR 变量未指定），系统使用 Nano 作为 Fallback 的编辑器，具体原因可以分为三个方面：

 - 技术方面
 
Gentoo 系统集合（system set）中的包（package）必须足够小且依赖足够少，这里的依赖包括了 USE 依赖。相对与 Emacs 和 Vim 这样的 USE 众多的包来说， Nano 的依赖树非常小。

 - [现实方面](http://xkcd.com/378/)
 
无论选择 Emacs 还是 Vim 都会让一部分人不满，选择 Nano 可能会让大部分人不满，但是至少不会引发圣战。

 - 新手友好方面
 
 相对于 Emacs 和 Vim 而言， Nano 对新手来说，无疑更容易上手，更简单得多。 