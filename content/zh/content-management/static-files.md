---
title: 静态文件
description: "在网站根目录静态(没有任何修改)提供的文件."
date: 2017-11-18
categories: [content management]
keywords: [source, directories]
menu:
  docs:
    parent: "content-management"
    weight: 130
weight: 130	#rem
aliases: [/static-files]
toc: true
---

缺省情况下, 站点项目中的目录`static/`被用来存放所有 **static files 静态文件** (即样式表、JavaScript脚本、图像等)。静态文件从站点根目录上提供(比如, 静态文件 `static/image.png`, 可以通过`http://{server-url}/image.png`访问, 在文档中可以使用 `![Example image](/image.png)`来包含).

可以配置Hugo查询不同目录，设置**多个目录**来存放静态文件，这通过在[site config][]站点配置中
配置`staticDir`参数来实现。在所有静态目录的所有文件会组成一个联合的文件系统.

这个联合文件系统从网站根提供服务,这样`<SITE PROJECT>/static/me.png`可以通过`<MY_BASEURL>/me.png`访问.

Here's an example of setting `staticDir` and `staticDir2` for a
multi-language site:
下面例子为一个多语言站点设置了两个静态目录 `staticDir` 和 `staticDir2` :

{{< code-toggle copy="false" file="config" >}}
staticDir = ["static1", "static2"]

[languages]
[languages.en]
staticDir2 = "static_en"
baseURL = "https://example.com"
languageName = "English"
weight = 2
title = "In English"
[languages.no]
staticDir = ["staticDir_override", "static_no"]
baseURL = "https://example.no"
languageName = "Norsk"
weight = 1
title = "På norsk"
{{</ code-toggle >}}

上面例子中, 并没有使用主题:

- 英语站点会从"static1","static2" 和 "static_en" 这三个目录中获得静态文件.
  如果文件重复，最右边的目录是会采用
- 挪威站点会从"staticDir_override" and "static_no"的混合目录中静态文件

注意 1
: 添加的这个 `staticDir2` 中的 **2** (可以是从数字0到10) 告诉Hugo您想 **添加** 这个目录
  到使用`staticDir`设定的全局静态资源目录集合中。使用`staticDir`在语言层次会替换全局值(如下面挪威站点中一样)

注意 2
: 上面的例子是多语言站点设置[multihost setup][]. 在通常的设置中, 所有的静态目录可以被所有站点访问.


[site config]: /getting-started/configuration/#all-configuration-settings
[multihost setup]: /content-management/multilingual/#configure-multilingual-multihost
