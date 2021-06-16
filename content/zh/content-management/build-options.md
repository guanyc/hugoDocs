---
title: 构建选项
linktitle: 构建选项
description: 构建选项有助于帮助定义当Hugo构建页面时如何处理一给定页面
date: 2020-03-02
publishdate: 2020-03-02
keywords: [build,content,front matter, page resources]
categories: ["content management"]
menu:
  docs:
    parent: "content-management"
    weight: 31
weight: 31	#rem
draft: false
aliases: [/content/build-options/]
toc: true
---

构建选项都存储在保留的名为`_build_`的前言设定对象中，具有如下默认值:

```yaml
_build:
  render: always
  list: always
  publishResources: true
```

#### render
If `always`, the page will be treated as a published page, holding its dedicated output files (`index.html`, etc...) and permalink.
如果设置成`always`,页面会作为一个发布的页面处理，保持它专用的输出文件(`index.html`等) 和 永久链接。

{{< new-in "0.76.0" >}} 从Hugo版本0.76.0开始我们将这个属性从boolean值扩张成了enum枚举值，有效的值包括是:

never
: 页面不会被任何页面集合包括。

always (default)
: 页面部署输出到磁盘，也不会获得相对链接`RelPermalink`等.

link
: 页面不会输出到disk，但是会获得一个`RelPermalink`.

#### list

从Hugo版本0.68.0开始我们将这个属性从boolean值扩展成了enum枚举值，

有效的值有:

never
: 页面不会被任何页面集合包括

always (default)
: 页面会被所有页面集合包括，比如 `site.RegularPages` 和 `$page.Pages`

local
: 页面会被任何 _local_ 页面结合包裹，比如 `$page.RegularPages`, `$page.Pages`.
一个案例是创建这个页面可以导航，但是无名内容区块。
One use case for this would be to create fully navigable, but headless content sections. {{< new-in "0.68.0" >}}

If true, the page will be treated as part of the project's collections and, when appropriate, returned by Hugo's listing methods (`.Pages`, `.RegularPages` etc...).

如果设置为真，页面会处理成项目集合的一部分，在合适的时候，由Hugo的列表方法返回(`.Pages`, `.RegularPages` 等)。

#### publishResources

If set to true the [Bundle's Resources]({{< relref "content-management/page-bundles" >}}) will be published.
如果设置为真，[Bundle's Resources]({{< relref "content-management/page-bundles" >}})包资源会被发布，可以访问。

Setting this to false will still publish Resources on demand (when a resource's `.Permalink` or `.RelPermalink` is invoked from the templates) but will skip the others.
设置为假，hugo仍然按需发布内容资源(比如,模板调用了资源的`.Permalink` 或 `.RelPermalink`函数)，不过会忽略其他资源。

{{% note %}}
任何页面，不论构建选项如何设置，在函数[`.GetPage`]({{< relref "functions/GetPage" >}})中都是可以访问的。
{{% /note %}}



###  说明用例

#### 不发布一个页面
Project needs a "Who We Are" content file for Front Matter and body to be used by the homepage but nowhere else.
项目需要一个提供“我们是谁”的内容文件来提供前端信息和body部分给主页使用，别的地方不需要使用这个页面。

```yaml
# content/who-we-are.md`
title: Who we are
_build:
 list: false
 render: false
```

```go-html-template
{{/* layouts/index.html */}}
<section id="who-we-are">
{{ with site.GetPage "who-we-are" }}
  {{ .Content }}
{{ end }}
</section>
```

#### 列表展示页面, 而不发布这些页面

网页需要显一些，几百个"感言"作为内容文件但是不发布它们任何一个。

为避免为每个感言设置构建选项，您可以使用[`cascade`]({{< relref "/content-management/front-matter#front-matter-cascade" >}})在感言区块的内容文件设置中

```yaml
#content/testimonials/_index.md
title: Testimonials
# section build options:
_build:
  render: true
# children build options with cascade
cascade:
  _build:
    render: false
    list: true # default
```

```go-html-template
{{/* layouts/_defaults/testimonials.html */}}
<section id="testimonials">
{{ range first 5 .Pages }}
  <blockquote cite="{{ .Params.cite }}">
    {{ .Content }}
  </blockquote>
{{ end }}
</section>
