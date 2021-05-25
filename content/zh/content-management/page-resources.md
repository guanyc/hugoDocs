---
title : "页面资源 Page Resources"
description : "页面资源 -- 图片、其他页、文档等. -- 具有相对于页面的URL值 和它们自己的元数据."
date: 2018-01-24
categories: ["content management"]
keywords: [bundle,content,resources]
weight: 4003
draft: false
toc: true
linktitle: "页面资源 TODO"
menu:
  docs:
    parent: "content-management"
    weight: 31
---

Page resources are available for [page bundles]({{< relref "/content-management/page-bundles" >}}) only,
i.e. a directory with either a `index.md`, or `_index.md` file at its root. Resources are only attached to
the lowest page they are bundled with, and simple which names does not contain `index.md` are not attached any resource.

页面资源仅仅可由[页面包]({{< relref "/content-management/page-bundles" >}})可以访问，
页面包就是一个根部包含`index.md`, 或者 `_index.md`文件的目录。资源仅仅与它们所属的最低层级的页面绑定。对于不包含`index.md` 的目录不附属任何资源。

## 属性 Properties

资源类型 ResourceType
:资源的[媒体类型](/templates/output-formats/#media-types)的主要类型。
比如Mime type是`image/jpeg`的文件具有`image`资源类型。
`Page`的资源类型值为`page`

{{< new-in "0.80.0" >}} Note that we in Hugo `v0.80.0` fixed a bug where non-image resources (e.g. video) would return the MIME sub type, e.g. `json`.

？在Hugo `0.80.0`版本中，我们修正了一个错误，非图片类型(比如视频)返回MIME自类型，比如`json`?.

资源名称 Name
: 默认值是文件名(相对于拥有页面), 可以在前端设置中设置。


Title
: 默认值和`.Name`一样也是是文件名(相对于拥有页面), 可以在前端设置中设置。


绝对URL Permalink
: 资源的绝对URL。 `page`类型的资源没有这个值。


相对URL elPermalink
: 资源的相对URL。 `page`类型的资源没有这个值。


内容 Content
: 资源本身的内容。对大多数资源来说，这返回一个文件内容的字符串。
这可以用来内联一些资源，比如 `<script>{{ (.Resources.GetMatch "myscript.js").Content | safeJS }}</script>` 或者 `<img src="{{ (.Resources.GetMatch "mylogo.png").Content | base64Encode }}">`

媒体类型 MediaType
: 资源的 MIME 类型, 如 `image/jpeg`.

MediaType.MainType
:文件Mime type的的主类型。比如具有`application/pdf`Mime type的文件主类型是 `application`。  

MediaType.SubType
: 资源文件MIME type的子类型。比如MIME Type为`application/pdf`的子类型是`pdf`. 注意，子类型和文件扩展名并不是一致--Powerpoint文件的子类型是 `vnd.mspowerpoint`.

MediaType.Suffixes
: 资源Mime 类型的可能存在的前缀 A slice of possible suffixes for the resource's MIME type.

## 方法 Methods
ByType
: 返回指定类型的页面资源 Returns the page resources of the given type.

```go
{{ .Resources.ByType "image" }}
```
Match
: 返回所有`Name`匹配指定的Glob模式([Glob模式例子](https://github.com/gobwas/glob/blob/master/readme.md))字符串的页面资源. 匹配是大小写无关的.

```go
{{ .Resources.Match "images/*" }}
```

GetMatch
: 同`match` 但是仅仅返回第一个匹配的页面资源

### 模式匹配 Pattern Matching
```go
// 使用下面模式能否发现 images/sunset.jpg
// Using Match/GetMatch to find this images/sunset.jpg ?  
.Resources.Match "images/sun*" ✅
.Resources.Match "**/sunset.jpg" ✅
.Resources.Match "images/*.jpg" ✅
.Resources.Match "**.jpg" ✅
.Resources.Match "*" 🚫
.Resources.Match "sunset.jpg" 🚫
.Resources.Match "*sunset.jpg" 🚫

```

## 页面资源 元数据 Page Resources Metadata

The page resources' metadata is managed from the corresponding page's front matter with an array/table parameter named `resources`. You can batch assign values using [wildcards](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm).
页面资源的元数据通过在页面的前端设定中的名为`resouces`的数组或表格参数来管理。
可以使用[匹配符号wildcards](https://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm)来批处理设置值。

{{% note %}}
Resources of type `page` get `Title` etc. from their own front matter.
具有`page`类型的资源从它们自己的前端设置获得`Title`等元数据
{{% /note %}}

name
: 设定`Name`的返回值.

{{% warning %}}
The methods `Match` and `GetMatch` use `Name` to match the resources.
{{%/ warning %}}

title
: 设定 `Title` 的返回值

params
: 定制的键/值对的字典.


###  例子

{{< code-toggle copy="false">}}
title: Application
date : 2018-01-25
resources :
- src : "images/sunset.jpg"
  name : "header"
- src : "documents/photo_specs.pdf"
  title : "Photo Specifications"
  params:
    icon : "photo"
- src : "documents/guide.pdf"
  title : "Instruction Guide"
- src : "documents/checklist.pdf"
  title : "Document Checklist"
- src : "documents/payment.docx"
  title : "Proof of Payment"
- src : "**.pdf"
  name : "pdf-file-:counter"
  params :
    icon : "pdf"
- src : "**.docx"
  params :
    icon : "word"
{{</ code-toggle >}}

From the example above:

- `sunset.jpg` will receive a new `Name` and can now be found with `.GetMatch "header"`.
- `documents/photo_specs.pdf` will get the `photo` icon.
- `documents/checklist.pdf`, `documents/guide.pdf` and `documents/payment.docx` will get `Title` as set by `title`.
- Every `PDF` in the bundle except `documents/photo_specs.pdf` will get the `pdf` icon.
- All `PDF` files will get a new `Name`. The `name` parameter contains a special placeholder [`:counter`](#the-counter-placeholder-in-name-and-title), so the `Name` will be `pdf-file-1`, `pdf-file-2`, `pdf-file-3`.
- Every docx in the bundle will receive the `word` icon.

{{% warning %}}
The __order matters__ --- Only the **first set** values of the `title`, `name` and `params`-**keys** will be used. Consecutive parameters will be set only for the ones not already set. In the above example, `.Params.icon` is first set to `"photo"` in `src = "documents/photo_specs.pdf"`. So that would not get overridden to `"pdf"` by the later set `src = "**.pdf"` rule.
{{%/ warning %}}

### The `:counter` placeholder in `name` and `title`

The `:counter` is a special placeholder recognized in `name` and `title` parameters `resources`.

The counter starts at 1 the first time they are used in either `name` or `title`.

For example, if a bundle has the resources `photo_specs.pdf`, `other_specs.pdf`, `guide.pdf` and `checklist.pdf`, and the front matter has specified the `resources` as:

{{< code-toggle copy="false">}}
[[resources]]
  src = "*specs.pdf"
  title = "Specification #:counter"
[[resources]]
  src = "**.pdf"
  name = "pdf-file-:counter"
{{</ code-toggle >}}

the `Name` and `Title` will be assigned to the resource files as follows:

| Resource file     | `Name`            | `Title`               |
|-------------------|-------------------|-----------------------|
| checklist.pdf     | `"pdf-file-1.pdf` | `"checklist.pdf"`     |
| guide.pdf         | `"pdf-file-2.pdf` | `"guide.pdf"`         |
| other\_specs.pdf  | `"pdf-file-3.pdf` | `"Specification #1"` |
| photo\_specs.pdf  | `"pdf-file-4.pdf` | `"Specification #2"` |
