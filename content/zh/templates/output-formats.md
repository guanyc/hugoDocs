---
title: 定制输出格式
linktitle: 定制输出格式
description: Hugo can output content in multiple formats, including calendar events, e-book formats, Google AMP, and JSON search indexes, or any custom text format.
date: 2017-03-22
publishdate: 2017-03-22
lastmod: 2019-12-11
categories: [templates]
keywords: ["amp","outputs","rss"]
menu:
  docs:
    parent: "templates"
    weight: 18
weight: 18
sections_weight: 18
draft: false
aliases: [/templates/outputs/,/extras/output-formats/,/content-management/custom-outputs/]
toc: true
---

本页描述了如何正确配置站点的媒体类型和输出格式, 以及为定制输出创建模板的路径

## 媒体类型

[媒体类型][media type] (也称为 *MIME type* 以及 *内容类型content type*) 是两部分组成的文件格式标志符, 也是网络上传输的内容格式。

这是Hugo内置的媒体类型的全部集合:

{{< datatable "media" "types" "type" "suffixes" >}}

**注意:**

* 可以添加定制媒体类型或者改变默认的媒体类型;比如，可以改变 `text/html` 的后缀为`asp`.
* 后缀值会用于Hugo中对应的媒体类型的URLs和文件名称.
* `Type` 是定义新的或者定制 `Output Formats` (见下面)必须使用的标志符.
* 媒体类型的全部集合在Hugo的内置开发服务器中注册，以确保可以被浏览器识别

添加或者修改一个媒体类型，请在[site configuration][config]中`mediaTypes` 部分添加或修改,
针对所有站点或者某给定语言.

{{< code-toggle file="config" >}}
[mediaTypes]
  [mediaTypes."text/enriched"]
  suffixes = ["enr"]
  [mediaTypes."text/html"]
  suffixes = ["asp"]
{{</ code-toggle >}}

上面例子添加了一个媒体类型,`text/enriched`, 同时改变了内置 `text/html` 媒体类型的后缀.

**Note:** these media types are configured for **your output formats**. If you want to redefine one of Hugo's default output formats (e.g. `HTML`), you also need to redefine the media type. So, if you want to change the suffix of the `HTML` output format from `html` (default) to `htm`:

**注意:** 这些媒体类型是为 **您的输出格式** 而配置的. 如果想重新定义Hugo的默认的输出格式中一个(比如HTML), 您也需要定义媒体类型。 下面配置改变了 `HTML` 输出格式从 默认的`html` 修改为 `htm`:


```toml
[mediaTypes]
[mediaTypes."text/html"]
suffixes = ["htm"]

# Redefine HTML to update its media type.
[outputFormats]
[outputFormats.HTML]
mediaType = "text/html"
```

**注意** 为上面代码工作, 您站点配置文件中需要一个`outputs` 定义.

## 输出格式定义 Output Format Definitions

给定一个媒体类型，添加一些配置，您就配置好了**输出格式**

这里是Hugo内置的输出格式的全部集合:

{{< datatable "output" "formats" "name" "mediaType" "path" "baseName" "rel" "protocol" "isPlainText" "isHTML" "noUgly" "permalinkable" >}}

* 每个页面能够以您需要的任意多输出格式输出, 而且您可以定义任意数量的输出格式， **只要他们解析到文件系统的唯一路径**.上表中,最佳的例子是`AMP` 和 `HTML` 对比。`AMP`的`路径`为`amp`, 所以它不覆盖`HTML`版本, 进而我们可以访问 `/index.html` 和 `/amp/index.html`.
* `MediaType`必须和已经定义的某个媒体类型的`Type`匹配
* 可以定义新的输出格式，或者重定义内置的输出格式，比如想将`AMP`页面置于不同路径.



添加或者修改一个输出类型，请在[site configuration][config]中`outputFormats` 部分添加或修改,
针对所有站点或者某给定语言.

{{< code-toggle file="config" >}}
[outputFormats.MyEnrichedFormat]
mediaType = "text/enriched"
baseName = "myindex"
isPlainText = true
protocol = "bep://"
{{</ code-toggle >}}

The above example is fictional, but if used for the homepage on a site with `baseURL` `https://example.org`, it will produce a plain text homepage with the URL `bep://example.org/myindex.enr`.
上面例子是想象出来的, 如果作为`baseURL`为`https://example.org`的站点的首页，当通过URL `bep://example.org/myindex.enr`访问时, Hugo会生成一个朴素的文本文件主页.

### 配置输出格式

下面是输出格式的配置选项的全部列表以及他们的默认值:

`name`
: 输出格式的标志符号. 用来定义页面的输出格式.

`mediaType`
: 必须与已经定义的媒体类型的`Type`匹配.

`path`
: 保存输出文件的sub path.

`baseName`
: 文件名称列表(首页等)的基础文件名称 **默认值:** `index`.

`rel`
: 可以用来在`link` 标签中创建 `rel`. **缺省值:** `alternate`.

`protocol`
: 替换输出格式的`baseURL`中的"http://" 或者 "https://".

`isPlainText`
: 如果为真, 将使用Go语言的朴素文本解析器解析模板. **缺省值:** `false`.

`isHTML`
: 仅仅在类似`HTML`格式的情景中有关,比如 页面别名.

`noUgly`
:当站点设置`uglyURLs`为`true`时, 关闭ugly URLs. **缺省值:** `false`.

`notAlternative`
: 如果在页面(如Css)的`AlternativeOutputFormats` 格式列表中包含它无意义的化，设置这个变量为真。 注意这里我们使用条目 *alternative* 而不是*alternate*, 因为它没有必要会替换其他格式.   enable if it doesn't make sense to include this format in an `AlternativeOutputFormats` format listing on `Page` (e.g., with `CSS`). Note that we use the term *alternative* and not *alternate* here, as it does not necessarily replace the other format. **缺省值:** `false`. ??

`permalinkable`
: 使得 `.Permalink` and `.RelPermalink` 返回呈现的输出格式而不是main([see below](#link-to-output-formats)). 对`HTML`和`AMP`默认是启用的。   make `.Permalink` and `.RelPermalink` return the rendering Output Format rather than main ([see below](#link-to-output-formats)). This is enabled by default for `HTML` and `AMP`. **缺省值:** `false`.

## 页面的输出格式

A `Page` in Hugo can be rendered to multiple *output formats* on the file
system.
Hugo中`页面`可以在文件系统中呈现出多种输出格式。

### 默认的输出格式
Every `Page` has a [`Kind`][page_kinds] attribute, and the default Output
Formats are set based on that.
每一页都有 [`Kind`][page_kinds] 属性, 默认的输出格式基于这个kind属性

| Kind           | Default Output Formats |
|--------------- |----------------------- |
| `page`         | HTML                   |
| `home`         | HTML, RSS              |
| `section`      | HTML, RSS              |
| `taxonomy`     | HTML, RSS              |
| `term`         | HTML, RSS              |

### 定制输出格式 Customizing Output Formats

This can be changed by defining an `outputs` list of output formats in either
the `Page` front matter or in the site configuration (either for all sites or
per language).
在页面的前端设置或者网站配置文件(对所有站点或者每个语言)中定义输出格式的`outputs`列表可以定制输出格式.

站点配置的例子:

{{< code-toggle file="config" >}}
[outputs]
  home = ["HTML", "AMP", "RSS"]
  page = ["HTML"]
{{</ code-toggle >}}


注意上面例子中, `section`, `taxonomy` 和 `term` 的*输出格式* 仍然是它们的默认值`["HTML",
"RSS"]`.

{{< new-in "0.73.0" >}} We have fixed the before confusing page kinds used for taxonomies (see the listing below) to be in line with the terms used when we talk about taxonomies. We have been careful to avoid site breakage, and you should get an ERROR in the console if you need to adjust your `outputs` section.

{{% page-kinds %}}

* The `outputs` definition is per [`Page` `Kind`][page_kinds] (`page`, `home`, `section`, `taxonomy`, or `term`).
* The names (e.g. `HTML`, `AMP`) used must match the `Name` of a defined *Output Format*.
  * These names are case insensitive.
* These can be overridden per `Page` in the front matter of content files.

* 输出的定义是针对每一个 [`Page` `Kind`][page_kinds] (`page`, `home`, `section`, `taxonomy`, or `term`).
* 输出格式的名称(如 `HTML` `AMP`)必须和定义的*输出格式*的`Name`匹配The names.
* 这些名称是大小写无关的.
* 可以在内容页面的前端设置重载这个设置.

下面是一个内容文件的`YAML`前端设置的例子，定义了输出的`Page`的输出格式:

```yaml
---
date: "2016-03-19"
outputs:
- html
- amp
- json
---
```

##输出格式的列表

每个`页面`都有`.OutputFormats` (所有格式, 包含当前的格式) 和 `.AlternativeOutputFormats`变量,
后者有助于在站点的`<head>`创建`link rel`的list:

```go-html-template
{{ range .AlternativeOutputFormats -}}
<link rel="{{ .Rel }}" type="{{ .MediaType.Type }}" href="{{ .Permalink | safeURL }}">
{{ end -}}
```

## 输出格式的链接

`.Permalink` and `.RelPermalink` on `Page` will return the first output format defined for that page (usually `HTML` if nothing else is defined). This is regardless of the template file they are being called from.
页面的`.Permalink` 和 `.RelPermalink` 会返回页面定义的输出格式中第一个(通常是HTML, 如果没有定义其他的)。这个调用它们的模板文件无关。

__from `single.json.json`:__
```go-html-template
{{ .RelPermalink }} > /that-page/
{{ with  .OutputFormats.Get "json" -}}
{{ .RelPermalink }} > /that-page/index.json
{{- end }}
```

In order for them to return the output format of the current template file instead, the given output format should have its `permalinkable` setting set to true.
为了上面代码段返回的是当前模板文件的输出格式, 给定的输出格式应该设置`permalinkable`为true.

__Same template file as above with json output format's `permalinkable` set to true:__
__同上面一样的模板文件，设置了json输出格式的`permalinkable`为真:__

```go-html-template
{{ .RelPermalink }} > /that-page/index.json
{{ with  .OutputFormats.Get "html" -}}
{{ .RelPermalink }} > /that-page/
{{- end }}
```

在内容文件中，可以使用[`ref` 或者 `relref` 短代码](/content-management/shortcodes/#ref-and-relref)
:

```go-html-template
[Neat]({{</* ref "blog/neat.md" "amp" */>}})
[Who]({{</* relref "about.md#who" "amp" */>}})
```

## 输出格式的模板 Templates for Your Output Formats

A new output format needs a corresponding template in order to render anything useful.
一个新的输出格式需要相应的模板，然后才能输出有用的内容。

{{% note %}}
The key distinction for Hugo versions 0.20 and newer is that Hugo looks at an output format's `Name` and MediaType's `Suffixes` when choosing the template used to render a given `Page`.
{{% /note %}}

The following table shows examples of different output formats, the suffix used, and Hugo's respective template [lookup order][]. All of the examples in the table can:
下面列出了不同的输出格式的例子, 包括后缀、hugo的对应的模板查询顺序. 表内例子都可以:

* 使用基础模板[base template][base].
* 包含局部部分模板[partial templates][partials]

{{< datatable "output" "layouts" "Example" "OutputFormat" "Suffix" "Template Lookup Order" >}}

Hugo的现在版本会检测部分模板的媒体类型和输出格式，如果可能的话，并且使用这些信息决定部分模板是否可以解析为纯文本模板。

Hugo will look for the name given, so you can name it whatever you want. But if you want it treated as plain text, you should use the file suffix and, if needed, the name of the Output Format. The pattern is as follows:

```
[partial name].[OutputFormat].[suffix]
```

The partial below is a plain text template (Output Format is `CSV`, and since this is the only output format with the suffix `csv`, we don't need to include the Output Format's `Name`):

```
{{ partial "mytextpartial.csv" . }}
```

[base]: /templates/base/
[config]: /getting-started/configuration/
[lookup order]: /templates/lookup/
[media type]: https://en.wikipedia.org/wiki/Media_type
[partials]: /templates/partials/
[page_kinds]: /templates/section-templates/#page-kinds
