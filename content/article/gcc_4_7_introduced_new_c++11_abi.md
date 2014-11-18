+++
date = "2014-10-27T10:58:39+08:00"
draft = false
title = "GCC 4.7 引入了新的 C++11 ABI"
author = "Admin"
+++

最近更新过 Portage 的用户都会看到这个消息，GCC 4.7 引入了新的 [2011 ISO C++ 标准](http://www.stroustrup.com/C++11FAQ.html)。
<!--more-->

对于普通用户来说并不需要做什么改动，因为对于从 Portage 中安装存的 GCC，不论是4.7、4.8还是4.9版本，默认使用的还是 gnu++98 标准，但是可以通过手动在 CXXFLAGS 设置中添加 **-std=c++11** 或者 **-std=gnu++11** 来启用。

对于想尝鲜的用户来说，需要特别注意的是，C++11 和 C++98 标准并不兼容，即使在不同版本的 GCC 之间，启用了 C++11 标准编译的代码也可能不兼容，例如[Bug 61758](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=61758)。对于 Gentoo 用户来说，如果你安装了多个版本的 GCC， 并且默认选择的是旧版本的 GCC， 有可能导致某些包编译失败，例如[Bug 513386](https://bugs.gentoo.org/show_bug.cgi?id=513386)。

下面是Gentoo News 原文：

>>> GCC 4.7 introduced the new experimental 2011 ISO C++ standard [1], along with its GNU variant.  This new standard is not the default in gcc-4.7, 4.8 or 4.9, the default is still gnu++98, but it can be enabled by passing -std=c++11 or -std=gnu++11 to CXXFLAGS.
>>>
>>> Users that wish to try C++11 should exercise caution because it is not ABI-compatible with C++98.  Nor is C++11 code compiled with gcc-4.7 guaranteed to be ABI-compatible with C++11 compiled with 4.8, or vice versa [2].  Thus linking C++98 and C++11, or C++11 compiled with different versions of gcc, is likely to cause breakage.  For packages which are self-contained or do not link against any libraries written in C++, there is no problem.  However, switching to C++11 and then building packages which link against any of the numerous libraries in an incompatible ABI can lead to a broken system.
>>>
>>> This is a precautionary news item and the typical user need not do anything. However, as C++11 gains in popularity and the number of packages using it increases, it is important that users understand these issues [3].
>>>
>>> For an ABI compliance checker, and more information about C++ ABIs, see [4].
>>>
>>> Ref.
>>>
>>> [1] http://www.stroustrup.com/C++11FAQ.html
>>>
>>> [2] Upstream GCC does not support ABI-compatibility between gcc-4.x and 4.y for any x != y .  See https://gcc.gnu.org/bugzilla/show_bug.cgi?id=61758.  Even having different versions of gcc installed simultaneously may lead to problems, especially if the older version of gcc is active.  An example is https://bugs.gentoo.org/show_bug.cgi?id=513386.
>>>
>>> [3] Note that some packages like www-client/chromium and net-libs/webkit-gtk are already using C++11 features.
>>>
>>> [4] http://ispras.linuxbase.org/index.php/ABI_compliance_checker