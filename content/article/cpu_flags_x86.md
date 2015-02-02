+++
date = "2015-02-02T17:41:12+08:00"
draft = false
title = "新的 USE FLAG GROUP：CPU_FLAGS_X86"
author = "admin"
tags = ["USE", "CPU_FLAGS_X86"]

+++

1月28日的更新引入了一个新的 USE flag group **CPU_FLAGS_X86**，Gentoo News 中描述是，与 x86 和 amd64 架构相关的 USE flags，将被转移到 **CPU_FLAGS_X86** 这个特定的 USE flag group 中。
<!--more-->

为了不失去针对 CPU 的优化，用户需要升级 make.conf 和 package.use 文件，例如，如果之前的 USE flag 是这样的：

``` !bash
USE="mmx mmxext sse sse2 sse3"
```

新的 make.conf 文件中要添加一行：

``` !bash
CPU_FLAGS_X86="mmx mmxext sse sse2 sse3"
```

注意 **CPU_FLAGS_X86** 变量可以被用于 x86 和 amd64 架构。

为了方便用户升级，开发者提供了一个 Python 脚本，它可以根据用户电脑上的 */proc/cpuinfo* 信息生成正确的 **CPU_FLAGS_X86** 的值。这个脚本已经在官方的 portage 中，用户只要执行下面的命令进行安装：

``` !bash
$ emerge -1v app-portage/cpuinfo2cpuflags
```

安装完毕后执行下面的命令，就会生成相应的 **CPU_FLAGS_X86**：

``` !bash
$ cpuinfo2cpuflags-x86
```

比如在作者的电脑上，输出的是：

```
CPU_FLAGS_X86="mmx mmxext sse sse2 sse3 sse4_1 ssse3"
```

为了保证迁移的安全和兼容性，开发者建议用户先**不要在 USE 变量中将删除**，而是保留一段时间，等所有的包都已经升级迁移完毕之后再删除。

下面是 News 原文：

> The USE flags corresponding to the instruction sets and other features specific to the x86 (amd64) architecture are being moved into a separate USE flag group called CPU_FLAGS_X86.
>
> In order not to lose CPU-specific optimizations, users will be required to update their make.conf (and package.use) file. For example, if the following USE flags were present:
>
> USE="mmx mmxext sse sse2 sse3"
>
> Those flags need to be copied into:
>
> CPU_FLAGS_X86="mmx mmxext sse sse2 sse3"
>
> Please note that the same CPU_FLAGS_X86 variable is used both on x86 and amd64 systems.
> 
> When in doubt, you can consult the flag descriptions using one of the commonly available tools, e.g. `equery uses` from gentoolkit:
> 
> $ equery uses media-video/ffmpeg
>
> Most of the flag names match /proc/cpuinfo names, with the notable exception of SSE3 which is called 'pni' in /proc/cpuinfo (please also do not confuse it with distinct SSSE3).
> 
> To help users enable the correct USE flags, we are providing a Python script that generates the correct value using /proc/cpuinfo. It can be found in the app-portage/cpuinfo2cpuflags package:
>
> $ emerge -1v app-portage/cpuinfo2cpuflags
> 
> $ cpuinfo2cpuflags-x86
>
> In order to ensure safe migration and maintain compatibility with external repositories, it is recommended to preserve the old USE settings for a period of one year or until no package of interest is still using them.
