---
title: 标签分类
linktitle: 标签分类
description: Hugo支持常见分类，也支持自定义的分类.
date: 2017-02-01
publishdate: 2017-02-01
keywords: [taxonomies,metadata,front matter,terms]
categories: [content management]
menu:
  docs:
    parent: "content-management"
    weight: 80
weight: 80	#rem
draft: false
aliases: [/taxonomies/overview/,/taxonomies/usage/,/indexes/overview/,/doc/indexes/,/extras/indexes]
toc: true
---

## 分类的定义

Hugo includes support for user-defined groupings of content called **taxonomies**. Taxonomies are classifications of logical relationships between content.
Hugo包含对用户定义的内容分类-**taxonomies 标签**的支持. 标签是对内容逻辑关系的分类.

### 定义 Definitions

标签 Taxonomy
: 可以被用来对内容分类的类型

条目 Term
: 分类中的键 a key within the taxonomy

值 Value
: 赋值某条目的一条内容 a piece of content assigned to a term


## 分类举例: 电影网站

Let's assume you are making a website about movies. You may want to include the following taxonomies:
我们假设在建的是关于电影的网站。您可能想包括如下分类:

* Actors 演员
* Directors 导演
* Studios 工作室
* Genre 电影类型
* Year 年份
* Awards 获奖

Then, in each of the movies, you would specify terms for each of these taxonomies (i.e., in the [front matter][] of each of your movie content files). From these terms, Hugo would automatically create pages for each Actor, Director, Studio, Genre, Year, and Award, with each listing all of the Movies that matched that specific Actor, Director, Studio, Genre, Year, and Award.
然后, 对每个电影, 需要声明这些分类的条目(比如，在每个电影内容文件的[front matter][]部分)。对于这些条目，Hugo自动为每个演员、导演、工作室、电影类型、年份和获奖情况创建对应页面，每个页面包含属于特定演员、导演、工作室、电影类型、年份或获奖情况的电影的列表。

### 电影分类组织 Movie Taxonomy Organization

继续上面电影网站例子, 下面展示了从**分类角度**的内容的关系:

```
Actor                    <- Taxonomy  分类
    Bruce Willis         <- Term      条目
        The Sixth Sense  <- Value     值
        Unbreakable      <- Value     值
        Moonrise Kingdom <- Value     值
    Samuel L. Jackson    <- Term      条目
        Unbreakable      <- Value     值
        The Avengers     <- Value     值
        xXx              <- Value     值
```

From the perspective of the content, the relationships would appear differently, although the data and labels used are the same:
从内容的角度看, 关系会以不同方式出现, 虽然数据和标记使用的都一样:

```
Unbreakable                 <- Value      值
    Actors                  <- Taxonomy   分类
        Bruce Willis        <- Term       值
        Samuel L. Jackson   <- Term       值
    Director                <- Taxonomy   分类
        M. Night Shyamalan  <- Term       值
    ...
Moonrise Kingdom            <- Value       值
    Actors                  <- Taxonomy    分类
        Bruce Willis        <- Term        值
        Bill Murray         <- Term        值
    Director                <- Taxonomy    分类
        Wes Anderson        <- Term        值
    ...
```

## Hugo的默认分类 {#default-taxonomies}

Hugo原生支持分类.

Without adding a single line to your [site config][config] file, Hugo will automatically create taxonomies for `tags` and `categories`. That would be the same as manually [configuring your taxonomies](#configuring-taxonomies) as below:
不需要在[配置文件][config] 中添加一行，Hugo自动为`tags`和`categories`创建分类.
自动创建的分类，同在配置文件中[配置分类](#configuring-taxonomies)效果一样:

{{< code-toggle copy="false" >}}
[taxonomies]
  tag = "tags"
  category = "categories"
{{</ code-toggle >}}

If you do not want Hugo to create any taxonomies, set `disableKinds` in your [site config][config] to the following:
如果不需要Hugo去创建任何分类, 在站点配置中设置 `disableKinds` 像下面这样:

{{< code-toggle copy="false" >}}
disableKinds = ["taxonomy","term"]
{{</ code-toggle >}}

{{< new-in "0.73.0" >}} We have fixed the before confusing page kinds used for taxonomies (see the listing below) to be in line with the terms used when we talk about taxonomies. We have been careful to avoid site breakage, and you should get an ERROR in the console if you need to adjust your `disableKinds` section.
我们修复了用于分类法的之前的导致混乱的页面的类型（请参见下面的清单），使其与讨论分类法时使用的术语一致。
我们尽量避免网站破坏，如果可能需要您调整原来的`disableKinds`部分，在console里面会提示ERROR错误。

<!--
{{% page-kinds %}}
-->

| Kind 类型 | Description 描述 | Example 例子 |
|----------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------|
| `home` | 主页的登陆页面  | `/index.html` |
| `page` | 特定页面的登陆页面 | `my-post` page (`/posts/my-post/index.html`) |
| `section` | 特定块的登陆页面 | `posts` section (`/posts/index.html`) |
| `taxonomy` | 分类标签的登陆页面 | `tags` taxonomy (`/tags/index.html`) |
| `term` | 标签条目的登陆页面 | term `awesome` in `tags` taxonomy (`/tags/awesome/index.html`) |


### 默认的目的 Default Destinations

When taxonomies are used---and [taxonomy templates][] are provided---Hugo will automatically create both a page listing all the taxonomy's terms and individual pages with lists of content associated with each term. For example, a `categories` taxonomy declared in your configuration and used in your content front matter will create the following pages:
当使用分类并且提供了[taxonomy templates][]分类模板---Hugo会自动创建展示所有分类条目的列表页面，以及为每一个条目创建单独页面展示条目相关的内容列表。 比如，在配置中声明的`categories`分类，在内容的前端设置中使用后Hugo会创建如下页面:

* A single page at `example.com/categories/` that lists all the [terms within the taxonomy][]
* [Individual taxonomy list pages][taxonomy templates] (e.g., `/categories/development/`) for each of the terms that shows a listing of all pages marked as part of that taxonomy within any content file's [front matter][]

* 一个展示列出所有[分类下条目][terms within the taxonomy]的一个单独页面
* 对分类内每个条目 创建[单独的标签列表页Individual taxonomy list pages][taxonomy templates] (比如 `/categories/development/`), 页面上列出在内容文件的前端设置[front matter][]中标明这个标签的所有页面



## 配置分类  

默认的分类之外[defaults](#default-taxonomies)定制的分类必须先在[站点配置][config]中定义,然后才能在站点中使用。
需要给每个条目提供复数和单数的label. 比如TOML中`singular key = "plural value"` 和 YAML中 `singular key: "plural value"`.

### 举例添加定制的分类，名称为"series"

{{% note %}}
在添加定制分类时，_如果也需要保留默认的分类_，也要在配置文件中设置.
{{% /note %}}

{{< code-toggle copy="false" >}}
[taxonomies]
  tag = "tags"
  category = "categories"
  series = "series"
{{</ code-toggle >}}

### 例子: 删除默认分类

如果想为网站仅仅保留默认的`tags` 分类，删除 `categories` 分类, 您可以在[站点配置][config]文件中
像下面这样修改`taxonomies` 值.

{{< code-toggle copy="false" >}}
[taxonomies]
  tag = "tags"
{{</ code-toggle >}}

If you want to disable all taxonomies altogether, see the use of `disableKinds` in [Hugo Taxonomy Defaults](#default-taxonomies).
如果想一次禁用所有分类标签, 参考上面`disableKinds`的用法[默认分类](#default-taxonomies)

{{% note %}}
You can add content and front matter to your taxonomy list and taxonomy terms pages. See [Content Organization](/content-management/organization/) for more information on how to add an `_index.md` for this purpose.
可以给标签列表页和标签条目页添加内容和前端设置。参考[内容组织](/content-management/organization/)获得如何为此目的添加`_index.md`的更多信息。

Much like regular pages, taxonomy list [permalinks](/content-management/urls/) are configurable, but taxonomy term page permalinks are not.
和普通页面非常相似, 标签列表的[永久链接 permalinks](/content-management/urls/)是可以配置的, 不过分类条目页的永久链接不能设置。

{{% /note %}}

{{% warning %}}
配置选项`preserveTaxonomyNames`在Hugo版本 0.55中被移除.

现在可以在相关标签节点的页面使用`.Page.Title`获得原来的值.
{{% /warning %}}

## Add Taxonomies to Content

Once a taxonomy is defined at the site level, any piece of content can be assigned to it, regardless of [content type][] or [content section][].
当站点层级定义了标签，任何内容可以赋值这个标签，不论[内容类型][content type]或者[内容块][content section].

Assigning content to a taxonomy is done in the [front matter][]. Simply create a variable with the *plural* name of the taxonomy and assign all terms you want to apply to the instance of the content type. 给内容赋值标签通过前端设置 [front matter][]内设定。 简单的使用标签的复数形式(英语)作为变量, 赋值所有想对内容实例应用的条目作为标签的值.

{{% note %}}
If you would like the ability to quickly generate content files with preconfigured taxonomies or terms, read the docs on [Hugo archetypes](/content-management/archetypes/).
{{% /note %}} 如果喜欢快速生成具有预先配置的标签和条目的内容文件，请参考[Hugo archetypes](/content-management/archetypes/)的文档.

### 例子: 前端设置标签

{{< code-toggle copy="false">}}
title = "Hugo: A fast and flexible static site generator"
tags = [ "Development", "Go", "fast", "Blogging" ]
categories = [ "Development" ]
series = [ "Go Web Dev" ]
slug = "hugo"
project_url = "https://github.com/gohugoio/hugo"
{{</ code-toggle >}}

## 标签排序

A content file can assign weight for each of its associate taxonomies. Taxonomic weight can be used for sorting or ordering content in [taxonomy list templates][] and is declared in a content file's [front matter][]. The convention for declaring taxonomic weight is `taxonomyname_weight`.
内容文件的每个相关标签可以设置一个weight值。 标签的weight值可以用来在[条目列表模板][taxonomy list templates]中排序，可以在内容页面的[front matter][]中设定。声明标签weight值的惯例是 `taxonomyname_weight`.

The following TOML and YAML examples show a piece of content that has a weight of 22, which can be used for ordering purposes when rendering the pages assigned to the "a", "b" and "c" values of the `tags` taxonomy. It has also been assigned the weight of 44 when rendering the "d" category page.
下面的TOML和YAML例子中显示的内容片段具有weight值22，在和tags分类的 "a", "b" and "c" 关联的标签条目页是显示时排序使用,
在category分类的'd'页面中排序时具有weight 44.

### Example: Taxonomic `weight`

{{< code-toggle copy="false" >}}
title = "foo"
tags = [ "a", "b", "c" ]
tags_weight = 22
categories = ["d"]
categories_weight = 44
{{</ code-toggle >}}

By using taxonomic weight, the same piece of content can appear in different positions in different taxonomies.
通过使用标签weight，相同内容在不同标签条目列表页内处于不同的位置.

{{% note "Limits to Ordering Taxonomies" %}}
Currently taxonomies only support the [default `weight => date` ordering of list content](/templates/lists/#default-weight-date). For more information, see the documentation on [taxonomy templates](/templates/taxonomy-templates/).
{{% /note %}}

## Add custom metadata to a Taxonomy or Term

If you need to add custom metadata to your taxonomy terms, you will need to create a page for that term at `/content/<TAXONOMY>/<TERM>/_index.md` and add your metadata in it's front matter. Continuing with our 'Actors' example, let's say you want to add a Wikipedia page link to each actor. Your terms pages would be something like this:
如果需要对标签条目添加定制的元数据，您需要创建创建条目页面在`/content/<TAXONOMY>/<TERM>/_index.md`, 在这个页面的前端设置中添加元数据。沿用上面的  'Actors'例子, 我们假设需要添加一个Wikipedia页面链接给每个演员。 标签条目页可能会像下面这样:

{{< code file="/content/actors/bruce-willis/_index.md" >}}
---
title: "Bruce Willis"
wikipedia: "https://en.wikipedia.org/wiki/Bruce_Willis"
---
{{< /code >}}


[`urlize` template function]: /functions/urlize/
[content section]: /content-management/sections/
[content type]: /content-management/types/
[documentation on archetypes]: /content-management/archetypes/
[front matter]: /content-management/front-matter/
[taxonomy list templates]: /templates/taxonomy-templates/#taxonomy-page-templates
[taxonomy templates]: /templates/taxonomy-templates/
[terms within the taxonomy]: /templates/taxonomy-templates/#taxonomy-terms-templates "See how to order terms associated with a taxonomy"
[config]: /getting-started/configuration/
