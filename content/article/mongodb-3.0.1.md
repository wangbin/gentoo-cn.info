+++
date = "2015-03-18T18:15:03+08:00"
draft = false
title = "mongoDB 3.0.1"
tags = ["mongodb",]
author = "admin"
+++

mongoDB 最新的3.0.1版 ebuild 现在已经在官方 Portage 中了。
<!--more-->
3.0版的 mongoDB 和之前的2.6版有了很多变化，如果你最近进行了更新，就会发现如下提示：


```
 * Messages for package dev-db/mongodb-3.0.1:

 * !! IMPORTANT !!
 *
 * mongodb configuration files have changed !
 *
 * Make sure you migrate from /etc/conf.d/mongodb to the new YAML standard in /etc/mongodb.conf
 *   http://docs.mongodb.org/manual/reference/configuration-options/
 *
 * Make sure you also follow the upgrading process :
 *   http://docs.mongodb.org/master/release-notes/3.0-upgrade/
 *
 * MongoDB 3.0 introduces the WiredTiger storage engine.
 * WiredTiger is incompatible with MMAPv1 and you need to dump/reload your data if you want to use it.
 * Once you have your data dumped, you need to set storage.engine: wiredTiger in /etc/mongodb.conf
 *   http://docs.mongodb.org/master/release-notes/3.0-upgrade/#change-storage-engine-to-wiredtiger
```

记得对照文档进行升级，以免丢失数据。

抛开 mongoDB 本身不说，这次新的 ebuild 也进行了不小的改动，Gentoo 开发者 [Ultrabug](http://www.ultrabug.fr) 写了一篇 [博客文章](http://www.ultrabug.fr/mongodb-3-0-1/) 介绍了与之前版本 ebuild 的不同之处。主要的改动有：

1. 用户自己的编译优化参数（例如 *-march=native* 和 *-O3*）会自动被忽略，以免造成 crash 或者其他奇怪的行为，例如 [Bug 536688](https://bugs.gentoo.org/show_bug.cgi?id=536688) 和 [Bug 526114](https://bugs.gentoo.org/show_bug.cgi?id=526114)。
2. **static-libs** USE flag 被移除了，新加了 **tools** USE flags。
3. 添加了几个新的 ebuild：
   * **app-admin/mongo-tools**
   * **app-admin/mms-agent**
   * **dev-libs/mongo-c(xx)-driver**。

更多地细节，请阅读 Ultrabug 的 博客文章 [mongoDB 3.0.1](http://www.ultrabug.fr/mongodb-3-0-1/)，感谢 Gentoo 开发者们的无私奉献！

