---
title: 部分模板
linktitle: 部分模版
description: 部分模式是规模小的、上下文感知的组件，用于list模版和页面模版中，可以很经济的使得模版符合DRY(dont repeat yourself)原则
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [lists,sections,partials]
menu:
  docs:
    parent: "templates"
    weight: 90
weight: 90
sections_weight: 90
draft: false
aliases: [/templates/partial/,/layout/chrome/,/extras/analytics/]
toc: true
---

{{< youtube pjS4pOLyB7c >}}

## 部分模版解析查询顺序

Partial templates---like [single page templates][singletemps] and [list page templates][listtemps]---have a specific [lookup order][]. However, partials are simpler in that Hugo will only check in two places:
部分模版--和 [single page templates][singletemps] and [list page templates][listtemps]类似, 具有特殊的 [lookup order][]。不过，部分模版查询顺序简单些，仅仅检查两个位置:

1. `layouts/partials/*<PARTIALNAME>.html`
2. `themes/<THEME>/layouts/partials/*<PARTIALNAME>.html`

This allows a theme's end user to copy a partial's contents into a file of the same name for [further customization][customize].
这允许主题的终端用户copy部分模版的内容到同名文件以进行[更多的定制][customize].

## 模版中使用部分模版

All partials for your Hugo project are located in a single `layouts/partials` directory. For better organization, you can create multiple subdirectories within `partials` as well:
HUgo项目中所有的部分模版位于单一目录 `layouts/partials`中.
为文件组织方便，您也可以在此目录中创建多个子目录:
```
.
└── layouts
    └── partials
        ├── footer
        │   ├── scripts.html
        │   └── site-footer.html
        ├── head
        │   ├── favicons.html
        │   ├── metadata.html
        │   ├── prerender.html
        │   └── twitter.html
        └── header
            ├── site-header.html
            └── site-nav.html
```

All partials are called within your templates using the following pattern:
所有部分模版在您的模版内以下面模式调用:

```
{{ partial "<PATH>/<PARTIAL>.html" . }}
```

{{% note %}}
One of the most common mistakes with new Hugo users is failing to pass a context to the partial call. In the pattern above, note how "the dot" (`.`) is required as the second argument to give the partial context. You can read more about "the dot" in the [Hugo templating introduction](/templates/introduction/).
新Hugo使用者的一个最常见的错误是忘记给部分模版调用传递一个上下文.上面的调用模式中,注意作为传递给部分部分模版的上下文的第二个参数的"the dot" (`.`). 更多关于"the dot"点号上下文的信息请参考[Hugo 模版导引](/templates/introduction/)

{{% /note %}}

{{% note %}}
`<PARTIAL>` including `baseof` is reserved. ([#5373](https://github.com/gohugoio/hugo/issues/5373))
请注意`<PARTIAL>` 包括 `baseof` 被Hugo保留使用.
{{% /note %}}

As shown in the above example directory structure, you can nest your directories within `partials` for better source organization. You only need to call the nested partial's path relative to the `partials` directory:
如上面例子目录结构中显示, 可以在`partials`目录中嵌套您的子目录以获得更好代码组织。
调用的时候使用嵌套的部分模版的相对路径即可:

```
{{ partial "header/site-header.html" . }}
{{ partial "footer/scripts.html" . }}
```

### 变量作用范围 Variable Scoping

The second argument in a partial call is the variable being passed down. The above examples are passing the `.`, which tells the template receiving the partial to apply the current [context][context].
部分模版调用的第二个参数是传递给它的变量。上面例子中传递的`.`, 通知接受部分模版的模版应用当前的上下文[context][context].

This means the partial will *only* be able to access those variables. The partial is isolated and *has no access to the outer scope*. From within the partial, `$.Var` is equivalent to `.Var`.
这意味着部分模版将 *仅仅* 可以访问传递的上下文的变量. 部分模版是隔离开的、*不能访问外部范围*.
在部分模版中,`$.Var`等同于 `.Var`


## 从部分模版返回值 Returning a value from a Partial

In addition to outputting markup, partials can be used to return a value of any type. In order to return a value, a partial must include a lone `return` statement.
输出标记以外, 部分模版可以被用来返回任何值。为返回一个值，部分模版必须包含一个独立的 `return` 语句.

## 内联的部分模版 Inline partials

{{< new-in "0.74.0" >}}

You can also define partials inline in the template. But remember that template namespace is global, so you need to make sure that the names are unique to avoid conflicts.
可以在模版中定义内联的部分模版.不过，请注意模版命名空间是全局的，所以需要注意保证内联模版名称要唯一，避免冲突.

```go-html-template
Value: {{ partial "my-inline-partial" . }}

{{ define "partials/my-inline-partial" }}
{{ $value := 32 }}
{{ return $value }}
{{ end }}
```

### 例子 获得具有特殊参数的页面 Example GetFeatured
```go-html-template
{{/* layouts/partials/GetFeatured.html */}}
{{ return first . (where site.RegularPages "Params.featured" true) }}
```

```go-html-template
{{/* layouts/index.html */}}
{{ range partial "GetFeatured.html" 5 }}
  [...]
{{ end }}
```
### 例子 获取图片 Example GetImage
```go-html-template
{{/* layouts/partials/GetImage.html */}}
{{ $image := false }}
{{ with .Params.gallery }}
  {{ $image = index . 0 }}
{{ end }}
{{ with .Params.image }}
  {{ $image = . }}
{{ end }}
{{ return $image }}
```

```go-html-template
{{/* layouts/_default/single.html */}}
{{ with partial "GetImage.html" . }}
  [...]
{{ end }}
```

{{% note %}}
Only one `return` statement is allowed per partial file.
每个部分模版文件仅能包含一个 `return` 语句.
{{% /note %}}

## 缓存的部分模版 Cached Partials

The [`partialCached` template function][partialcached] can offer significant performance gains for complex templates that don't need to be re-rendered on every invocation. The simplest usage is as follows:
模版函数[`partialCached` ][partialcached]对于并不需要每次调用都重新呈现的复杂模版, 可以提供很多的性能提升. 最简单的用法如下:

```
{{ partialCached "footer.html" . }}
```

You can also pass additional parameters to `partialCached` to create *variants* of the cached partial.
也可以提供额外参数给 `partialCached`来创建缓存的部分模版的变体。

For example, you can tell Hugo to only render the partial `footer.html` once per section:
比如，可以通知Hugo仅为每个section部分的`footer.html`呈现一次.

```
{{ partialCached "footer.html" . .Section }}
```

If you need to pass additional parameters to create unique variants, you can pass as many variant parameters as you need:
如果需要传递更多参数来创建唯一的变体, 可以按需传递更多变体参数:

```
{{ partialCached "footer.html" . .Params.country .Params.province }}
```

Note that the variant parameters are not made available to the underlying partial template. They are only use to create a unique cache key.
请注意变体参数对底层的部分模版不可见。他们仅仅用于创建一个唯一的缓存键.

### 例子 Example `header.html`

The following `header.html` partial template is used for [spf13.com](https://spf13.com/):
下面的 `header.html`部分模版使用在[spf13.com](https://spf13.com/):

{{< code file="layouts/partials/header.html" download="header.html" >}}
<!DOCTYPE html>
<html class="no-js" lang="en-US" prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb#">
<head>
    <meta charset="utf-8">

    {{ partial "meta.html" . }}

    <base href="{{ .Site.BaseURL }}">
    <title> {{ .Title }} : spf13.com </title>
    <link rel="canonical" href="{{ .Permalink }}">
    {{ if .RSSLink }}<link href="{{ .RSSLink }}" rel="alternate" type="application/rss+xml" title="{{ .Title }}" />{{ end }}

    {{ partial "head_includes.html" . }}
</head>
<body lang="en">
{{< /code >}}

{{% note %}}
The `header.html` example partial was built before the introduction of block templates to Hugo. Read more on [base templates and blocks](/templates/base/) for defining the outer chrome or shell of your master templates (i.e., your site's head, header, and footer). You can even combine blocks and partials for added flexibility.
注意这个例子中的`header.html`是在Hugo引进block templates之前构建的。阅读[基础模版和块](/templates/base/)获得定义主模版的外壳(比如站点的head、header、和footer)的信息。也可以结合区块和部分模版获得更多的灵活性.
{{% /note %}}


### Example `footer.html`

The following `footer.html` partial template is used for [spf13.com](https://spf13.com/):
下面的 `footer.html` 部分模版使用在[spf13.com](https://spf13.com/):

{{< code file="layouts/partials/footer.html" download="footer.html" >}}
<footer>
  <div>
    <p>
    &copy; 2013-14 Steve Francia.
    <a href="https://creativecommons.org/licenses/by/3.0/" title="Creative Commons Attribution">Some rights reserved</a>;
    please attribute properly and link back.
    </p>
  </div>
</footer>
{{< /code >}}

[context]: /templates/introduction/ "The most easily overlooked concept to understand about Go templating is how the dot always refers to the current context."
[customize]: /themes/customizing/ "Hugo provides easy means to customize themes as long as users are familiar with Hugo's template lookup order."
[listtemps]: /templates/lists/ "To effectively leverage Hugo's system, see how Hugo handles list pages, where content for sections, taxonomies, and the homepage are listed and ordered."
[lookup order]: /templates/lookup-order/ "To keep your templating dry, read the documentation on Hugo's lookup order."
[partialcached]: /functions/partialcached/ "Use the partial cached function to improve build times in cases where Hugo can cache partials that don't need to be rendered with every page."
[singletemps]: /templates/single-page-templates/ "The most common form of template in Hugo is the single content template. Read the docs on how to create templates for individual pages."
[themes]: /themes/
