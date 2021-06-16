---
title: Hugo 特性
linktitle: Hugo特性
description: HUgo速度极快、内容管理系统健壮、模板系统强大，适合创建所有静态网站类型.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
menu:
  docs:
    parent: "about"
    weight: 20
weight: 20
sections_weight: 20
draft: false
toc: true
---

## 通用特点

* 超级快的构建时间(每个页面1毫秒) [Extremely fast][] build times (&lt; 1 ms per page)
* 完全跨平台, 在macOS、Linux、windows等平台上[安装简单][install]  Completely cross platform, with [easy installation][install] on macOS, Linux, Windows, and more
* 开发时[热加载][LiveReload]随时呈现变化 Renders changes on the fly with [LiveReload][] as you develop
* [强大的主题][Powerful theming]
* [任何地方都可以托管部署][hostanywhere] [Host your site anywhere][hostanywhere]

## 项目组织特点 Organization

* 直观的[项目组织][organization for your projects], 包括网站的区块. Straightforward [organization for your projects][], including website sections
* 可定制的[URLs][] Customizable [URLs][]
* 支持可配置[taxonomies][]分类,包括类别categories和标签tags。  Support for configurable [taxonomies][], including categories and tags
* 通过强大的模板函数实现随意的[内容排序][Sort content].  [Sort content][] as you desire through powerful template [functions][]
* 自动[文章摘要][table of contents]生成 Automatic [table of contents][] generation
* [动态菜单创建][Dynamic menu]. [Dynamic menu][] creation
* [漂亮URLs][Pretty URLs]支持. [Pretty URLs][] support
* 链接[Permalink][]模式支持 [Permalink][] pattern support
* [aliases][]支持的重定向  Redirects via [aliases][]

## 内容 Content

* 原生的Markdown和Emacs Org-Mode支持、同时通过外部命令支持其他语言格式 Native Markdown and Emacs Org-Mode support, as well as other languages via *external helpers* (see [supported formats][])
* [front matter][]前言设定中支持TOML, YAML, and JSON元数据.  TOML, YAML, and JSON metadata support in [front matter][]
* 可定制的主页[homepage][]  Customizable [homepage][]
* 支持多种[内容类型][content types] Multiple [content types][]
* 自动的和用户定义的内容摘要[content summaries][] Automatic and user defined [content summaries][]
* 加强markdown表现力的 [短代码][Shortcodes] [Shortcodes][] to enable rich content inside of Markdown
* 阅读时间功能  ["Minutes to Read"][pagevars] functionality
* 词数统计功能  ["WordCount"][pagevars] functionality

## 其他特性 Additional Features

* 集成的[Disqus][]评论模板 Integrated [Disqus][] comment support
* 集成的 [Google Analytics][]支持 Integrated [Google Analytics][] support
* 自动 [RSS][] 创建 Automatic [RSS][] creation
* 支持[Go][]语言HTML模板。Support for [Go][] HTML templates
* 支持[Chroma][][Syntax highlighting][]格式化高亮  [Syntax highlighting][] powered by [Chroma][]


[aliases]: /content-management/urls/#aliases
[Chroma]: https://github.com/alecthomas/chroma
[content summaries]: /content-management/summaries/
[content types]: /content-management/types/
[Disqus]: https://disqus.com/
[Dynamic menu]: /templates/menus/
[Extremely fast]: https://github.com/bep/hugo-benchmark
[front matter]: /content-management/front-matter/
[functions]: /functions/
[Go]: https://golang.org/pkg/html/template/
[Google Analytics]: https://google-analytics.com/
[homepage]: /templates/homepage/
[hostanywhere]: /hosting-and-deployment/
[install]: /getting-started/installing/
[LiveReload]: /getting-started/usage/
[organization for your projects]: /getting-started/directory-structure/
[pagevars]: /variables/page/
[Permalink]: /content-management/urls/#permalinks
[Powerful theming]: /themes/
[Pretty URLs]: /content-management/urls/
[RSS]: /templates/rss/
[Shortcodes]: /content-management/shortcodes/
[sort content]: /templates/
[supported formats]: /content-management/formats/
[Syntax highlighting]: /tools/syntax-highlighting/
[table of contents]: /content-management/toc/
[taxonomies]: /content-management/taxonomies/
[URLs]: /content-management/urls/
