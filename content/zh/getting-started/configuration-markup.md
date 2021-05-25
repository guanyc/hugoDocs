---
title: 配置Markup
description: Markdown和其他标记相关配置
date: 2019-11-15
categories: [getting started,fundamentals]
keywords: [configuration,highlighting]
menu:
  docs:
    parent: "getting-started"
    weight: 65
weight: 65
sections_weight: 65
slug: configuration-markup
toc: true
---

## 配置Markup

{{< new-in "0.60.0" >}}

参考 [Goldmark](#goldmark) 阅读关于Hugo中Markdown处理默认方式相关的设置

下面是Hugo中标记相关的配置以及默认值:

{{< code-toggle config="markup" />}}

** 细节见每个下面每个部分说明.**

### Goldmark

从Hugo版本0.60开始[Goldmark](https://github.com/yuin/goldmark/)是解析Markdown的默认标记库。速度很快，兼容于[CommonMark](https://spec.commonmark.org/0.29/), 并且非常灵活。注意Goldmark的特性集合与 Blackfriday 并不相同。得到很多收获的时候，也会失去一些，我们后续版本中会尽力弥合这些差距。


下面是缺省配置:

{{< code-toggle config="markup.goldmark" />}}

对于markdown扩展的详细信息, 请参考[Goldmark文档](https://github.com/yuin/goldmark/#built-in-extensions)

下面是一些设置的说明:

unsafe 不安全的元素
: 默认Goldmark不会输出原生的HTMLs和可能的危险链接.如果文档内有许多内嵌的HTML和或者Javascript，您可能需要将这个开关打开 。

typographer
: 此扩展程序将标点符号替换为[smartypant](https://daringfireball.net/projects/smartypants/)等印刷实体.

attribute
: 通过在单花括号内添加属性列表，并将单花括号置于其所修饰的Markdown元素后面, 提供对titles和blocks定制属性支持。 对titles在同一行，对blocks在区块后新一行。

{{< new-in "0.81" >}} Hugo 0.81.0 版本中我们对markdonw区块,比如 表格、列表、段落等提供了属性支持(比如，通过CSS类)


一个具有CSS类的引用段:

```md
> foo
> bar
{.myclass}
```

还是有一些限制:对表格现在只能对整个表格添加属性，对列表仅仅支持`ul`/`ol`节点上添加属性,比如:

```md
* Fruit
  * Apple
  * Orange
  * Banana
  {.fruits}
* Dairy
  * Milk
  * Cheese
  {.dairies}
{.list}
```

autoHeadingIDType ("github") {{< new-in "0.62.2" >}}
: The strategy used for creating auto IDs (anchor names). Available types are `github`, `github-ascii` and `blackfriday`. `github` produces GitHub-compatible IDs, `github-ascii` will drop any non-Ascii characters after accent normalization, and `blackfriday` will make the IDs work as with [Blackfriday](#blackfriday), the default Markdown engine before Hugo 0.60. Note that if Goldmark is your default Markdown engine, this is also the strategy used in the [anchorize](/functions/anchorize/) template func.

### Blackfriday


[Blackfriday](https://github.com/russross/blackfriday) was Hugo's default Markdown rendering engine, now replaced with Goldmark. But you can still use it: Just set `defaultMarkdownHandler` to `blackfriday` in your top level `markup` config.

This is the default config:

{{< code-toggle config="markup.blackFriday" />}}

### Highlight

This is the default `highlight` configuration. Note that some of these settings can be set per code block, see [Syntax Highlighting](/content-management/syntax-highlighting/).

{{< code-toggle config="markup.highlight" />}}

For `style`, see these galleries:

* [Short snippets](https://xyproto.github.io/splash/docs/all.html)
* [Long snippets](https://xyproto.github.io/splash/docs/longer/all.html)

For CSS, see [Generate Syntax Highlighter CSS](/content-management/syntax-highlighting/#generate-syntax-highlighter-css).

### Table Of Contents

{{< code-toggle config="markup.tableOfContents" />}}

These settings only works for the Goldmark renderer:

startLevel
: The heading level, values starting at 1 (`h1`), to start render the table of contents.

endLevel
: The heading level, inclusive, to stop render the table of contents.

ordered
: Whether or not to generate an ordered list instead of an unordered list.


## Markdown Render Hooks

{{< new-in "0.62.0" >}}

Note that this is only supported with the [Goldmark](#goldmark) renderer.

Render Hooks allow custom templates to override markdown rendering functionality. You can do this by creating templates with base names `render-{feature}` in `layouts/_default/_markup`.

You can also create type/section specific hooks in `layouts/[type/section]/_markup`, e.g.: `layouts/blog/_markup`.{{< new-in "0.71.0" >}}

The features currently supported are:

* `image`
* `link`
* `heading` {{< new-in "0.71.0" >}}

You can define [Output-Format-](/templates/output-formats) and [language-](/content-management/multilingual/)specific templates if needed. Your `layouts` folder may look like this:

```bash
layouts
└── _default
    └── _markup
        ├── render-image.html
        ├── render-image.rss.xml
        └── render-link.html
```

Some use cases for the above:

* Resolve link references using `.GetPage`. This would make links portable as you could translate `./my-post.md` (and similar constructs that would work on GitHub) into `/blog/2019/01/01/my-post/` etc.
* Add `target=_blank` to external links.
* Resolve and [process](/content-management/image-processing/) images.
* Add [header links](https://remysharp.com/2014/08/08/automatic-permalinks-for-blog-posts).

### Render Hook Templates

The `render-link` and `render-image` templates will receive this context:

Page
: The [Page](/variables/page/) being rendered.

Destination
: The URL.

Title
: The title attribute.

Text
: The rendered (HTML) link text.

PlainText
: The plain variant of the above.

The `render-heading` template will receive this context:

Page
: The [Page](/variables/page/) being rendered.

Level
: The header level (1--6)

Anchor
: An auto-generated html id unique to the header within the page

Text
: The rendered (HTML) text.

PlainText
: The plain variant of the above.

#### Link with title Markdown example:

```md
[Text](https://www.gohugo.io "Title")
```

Here is a code example for how the render-link.html template could look:

{{< code file="layouts/_default/_markup/render-link.html" >}}
<a href="{{ .Destination | safeURL }}"{{ with .Title}} title="{{ . }}"{{ end }}{{ if strings.HasPrefix .Destination "http" }} target="_blank" rel="noopener"{{ end }}>{{ .Text | safeHTML }}</a>
{{< /code >}}

#### Image Markdown example:

```md
![Text](https://d33wubrfki0l68.cloudfront.net/c38c7334cc3f23585738e40334284fddcaf03d5e/2e17c/images/hugo-logo-wide.svg "Title")
```

Here is a code example for how the render-image.html template could look:

{{< code file="layouts/_default/_markup/render-image.html" >}}
<p class="md__image">
  <img src="{{ .Destination | safeURL }}" alt="{{ .Text }}" {{ with .Title}} title="{{ . }}"{{ end }} />
</p>
{{< /code >}}

#### Heading link example

Given this template file

{{< code file="layouts/_default/_markup/render-heading.html" >}}
<h{{ .Level }} id="{{ .Anchor | safeURL }}">{{ .Text | safeHTML }} <a href="#{{ .Anchor | safeURL }}">¶</a></h{{ .Level }}>
{{< /code >}}

And this markdown

```md
### Section A
```

The rendered html will be

```html
<h3 id="section-a">Section A <a href="#section-a">¶</a></h3>
```
