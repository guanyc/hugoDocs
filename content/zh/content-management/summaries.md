---
title: 内容概要
linktitle: 概要
description: Hugo会生成网站内容的摘要.
date: 2017-01-10
publishdate: 2017-01-10
lastmod: 2017-01-10
categories: [content management]
keywords: [summaries,abstracts,read more]
menu:
  docs:
    parent: "content-management"
    weight: 90
weight: 90	#rem
draft: false
aliases: [/content/summaries/,/content-management/content-summaries/]
toc: true
---

With the use of the `.Summary` [page variable][pagevariables], Hugo generates summaries of content to use as a short version in summary views.
通过使用`.Summary` [页面变量][pagevariables], Hugo生成内容摘要，可以在摘要视图中作为短摘要使用。

## 摘要截取方式  Summary Splitting Options

* 自动截取摘要 Automatic Summary Split
* 手工截取摘要 Manual Summary Split
* 前言设定设置摘要 Front Matter Summary

在摘要上附上到达原文的链接是很自然的想法，一个常见的设计模式是以"阅读更多..."按钮的形式构建这个链接.
 参考[页面变量][pagevariables]中`.RelPermalink`, `.Permalink`, 和 `.Truncated`文档.

### 自动截取摘要

By default, Hugo automatically takes the first 70 words of your content as its summary and stores it into the `.Summary` page variable for use in your templates. You may customize the summary length by setting `summaryLength` in your [site configuration](/getting-started/configuration/).

缺省情况下, Hugo自动提取内容的头70个词作为内容摘要并且保存在 `.Summary` 页面变量中，以供模板使用.
可以通过在[站点配置](/getting-started/configuration/)文件中设定`summaryLength`来定制这个摘要长度.

{{% note %}}
可以使用如`plainify` 和 `safeHTML`等函数定制摘要中HTML标签的加载方式.
{{% /note %}}

{{% note %}}
The Hugo-defined summaries are set to use word count calculated by splitting the text by one or more consecutive whitespace characters. If you are creating content in a `CJK` language and want to use Hugo's automatic summary splitting, set `hasCJKLanguage` to `true` in your [site configuration](/getting-started/configuration/).
Hugo自动截取的摘要设置使用的是词数，也就是被一个或者多个连续空白字符分隔的文本数。如果使用`中日韩`语言，并希望Hugo自动截取摘要表现正确, 请在[站点配置中](/getting-started/configuration/)中设置 `hasCJKLanguage`为`true`
{{% /note %}}

### 手动截取摘要 Manual Summary Splitting

Alternatively, you may add the <code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code> summary divider where you want to split the article.
另外一种方式，您可以在文章中添加 <code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code>作为文本中摘要分隔.

For [Org mode content][org], use `# more` where you want to split the article.
对于 [Org mode content][org]文档, 使用 `# more` 作为文章中分割摘要和其余部分的分割

在摘要分隔前的内容将作为内容的摘要，并存储在页面变量`.Summary` 中并且保存所有HTML格式不变.

{{% note "Summary Divider"%}}
The concept of a *summary divider* is not unique to Hugo. It is also called the "more tag" or "excerpt separator" in other literature.
{{% /note %}}

Pros 支持
: Freedom, precision, and improved rendering.  All HTML tags and formatting are preserved.
: 自由，改进的显示。所有HTML标签和格式都被保留

Cons 反对
: 内容作者需要更多工作。他们需要敲键盘输入<code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code> ( `# more` 在 [org content][org]文档中)在每个内容文件中. 这个可以通过在[原型archetype](/content-management/archetypes/)中在前言设定后面添加摘要分隔来自动化完成。

{{% warning "Be Precise with the Summary Divider" %}}
注意正确输入<code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code> 都是小写并且没有空白。
{{% /warning %}}

### 前言设定摘要 Front Matter Summary

You might want your summary to be something other than the text that starts the article.  In this case you can provide a separate summary in the `summary` variable of the article front matter.
您可能想内容摘要是与文章开头文本不同的一些内容。这种需求可以通过在文章的front matter部分提供一个不同摘要文本的`summary`变量来实现。

Pros
: Complete freedom of text independent of the content of the article.  Markup can be used within the summary. 完全和文章内容独立。标记可以用在摘要中。

Cons
: Extra work for content authors as they need to write an entirely separate piece of text as the summary of the article. 内容作者需要写完全不同的一段文本

## 摘要选择顺序

Because there are multiple ways in which a summary can be specified it is useful to understand the order of selection Hugo follows when deciding on the text to be returned by `.Summary`.  It is as follows:
由于摘要可以通过好几种方式提供，理解Hugo摘要选择并通过`.Summary`变量返回摘要的顺序很有帮助。
正如下面这样:

1. 如果文章中存在<code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code>摘要分隔符，则该摘要文本直到分隔符，将按照手动摘要拆分方法获得
2. 如果文章的前言设定中包含`summary` 变量，变量的值通过前言设定摘要法获得
3. 文章开始的文本内容会被自动摘要截取法使用

{{% warning "Competing selections" %}}
Hugo uses the _first_ of the above steps that returns text.  So if, for example, your article has both `summary` variable in its front matter and a <code>&#60;&#33;&#45;&#45;more&#45;&#45;&#62;</code> summary divider Hugo will use the manual summary split method.
{{% /warning %}}

## 例子: 具有摘要的前10片文章

You can show content summaries with the following code. You could use the following snippet, for example, in a [section template][].
用下面代码可以显示内容摘要，使用下面代码短，比如用在[块模板][section template]中

{{< code file="page-list-with-summaries.html" >}}
{{ range first 10 .Pages }}
    <article>
      <!-- this <div> includes the title summary -->
      <div>
        <h2><a href="{{ .RelPermalink }}">{{ .Title }}</a></h2>
        {{ .Summary }}
      </div>
      {{ if .Truncated }}
      <!-- This <div> includes a read more link, but only if the summary is truncated... -->
      <div>
        <a href="{{ .RelPermalink }}">Read More…</a>
      </div>
      {{ end }}
    </article>
{{ end }}
{{< /code >}}

Note how the `.Truncated` boolean variable value may be used to hide the "Read More..." link when the content is not truncated; i.e., when the summary contains the entire article.

[org]: /content-management/formats/
[pagevariables]: /variables/page/
[section template]: /templates/section-templates/
