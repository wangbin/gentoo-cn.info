+++
date = "2015-03-30T23:03:33+08:00"
draft = false
title = "64位机器的 真·multilib 支持"
author = "admin"
tags = ["amd64", "multilib"]
+++

来自3月28日的 Gentoo News 《True multilib support on amd64》，自3月29日起，在64位机器上的 Gentoo 系统将开启真正的 multilib 支持，之前的 *emul-linux-x86* 诸包已经被 Mask，并将被移除。对于用户而言这样的好处是用户可以不必依赖预编译好的二进制包，直接从源码编译32为的库，从而获得与其他 ebuild 一样的灵活性和安全性。
<!--more-->
对于使用 multilib profile 的用户来说，可能需要一些特定的操作，因为新系统可能和旧系统不兼容。例如来自第三方的源可能尚未支持这一特性，需要手动移除。

用户可以通过对某个包启用 **abi_x86_32** USE flag，使其编译必要的32位库。例如在 */etc/portage/package.use* 中添加下面一行：

```
sys-libs/zlib abi_x86_32
```

多数情况下，**emerge** 配合 **--autounmask** 选项，Portage 会提供正确的建议。如果用户倾向将 **ABI_X86** 设置为全局有效，可以在 package.use 中添加下面一行：

```
*/* abi_x86_32
```

如果升级中发生问题（特别是 包之间互相 **block**），建议手工删除之前安装的所有 *emul-linux-x86* 包，再尝试进行升级：

```
$ emerge -C 'app-emulation/emul-linux-x86*'
```

注意，删除之后，有些32位的程序可能无法运行，因此建议立刻进行 **@world** 升级。

下面是原文：

> Starting on 2015-03-29, we are enabling true multilib support on amd64
and masking the old emul-linux-x86 package sets for removal. This
change provides our users with the opportunity to build 32-bit libraries
from source with all the flexibility given by ebuilds and the security
of using mainline ebuilds, rather than relying on pre-packaged binary
versions of them.
>
> The switch to the new system is likely to require a specific action from
the users of our multilib profiles. Since the new system collides with
the old one, the Package Manager must be able to clearly satisfy all
the dependencies using the new system in order to proceed. This may
require unmerging packages installed from third-party repositories that
have not been updated to support the new system.
>
> In order to enable building necessary 32-bit libraries, users will be
required to enable the abi_x86_32 USE flag on respective packages.
This can be done using /etc/portage/package.use entries alike
the following:
>
>     sys-libs/zlib abi_x86_32
>
> In most of the cases, Portage will be able to deliver correct
suggestions for that when using the --autounmask feature. However, some
users may prefer setting ABI_X86 globally to enable 32-bit libraries
in all packages that support building them. This can be done using
the following package.use entry:
>
>     */* abi_x86_32
>
> In case of issues, blockers especially, users are recommended
to manually uninstall any emul-linux-x86 packages that may have been
installed on their systems. This will aid the Package Manager
in choosing the correct dependency resolution path. If using Portage,
this can be done using the following command:

>     $ emerge -C 'app-emulation/emul-linux-x86*'
>
> Note: 32-bit applications may be temporarily broken after this step.
Therefore, it should be followed by a @world upgrade immediately.