---
title: Hugo 内容List
linktitle: List模板
description: Lists在Hugo中具有特别意义和用法, 特别是在显示首页、区块页、标签页、标签条目list时.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [lists,sections,rss,taxonomies,terms]
menu:
  docs:
    parent: "templates"
    weight: 22
weight: 22
sections_weight: 22
draft: false
aliases: [/templates/list/,/layout/indexes/]
toc: true
---

## List 页面模板?

{{< youtube 8b2YTSMdMps >}}

A list page template is a template used to render multiple pieces of content in a single HTML page. The exception to this rule is the homepage, which is still a list but has its own [dedicated template][homepage].

List页面模板用于在一个HTML页面内显示多条内容。
例外是主页, 虽然是一个list但是主页有自己的 [专用模板][homepage].

Hugo uses the term *list* in its truest sense; i.e. a sequential arrangement of material, especially in alphabetical or numerical order. Hugo uses list templates on any output HTML page where content is traditionally listed:

Hugo中条目*list* 表达了最真实的需求; 那就是，内容的序列安排，特别是按照字母或者数字顺序。
Hugo对传统上是列表内容的任何输出的HTML页面应用list模板:


* [标签条目页 Taxonomy terms pages][taxterms]
* [标签内容list页 Taxonomy list pages][taxlists]
* [块list页 Section list pages][sectiontemps]
* [RSS][rss]

For template lookup order, see [Template Lookup](/templates/lookup-order/).
模版查询顺序, 请参考[Template Lookup](/templates/lookup-order/)

The idea of a list page comes from the [hierarchical mental model of the web][mentalmodel] and is best demonstrated visually:
页面列表list的想法来源于[网络的分级心智模型 hierarchical mental model of the web][mentalmodel]
最好是使用图片展示:

[![Image demonstrating a hierarchical website sitemap.](/images/site-hierarchy.svg)](/images/site-hierarchy.svg)

## 默认list

### 默认的模板 Default Templates

区块的list和特定标签的list考虑它们的模板都是*lists*， 在它们模板查询顺序中都具有确定的默认模板 `_default/list.html` 或者 `themes/<THEME>/layouts/_default/list.html`. 另外,[section lists][sectiontemps] 和 [taxonomy lists][taxlists] 具有它们默认的在目录`_default`中的list模版.


参考 [Template Lookup Order](/templates/lookup-order/) 获得完整信息

## 在list页面中添加内容和Front matter

Since v0.18, [everything in Hugo is a `Page`][bepsays]. This means list pages and the homepage can have associated content files (i.e. `_index.md`) that contain page metadata (i.e., front matter) and content.
从版本V0.18开始，[Hugo中所有内容都是`Page`][bepsays]。这意味着list页面和主页也具有相关的包含页面元数据(如front matter)和页面内容的内容文件(如`_index.md`)

这个新模型允许您包含通过`.Params`包含list特定的front matter设置，同时也意味着list模板list templates (如 `layouts/_default/list.html`)具有访问所有[页面变量page variables][pagevars]的权限。

{{% note %}}
It is important to note that all `_index.md` content files will render according to a *list* template and not according to a [single page template](/templates/single-page-templates/).
重要提示, 请注意所有 `_index.md` 内容文件会根据一个*list*模板显示，而不会根据[单页模版](/templates/single-page-templates/)显示
{{% /note %}}

### 项目目录举例

The following is an example of a typical Hugo project directory's content:
下面是典型的Hugo项目的目录内容:

```
.
...
├── content
|   ├── posts
|   |   ├── _index.md
|   |   ├── post-01.md
|   |   └── post-02.md
|   └── quote
|   |   ├── quote-01.md
|   |   └── quote-02.md
...
```

Using the above example, let's assume you have the following in `content/posts/_index.md`:
使用上面例子,我们假设文件 `content/posts/_index.md`具有下面的内容:

{{< code file="content/posts/_index.md" >}}
---
title: My Go Journey
date: 2017-03-23
publishdate: 2017-03-24
---

I decided to start learning Go in March 2017.

Follow my journey through this new blog.
{{< /code >}}

You can now access this `_index.md`'s' content in your list template:

{{< code file="layouts/_default/list.html" download="list.html" >}}
{{ define "main" }}
<main>
    <article>
        <header>
            <h1>{{.Title}}</h1>
        </header>
        <!-- "{{.Content}}" pulls from the markdown content of the corresponding _index.md -->
        {{.Content}}
    </article>
    <ul>
    <!-- 遍历 content/posts/*.md -->
    {{ range .Pages }}
        <li>
            <a href="{{.Permalink}}">{{.Date.Format "2006-01-02"}} | {{.Title}}</a>
        </li>
    {{ end }}
    </ul>
</main>
{{ end }}
{{< /code >}}

上面模版会输出如下的HTML:

{{< code file="example.com/posts/index.html" copy="false" >}}
<!--top of your baseof code-->
<main>
    <article>
        <header>
            <h1>My Go Journey</h1>
        </header>
        <p>I decided to start learning Go in March 2017.</p>
        <p>Follow my journey through this new blog.</p>
    </article>
    <ul>
        <li><a href="/posts/post-01/">Post 1</a></li>
        <li><a href="/posts/post-02/">Post 2</a></li>
    </ul>
</main>
<!--bottom of your baseof-->
{{< /code >}}

### 不需要`_index.md`的List页面

You do *not* have to create an `_index.md` file for every list page (i.e. section, taxonomy, taxonomy terms, etc) or the homepage. If Hugo does not find an `_index.md` within the respective content section when rendering a list template, the page will be created but with no `{{.Content}}` and only the default values for `.Title` etc.
不需要为每一个list页面(如section, taxonomy标签页, taxonomy terms标签条目等页面)或者首页创建 `_index.md`文件。如果Hugo在显示list模版时在相应的内容section内没有找到 `_index.md`, hugo会创建list页面, 但是不包含`{{.Content}}`，仅仅包含 `.Title`的默认值.

Using this same `layouts/_default/list.html` template and applying it to the `quotes` section above will render the following output. Note that `quotes` does not have an `_index.md` file to pull from:
使用上面相同的 `layouts/_default/list.html` 模板, 对上面的 `quotes` 区块应用这个模板会产生下面的输出. 请注意 `quotes` 并没有一个可以提取内容的`_index.md`:


{{< code file="example.com/quote/index.html" copy="false" >}}
<!--baseof-->
<main>
    <article>
        <header>
        <!-- Hugo assumes that .Title is the name of the section since there is no _index.md content file from which to pull a "title:" field -->
            <h1>Quotes</h1>
        </header>
    </article>
    <ul>
        <li><a href="https://example.com/quote/quotes-01/">Quote 1</a></li>
        <li><a href="https://example.com/quote/quotes-02/">Quote 2</a></li>
    </ul>
</main>
<!--baseof-->
{{< /code >}}

{{% note %}}
The default behavior of Hugo is to pluralize list titles; hence the inflection of the `quote` section to "Quotes" when called with the `.Title` [page variable](/variables/page/). You can change this via the `pluralizeListTitles` directive in your [site configuration](/getting-started/configuration/).
Hugo的默认行为是将list的title复数化, 因此`quote`区块名称在`.Title` [page variable](/variables/page/)变量被调用是变成了复数 "Quotes". 可以通过在[站点配置](/getting-started/configuration/)中设置 `pluralizeListTitles` 指令来改变这个默认行为.
{{% /note %}}

## List Templates 举例

### 块模板

list模版已经被修改，同[spf13.com](http://spf13.com/)原来使用的模板稍微不同。 为了所生成页面的拼接，它使用了[partial templates][partials]，而没有使用 [base template][base]. 下面的例子也使用了[内容视图模板][views] `li.html` 或者 `summary.html`

{{< code file="layouts/section/posts.html" >}}
{{ partial "header.html" . }}
{{ partial "subheader.html" . }}
<main>
  <div>
   <h1>{{ .Title }}</h1>
        <ul>
        <!-- 对每个content/posts/*.md 输出li.html内容视图 -->
            {{ range .Pages }}
                {{ .Render "li"}}
            {{ end }}
        </ul>
  </div>
</main>
{{ partial "footer.html" . }}
{{< /code >}}

### 标签模板

{{< code file="layouts/_default/taxonomy.html" download="taxonomy.html" >}}
{{ define "main" }}
<main>
  <div>
   <h1>{{ .Title }}</h1>
   <!-- 对和特定标签相关的内容文件循环,
   输出summary.html内容视图 -->
    {{ range .Pages }}
        {{ .Render "summary"}}
    {{ end }}
  </div>
</main>
{{ end }}
{{< /code >}}

## 对内容排序

Hugo lists render the content based on metadata you provide in [front matter][]. In addition to sane defaults, Hugo also ships with multiple methods to make quick work of ordering content inside list templates:
Hugo列表list基于内容的前端设置 [front matter][]中的元数据来生成内容. 除了缺省显示, hugo也提供了多个方法在list模板内对内容进行快速排序。

### 缺省的排序顺序: Weight > Date > LinkTitle > FilePath


{{< code file="layouts/partials/default-order.html" >}}
<ul>
    {{ range .Pages }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### By Weight

越小的weight值有越高的优先级。具有低weight值的内容会先出现.

{{< code file="layouts/partials/by-weight.html" >}}
<ul>
    {{ range .Pages.ByWeight }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### By Date

{{< code file="layouts/partials/by-date.html" >}}
<ul>
    <!-- 基于前端设定中的“date”值对内容排序 -->
    {{ range .Pages.ByDate }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### By Publish Date

{{< code file="layouts/partials/by-publish-date.html" >}}
<ul>
    <!-- 基于前端设定中的“publishdate”值对内容排序-->
    {{ range .Pages.ByPublishDate }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### By Expiration Date

{{< code file="layouts/partials/by-expiry-date.html" >}}
<ul>
    {{ range .Pages.ByExpiryDate }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### 按最后修改日期排序

{{< code file="layouts/partials/by-last-mod.html" >}}
<ul>
    <!-- 基于前端设定中的“lastmod”值对内容排序-->
    {{ range .Pages.ByLastmod }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### 按内容长度升序排列

{{< code file="layouts/partials/by-length.html" >}}
<ul>
    <!-- 按照内容长度升序排序(最短的内容最先出现)-->
    {{ range .Pages.ByLength }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### 按title排序

{{< code file="layouts/partials/by-title.html" >}}
<ul>
    <!-- 按前端设定中"title"的升序排列-->
    {{ range .Pages.ByTitle }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### 按链接Title排序

{{< code file="layouts/partials/by-link-title.html" >}}
<ul>
    <!-- 按前端设定中"linktitle"的升序排列.如果未设定"LinkTitle",使用"title"作为"linktitle" -->
    {{ range .Pages.ByLinkTitle }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .LinkTitle }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

### By Parameter 按指定参数排序

Order based on the specified front matter parameter. Content that does not have the specified front matter field  will use the site's `.Site.Params` default. If the parameter is not found at all in some entries, those entries will appear together at the end of the ordering.

{{< code file="layouts/partials/by-rating.html" >}}
<!-- Ranges through content according to the "rating" field set in front matter -->
{{ range (.Pages.ByParam "rating") }}
  <!-- ... -->
{{ end }}
{{< /code >}}

If the targeted front matter field is nested beneath another field, you can access the field using dot notation.

{{< code file="layouts/partials/by-nested-param.html" >}}
{{ range (.Pages.ByParam "author.last_name") }}
  <!-- ... -->
{{ end }}
{{< /code >}}

### Reverse Order  反序排列

对上面任意一个排序方法可以进行反序排列. 下面以`ByDate`排序作为例子:

{{< code file="layouts/partials/by-date-reverse.html" >}}
<ul>
    {{ range .Pages.ByDate.Reverse }}
        <li>
            <h1><a href="{{ .Permalink }}">{{ .Title }}</a></h1>
            <time>{{ .Date.Format "Mon, Jan 2, 2006" }}</time>
        </li>
    {{ end }}
</ul>
{{< /code >}}

## Group Content 内容分组

Hugo提供了按照区块、类型、日期将内容分组的函数.

### By Page Field 通过页面字段分组

{{< code file="layouts/partials/by-page-field.html" >}}
<!-- 按内容section区块分组. ".Key"这里指的是section的标题-->
{{ range .Pages.GroupBy "Section" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

In the above example, you may want `{{.Title}}` to point the `title` field you have added to your `_index.md` file instead. You can access this value using the [`.GetPage` function][getpage]:

在上面例子中, 如果希望使用添加到"_index.md"文件中的`title`字段作为`{{.Title}}`. 可以使用函数[`.GetPage` function][getpage]获得这个值:

{{< code file="layouts/partials/by-page-field.html" >}}
<!-- Groups content according to content section.-->

{{ range .Pages.GroupBy "Section" }}
<!-- Checks for existence of _index.md for a section; if available, pulls from "title" in front matter -->
{{ with $.Site.GetPage "section" .Key }}
<h3>{{.Title}}</h3>
{{ else }}
<!-- If no _index.md is available, ".Key" defaults to the section title and filters to title casing -->
<h3>{{ .Key | title }}</h3>
{{ end }}
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 按日期分组 By Date

{{< code file="layouts/partials/by-page-date.html" >}}
<!-- 基于前端设置的"date"中的月份将内容分组 -->
{{ range .Pages.GroupByDate "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 按发布日期分组 By Publish Date

{{< code file="layouts/partials/by-page-publish-date.html" >}}
<!-- 基于前端设置的"publishDate"中的月份将内容分组 -->
{{ range .Pages.GroupByPublishDate "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .PublishDate.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}


### 按最后修改时间分组 By Lastmod

{{< code file="layouts/partials/by-page-lastmod.html" >}}
<!-- 基于前端设置的"lastMod"中的月份将内容分组  -->
{{ range .Pages.GroupByLastmod "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Lastmod.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 按过期时间分组 By Expiry Date

{{< code file="layouts/partials/by-page-expiry-date.html" >}}
<!-- Groups content by month according to the "expiryDate" field in front matter -->
{{ range .Pages.GroupByExpiryDate "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .ExpiryDate.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 基于页面参数分组 By Page Parameter

{{< code file="layouts/partials/by-page-param.html" >}}
<!-- Groups content according to the "param_key" field in front matter -->
{{ range .Pages.GroupByParam "param_key" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 基于日期格式的页面参数分组  By Page Parameter in Date Format

The following template takes grouping by `date` a step further and uses Go's layout string. See the [`Format` function][] for more examples of how to use Go's layout string to format dates in Hugo.

{{< code file="layouts/partials/by-page-param-as-date.html" >}}
<!-- Groups content by month according to the "param_key" field in front matter -->
{{ range .Pages.GroupByParamDate "param_key" "2006-01" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

### 反转key顺序 Reverse Key Order

Ordering of groups is performed by keys in alphanumeric order (A–Z, 1–100) and in reverse chronological order (i.e., with the newest first) for dates.
分组内排序基于键值的字母顺序，和日期的反向顺序(最新的最先出现)。

While these are logical defaults, they are not always the desired order. There are two different syntaxes to change Hugo's default ordering for groups, both of which work the same way.
逻辑默认如此, 但有时并不总是想要的顺序。下面是两钟改变Hugo默认分组内排序的格式，效果一样:

#### 1. 添加Reverse方法

```
{{ range (.Pages.GroupBy "Section").Reverse }}
```

```
{{ range (.Pages.GroupByDate "2006-01").Reverse }}
```

#### 2. 提供不同的方向

```
{{ range .Pages.GroupByDate "2006-01" "asc" }}
```

```
{{ range .Pages.GroupBy "Section" "desc" }}
```

### 组内排序 Order Within Groups

Because Grouping returns a `{{.Key}}` and a slice of pages, all of the ordering methods listed above are available.
对于内容分组返回的`{{.Key}}` 和页面list的切片, 上面提到的排序方法都适用:

下面例子中的排序:

1. 内容按照前端设置中`date`的月份分组
2. 不同组按照升序排列(最早的组先出来)
3. 相应的每个组内页面通过`title`按字母顺序排序

{{< code file="layouts/partials/by-group-by-page.html" >}}
{{ range .Pages.GroupByDate "2006-01" "asc" }}
<h3>{{ .Key }}</h3>
<ul>
    {{ range .Pages.ByTitle }}
    <li>
    <a href="{{ .Permalink }}">{{ .Title }}</a>
    <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

## list过滤和条件选择 Filtering and Limiting Lists {#filtering-and-limiting-lists}

Sometimes you only want to list a subset of the available content. A
common is to only display posts from [**main sections**][mainsections]
on the blog's homepage.
有时候想获得可访问内容的子集列表，比如常见的在博客首页仅仅显示来自[**main sections**][mainsections]的post的。

列表过滤和条件选择，可以参考 [`where` function][wherefunction] 和
[`first` function][firstfunction] 文档获得更多信息.

[base]: /templates/base/
[directorystructure]: /getting-started/directory-structure/
[`Format` function]: /functions/format/
[front matter]: /content-management/front-matter/
[getpage]: /functions/getpage/
[homepage]: /templates/homepage/
[homepage]: /templates/homepage/

[pagevars]: /variables/page/
[partials]: /templates/partials/

[rss]: /templates/rss/
[sections]: /content-management/sections/
[sectiontemps]: /templates/section-templates/
[sitevars]: /variables/site/
[taxlists]: /templates/taxonomy-templates/#taxonomy-list-templates
[taxterms]: /templates/taxonomy-templates/#taxonomy-terms-templates
[taxvars]: /variables/taxonomy/
[views]: /templates/views/
[wherefunction]: /functions/where/
[firstfunction]: /functions/first/
[mainsections]: /functions/where/#mainsections

[RSS 2.0]: https://cyber.harvard.edu/rss/rss.html "RSS 2.0 Specification"
[mentalmodel]: https://webstyleguide.com/wsg3/3-information-architecture/3-site-structure.html
[bepsays]: https://bepsays.com/en/2016/12/19/hugo-018/
