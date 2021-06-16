---
title: 前言设定 Front Matter
linktitle: 前言设定
description: Hugo允许在内容文件中添加yaml,TOML或者json格式的前言设定部分
date: 2017-01-09
publishdate: 2017-01-09
lastmod: 2017-02-24
categories: [content management]
keywords: ["front matter", "yaml", "toml", "json", "metadata", "archetypes"]
menu:
  docs:
    parent: "content-management"
    weight: 30
weight: 30	#rem
draft: false
aliases: [/content/front-matter/]
toc: true
---

**Front matter** allows you to keep metadata attached to an instance of a [content type][]---i.e., embedded inside a content file---and is one of the many features that gives Hugo its strength.
**Front matter

前言设定** 允许您将元数据绑定到[内容类型][content type]的实例---嵌入到内容文件---这也是Hugo众多特性中的一个, 这些特性使得Hugo无比强大

{{< youtube Yh2xKRJGff4 >}}

## 前言设定格式

Hugo supports four formats for front matter, each with their own identifying tokens.
Hugo支持四种前言设定格式,每种格式都有它们自己的标记符号.

TOML
: 以 `+++` 标记符号开始和结束

YAML
: 以 `---` 标记符号开始和结束


JSON
: 一个JSO格式的对象由'`{`' 和 '`}`'包围, 后面是一新行.

ORG
: a group of Org mode keywords in the format '`#+KEY: VALUE`'. Any line that does not start with `#+` ends the front matter section.
  Keyword values can be either strings (`#+KEY: VALUE`) or a whitespace separated list of strings (`#+KEY[]: VALUE_1 VALUE_2`).
: 一组Org mode 关键字, 格式是 '`#+KEY: VALUE`'. 任何不以 `#+` 开头的一行结束前言设定部分.
键值的值可以是字符串(`#+KEY: VALUE`)或者由空白分隔的字符串列表(`#+KEY[]: VALUE_1 VALUE_2`).


### 例子

{{< code-toggle >}}
title = "spf13-vim 3.0 release and new website"
description = "spf13-vim is a cross platform distribution of vim plugins and resources for Vim."
tags = [ ".vimrc", "plugins", "spf13-vim", "vim" ]
date = "2012-04-06"
categories = [
  "Development",
  "VIM"
]
slug = "spf13-vim-3-0-release-and-new-website"
{{< /code-toggle >}}

## 前言设定变量

### 预定义变量

有一些Hugo可以处理的预定义的变量。
参考[页面变量 Page Variables][pagevars]获得
如何在模板中调用这些众多预定义变量的方法.

别名 aliases
: an array of one or more aliases (e.g., old published paths of renamed content) that will be created in the output directory structure . See [Aliases][aliases] for details.

一个或者多个将在输出目录结构中创建的别名的数组(比如, 改名后内容的原发布路径).
详细信息参考[别名][aliases]

音频 audio
: 页面相关的音频文件路径的数组; 短代码`opengraph` (参考[internal template](/templates/internal)) 使用这些信息来填充 `og:audio`

级联信息 cascade
前言设定的键值对的字典,它们的值会传递给页面的后代，除非被后代改写或者被后代的近端祖先的cascade改写.
详细信息参考[前言设定级联](#front-matter-cascade).

日期 date
: 页面的日期, 这通常由前言设定的`date`字段读取, 也是可配置的

description
: 内容的描述.

draft
: 如果设定为`true`, 内容不会被输出, 除非传递给`hugo`命令`--buildDrafts`标记.

expiryDate
: 过期日期，过了这个日期，内容不会被hugo发布。过期内容不会被发布除非给hugo命令传递了`--buildExpired`标记参数.

headless
: 设置为真，将叶子包设置为[无名包][headless-bundle].

images
: 页面相关的图像路径数组。内部模板[internal templates](/templates/internal) 如 `_internal/twitter_cards.html` 会使用这个变量.

isCJKLanguage 是否中日韩文
:设置为真，hugo会将内容作为中日韩文处理，然后`.Summary` and `.WordCount` 在中日韩语言中会正确工作.

keywords 关键字
: 内容的元数据关键字.

layout
: hugo从布局[查询顺序][lookup]中选择用来呈现内容的布局。如果在前言设定中没有声明`layout`,hugo会在布局目录中查找和内容区块同名的布局文件。参考 ["定义内容类型"][definetype]


lastmod
: 内容最后修改时间

linkTitle
: 创建内容链接使用的title。如果设置了linkTitle，Hugo默认会在 `title`之前使用`linktitle`.
Hugo也能够[对内容列表按照`linktitle`排序][bylinktitle].

markup
: **实验性质的设置**; 声明 `"rst"` 为 reStructuredText (需要`rst2html`) 或者 `"md"` (默认) 为 Markdown.

outputs
: 允许声明内容特定的输出格式。请参考[输出格式][outputs].

publishDate
: 发布日期. 如果此日期在未来，内容不会被呈现, 除非`--buildFuture` 标记被传递给`hugo`.

resources
: 配置页面包资源。参考[Page Resources][page-resources].

series
: an array of series this page belongs to, as a subset of the `series` [taxonomy](/content-management/taxonomies/); used by the `opengraph` [internal template](/templates/internal) to populate `og:see_also`.

slug
: 呈现为输出的URL的尾部。在前言设定的slug值会覆盖从文件名称推断出来的URL的对应部分.

summary
:在`.Summary`页面变量中提供文章的summary时使用的文本。细节请参考[content-summaries](/content-management/summaries/)部分文档。

title
: 内容的标题

type
: 内容的类型。这个值如果在前言设定没声明，会自动从目录([区块][section])推断出来

url
: 内容相对于web root的完整路径。这个设置不对内容文件的路径做任何假设。也忽略多语言特性中的任何语言前缀。

videos
: 页面相关的视频路径数组, `opengraph` [内部模板](/templates/internal) 使用这些信息填充 `og:video`.

weight
: [列表内容排序时使用][ordering]. 值小的weight具有更高优先级。所以具有小值的内容会先出现。如果设置这个参数, weight应该是非零值, 0会被解析为*未设置*weight值.

\<taxonomies\>
: 复数形式的索引的域名称。参考上面前言设定例子中的 `tags` 和 `categories`.
_注意复数形式的用户定义的标签不能和hugo预定义的任何前言设定变量相同。
Note that the plural form of user-defined taxonomies cannot be the same as any of the predefined front matter variables._
field name of the *plural* form of the index. See `tags` and `categories` in the above front matter examples. _Note that the plural form of user-defined taxonomies cannot be the same as any of the predefined front matter variables._

{{% note "Hugo's Default URL Destinations" %}}
如果 `slug` 和 `url` 没设置，并且在站点 `config`文件中没定义[permalinks](/content-management/urls/#permalinks),Hugo会使用内容文件的名称去创建输出的URL。参考 [内容组织](/content-management/organization) 学习Hugo中路径的解释， 以及 [URL Management](/content-management/urls/)学习如何定制Hugo的默认行为.
{{% /note %}}

### 用户定义的设置键值

可以在前言设定中随意添加域以满足建立站点需求. 这些用户定义的键值对可以在站点模板中通过一个 `.Params` 变量访问使用.

下面的域可以相应的通过  `.Params.include_toc` 和 `.Params.show_comments` 访问。
[变量Variables][] 部分提供了关于使用Hugo的页面变量和站点级别变量的更多信息.

{{< code-toggle copy="false" >}}
include_toc: true
show_comments: false
{{</ code-toggle >}}

## 前言设定级联设置

Any node or section can pass down to descendents a set of Front Matter values as long as defined underneath the reserved `cascade` Front Matter key.
任何节点和区块可以向下传递任何在只要保留在 `cascade` 前言设定键下面的前言设定值。

### Target Specific Pages 针对特定目标的页面

{{< new-in "0.76.0" >}}

从Hugo 0.76开始，`cascade`区块可以由可选的`_target`关键字切块，
允许以不同页面集合为目标设定多个`cascade`值.

{{< code-toggle copy="false" >}}
title ="Blog"
[[cascade]]
background = "yosemite.jpg"
[cascade._target]
path="/blog/**"
lang="en"
kind="page"
[[cascade]]
background = "goldenbridge.jpg"
[cascade._target]
kind="section"
{{</ code-toggle >}}


`_target` 可以使用的关键词在:

path
: A [Glob](https://github.com/gobwas/glob) pattern matching the content path below /content. Expects Unix-styled slashes. Note that this is the virtual path, so it starts at the mount root. The matching support double-asterisks so you can match for patterns like `/blog/*/**` to match anything from the third level and down.

和在/content之下的内容路径匹配的[Glob](https://github.com/gobwas/glob)模式。期望的类Unix的斜杠。
请注意这是虚拟路径, 所以它从加载的根开始。模式匹配支持双星号，因此您可以使用类似 `/blog/*/**` 的模式去匹配任何第三级内容或它们后代。

kind
: 模式字符串和页面类型(比如{home, section})匹配
A Glob pattern matching the Page's Kind(s), e.g. "{home,section}".

lang
: 模式字符串和页面语言(比如{en, sv})匹配
A Glob pattern matching the Page's language, e.g. "{en,sv}".

上面的任何一条可以省略.

### 例子

In `content/blog/_index.md`

{{< code-toggle copy="false" >}}
title: Blog
cascade:
  banner: images/typewriter.jpg
{{</ code-toggle >}}

With the above example the Blog section page and its descendents will return `images/typewriter.jpg` when `.Params.banner` is invoked unless:
上面例子中区块Blog中页面和后代页面在调用 `.Params.banner` 会返回 `images/typewriter.jpg`,除非:

- 后代页面声明了自己的 `banner` 值集合
- 或者更近的祖先声明了 `cascade.banner`值集合



## 通过前言设定对内容排序

You can assign content-specific `weight` in the front matter of your content. These values are especially useful for [ordering][ordering] in list views. You can use `weight` for ordering of content and the convention of [`<TAXONOMY>_weight`][taxweight] for ordering content within a taxonomy. See [Ordering and Grouping Hugo Lists][lists] to see how `weight` can be used to organize your content in list views.

可以通过在页面的前言设定中对页面赋值内容特定的`weight`. 这些值对于列表的[排序ordering][ordering]特别有用。
可以使用`weight`对内容排序，以及对标签内内容使用[`<TAXONOMY>_weight`][taxweight]进行排序。
参考[Ordering and Grouping Hugo Lists][lists] 获得早list视图中组织内容时 `weight`的应用方式.

## 重载全局的Markdown配置

It's possible to set some options for Markdown rendering in a content's front matter as an override to the [rendering options set in your project configuration][config].

可以在内容的前言设定中设置一些Markdown输出的选项，作为全局设定的设置覆盖。

## 前言设定格式规范

* [TOML Spec][toml]
* [YAML Spec][yaml]
* [JSON Spec][json]

[variables]: /variables/
[aliases]: /content-management/urls/#aliases
[archetype]: /content-management/archetypes/
[bylinktitle]: /templates/lists/#by-link-title
[config]: /getting-started/configuration/ "Hugo documentation for site configuration"
[content type]: /content-management/types/
[contentorg]: /content-management/organization/
[definetype]: /content-management/types/#defining-a-content-type "Learn how to specify a type and a layout in a content's front matter"
[headless-bundle]: /content-management/page-bundles/#headless-bundle
[json]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "Specification for JSON, JavaScript Object Notation"
[lists]: /templates/lists/#ordering-content "See how to order content in list pages; for example, templates that look to specific _index.md for content and front matter."
[lookup]: /templates/lookup-order/ "Hugo traverses your templates in a specific order when rendering content to allow for DRYer templating."
[ordering]: /templates/lists/ "Hugo provides multiple ways to sort and order your content in list templates"
[outputs]: /templates/output-formats/ "With the release of v22, you can output your content to any text format using Hugo's familiar templating"
[page-resources]: /content-management/page-resources/
[pagevars]: /variables/page/
[section]: /content-management/sections/
[taxweight]: /content-management/taxonomies/
[toml]: https://github.com/toml-lang/toml "Specification for TOML, Tom's Obvious Minimal Language"
[urls]: /content-management/urls/
[variables]: /variables/
[yaml]: https://yaml.org/spec/ "Specification for YAML, YAML Ain't Markup Language"
