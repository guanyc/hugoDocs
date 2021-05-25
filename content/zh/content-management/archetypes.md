---
title: 原型
linktitle: 原型
description: 原型是创建新内容时使用的模板.
date: 2017-02-01
publishdate: 2017-02-01
keywords: [archetypes,generators,metadata,front matter]
categories: ["content management"]
menu:
  docs:
    parent: "content-management"
    weight: 70
  quicklinks:
weight: 70	#rem
draft: false
aliases: [/content/archetypes/]
toc: true
---

## What are Archetypes?

**Archetypes** are content template files in the [archetypes directory][] of your project that contain preconfigured [front matter][] and possibly also a content disposition for your website's [content types][]. These will be used when you run `hugo new`.

**原型** 是项目的[原型目录][archetypes directory]内的内容模板文件, 这些模板包含预定义的[前端设置][front matter], 同时也可能是您站点[内容类型][content types]的内容配置目录。在使用 `hugo new`命令创建新内容时会使用原型.


`hugo new` 命令使用`content-section`来寻找项目中最合适的原型模板。如果项目中不包含任何原型文件，命令也会查找theme主题目录中原型文件。

{{< code file="archetype-example.sh" >}}
hugo new posts/my-first-post.md
{{< /code >}}

上面命令创建了一个新的内容文件在`content/posts/my-first-post.md`，创建中使用了下面原型模板中的第一个:

1. `archetypes/posts.md`
2. `archetypes/default.md`
3. `themes/my-theme/archetypes/posts.md`
4. `themes/my-theme/archetypes/default.md`

列表中最后两个文件仅仅在您使用了主题并且创建新内容的`hugo new`命令使用的路径包含`my-theme`这个主题名称。

## 创建新原型模板

A fictional example for the section `newsletter` and the archetype file `archetypes/newsletter.md`. Create a new file in `archetypes/newsletter.md` and open it in a text editor.
下面是一个虚构的为区块 `newsletter`的原型例子, 原型文件是`archetypes/newsletter.md`.
创建一个新文件`archetypes/newsletter.md` 然后用编辑器打开它.

{{< code file="archetypes/newsletter.md" >}}
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
---

**Insert Lead paragraph here.**

## New Cool Posts

{{ range first 10 ( where .Site.RegularPages "Type" "cool" ) }}
* {{ .Title }}
{{ end }}
{{< /code >}}

当使用下面命令创建新 newsletter时:

```bash
hugo new newsletter/the-latest-cool.stuff.md
```

hugo会创建一个新的newsletter类型的内容文件，文件内容基于原型模板.

**注意:** 这个站点仅在原型文件中使用了`.Site`变量的情况下才用被构建。这在大网站时会很消耗时间.

上面的 _newsletter 类型原型_显示了可能性。全站点变量 `.Site` 和所有Hugo的模板函数可以在原型文件中使用


## 基于目录的原型

Since Hugo `0.49` you can use complete directories as archetype templates. Given this archetype directory:
从Hugo版本`0.49`开始，可以使用完整的目录作为原型模板。如下面这个原型目录:

```bash
archetypes
├── default.md
└── post-bundle
    ├── bio.md
    ├── images
    │   └── featured.jpg
    └── index.md
```

```bash
hugo new --kind post-bundle posts/my-post
```

基于上面模板目录，这个命令 会在`/content/posts/my-post`目录创建同`post-bundle`原型目录相同结构的文件集合。所有的内容文件(如`index.md`)可以包含模板逻辑, 会接收到对于内容语言正确的 `.Site` 变量.



[archetypes directory]: /getting-started/directory-structure/
[content types]: /content-management/types/
[front matter]: /content-management/front-matter/
