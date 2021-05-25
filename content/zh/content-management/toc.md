---
title: 文章目录
linktitle:
description: Hugo可以自动解析Markdown文档, 创建可以在您模板中可以使用的文章目录.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [content management]
keywords: [table of contents, toc]
menu:
  docs:
    parent: "content-management"
    weight: 130
weight: 130	#rem
draft: false
aliases: [/extras/toc/]
toc: true
---

{{% note "TOC Heading Levels are Fixed" %}}

Hugo早期版本, 没有现成的方案来指定Toc中显示的标题层级。[参考Github相关讨论(#1778)](https://github.com/gohugoio/hugo/issues/1778). 那时, 从 `{{.Content}}`提取产生的`<nav id="TableOfContents"><ul></ul></nav>`会从`<h1>`开始.


Hugo[版本v0.60.0](https://github.com/gohugoio/hugo/releases/tag/v0.60.0)转向[Goldmark](https://github.com/yuin/goldmark/)作为Markdown默认的处理库, 具有改进的和可配置的TOC实现。
对于Goldmark呈现器中[如何配置TOC的文档](/getting-started/configuration-markup/#table-of-contents).

{{% /note %}}

##  用法

按通常方式创建您的markdown文档, 包含适当的headings. 下面是markdown内容举例:

```
<!-- Your front matter up here -->

## Introduction

One morning, when Gregor Samsa woke from troubled dreams, he found himself transformed in his bed into a horrible vermin.

## My Heading

He lay on his armour-like back, and if he lifted his head a little he could see his brown belly, slightly domed and divided by arches into stiff sections. The bedding was hardly able to cover it and seemed ready to slide off any moment.

### My Subheading

A collection of textile samples lay spread out on the table - Samsa was a travelling salesman - and above it there hung a picture that he had recently cut out of an illustrated magazine and housed in a nice, gilded frame. It showed a lady fitted out with a fur hat and fur boa who sat upright, raising a heavy fur muff that covered the whole of her lower arm towards the viewer. Gregor then turned to look out the window at the dull weather. Drops
```

Hugo会处理这个markdown文档, 从 `## Introduction`, `## My Heading` 和 `### My Subheading` 创建目录,  并保存在[页面变量][pagevars]`.TableOfContents`中.

内置的 `.TableOfContents` 变量输出 `<nav id="TableOfContents">` 元素，这个元素具有子元素`<ul>`, `<ul>`的子元素`<li>`以合适的HTML标题开始. 参考 [目录配置选项](/getting-started/configuration-markup/#table-of-contents) 文档了解如何配置目录标题层级的信息.

{{% note "Table of contents not available for MMark" %}}
使用[MMark](/content-management/formats/#mmark)markdown方言创建的hugo文档不显示TOCs. 使用其他Markdown格式创建的Markdown文档都与TOCs兼容.
{{% /note %}}

## 模板举例:基础的目录

The following is an example of a very basic [single page template][]:
下面是一个非常简单的[single page template][] 单独页面模板

{{< code file="layout/_default/single.html" download="single.html" >}}
{{ define "main" }}
<main>
    <article>
    <header>
        <h1>{{ .Title }}</h1>
    </header>
        {{ .Content }}
    </article>
    <aside>
        {{ .TableOfContents }}
    </aside>
</main>
{{ end }}
{{< /code >}}

## 模板举例: TOC 部分模板

下面的[部分模板][partials]添加稍微多一点对于TOC的页面级别控制逻辑. 它假设您的内容的[front matter][]使用了`toc`域, 除非特定声明为`false`, hugo会在任何具有`.WordCount` (参考[Page Variables][pagevars]) 词数大于400的页面展示目录.这个例子还展示了如何在模板中使用条件逻辑[conditionals][]:

{{< code file="layouts/partials/toc.html" download="toc.html" >}}
{{ if and (gt .WordCount 400 ) (.Params.toc) }}
<aside>
    <header>
    <h2>{{.Title}}</h2>
    </header>
    {{.TableOfContents}}
</aside>
{{ end }}
{{< /code >}}

{{% note %}}
With the preceding example, even pages with > 400 words *and* `toc` not set to `false` will not render a table of contents if there are no headings in the page for the `{{.TableOfContents}}` variable to pull from.
{{% /note %}}

## 使用 AsciiDoc

Hugo supports table of contents with AsciiDoc content format.
hugo支持asciiDoc内容格式的目录

In the header of your content file, specify the AsciiDoc TOC directives necessary to ensure that the table of contents is generated. Hugo will use the generated TOC to populate the page variable `.TableOfContents` in the same way as described for Markdown. See example below:
在内容文件的头部，声明AsciiDoc的Toc指令以确保目录可以生成。HUGO会使用生成的TOC来赋值给页面变量`.TableOfContents`, 和Markdown文件的方式一样。例子如下:

```asciidoc
// <!-- Your front matter up here -->
:toc:
// Set toclevels to be at least your hugo [markup.tableOfContents.endLevel] config key
:toclevels: 4

== Introduction

One morning, when Gregor Samsa woke from troubled dreams, he found himself transformed in his bed into a horrible vermin.

== My Heading

He lay on his armour-like back, and if he lifted his head a little he could see his brown belly, slightly domed and divided by arches into stiff sections. The bedding was hardly able to cover it and seemed ready to slide off any moment.

=== My Subheading

A collection of textile samples lay spread out on the table - Samsa was a travelling salesman - and above it there hung a picture that he had recently cut out of an illustrated magazine and housed in a nice, gilded frame. It showed a lady fitted out with a fur hat and fur boa who sat upright, raising a heavy fur muff that covered the whole of her lower arm towards the viewer. Gregor then turned to look out the window at the dull weather. Drops
```
Hugo will take this AsciiDoc and create a table of contents store it in the page variable `.TableOfContents`, in the same as described for Markdown.
Hugo会处理这个AsciiDoc文档，创建内容目录，并保存在`.TableOfContents`页面变量中,如上面Markdown中描述的一样.

[conditionals]: /templates/introduction/#conditionals
[front matter]: /content-management/front-matter/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[single page template]: /templates/single-page-templates/
