---
title: 链接和交叉引用
description: 创建文档引用的短代码
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-03-31
categories: [content management]
keywords: ["cross references","references", "anchors", "urls"]
menu:
  docs:
    parent: "content-management"
    weight: 100
weight: 100	#rem
aliases: [/extras/crossreferences/]
toc: true
---

短代码 `ref` 和 `relref`相应的显示到达文档的绝对链接和相对链接.

## 使用 `ref` 和 `relref`

```go-html-template
{{</* ref "document" */>}}
{{</* ref "document#anchor" */>}}
{{</* ref "document.md" */>}}
{{</* ref "document.md#anchor" */>}}
{{</* ref "#anchor" */>}}
{{</* ref "/blog/my-post" */>}}
{{</* ref "/blog/my-post.md" */>}}
{{</* relref "document" */>}}
{{</* relref "document.md" */>}}
{{</* relref "#anchor" */>}}
{{</* relref "/blog/my-post.md" */>}}
```

在markdown中使用 `ref` 或者 `relref` 生成超链接:

```md
[About]({{</* ref "document/en/page/about" */>}} "About Us")
```
短代码 `ref` and `relref` 需要一个参数: 到达内容文档的路径, 包含或者不包含文件扩展名，包含或者不包含页面锚点.

**不包含开头 `/` 的路径首先会相对于当前页面解析，然后相对于整个站点其余部分解析.**

Hugo emits an error or warning if a document cannot be uniquely resolved. The error behavior is configurable; see below.
如果文档无法被唯一解析，Hugo会生成错误. 错误行为可以配置，参见下面.

### 链接到另一语言版本

链接到文档的另一个语言的版本，使用这样格式:

```go-html-template
{{</* relref path="document.md" lang="ja" */>}}
```

### Get another Output Format

To link to another Output Format of a document, use this syntax:

```go-html-template
{{</* relref path="document.md" outputFormat="rss" */>}}
```

### Heading IDs

When using Markdown document types, Hugo generates element IDs for every heading on a page. For example:

```md
## Reference
```

produces this HTML:

```html
<h2 id="reference">Reference</h2>
```

Get the permalink to a heading by appending the ID to the path when using the `ref` or `relref` shortcodes:

```go-html-template
{{</* ref "document.md#reference" */>}}
{{</* relref "document.md#reference" */>}}
```

Generate a custom heading ID by including an attribute. For example:

```md
## Reference A {#foo}
## Reference B {id="bar"}
```

produces this HTML:

```html
<h2 id="foo">Reference A</h2>
<h2 id="bar">Reference B</h2>
```

Hugo will generate unique element IDs if the same heading appears more than once on a page. For example:

```md
## Reference
## Reference
## Reference
```

produces this HTML:

```html
<h2 id="reference">Reference</h2>
<h2 id="reference-1">Reference</h2>
<h2 id="reference-2">Reference</h2>
```

## Ref and RelRef Configuration

The behavior can, since Hugo 0.45, be configured in `config.toml`:

refLinksErrorLevel ("ERROR")
: When using `ref` or `relref` to resolve page links and a link cannot resolved, it will be logged with this log level. Valid values are `ERROR` (default) or `WARNING`. Any `ERROR` will fail the build (`exit -1`).

refLinksNotFoundURL
: URL to be used as a placeholder when a page reference cannot be found in `ref` or `relref`. Is used as-is.


[lists]: /templates/lists/
[output formats]: /templates/output-formats/
[shortcode]: /content-management/shortcodes/
[bfext]: /content-management/formats/#blackfriday-extensions
