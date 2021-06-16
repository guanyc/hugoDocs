---
title: Content Sections 内容块
linktitle: 内容块
description: "Hugo生成匹配站点内容的 **section tree 内容块的树 ** ."
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [content management]
keywords: [lists,sections,content types,organization]
menu:
  docs:
    parent: "content-management"
    weight: 50
weight: 50	#rem
draft: false
aliases: [/content/sections/]
toc: true
---

一个**内容块Section**由基于`content/`目录下的组织结构而定义的页面集合.

默认情况下, 在`content/`目录下的**第一级**目录构成它们自己的内容块(**根块**)

If a user needs to define a section `foo` at a deeper level, they need to create
a directory named `foo` with an `_index.md` file (see [Branch Bundles][branch bundles]
for more information).
如果用户需要在更深层定义n内容块,比如`foo`, 那么需要创建一个名为`foo`的目录并创建一个`_index.md`文件(参考 [Branch Bundles][branch bundles]获得更多信息)

{{% note %}}
**内容块**不能被前言设定参数定义或者重载--内容块严格从内容组织结构派生出来.
{{% /note %}}

## 嵌套块

内容块可以在任意深度嵌套

```bash
content
└── blog        <-- 内容块，因为是content/下第一级目录
    ├── funny-cats
    │   ├── mypost.md
    │   └── kittens         <-- 内容块 包含 _index.md
    │       └── _index.md
    └── tech                <-- 内容块 包含 _index.md
        └── _index.md
```

**The important part to understand is, that to make the section tree fully navigational, at least the lower-most section needs a content file. (e.g. `_index.md`).**
**需要记住的最重要的是, 为了使内容块可以完全导航, 至少最底层的块需要一个内容文件(比如,`_index.md`).**


{{% note %}}
When we talk about a **section** in correlation with template selection, it is
currently always the *root section* only (`/blog/funny-cats/mypost/ => blog`).
当讨论和模板选择有关的 **块**时, 当前总是指的 *根块* (`/blog/funny-cats/mypost/ => blog`).

If you need a specific template for a sub-section, you need to adjust either the `type` or `layout` in front matter.
如果需要为子块选择特定模板，需要调整内容的前言设定中的`type` 或者 `layout`.
{{% /note %}}

## 例子: 面包屑导航

With the available [section variables and methods](#section-page-variables-and-methods) you can build powerful navigation. One common example would be a partial to show Breadcrumb navigation:

内容块提供了[块页面变量和方法](#section-page-variables-and-methods)，使用这些变量和方法您可以构建强大的页面导航. 一个常见例子是显示面包屑导航的部分模板:


{{< code file="layouts/partials/breadcrumb.html" download="breadcrumb.html" >}}
<ol  class="nav navbar-nav">
  {{ template "breadcrumbnav" (dict "p1" . "p2" .) }}
</ol>
{{ define "breadcrumbnav" }}
{{ if .p1.Parent }}
{{ template "breadcrumbnav" (dict "p1" .p1.Parent "p2" .p2 )  }}
{{ else if not .p1.IsHome }}
{{ template "breadcrumbnav" (dict "p1" .p1.Site.Home "p2" .p2 )  }}
{{ end }}
<li{{ if eq .p1 .p2 }} class="active"{{ end }}>
  <a href="{{ .p1.Permalink }}">{{ .p1.Title }}</a>
</li>
{{ end }}
{{< /code >}}

## 块页面变量和方法

也请参考[页变量](/variables/page/).

<!-- todo -->
{{< readfile file="/content/en/readfiles/sectionvars.md" markdown="true" >}}

## 内容块列表 Content Section Lists

Hugo will automatically create pages for each *root section* that list all of the content in that section. See the documentation on [section templates][] for details on customizing the way these pages are rendered.
Hugo自动为每一个*根块*创建页面，页面上列出了本块包含的内容. 参考[块模板][section templates]获得定制呈现这些页面的细节.

## Content *Section* vs Content *Type*

By default, everything created within a section will use the [content `type`][content type] that matches the *root section* name. For example, Hugo will assume that `posts/post-1.md` has a `posts` content `type`. If you are using an [archetype][] for your `posts` section, Hugo will generate front matter according to what it finds in `archetypes/posts.md`.

缺省情况下, 在内容块内创建的任何内容都使用和  *root section 根块* 名称匹配的 [内容 `类型`][content type]. 比如hugo会假设`posts/post-1.md`具有`posts` 内容 `type`. 如果对于`posts`块使用了[archetype][]，Hugo会从`archetypes/posts.md`原型中穿件内容的前言设定.

[archetype]: /content-management/archetypes/
[content type]: /content-management/types/
[directory structure]: /getting-started/directory-structure/
[section templates]: /templates/section-templates/
[branch bundles]: /content-management/page-bundles/#branch-bundles
