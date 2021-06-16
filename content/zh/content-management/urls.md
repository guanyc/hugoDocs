---
title: 链接处理
linktitle: 链接处理
description: Hugo支持永久链接、别名、链接规范化和多种处理相对链接相对于绝对链接的选项.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-03-09
keywords: [aliases,redirects,permalinks,urls]
categories: [content management]
menu:
  docs:
    parent: "content-management"
    weight: 110
weight: 110	#rem
draft: false
aliases: [/extras/permalinks/,/extras/aliases/,/extras/urls/,/doc/redirects/,/doc/alias/,/doc/aliases/]
toc: true
---

## 永久链接

Hugo构建网站默认的输出目录是 `public/`, 但是您可以改变这个目录值，通过在[站点配置][config]中声明不同的 `publishDir` 来实现。网站构建是内容块的目录反映了`content` 目录内内容的目录位置，并且namespace和`contentdir`目录中的布局匹配。

The `permalinks` option in your [site configuration][config] allows you to adjust the directory paths (i.e., the URLs) on a per-section basis. This will change where the files are written to and will change the page's internal "canonical" location, such that template references to `.RelPermalink` will honor the adjustments made as a result of the mappings in this option.
[site configuration][config]中的 `permalinks` 选项允许您基于内容块调整目录路径(URls).
这样会改变文件写入的目录，并且会改变页面内部的"规范化"位置，然后模板中`.RelPermalink`变量的引用会
兑现配置选项中这些映射调整。


{{% note "Default Publish and Content Folders" %}}
These examples use the default values for `publishDir` and `contentDir`; i.e., `public` and `content`, respectively. You can override the default values in your [site's `config` file](/getting-started/configuration/).
这些例子使用默认的`publishDir` 和 `contentDir`的值，相应的也就是`public` 和 `content`.
可以在 [站点配置文件](/getting-started/configuration/)中覆盖这些默认值
{{% /note %}}

For example, if one of your [sections][] is called `posts` and you want to adjust the canonical path to be hierarchical based on the year, month, and post title, you could set up the following configurations in YAML and TOML, respectively.
假设站点的一个[块sections][]名为`posts`, 您想基于年份、月份和文章标题来构建规范化的路径,可以像下面相应的YAML和TOML的配置中一样来设置:

### 永久链接配置举例

{{< code-toggle file="config" copy="false" >}}
permalinks:
  posts: /:year/:month/:title/
{{< /code-toggle >}}

Only the content under `posts/` will have the new URL structure. For example, the file `content/posts/sample-entry.md` with `date: 2017-02-27T19:20:00-05:00` in its front matter will render to `public/2017/02/sample-entry/index.html` at build time and therefore be reachable at `https://example.com/2017/02/sample-entry/`.
仅仅在 `posts/` 下面的内容会拥有这个新的URL结构。比如文件`content/posts/sample-entry.md` 在前言设定中设置了日期`date: 2017-02-27T19:20:00-05:00`在Hugo构建时会输出到`public/2017/02/sample-entry/index.html`, 进而可以在`https://example.com/2017/02/sample-entry/`访问到。

To configure the `permalinks` option for pages in the "root" section, use **/** as the key:
为根块中的页面配置`permalinks`永久链接选项，使用**/**作永久链接的键

{{< code-toggle file="config" copy="false" >}}
permalinks:
  /: /:year/:month/:filename/
{{< /code-toggle >}}

If the standard date-based permalink configuration does not meet your needs, you can also format URL segments using [Go time formatting directives](https://golang.org/pkg/time/#Time.Format). For example, a URL structure with two digit years and month and day digits without zero padding can be accomplished with:
如果标准的基于日期的永久链接配置不能满足您的需求，你也可以使用[Go time formatting directives](https://golang.org/pkg/time/#Time.Format)格式化URL的各小节. 比如，要求URL结构中包含2位数的年份、月份、和日期，不含补位的形式，可以通过如下设置实现:

{{< code-toggle file="config" copy="false" >}}
permalinks:
  posts: /:06/:1/:2/:title/
{{< /code-toggle >}}

You can also configure permalinks of taxonomies with the same syntax, by using the plural form of the taxonomy instead of the section. You will probably only want to use the configuration values `:slug` or `:title`.
对于标签分类的永久链接也可以如此配置，使用标签的复数形式而不是使用标签块。可能仅仅需要配置`:slug` 或者 `:title`


### 永久链接配置的值

The following is a list of values that can be used in a `permalink` definition in your site `config` file. All references to time are dependent on the content's date.
下面列表是可以使用在站点配置文件中的永久链接定义部分的值. 对时间的引用依赖于内容的日期.

`:year`
: the 4-digit year  4位数年份

`:month`
: the 2-digit month 2位数月份

`:monthname`
: the name of the month  月份的名称

`:day`
: the 2-digit day   两位数的月份内日期

`:weekday`   
: the 1-digit day of the week (Sunday = 0)  1位的星期几(周日是0)

`:weekdayname`
: the name of the day of the week   星期几的名称

`:yearday`
: the 1- to 3-digit day of the year   1到3位数的年份内日期序数

`:section`
: the content's section    内容块名称

`:sections`
: the content's sections hierarchy   内容块的层级

`:title`
: the content's title   内容的title

`:slug`
: the content's slug (or title if no slug is provided in the front matter)
内容的slug(或者是title，如果没有在前言设定中提供slug)

`:filename`
: the content's filename (without extension)  内容的文件名(不包含扩展名)

另外，用`:`作为前缀的Go语言的时间模式字符串可以使用

## 别名 Aliases

别名用来创建重定向到网站页面的其他URLs.

别名有两种呈现方式:

1. 以 `/` 开始的别名意味着他们相对于 `BaseURL`, 例如 `/posts/my-blogpost/`
2. 以相对于页面的方式定义, 比如 `my-blogpost` 或者甚至是像 `../blog/my-blogpost` (Hugo 0.55以后的新特性).


### 别名举例

Let's assume you create a new piece of content at `content/posts/my-awesome-blog-post.md`. The content is a revision of your previous post at `content/posts/my-original-url.md`. You can create an `aliases` field in the front matter of your new `my-awesome-blog-post.md` where you can add previous paths. The following examples show how to create this field in TOML and YAML front matter, respectively.
假设您在`content/posts/my-awesome-blog-post.md`创建了新的内容。这个内容是原先位于`content/posts/my-original-url.md`的内容的修订版。 您可以在新内容`my-awesome-blog-post.md`的前言设定中创建`aliases`，添加原文章的路径作为alias的一个值。下面例子中显示了如何在TOML和YAML前言设定中分别创建这个field.

#### TOML Front Matter

{{< code file="content/posts/my-awesome-post.md" copy="false" >}}
+++
aliases = [
    "/posts/my-original-url/",
    "/2010/01/01/even-earlier-url.html"
]
+++
{{< /code >}}

#### YAML Front Matter

{{< code file="content/posts/my-awesome-post.md" copy="false" >}}
---
aliases:
    - /posts/my-original-url/
    - /2010/01/01/even-earlier-url.html
---
{{< /code >}}

Now when you visit any of the locations specified in aliases---i.e., *assuming the same site domain*---you'll be redirected to the page they are specified on. For example, a visitor to `example.com/posts/my-original-url/` will be immediately redirected to `example.com/posts/my-awesome-post/`.
现在您访问在aliases中声明的任何位置---假设在相同的站点域名中---您会被重定向到别名声明所在的页面.
比如， 访问 `example.com/posts/my-original-url/` 会立即被重定向到 `example.com/posts/my-awesome-post/`.

### 多语言网站的别名应用举例

 在[多语言站点][multilingual]中, 页面的每种翻译都可以拥有唯一的别名。在多语言站点中使用相同的别名，请在别名值前加上语言代码.

在 `/posts/my-new-post.es.md`中:

```
---
aliases:
    - /es/posts/my-original-post/
---
```

From Hugo 0.55 you can also have page-relative aliases, so ` /es/posts/my-original-post/` can be simplified to the more portable `my-original-post/`
Hugo 版本0.55开始也可以使用相对页面的别名，` /es/posts/my-original-post/`可以简化为更可移植的 `my-original-post/`.

### Hugo别名如何工作？

当别名声明后, HUGO会创建一个匹配别名条目的目录. 在这个目录中Hugo创建一个`.html`文件，文件中声明页面的规范化URL和新的重定向目标

比如, 一个内容文件`posts/my-intended-url.md`具有如下的前言设定:

```
---
title: My New post
aliases: [/posts/my-old-url/]
---
```

Assuming a `baseURL` of `example.com`, the contents of the auto-generated alias `.html` found at `https://example.com/posts/my-old-url/` will contain the following:
假设站点的基础 `baseURL` 是 `example.com`, 自动生成的别名`.html`可以在`https://example.com/posts/my-old-url/`获得， 并且包含如下内容文本:

```
<!DOCTYPE html>
<html>
  <head>
    <title>https://example.com/posts/my-intended-url</title>
    <link rel="canonical" href="https://example.com/posts/my-intended-url"/>
    <meta name="robots" content="noindex">
    <meta http-equiv="content-type" content="text/html; charset=utf-8"/>
    <meta http-equiv="refresh" content="0; url=https://example.com/posts/my-intended-url"/>
  </head>
</html>
```

The `http-equiv="refresh"` line is what performs the redirect, in 0 seconds in this case. If an end user of your website goes to `https://example.com/posts/my-old-url`, they will now be automatically redirected to the newer, correct URL. The addition of `<meta name="robots" content="noindex">` lets search engine bots know that they should not crawl and index your new alias page. //TODO

包含`http-equiv="refresh"`的那行代码导致浏览器执行重定向,此处是0秒完成。如果网站的终端用户访问了
`https://example.com/posts/my-old-url`，他们会自动被重定向到新的、正确的URL。`<meta name="robots" content="noindex">`额外添加的这行使得查询引擎机器人知道他们应该不要爬和索引旧的别名页。

### 定制
You may customize this alias page by creating an `alias.html` template in the
layouts folder of your site (i.e., `layouts/alias.html`). In this case, the data passed to the template is
可以通过在站点布局目录创建`alias.html` 模板(如 `layouts/alias.html`)来定制别名页.
在此情况下, 传递给模板的数据如下:

`Permalink`
: 被别名化的页面的链接

`Page`
: 被别名化的页面的Page数据

### 别名的重要行为

1. Hugo对别名不做任何假设,也不会因为UglyURLs设置而改变。别名需要提供相对于站点根部的绝对路径和完整的文件名称或路径.
2. 别名在内容输出*前*已经呈现,因此会覆盖在同一位置的任何内容.

## 漂亮的URL

Hugo默认的行为是使用 "pretty" URLs展示内容。这些漂亮URLs的功能不需要服务器端的任何非标准设置.
下面展示了漂亮URL的概念:

```
content/posts/_index.md
=> example.com/posts/index.html
content/posts/post-1.md
=> example.com/posts/post-1/
```

## 丑陋的URLs

如果想要通常所谓的"ugly URLs" (比如, example.com/urls.html), 相应的在您站点的 `config.toml` 或者 `config.yaml`文件中设置 `uglyurls = true` 或者 `uglyurls: true`.

如果需要特定一条内容具有精确的URL,可以在[front matter][] 的 `url` 键下面声明这个值.
下面例子是具有相同内容目录的站点在默认情况下的最终URL结构:

参考 [Content Organization][contentorg] 获得关于路径更多信息.

```
.
└── content
    └── about
    |   └── _index.md  // <- https://example.com/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

同样的网站组织，使用 `hugo --uglyURLs` 运行时:

```
.
└── content
    └── about
    |   └── _index.md  // <- https://example.com/about.html
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost.html
    |   ├── happy
    |   |   └── ness.md    // <- https://example.com/posts/happy/ness.html
    |   └── secondpost.md  // <- https://example.com/posts/secondpost.html
    └── quote
        ├── first.md       // <- https://example.com/quote/first.html
        └── second.md      // <- https://example.com/quote/second.html
```


## 规范化 Canonicalization

By default, all relative URLs encountered in the input are left unmodified, e.g. `/css/foo.css` would stay as `/css/foo.css`. The `canonifyURLs` field in your site `config` has a default value of `false`.
缺省情况下,所有在内容中遇到的相对的URL都会保持不变, 比如`/css/foo.css`会保持为`/css/foo.css`.
规范化URL`canonifyURLs`的键在站点配置文件中的缺省值是 `false`.

设置`canonifyURLs` 为 `true`, 所有的相对URL会被替代为使用 `baseURL`的*规范化*URL.
例如,假设 `baseURL = https://example.com/`, 相对URL是`/css/foo.css`, 替换以后的规范化绝对URL是`https://example.com/css/foo.css`.

规范化的好处包括将所有URL绝对化, 这可能对一些解析任务有帮助。不过请注意,所有客户端的现代浏览器会没有任何问题的解决这个问题。

非规范化的好处包括能够使用基于范式的资源包含(scheme-relative resource inclusion); 比如，可以根据页面如何获得确定是http还是https.

{{% note "`canonifyURLs` default change" %}}
In the May 2014 release of Hugo v0.11, the default value of `canonifyURLs` was switched from `true` to `false`, which we think is the better default and should continue to be the case going forward. Please verify and adjust your website accordingly if you are upgrading from v0.10 or older versions.
{{% /note %}}

To find out the current value of `canonifyURLs` for your website, you may use the handy `hugo config` command added in v0.13.

```
hugo config | grep -i canon
```

Or, if you are on Windows and do not have `grep` installed:

```
hugo config | FINDSTR /I canon
```

## 前言设定中设定URL

In addition to specifying permalink values in your site configuration for different content sections, Hugo provides even more granular control for individual pieces of content.
在站点配置中为不同内容块声明链接外，Hugo对单独页面提供了更细粒度的控制

在单独页面前言设定中可以设置`slug`和`url`。
更多关于构建时内容目的地的信息，请参考[Content Organization][contentorg].

从Hugo 0.55开始，可以使用相对于当前站点上下文(语言)的相对URL，这使得维护变得简单。
对于日文翻译，下面两个例子的都会获得相同的URL:

```markdown
---
title: "Custom URL!"
url: "/jp/custom/foo"
---
```

```markdown
---
title: "Custom URL!"
url: "custom/foo"
---
```


## 相对URL

By default, all relative URLs are left unchanged by Hugo, which can be problematic when you want to make your site browsable from a local file system.
缺省情况下,hugo会保持相对URL不变，这会给从本地文件系统浏览站点带来麻烦。

Setting `relativeURLs` to `true` in your [site configuration][config] will cause Hugo to rewrite all relative URLs to be relative to the current content.
在[站点配置][config]中设置 `relativeURLs` 为 `true`, 会导致Hugo重新写所有相对URL为相对于当前上下文

For example, if your `/posts/first/` page contains a link to `/about/`, Hugo will rewrite the URL to `../../about/`.
例如，如果`/posts/first/` 页面包含链接到达 `/about/`, Hugo 会重写这个URL为 `../../about/`

[config]: /getting-started/configuration/
[contentorg]: /content-management/organization/
[front matter]: /content-management/front-matter/
[multilingual]: /content-management/multilingual/
[sections]: /content-management/sections/
[usage]: /getting-started/usage/
