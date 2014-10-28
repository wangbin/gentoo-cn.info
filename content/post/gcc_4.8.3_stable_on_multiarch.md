+++
date = "2014-10-28T11:20:08+08:00"
draft = false
title = "GCC 稳定版本升级到 4.8.3"
author = "admin"
tags = ["gcc"]
+++

10月24日的 Portage 更新将 **=sys-devel-gcc-4.8.3** 在 amd64、arm、hppa、ppc、ppc64 这些架构下标示为稳定（stable）版本，更多信息可以参考 [Bug 516152](https://bugs.gentoo.org/show_bug.cgi?id=516152)。
<!--more-->

GCC 4.8.3 默认启用了 **Stack Smashing Protection (SSP)** 这一安全特性，下面是 Gentoo News 原文：

>>> Beginning with GCC 4.8.3, Stack Smashing Protection (SSP) will be enabled by default.  The 4.8 series will enable -fstack-protector while 4.9 and later enable -fstack-protector-strong.
>>>
>>> SSP is a security feature that attempts to mitigate stack-based buffer overflows by placing a canary value on the stack after the function return pointer and checking for that value before the function returns. If a buffer overflow occurs and the canary value is overwritten, the program aborts.
>>> 
>>> There is a small performance cost to these features.  They can be disabled with -fno-stack-protector.
>>>
>>> For more information these options, refer to the GCC Manual, or the following articles.
>>>
>>> http://en.wikipedia.org/wiki/Buffer_overflow_protection
>>> http://en.wikipedia.org/wiki/Stack_buffer_overflow
>>> https://securityblog.redhat.com/tag/stack-protector
>>> http://www.outflux.net/blog/archives/2014/01/27/fstack-protector-strong
