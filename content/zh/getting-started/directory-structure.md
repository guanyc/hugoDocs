---
title: 目录结构
linktitle: 目录结构
description: Hugo的命令行建立了项目目录的脚手架结构，然后从此目录开始输入创建完整的站点.
date: 2017-01-02
publishdate: 2017-02-01
lastmod: 2017-03-09
categories: [getting started,fundamentals]
keywords: [source, organization, directories]
menu:
  docs:
    parent: "getting-started"
    weight: 50
weight: 50
sections_weight: 50
draft: false
aliases: [/overview/source-directory/]
toc: true
---

## 新站点的脚手架结构

{{< youtube sB0HLHjgQ7E >}}

在命令行运行 `hugo new site` 生成器命令会创建一个目录结构，目录内包含如下元素:

```
.
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```


## 目录结构说明

以下部分是每个目录的高层概述，以及在Hugo文档中它们相应部分的链接。

原型 [`archetypes`](/content-management/archetypes/)
: 在Hugo中使用`hugo new`命令可以创建新的内容文件. 默认情况下，Hugo创建的新的内容文件至少设置了 `date`, `title` (从文件名推断出来), 和 `draft = true`. 这节省了时间，而且对于使用多种内容类型的站点来说提升了一致性。可以创建自己的 [原型][archetypes], 定制预先配置的前端设置.

资源[`assets`][]
: 保存将被[Hugo Pipes]({{< ref "/hugo-pipes" >}})处理的所有文件。 只有`.Permalink` 和 `.RelPermalink` 被使用的文件会被发布到 `public` 目录。 注意: 默认并没有创建资源目录。

配置[`config`](/getting-started/configuration/)
: Hugo配备了大量的[配置指令](https://gohugo.io/getting-started/configuration/#all-variables-yaml)。 [配置目录](/getting-started/configuration/#configuration-directory)是JSON,YAML或者TOML格式的指令文件的存储目录。每个根设置对象可以作为独立文件和环境结构存在。最少设置并且对环境没有要求的项目可以使用单独的 `config.toml` 文件作为根设置文件。

很多站点可能需要很少配置，或者不需要配置。不过Hugo带来了大量的[配置指令][configuration directives]，提供更详细的指令来满足构建站点的要求。注意: 默认并没有创建配置目录

内容[`content`][]
: 此目录用于包含网站的所有内容。每个Hugo顶层目录被视为一个[内容部分][content section].比如，如果您的网站包含三个主要部分-- `blog`, `articles` 和 `tutorials`---那么在您会在content下面包含三个目录 `content/blog`, `content/articles`和 `content/tutorials`.Hugo使用内容部分分配默认的[内容类型][content types]

数据[`data`](/templates/data-templates/)
: 此目录用于保存Hugo生成站点时所用的配置文件。这些文件可以使用YAML, JSON 或者 TOML 格式编写. 除了添加到这个目录的文件以外, 还可以创建从动态内容获取数据的[数据模板][data templates][].

布局 [`layouts`][]
: 此目录存储`.html`文件作为布局模板，这些模板声明了内容视图在静态站点的呈现。包含的模板有[列表页][lists], 您的 [主页][homepage], [标签模板][taxonomy templates], [部分页面模板][partials], [单独页模板][singles]


静态资源[`static`][]
: 此目录用于存放静态内容:图片、CSS、javascript等。 当构建站点时，这些静态目录内资源会被原封不动copy过去。一个好的使用`static`目录的例子是[在谷歌查询界面中验证网站所有权][searchconsole]的操作,您想Hugo不修改内容，直接copy完整的HTML文件。


{{% note %}}
从 **Hugo 0.31** 版本开始，可以有多个静态资源目录
{{% /note %}}

资源 resources
: 加速构建的缓存文件。也可以为模板作者分发构建的SASS文件使用, 由此不用安装hugo预处理器。注意: 默认不创建这个目录。


[archetypes]: /content-management/archetypes/
[configuration directives]: /getting-started/configuration/#all-variables-yaml
[`content`]: /content-management/organization/
[content section]: /content-management/sections/
[content types]: /content-management/types/
[data templates]: /templates/data-templates/
[homepage]: /templates/homepage/
[`layouts`]: /templates/
[`static`]: /content-management/static-files/
[lists]: /templates/list/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[searchconsole]: https://support.google.com/analytics/answer/1142414?hl=en
[singles]: /templates/single-page-templates/
[starters]: /tools/starter-kits/
[taxonomies]: /content-management/taxonomies/
[taxonomy templates]: /templates/taxonomy-templates/
[types]: /content-management/types/
[`assets`]: {{< ref "/hugo-pipes/introduction#asset-directory" >}}
