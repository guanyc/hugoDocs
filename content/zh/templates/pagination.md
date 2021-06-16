---
title: 分页模板
linktitle: 分页模板
description: HUgo支持主页、区块页和标签页面的分页浏览.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [lists,sections,pagination]
menu:
  docs:
    parent: "templates"
    weight: 140
weight: 140
sections_weight: 140
draft: false
aliases: [/extras/pagination,/doc/pagination/]
toc: true
---

The real power of Hugo pagination shines when combined with the [`where` function][where] and its SQL-like operators: [`first`][], [`last`][], and [`after`][]. You can even [order the content][lists] the way you've become used to with Hugo.
HUgo提供的分页机制与[`where` 函数][where] 以及函数中类似SQL的操作符: [`first`][], [`last`][], and [`after`][] 结合使用时Hugo的真正强大力量会熠熠生辉发出耀眼光芒:)。 也可以按您已经习惯的HUgo方式[对内容排序][lists].


##  Configure Pagination####配置分页

Pagination can be configured in your [site configuration][configuration]:####分页可以在[站点配置文件][configuration]中配置。

`Paginate`
: default = `10`. This setting can be overridden within the template.####默认值 = `10`, 设置以后可以在模板内重载这个值.

`PaginatePath`
: default = `page`. Allows you to set a different path for your pagination pages. 分页路径 默认值 =  `page` 允许您设置分页页面的不同路径.

Setting `Paginate` to a positive value will split the list pages for the homepage, sections and taxonomies into chunks of that size. But note that the generation of the pagination pages for sections, taxonomies and homepage is *lazy* --- the pages will not be created if not referenced by a `.Paginator` (see below). #### 设置`Paginate`为正数会将主页、区块和分类页面分成包含设置的数值大小的页面块。但是请注意,为区块页、分类标签页和主页的分页页面的生成是*lazy懒加载的*--页面不会被创建如果没有被 `.Paginator` (见下面说明)引用.

`PaginatePath` is used to adapt the `URL` to the pages in the paginator (the default setting will produce URLs on the form `/page/1/`.####`PaginatePath`用于在页面分页器中定制`URL`(默认设置会生成的URL形如 `/page/1/`)

## List Paginator Pages####展示分页页面列表

{{% warning %}}
`.Paginator` is provided to help you build a pager menu. This feature is currently only supported on homepage and list pages (i.e., taxonomies and section lists).#### Hugo提供的`.Paginator` 帮助您构建分页菜单.这个特性当前仅支持主页和list页面(如分类页面和区块页)
{{% /warning %}}

There are two ways to configure and use a `.Paginator`:####有两种配置和使用`.Paginator`的方式:

1. The simplest way is just to call `.Paginator.Pages` from a template. It will contain the pages for *that page*.####最简单的方式是在模板中调用 `.Paginator.Pages`, 这里面包含 *那个页面* 所属的页面.  
2. Select another set of pages with the available template functions and ordering options, and pass the slice to `.Paginate`, e.g.####使用可用的模板函数和排序选项选择另外的页面集合, 然后传递选择的slice给`.Paginate`使用. 比如.
  * `{{ range (.Paginate ( first 50 .Pages.ByTitle )).Pages }}` 或者
  * `{{ range (.Paginate .RegularPagesRecursive).Pages }}`.

For a given **Page**, it's one of the options above. The `.Paginator` is static and cannot change once created. ####对于指定的 **Page**, 它是上面两种方式之一.  `.Paginator`是静态的、一经创建无法改变.

If you call `.Paginator` or `.Paginate` multiple times on the same page, you should ensure all the calls are identical. Once *either* `.Paginator` or `.Paginate` is called while generating a page, its result is cached, and any subsequent similar call will reuse the cached result. This means that any such calls which do not match the first one will not behave as written.####如果在同一页面调用`.Paginator` or `.Paginate`多次, 必须保证所有的调用是相同的. 在生成页面时一旦调用`.Paginator` 或者 `.Paginate`, 结果会被缓存, 任何后续的相似调用会重用缓存结果. 这意味着任何不与第一个调用匹配的后续调用不会得到想要的结果.


(Remember that function arguments are eagerly evaluated, so a call like `$paginator := cond x .Paginator (.Paginate .RegularPagesRecursive)` is an example of what you should *not* do. Use `if`/`else` instead to ensure exactly one evaluation.)####(谨记函数参数会提前评估,所以类似`$paginator := cond x .Paginator (.Paginate .RegularPagesRecursive)`这样的调用是您*不应该*使用的例子. 应该使用`if`/`else`来保证仅仅调用一次评估.

The global page size setting (`Paginate`) can be overridden by providing a positive integer as the last argument. The examples below will give five items per page:####全局页面分页数值设定 (`Paginate`) 可以被提供的最后一个参数的正数重载. 下面的例子会生成每个页面五个条目:

* `{{ range (.Paginator 5).Pages }}`
* `{{ $paginator := .Paginate (where .Pages "Type" "posts") 5 }}`

It is also possible to use the `GroupBy` functions in combination with pagination:####也可以将GroupBy函数与分页结合使用:

```
{{ range (.Paginate (.Pages.GroupByDate "2006")).PageGroups  }}
```

## Build the navigation####构建分页导航

The `.Paginator` contains enough information to build a paginator interface.
`.Paginator`包含构建分页界面的足够信息.

The easiest way to add this to your pages is to include the built-in template (with `Bootstrap`-compatible styles):
最简单的方式是在您页面内包含hugo内置的模板(`Bootstrap`兼容的模式)

```
{{ template "_internal/pagination.html" . }}
```

{{% note "When to Create `.Paginator`" %}}
If you use any filters or ordering functions to create your `.Paginator` *and* you want the navigation buttons to be shown before the page listing, you must create the `.Paginator` before it's used.
如果使用了过滤或者排序函数来创建 `.Paginator`, 并且您需要分页导航按钮在页面list前显示,您需要在`.Paginator`使用前创建它。
{{% /note %}}

The following example shows how to create `.Paginator` before its used:
下面例子展示了如何在使用前创建`.Paginator`:

```
{{ $paginator := .Paginate (where .Pages "Type" "posts") }}
{{ template "_internal/pagination.html" . }}
{{ range $paginator.Pages }}
   {{ .Title }}
{{ end }}
```

Without the `where` filter, the above example is even simpler:
如果没有`where` 过滤, 上面例子更简单:

```
{{ template "_internal/pagination.html" . }}
{{ range .Paginator.Pages }}
   {{ .Title }}
{{ end }}
```

If you want to build custom navigation, you can do so using the `.Paginator` object, which includes the following properties:
如果想构建定制的分页导航, 您可以使用 `.Paginator`对象来达成，`.Paginator`对象包含下面的属性:

`PageNumber`
: The current page's number in the pager sequence 当前页在页面序列中的数值

`URL`
: The relative URL to the current pager 当前页的相对URL

`Pages`
: The pages in the current pager 分页器所管理的页面

`NumberOfElements`
: The number of elements on this page  当前页的元素数量

`HasPrev`
: Whether there are page(s) before the current 当前页是否有前一页

`Prev`
: The pager for the previous page 当前页的前一页

`HasNext`
: Whether there are page(s) after the current 当前页是否具有下一页

`Next`
: The pager for the next page 当前页的下一页

`First`
: The pager for the first page 分页的第一页

`Last`
: The pager for the last page 分页的最后一页

`Pagers`
: A list of pagers that can be used to build a pagination menu  分页的list用来构建分页的菜单

`PageSize` 每页的元素数量
: Size of each pager

`TotalPages` 分页器中所有页
: The number of pages in the paginator

`TotalNumberOfElements` 分页器中所有页上的元素数目
: The number of elements on all pages in this paginator

## 补充信息 Additional information

The pages are built on the following form (`BLANK` means no value):
所有分页器的页面构建形式如下(`BLANK`意味着没有值):

```
[SECTION/TAXONOMY/BLANK]/index.html
[SECTION/TAXONOMY/BLANK]/page/1/index.html => redirect to  [SECTION/TAXONOMY/BLANK]/index.html
[SECTION/TAXONOMY/BLANK]/page/2/index.html
....
```


[`first`]: /functions/first/
[`last`]: /functions/last/
[`after`]: /functions/after/
[configuration]: /getting-started/configuration/
[lists]: /templates/lists/
[where]: /functions/where/
