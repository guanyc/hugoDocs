---
title: 首页模板
linktitle: 首页模板
description: 网站的首页总是和其他页面采用不同的格式。基于这个原因, 定义新站点首页作为一个独特的模板，Hugo中很简单
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [homepage]
menu:
  docs:
    parent: "templates"
    weight: 30
weight: 30
sections_weight: 30
draft: false
aliases: [/layout/homepage/,/templates/homepage-template/]
toc: true
---

Homepage is a `Page` and therefore has all the [page variables][pagevars] and [site variables][sitevars] available for use.
首页也是一个`页面`, 因此所有的[page variables][pagevars] 和 [site variables][sitevars]都可以访问.

{{% note "The Only Required Template" %}}
The homepage template is the *only* required template for building a site and therefore useful when bootstrapping a new site and template. It is also the only required template if you are developing a single-page website.
首页模板是构建站点*唯一*必须的模板, 因此有助于构建新站点和其他模板。如果在开发单页面模板，首页模板也是唯一需要的模板
{{% /note %}}

{{< youtube ut1xtRZ1QOA >}}

## 首页模板查询顺序

参考 [Template Lookup](/templates/lookup-order/).

## 为首页添加内容和前言设定

首页, 同其他[Hugo 列表页][lists]类似，可以接受 `_index.md` 文件里的内容和前言设定. 这个文件应该位于`content`的根目录(也就是`content/_index.md`).  然后可以给首页添加主题内容和元数据，和其他内容文件一样。

参考下面的首页模板或者[内容组织][contentorg], 获得更多关于在列表页中添加内容和前言设定时`_index.md`文件的作用的信息。

## 首页模板例子

下面是首页模板的例子, 使用了[partial][partials], [base][] 模板和位于`content/_index.md`的内容文件来填充`{{.Title}}` 和 `{{.Content}}` [页面变量][pagevars].

{{< code file="layouts/index.html" download="index.html" >}}
{{ define "main" }}
    <main aria-role="main">
      <header class="homepage-header">
        <h1>{{.Title}}</h1>
        {{ with .Params.subtitle }}
        <span class="subtitle">{{.}}</span>
        {{ end }}
      </header>
      <div class="homepage-content">
        <!-- Note that the content for index.html, as a sort of list page, will pull from content/_index.md -->
        {{.Content}}
      </div>
      <div>
        {{ range first 10 .Site.RegularPages }}
            {{ .Render "summary"}}
        {{ end }}
      </div>
    </main>
{{ end }}
{{< /code >}}

[base]: /templates/base/
[contentorg]: /content-management/organization/
[lists]: /templates/lists/
[lookup]: /templates/lookup-order/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[sitevars]: /variables/site/
