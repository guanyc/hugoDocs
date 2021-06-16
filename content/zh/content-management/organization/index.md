---
title: 内容组织
linktitle: 组织
description: Hugo假设, 能够处理好站点源内容的目录架构同样适合于生成的站点
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [content management,fundamentals]
keywords: [sections,content,organization,bundle,resources]
menu:
  docs:
    parent: "content-management"
    weight: 10
weight: 10	#rem
draft: false
aliases: [/content/sections/]
toc: true
---

## 网页包裹 Page Bundles

Hugo `0.32`版本宣布支持 相对于网页的图像和其他资源打包成`网页包裹`形式


这些条目是相互关联的，您也需要阅读 [网页资源 Page Resources]({{< relref "/content-management/page-resources" >}}) 和 [图像处理 Image Processing]({{< relref "/content-management/image-processing" >}}) 来获得全面的理解。

{{< imgproc 1-featured Resize "300x" >}}
上图描述了3个包裹。注意主页包裹不能包含其他内容页面，但是可以包含其他文件(如图像)。
{{< /imgproc >}}


{{% note %}}
本文档编写还在**进行中**. 我们会尽快发布关于组织的更详细文档。
{{% /note %}}


# 内容源码的组织

Hugo中, 网站内容的组织方式应该以某种方式映射生成的网站的架构

虽然hugo支持在任何目录层嵌套内容, 但是最顶层(也就是 `content/<DIRECTORIES>`)在Hugo中是特殊的, 被视为内容类型并且决定了内容的布局。更多区块信息,包括它们如何嵌套,参考[区块][sections]。


不用任何附加的配置条件, 下面架构可以很好工作:

```
.
└── content
    └── about
    |   └── index.md  // <- https://example.com/about/
    ├── posts
    |   ├── firstpost.md   // <- https://example.com/posts/firstpost/
    |   ├── happy
    |   |   └── ness.md  // <- https://example.com/posts/happy/ness/
    |   └── secondpost.md  // <- https://example.com/posts/secondpost/
    └── quote
        ├── first.md       // <- https://example.com/quote/first/
        └── second.md      // <- https://example.com/quote/second/
```

## Hugo中路径解析

下面展示内容架构和生成的hugo网站的输出URL结构之间的关系。例子中假设您[使用了漂亮的 URLs][pretty],这是hugo默认的行为。这个例子里假设在您的 [站点配置文件][config]中具有一个键值对`baseURL = "https://example.com"`.

### 索引页面: `_index.md`


`_index.md`索引页面在Hugo内容中是个特殊角色。它允许您在[列表模板][lists]中添加前置设置和内容。这些列表模板包括[区块模板][section templates], [tag模板][taxonomy templates],[tag列表模板][taxonomy terms templates]和您的[主页模板][homepage template]。

{{% note %}}
**提示:** 在 `_index.md` 中可以通过[`.Site.GetPage` function](/functions/getpage/)获得对内容和元数据的引用.
{{% /note %}}

您可以在主页处保存有 `_index.md` 索引文件, 在每个内容区块、tag和tag列表中保存有 `_index.md`. 下图显示了hugo站点中包含内容和前置设置的`posts`区块列表页的`_index.md`典型的位置:

```
.         url
.       ⊢--^-⊣
.        path    slug
.       ⊢--^-⊣⊢---^---⊣
.           filepath
.       ⊢------^------⊣
content/posts/_index.md
```
构建生成的时候会输出下面的具有关联值的目标页面:

```

                     url ("/posts/")
                    ⊢-^-⊣
       baseurl      section ("posts")
⊢--------^---------⊣⊢-^-⊣
        permalink
⊢----------^-------------⊣
https://example.com/posts/index.html
```

区块[sections][]可以嵌套。需要理解的最重要部分一点是，为了区块可以自由的全链路导航，至少要在最下层区块部分有一个内容文件(比如， `_index.md`).


### 区块中独立页面

每个区块中的单独的内容文件将会被显示为 [单独页模板][singles]。下面是`posts`区块里面一个`post`页面的例子:

```
                   path ("posts/my-first-hugo-post.md")
.       ⊢-----------^------------⊣
.      section        slug
.       ⊢-^-⊣⊢--------^----------⊣
content/posts/my-first-hugo-post.md
```

Hugo 构建站点时, 此内容会被输出成如下目标:

```

                               url ("/posts/my-first-hugo-post/")
                   ⊢------------^----------⊣
       baseurl     section     slug
⊢--------^--------⊣⊢-^--⊣⊢-------^---------⊣
                 permalink
⊢--------------------^---------------------⊣
https://example.com/posts/my-first-hugo-post/index.html
```


## 路径详解


请查看下面的概念，它们会提供给您关于您项目结构和Hugo构建输出站点的默认行为之间关系的更多的细节。

### `区块` `section`


默认的内容类型由内容区块决定。`区块`由其在项目`content`目录中的位置决定。
`section` *不能*在前言设定中被声明或者重载。

### `slug`

内容的`slug`是`name.extension`或者是`name/`. slug的值由下面确定:

* 内容文件的名称 (如 `lollapalooza.md`) 或者
* 前言设定的重载设定

### `path`

内容的`path` 由区块到达文件的路径决定。文件路径

* 由文件相对于内容的位置确定，并且
* 不包含slug

### `url`

`url`指的是一条内容的相对URL。 `url`

* 基于内容在目录结构中位置 或者
* 由前言设定或者*重载上面的*值确定

## 使用前言设定front matter重载目标路径

Hugo相信客户是带着目的组织自己的内容的。组织源内容的机构也同样用于组织生成的站点。如上面展示，内容源码的结构映射到目的网站内容上。

有时候需要对内容的更多控制。这样需求可以通过在前言设定中声明参数来实现，声明参数来决定特定内容的最终访问地址。

下面参数是按特定原因排序的: 更后面的参数设定可以覆盖前面的参数设定，不是所有都可以在前言设定中设定:


### `filename`

这个不在front matter中设定，是实际文件名称除去后缀。这和文件在最后访问地址一样(比如，
`content/posts/my-post.md` 变成 `example.com/posts/my-post/`)

### `slug`

在front matter里面设定，`slug`替换了访问地址中的文件名称。

{{< code file="content/posts/old-post.md" >}}
---
title: A new post with the filename old-post.md
slug: "new-post"
---
{{< /code >}}

这样的配置默认会通过下面地址访问上面内容文件。

```
example.com/posts/new-post/
```

### `section`

`section`由内容在磁盘上位置目录决定，*不能*通过front matter设定声明。更多信息参考 [sections][]

### `type`

内容的`type`类型也通过磁盘位置确定，但是和 `section`不同， 它`能够`在front matter 中声明。参考[types][]。这对于想使用不同布局显示的一些内容会特别灵巧。下面例子中，你创建了一个位于 `layouts/new/mylayout.html` 的布局, hugo会用来显示内容，甚至在很多其他posts中。

{{< code file="content/posts/my-post.md" >}}
---
title: My Post
type: new
layout: mylayout
---
{{< /code >}}
<!-- See https://discourse.gohugo.io/t/path-not-works/6387 -->
<!-- ### `path`-->

<!--`path` can be provided in the front matter. This will replace the actual path to the file on disk. Destination will create the destination with the same path, including the section. -->

### `url`

也可以提供完整的URL, 由于和最终目标地址有关，导致会覆盖上面所有的设置。必须是从baseURL(以`/`开始)开始的路径。在front matter中设置的`url`会被完整使用，并且会忽略站点配置中的`--uglyURLs`设置:

{{< code file="content/posts/old-url.md" >}}
---
title: Old URL
url: /blog/new-url/
---
{{< /code >}}

假设您的`baseURL`[配置][config]是`https://example.com`, 上面在front matter中添加的`url`配置
会使得内容文件`content/posts/old-url.md`输出到如下的访问地址:

```
https://example.com/blog/new-url/
```

更多如何控制输出路径的信息请参考[URL 管理][urls].

[config]: /getting-started/configuration/
[formats]: /content-management/formats/
[front matter]: /content-management/front-matter/
[getpage]: /functions/getpage/
[homepage template]: /templates/homepage/
[homepage]: /templates/homepage/
[lists]: /templates/lists/
[pretty]: /content-management/urls/#pretty-urls
[section templates]: /templates/section-templates/
[sections]: /content-management/sections/
[singles]: /templates/single-page-templates/
[taxonomy templates]: /templates/taxonomy-templates/
[taxonomy terms templates]: /templates/taxonomy-templates/
[types]: /content-management/types/
[urls]: /content-management/urls/
