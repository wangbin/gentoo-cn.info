+++
date = "2015-08-15T14:39:43+08:00"
draft = false
title = "OpenSSH 7.0"
author = "admin"
tags = ["OpenSSH"]
+++

OpenSSH 7.0 目前已经在 Portage 中，不过是被 Masked 的状态，同时官方发布了一则消息提醒用户未来 OpenSSH 会放弃对 DSA key 的支持，原因是 OpenSSH 的开发者认为 DSA 加密算法不够强壮。
<!--more-->

简单来说，7.0 版的 OpenSSH 默认关闭了对 ssh-dss key 的支持，如果你的密钥是用的 DSA 生成的话，那么当你连接启动了 7.0 版本 OpenSSH 服务器的时候，除非你采取一些特定的配置，否则你将无法连接成功。这也是新版本的 OpenSSH 被暂时 Mask 的原因：

>>>Disables RSA & DSA keys without warning, effectively blocking incoming SSH connections.

（OpenSSH 官方 [Release note](http://lists.mindrot.org/pipermail/openssh-unix-announce/2015-August/000122.html) 中提到长度小于 1024 bits 的 RSA keys 也会被拒绝连接。）

推荐的解决方法是使用更强的算法（比如 rsa、ecdsa 或者 ed25519）来生成 keys。RSA 生成的 keys 在服务器和客户端支持性更好，而 ed25519 安全性更强一些，但是除了 OpenSSH 之外，其他的服务器和客户端也许需要更新以支持这一特性。

如果因为某些原因，你必须使用 DSA 生成的 key，你需要更新 OpenSSH 服务器配置文件 **sshd_config** 和客户端配置文件 **~/.ssh/config**，添加下面一行：

```
PubkeyAcceptedKeyTypes=+ssh-dss
```

因为上游开发者已经决定最终去掉对 DSA keys 的支持，这一解决办法只是临时性的。

补充一下，DSS（Digital Signature Standard）和 DSA（Digital Signature Algorithm）的关系，简单来说就是规范和实现的关系，具体可以参看 [StackExchange 上的回答](http://security.stackexchange.com/questions/51567/why-are-dsa-keys-referred-to-as-dss-keys-when-used-with-ssh)。

下面是消息的原文：

>>>Title: OpenSSH 7.0 disables ssh-dss keys by default

>>>Author: Mike Frysinger <vapier@gentoo.org>

>>>Content-Type: text/plain

>>>Posted: 2015-08-13

>>>Revision: 1

>>>News-Item-Format: 1.0

>>>Display-If-Installed: net-misc/openssh

>>>Starting with the 7.0 release of OpenSSH, support for ssh-dss keys has been disabled by default at runtime due to their inherit weakness.  If you rely on these key types, you will have to take corrective action or risk being locked out.

>>>Your best option is to generate new keys using strong algos such as rsa or ecdsa or ed25519.  RSA keys will give you the greatest portability with other clients/servers while ed25519 will get you the best security with OpenSSH (but requires recent versions of client & server).

>>>If you are stuck with DSA keys, you can re-enable support locally by updating your sshd_config and ~/.ssh/config files with lines like so:
   
>>>        PubkeyAcceptedKeyTypes=+ssh-dss

>>>Be aware though that eventually OpenSSH will drop support for DSA keys entirely, so this is only a stop gap solution.

>>>More details can be found on OpenSSH's website:

>>>        http://www.openssh.com/legacy.html