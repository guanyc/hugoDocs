---
title: 内容类型
description: Hugo is built around content organized in sections.
date: 2017-02-01
categories: [content management]
keywords: [lists,sections,content types,types,organization]
menu:
  docs:
    parent: "content-management"
    weight: 60
weight: 60	#rem
draft: false
aliases: [/content/types]
toc: true
---

A **content type** is a way to organize your content. Hugo resolves the content type from either the `type` in front matter or, if not set, the first directory in the file path. E.g. `content/blog/my-first-event.md` will be of type `blog` if no `type` set.
**内容类型**是组织内容的方式. hugo从前端设定中的`type`解析内容类型, 如果`type`没设定, 选择文件路径的第一个目录作为内容乐行。比如`content/blog/my-first-event.md`的类型会是`blog`如果`type`未设定.

A content type is used to

* Determine how the content is rendered. See [Template Lookup Order](/templates/lookup-order/) and [Content Views](https://gohugo.io/templates/views) for more.
* Determine which [archetype](/content-management/archetypes/) template to use for new content.

内容类型被用于:

* 决定内容如何显示. 更多信息参考 [Template Lookup Order](/templates/lookup-order/) 和 [Content Views](https://gohugo.io/templates/views).
* 决定对新内容使用哪一个[archetype](/content-management/archetypes/) 模板.
