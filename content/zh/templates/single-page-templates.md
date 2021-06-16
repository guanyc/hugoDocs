---
title: 单独页面模板
linktitle: 单独页面
description: The primary view of content in Hugo is the single view. Hugo will render every Markdown file provided with a corresponding single template. Hugo中内容的主要视图是单独页面. Hugo会显示每个提供了单独模板的Markdown文件.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-04-06
categories: [templates]
keywords: [page,templates]
menu:
  docs:
    parent: "templates"
    weight: 60
weight: 60
sections_weight: 60
draft: false
aliases: [/layout/content/]
toc: true
---

## 单独页面模板查询顺序 Single Page Template Lookup Order

参考 [Template Lookup](/templates/lookup-order/).

## 例子 Example Single Page Templates

Content pages are of the type `page` and will therefore have all the [page variables][pagevars] and [site variables][] available to use in their templates.
类型 `page`的内容页面, 具有所有的 [page variables][pagevars] 页面变量和 [site variables][] 站点变量，可以在他们的模板中访问使用.

### `posts/single.html`

This single page template makes use of Hugo [base templates][], the [`.Format` function][] for dates, the [`.WordCount` page variable][pagevars], and ranges through the single content's specific [taxonomies][pagetaxonomy]. [`with`][] is also used to check whether the taxonomies are set in the front matter.
下面的单独页面模板使用了Hugo的[base templates][]基础模板,  处理日期的[`.Format` function][]函数, [`.WordCount` page variable][pagevars]页面变量, 遍历了单独页面内容的特定[taxonomies][pagetaxonomy].
使用了[`with`][]函数检测前端是否设置了标签.

{{< code file="layouts/posts/single.html" download="single.html" >}}
{{ define "main" }}
<section id="main">
  <h1 id="title">{{ .Title }}</h1>
  <div>
        <article id="content">
           {{ .Content }}
        </article>
  </div>
</section>
<aside id="meta">
    <div>
    <section>
      <h4 id="date"> {{ .Date.Format "Mon Jan 2, 2006" }} </h4>
      <h5 id="wordcount"> {{ .WordCount }} Words </h5>
    </section>
    {{ with .Params.topics }}
    <ul id="topics">
      {{ range . }}
        <li><a href="{{ "topics" | absURL}}{{ . | urlize }}">{{ . }}</a> </li>
      {{ end }}
    </ul>
    {{ end }}
    {{ with .Params.tags }}
    <ul id="tags">
      {{ range . }}
        <li> <a href="{{ "tags" | absURL }}{{ . | urlize }}">{{ . }}</a> </li>
      {{ end }}
    </ul>
    {{ end }}
    </div>
    <div>
        {{ with .PrevInSection }}
          <a class="previous" href="{{.Permalink}}"> {{.Title}}</a>
        {{ end }}
        {{ with .NextInSection }}
          <a class="next" href="{{.Permalink}}"> {{.Title}}</a>
        {{ end }}
    </div>
</aside>
{{ end }}
{{< /code >}}

To easily generate new instances of a content type (e.g., new `.md` files in a section like `project/`) with preconfigured front matter, use [content archetypes][archetypes].
简单的使用预定义的前端内容设置生成内容类型的新实例(比如在`project/`区块中创建新的`.md`文件), 请使用 [content archetypes][archetypes]

[archetypes]: /content-management/archetypes/
[base templates]: /templates/base/
[config]: /getting-started/configuration/
[content type]: /content-management/types/
[directory structure]: /getting-started/directory-structure/
[dry]: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself
[`.Format` function]: /functions/format/
[front matter]: /content-management/front-matter/
[pagetaxonomy]: /templates/taxonomy-templates/#displaying-a-single-piece-of-content-s-taxonomies
[pagevars]: /variables/page/
[partials]: /templates/partials/
[section]: /content-management/sections/
[site variables]: /variables/site/
[spf13]: https://spf13.com/
[`with`]: /functions/with/
