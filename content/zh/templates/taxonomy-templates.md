---
title: 标签分类模板
linktitle: 标签模板
description: 标签分类模板包括标签list, 标签条目list页,以及在单一页面模版中使用分类信息.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [taxonomies,metadata,front matter,terms,templates]
menu:
  docs:
    parent: "templates"
    weight: 50
weight: 50
sections_weight: 50
draft: false
aliases: [/taxonomies/displaying/,/templates/terms/,/indexes/displaying/,/taxonomies/templates/,/indexes/ordering/, /templates/taxonomies/, /templates/taxonomy/]
toc: true
---

<!-- NOTE! Check on https://github.com/gohugoio/hugo/issues/2826 for shifting of terms' pages to .Data.Pages AND
https://discourse.gohugo.io/t/how-to-specify-category-slug/4856/15 for original discussion.-->

Hugo includes support for user-defined groupings of content called **taxonomies**. Taxonomies are classifications that demonstrate logical relationships between content. See [Taxonomies under Content Management](/content-management/taxonomies) if you are unfamiliar with how Hugo leverages this powerful feature.
Hugo支持用户定义的内容分组，也就是**taxonomies 标签分类**。 标签是展示内容逻辑联系的分类. 参考[内容管理中标签](/content-management/taxonomies), 如果您不熟悉Hugo使用这个强大特性的方式。

Hugo提供了在项目模版中使用标签的多种方式::

* 对展示在[taxonomy list template](#taxonomy-list-template)中的一个标签条目相关的内容的排序
* 对展示在[taxonomy terms template](#taxonomy-terms-template)中的条目排序方式的支持
* 在单一页面模版[single page template][]中对单一页面内容的标签条目列表展示

## 标签列表模版 Taxonomy List Templates

Taxonomy list page templates are lists and therefore have all the variables and methods available to [list pages][lists].
标签列表模版是list页面，因此可以具有[list pages][lists]的所有变量,可以访问[list pages][lists]的所有方法.

### 标签列表模版查询顺序 Taxonomy List Template Lookup Order

参考[Template Lookup](/templates/lookup-order/).

## 标签条目模版

### 标签条目模版查询顺序 Taxonomy Terms Templates Lookup Order

参考 [Template Lookup](/templates/lookup-order/).

### 方法 Taxonomy Methods

A Taxonomy is a `map[string]WeightedPages`.
一个标签是一个 `map[string]WeightedPages` 以字符串为键、以带有权重的页面集合为值的映射字典.

.Get(term)
: Returns the WeightedPages for a term. 返回条目相关的权重页面

.Count(term)
: The number of pieces of content assigned to this term. 返回条目相关的内容数量

.Alphabetical
: Returns an OrderedTaxonomy (slice) ordered by Term. 返回按条目排序的有序标签(slice)

.ByCount
: Returns an OrderedTaxonomy (slice) ordered by number of entries. 返回按条目数目排序的有序标签(slice)

.Reverse
: Returns an OrderedTaxonomy (slice) in reverse order. Must be used with an OrderedTaxonomy.
返回有序标签列表的倒序。必须和有序标签使用。

### 排序的标签 OrderedTaxonomy

Since Maps are unordered, an OrderedTaxonomy is a special structure that has a defined order.
由于Maps是无序的、一个有序的标签集合是具有指定顺序的特殊结构.

```go
[]struct {
    Name          string
    WeightedPages WeightedPages
}
```

每个slice的元素都具有:

.Term
: 标签条目

.WeightedPages
: 有权重页面的slice

.Count
: 赋予本条目的内容的数目.

.Pages
: 所有赋值了本条目的页面。 可以访问[list methods][renderlists] 的所有方法.

## 权重页面 WeightedPages

WeightedPages is simply a slice of WeightedPage.
权重页面WeightedPages是权重页面WeightedPage的slice

```go
type WeightedPages []WeightedPage
```

.Count(term)
: The number of pieces of content assigned to this term.
赋值标签条目的内容的数量

.Pages
: Returns a slice of pages, which then can be ordered using any of the [list methods][renderlists].
返回页面的slice, 此slice随后可以通过[list methods][renderlists]的任何函数排序.

## Displaying custom metadata in Taxonomy Terms Templates

If you need to display custom metadata for each taxonomy term, you will need to create a page for that term at `/content/<TAXONOMY>/<TERM>/_index.md` and add your metadata in its front matter, [as explained in the taxonomies documentation](/content-management/taxonomies/#add-custom-meta-data-to-a-taxonomy-term). Based on the Actors taxonomy example shown there, within your taxonomy terms template, you may access your custom fields by iterating through the variable `.Pages` as such:

如果希望对每个标签条目展示定制的元数据, 您需要为此条目创建页面`/content/<TAXONOMY>/<TERM>/_index.md` 并在前端设置中添加您的元数据, [如同标签文档中说明的一样](/content-management/taxonomies/#add-custom-meta-data-to-a-taxonomy-term)， 类似那里的演员标签例子中显示的那样，在标签条目模版的内部，您可以遍历页面变量 `.Pages` 访问定制的字段:


```go-html-template
<ul>
    {{ range .Pages }}
        <li>
            <a href="{{ .Permalink }}">{{ .Title }}</a>
            {{ .Params.wikipedia }}
        </li>
    {{ end }}
</ul>
```

<!-- Begin /taxonomies/ordering/ -->

## 标签排序 Order Taxonomies

Taxonomies can be ordered by either alphabetical key or by the number of content pieces assigned to that key.
标签可以按照键的字母顺序或者赋予标签值的内容的数量来排序:

### 按字母顺序排序的例子 Order Alphabetically Example

```go-html-template
<ul>
    {{ range .Data.Terms.Alphabetical }}
            <li><a href="{{ .Page.Permalink }}">{{ .Page.Title }}</a> {{ .Count }}</li>
    {{ end }}
</ul>
```

<!-- [See Also Taxonomy Lists](/templates/list/) -->

## 标签内容排序 Order Content within Taxonomies

Hugo uses both `date` and `weight` to order content within taxonomies.
在标签内容内部可以使用 `date` 和 `weight` 来排序

Each piece of content in Hugo can optionally be assigned a date. It can also be assigned a weight for each taxonomy it is assigned to.
Hugo中每个内容可以可选择性的设置一个日期。同时也可以对赋予的每个标签设置一个weight权重值.

When iterating over content within taxonomies, the default sort is the same as that used for section and list pages: first by weight, then by date. This means that if the weights for two pieces of content are the same, then the more recent content will be displayed first.
在标签内遍历内容时, 默认的排序同区块和list页面的排序规则一样, 首先按weight, 然后按日期. 这意味着如果两个内容具有相同的标签权重, 那么日期上最近的内容会先展示.


The default weight for any piece of content is 0. Zero means "does not have a weight", not "has a weight of numerical value zero".
任何内容的默认权重是0. 零表示"不具有权重", 而不是"具有权重值为0".

Weights of zero are thus treated specially: if two pages have unequal weights, and one of them is zero, then the zero-weighted page will always appear after the other one, regardless of the other's weight. Zero weights should thus be used with care: for example, if both positive and negative weights are used to extend a sequence in both directions, a zero-weighted page will appear not in the middle of the list, but at the end.
零权重的内容会被特殊对待: 如果两个页面具有不同的权重, 其中一个为零, 那么零权重的页面总是会在另一个页面的后面出现, 不论另一个页面的权重是多少. 因此零权重的应该谨慎使用: 比如, 如果正数和负数权重被用在一个序列的双向扩展中, 零权重的页面不会出现在list的中间， 而是出现在最后(这不和常理).

### 赋值权重  Assign Weight

Content can be assigned weight for each taxonomy that it's assigned to.
内容可以为每个赋予的标签赋值

```
+++
tags = [ "a", "b", "c" ]
tags_weight = 22
categories = ["d"]
title = "foo"
categories_weight = 44
+++
Front Matter with weighted tags and categories
```

The convention is `taxonomyname_weight`.
赋值惯例是  `标签名称_weight`

In the above example, this piece of content has a weight of 22 which applies to the sorting when rendering the pages assigned to the "a", "b" and "c" values of the 'tag' taxonomy.
上例中，这个页面内容在赋值了标签值"a", "b" 和 "c"的页面呈现时, 排序所使用的权重是22.

It has also been assigned the weight of 44 when rendering the 'd' category.
同时在category `d`的呈现时使用的的权重是44.

With this the same piece of content can appear in different positions in different taxonomies.
在不同标签分类中，同一篇内容会出现在不同的位置.

Currently taxonomies only support the default ordering of content which is weight -> date.
当前标签仅仅支持默认的内容排序,也就是先按weight然后按日期.

<!-- Begin /taxonomies/templates/ -->

There are two different templates that the use of taxonomies will require you to provide.
使用标签分类您需要提供两种不同的模板

Both templates are covered in detail in the templates section.
两种模版在模版部分都有详尽的描述

A [list template](/templates/list/) is any template that will be used to render multiple pieces of content in a single html page. This template will be used to generate all the automatically created taxonomy pages.
[列表模版](/templates/list/)是指用来在单一html页面中展示多个内容的任何模版.这个模板被用来生成全部的自动生成的标签页面。

A [taxonomy terms template](/templates/terms/) is a template used to
generate the list of terms for a given template.
[标签条目模版](/templates/terms/)是用来生成标签条目list的任何模版.

<!-- Begin /taxonomies/displaying/ -->

There are four common ways you can display the data in your
taxonomies in addition to the automatic taxonomy pages created by hugo
using the [list templates](/templates/list/):
在Hugo自动创建的标签页之外, 还可以有四种使用[list templates](/templates/list/)的常见方式来展示标签的数据:

1. 对指定内容, 可以列出所有的赋予的标签条目 For a given piece of content, you can list the terms attached
2. 对指定内容，可以列出具有相同标签条目的其他内容 For a given piece of content, you can list other content with the same term
3. 可以列出一个标签分类的所有条目 You can list all terms for a taxonomy
4. 可以列出所有标签分类(和他们的条目)You can list all taxonomies (with their terms)

## 展示页面的内容标签 Display a Single Piece of Content's Taxonomies

Within your content templates, you may wish to display the taxonomies that piece of content is assigned to.
在内容模版中，您可能希望展示内容被赋予的标签条目

Because we are leveraging the front matter system to define taxonomies for content, the taxonomies assigned to each content piece are located in the usual place (i.e., `.Params.<TAXONOMYPLURAL>`).
由于我们使用前言设定来定义内容的标签, 赋予每个内容的标签位于通常位置 (比如, `.Params.<TAXONOMYPLURAL>`)

### 例子: 在单页模版内列出标签

```go-html-template
<ul>
    {{ range (.GetTerms "tags") }}
        <li><a href="{{ .Permalink }}">{{ .LinkTitle }}</a></li>
   {{ end }}
</ul>
```

If you want to list taxonomies inline, you will have to take care of optional plural endings in the title (if multiple taxonomies), as well as commas. Let's say we have a taxonomy "directors" such as `directors: [ "Joel Coen", "Ethan Coen" ]` in the TOML-format front matter.
为列出内联的标签， 需要注意可选的标题的复数结尾(多个标签里)，以及逗号。 假设页面有标签 "directors"，在toml格式的
前端设置里面有 `directors: [ "Joel Coen", "Ethan Coen" ]`


展示标签，使用下面例子:

### 例子: 单页模版中逗号分隔的标签

```go-html-template
{{ $taxo := "directors" }} <!-- Use the plural form here -->
{{ with .Param $taxo }}
    <strong>Director{{ if gt (len .) 1 }}s{{ end }}:</strong>
    {{ range $index, $director := . }}
        {{- if gt $index 0 }}, {{ end -}}
        {{ with $.Site.GetPage (printf "/%s/%s" $taxo $director) -}}
            <a href="{{ .Permalink }}">{{ $director }}</a>
        {{- end -}}
    {{- end -}}
{{ end }}
```

Alternatively, you may use the [delimit template function][delimit] as a shortcut if the taxonomies should just be listed with a separator. See {{< gh 2143 >}} on GitHub for discussion.、
或者, 如果标签是逗号或者其他分隔符分隔的, 您可以使用[delimit template function][delimit]作为短代码,

## 具有相同标签条目的内容列表 List Content with the Same Taxonomy Term

If you are using a taxonomy for something like a series of posts, you can list individual pages associated with the same taxonomy. This is also a quick and dirty method for showing related content:
如果对一系列posts应用了相同的标签, 您可以列出和这个相同标签关联的单独页面的list。
这也是显示相关内容的快速方法,虽然方法丑陋了点。

### 例子: 显示同一系列中的内容

```go-html-template
<ul>
    {{ range .Site.Taxonomies.series.golang }}
        <li><a href="{{ .Page.RelPermalink }}">{{ .Page.Title }}</a></li>
    {{ end }}
</ul>
```

## 对给定标签列出所有内容 List All content in a Given taxonomy

This would be very useful in a sidebar as “featured content”. You could even have different sections of “featured content” by assigning different terms to the content.
这个在提供侧边栏的功能比如 “featured content”时很有用。通过给内容赋予不同的条目,甚至可以有多个不同的 “featured content” 部分.

### 例子:  "Featured" 内容分组

```go-html-template
<section id="menu">
    <ul>
        {{ range $key, $taxonomy := .Site.Taxonomies.featured }}
        <li>{{ $key }}</li>
        <ul>
            {{ range $taxonomy.Pages }}
            <li hugo-nav="{{ .RelPermalink}}"><a href="{{ .Permalink}}">{{ .LinkTitle }}</a></li>
            {{ end }}
        </ul>
        {{ end }}
    </ul>
</section>
```

## 呈现站点的标签 Render a Site's Taxonomies

If you wish to display the list of all keys for your site's taxonomy, you can retrieve them from the [`.Site` variable][sitevars] available on every page.
如果需要显示站点标签的所有key, 可以从每个页面可以访问的[`.Site` variable][sitevars]来获取。

This may take the form of a tag cloud, a menu, or simply a list.
可以采取标签云的形式，或者菜单、或者就是一个简单的列表。

The following example displays all terms in a site's tags taxonomy:
下面例子展示了站点tags标签的所有条目:

### 例子: 所有标签的列表 {#example-list-all-site-tags}

```go-html-template
<ul>
    {{ range .Site.Taxonomies.tags }}
            <li><a href="{{ .Page.Permalink }}">{{ .Page.Title }}</a> {{ .Count }}</li>
    {{ end }}
</ul>
```

### 例子: 累出所有标签、条目、和被赋值的内容

This example will list all taxonomies and their terms, as well as all the content assigned to each of the terms.
这个例子list所有的标签，他们的条目，以及被赋予条目的所有内容

{{< code file="layouts/partials/all-taxonomies.html" download="all-taxonomies.html" download="all-taxonomies.html" >}}
<section>
    <ul id="all-taxonomies">
        {{ range $taxonomy_term, $taxonomy := .Site.Taxonomies }}
            {{ with $.Site.GetPage (printf "/%s" $taxonomy_term) }}
                <li><a href="{{ .Permalink }}">{{ $taxonomy_term }}</a>
                    <ul>
                        {{ range $key, $value := $taxonomy }}
                            <li>{{ $key }}</li>
                            <ul>
                                {{ range $value.Pages }}
                                    <li hugo-nav="{{ .RelPermalink}}">
                                        <a href="{{ .Permalink}}">{{ .LinkTitle }}</a>
                                    </li>
                                {{ end }}
                            </ul>
                        {{ end }}
                    </ul>
                </li>
            {{ end }}
        {{ end }}
    </ul>
</section>
{{< /code >}}

## `.Site.GetPage` for Taxonomies

Because taxonomies are lists, the [`.GetPage` function][getpage] can be used to get all the pages associated with a particular taxonomy term using a terse syntax. The following ranges over the full list of tags on your site and links to each of the individual taxonomy pages for each term without having to use the more fragile URL construction of the ["List All Site Tags" example above]({{< relref "#example-list-all-site-tags" >}}):
因为taxonomies是list,所以可以使用[`.GetPage` ][getpage]函数以紧凑的形式获得与特定条目关联的所有页面。下例遍历站点的tags的完整list, 链接到每个条目独立的标签页，不需要用上面例子["List All Site Tags" example above]({{< relref "#example-list-all-site-tags" >}})中的脆弱的URL构建方式.

{{< code file="links-to-all-tags.html" >}}
{{ $taxo := "tags" }}
<ul class="{{ $taxo }}">
    {{ with ($.Site.GetPage (printf "/%s" $taxo)) }}
        {{ range .Pages }}
            <li><a href="{{ .Permalink }}">{{ .Title}}</a></li>
        {{ end }}
    {{ end }}
</ul>
{{< /code >}}

<!-- TODO: ### `.Site.GetPage` Taxonomy List Example -->



[delimit]: /functions/delimit/
[getpage]: /functions/getpage/
[lists]: /templates/lists/
[renderlists]: /templates/lists/
[single page template]: /templates/single-page-templates/
[sitevars]: /variables/site/
