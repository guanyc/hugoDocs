---
title: Hugo Modules
linktitle: Hugo Modules Overview
description: How to use Hugo Modules.
date: 2017-02-01
publishdate: 2017-02-01
menu:
  docs:
    parent: "modules"
    weight: 01
weight: 01
sections_weight: 01
categories: [hugo modules]
keywords: [themes,modules]
draft: false
aliases: [/themes/overview/,/themes/]
toc: true
---

**Hugo Modules** 是Hugo的核心构建块.  _模块_ 可以是您主项目或者是提供Hugo定义的下列七个类型中一个或多个类型的小一些的模块: **static**, **content**, **layouts**, **data**, **assets**, **i18n**, 和 **archetypes**.


You can combine modules in any combination you like, and even mount directories from non-Hugo projects, forming a big, virtual union file system.
可以随意组合模块，甚至从非Hugo项目加载目录，组成一个大的、虚拟的联合文件系统。

Hugo模块基于Go模块构建。更多Go模块信息，请参考:

- [https://github.com/golang/go/wiki/Modules](https://github.com/golang/go/wiki/Modules)
- [https://blog.golang.org/using-go-modules](https://blog.golang.org/using-go-modules)

Hugo模块是(全部非常)全新的, 仅仅有少数几个例子项目:

- [https://github.com/bep/docuapi](https://github.com/bep/docuapi) 是测试hugo模块时候被移植到Hugo模块的一个主题。这是非hugo项目加载到Hugo的目录结构的良好的例子。它甚至还包含一个由常规Go模板JS绑定的实现。

- [https://github.com/bep/my-modular-site](https://github.com/bep/my-modular-site) 是一个用于测试的简单站点
