---
title: 文章评论
linktitle: 文章评论
description: Hugo附带了一个内部Disqus模板, 不过这并不是能在您Hugo站点工作的唯一评论系统.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-03-09
keywords: [sections,content,organization]
categories: [project organization, fundamentals]
menu:
  docs:
    parent: "content-management"
    weight: 140
weight: 140	#rem
draft: false
aliases: [/extras/comments/]
toc: true
---

Hugo附带了对[Disqus](https://disqus.com/)的支持, Disqus是一个通过JavaScript给站点提供评论和社区功能的第三方服务.

您使用的主题可能已经支持Disqus，如果没有, 在您的模板中添加 [Hugo的内置Disques 部分][disquspartial] 也很简单.

## 添加Disqus

Hugo comes with all the code you need to load Disqus into your templates. Before adding Disqus to your site, you'll need to [set up an account][disqussetup].
Hugo带来了在您的模板中加载Disques的所有代码。在给站点添加Disqus前, 您需要[设置一个Disqus账户][disqussetup]

### 配置 Disqus

Disqus评论功能需要您设置一个唯一参数在[站点配置文件][configuration], 类似这样:

{{< code-toggle copy="false" >}}
disqusShortname = "yourdiscussshortname"
{{</ code-toggle >}}

对许多站点来说, 这样就已经足够。不过，您拥有在单一内容文件的前端设置中设置更多选项的选择:

* `disqus_identifier`
* `disqus_title`
* `disqus_url`

### 显示Hugo的内建Disqus部分模版

Disqus has its own [internal template](https://gohugo.io/templates/internal/#disqus) available, to render it add the following code where you want comments to appear:

Disqus 有它自己的 [内部模板](https://gohugo.io/templates/internal/#disqus), 显示Disques模板请添加 下面代码在您希望评论显示的地方:

```
{{ template "_internal/disqus.html" . }}
```

## 其他评论

如果不想使用Disqus, 也有几个可选的静态站点评论系统:

* [Staticman](https://staticman.net/)
* [Talkyard](https://www.talkyard.io/blog-comments) (Open source, & serverless hosting)
* [IntenseDebate](https://intensedebate.com/)
* [Graph Comment][]
* [Muut](https://muut.com/)
* [Isso](https://posativ.org/isso/) (Self-hosted, Python)
    * [Tutorial on Implementing Isso with Hugo][issotutorial]
* [Utterances](https://utteranc.es/) (Open source, GitHub comments widget built on GitHub issues)
* [Remark](https://github.com/umputun/remark) (Open source, Golang, Easy to run docker)
* [Commento](https://commento.io/) (Open Source, available as a service, local install, or docker image)
* [Hyvor Talk](https://talk.hyvor.com/) (Available as a service)


[configuration]: /getting-started/configuration/
[disquspartial]: /templates/partials/#disqus
[disqussetup]: https://disqus.com/profile/signup/
[forum]: https://discourse.gohugo.io
[front matter]: /content-management/front-matter/
[Graph Comment]: https://graphcomment.com/
[kaijuissue]: https://github.com/spf13/kaiju/issues/new
[issotutorial]: https://stiobhart.net/2017-02-24-isso-comments/
[partials]: /templates/partials/
[MongoDB]: https://www.mongodb.com/
[tweet]: https://twitter.com/spf13
