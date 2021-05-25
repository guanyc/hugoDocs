---
title: 区块页模版
linktitle: 区块页模版
description: 区块页的模版是list, 可以访问list页面的所有变量和方法
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [lists,sections,templates]
menu:
  docs:
    parent: "templates"
    weight: 40
weight: 40
sections_weight: 40
draft: false
aliases: [/templates/sections/]
toc: true
---

## 为区块模版添加内容和前端设定

为有效使用区块页模版, 建议您先理解hugo的[内容组织](/content-management/organization/), 特别的，理解
`_index.md`文件在给section和其他list页面添加内容和前端设置时的作用。

## 区块模版查询顺序

参考模版查询顺序 [Template Lookup](/templates/lookup-order/).

## 页面类型 Page Kinds

Every `Page` in Hugo has a `.Kind` attribute.
Hugo中每个`Page`具有`.Kind`属性

<!--
{{% page-kinds %}}
-->

| 类型 Kind | 描述 Description | 例子 Example |
|----------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------|
| `home` | 首页的登陆页 The landing page for the home page | `/index.html` |
| `page` | 指定页面的登陆页 The landing page for a given page | `my-post` page (`/posts/my-post/index.html`) |
| `section` | 指定区块的登陆页 The landing page of a given section | `posts` section (`/posts/index.html`) |
| `taxonomy` | 标签列表的登陆页 The landing page for a taxonomy | `tags` taxonomy (`/tags/index.html`) |
| `term` | 标签条目列表的登录页 The landing page for one taxonomy's term | term `awesome` in `tags` taxonomy (`/tags/awesome/index.html`) |


## 在区块内使用`.Site.GetPage`

`Kind` can easily be combined with the [`where` function][where] in your templates to create kind-specific lists of content. This method is ideal for creating lists, but there are times where you may want to fetch just the index page of a single section via the section's path.
`kind`类型可以在模版内很容易的和[`where` 函数][where]配合创建类型特定的内容列表。 对于创建list这个方法很理想, 不过有时您可能想通过区块的路径获取仅仅一个单独区块的index页面。

The [`.GetPage` function][getpage] looks up an index page of a given `Kind` and `path`.
函数 [`.GetPage` ][getpage]查询指定`Kind`和`path`的索引页。

You can call `.Site.GetPage` with two arguments: `kind` (one of the valid values
of `Kind` from above) and `kind value`.
可以使用两个参数`kind`类型 (上表的有效值中选一个) 和 `kind value` 类型值调用`.Site.GetPage`函数


举例:

- `{{ .Site.GetPage "section" "posts" }}`
- `{{ .Site.GetPage "page" "search" }}`

## 例子: 创建默认的区块模版

{{< code file="layouts/_default/section.html" download="section.html" >}}
{{ define "main" }}
  <main>
      {{ .Content }}
          <ul class="contents">
          {{ range .Paginator.Pages }}
              <li>{{.Title}}
                  <div>
                    {{ partial "summary.html" . }}
                  </div>
              </li>
          {{ end }}
          </ul>
      {{ partial "pagination.html" . }}
  </main>
{{ end }}
{{< /code >}}

### 例子: 使用 `.Site.GetPage`

下面的 `.Site.GetPage`例子假设项目具有如下的目录结构:

```
.
└── content
    ├── blog
    │   ├── _index.md # "title: My Hugo Blog" in the front matter
    │   ├── post-1.md
    │   ├── post-2.md
    │   └── post-3.md
    └── events #Note there is no _index.md file in "events"
        ├── event-1.md
        └── event-2.md
```

`.Site.GetPage` will return `nil` if no `_index.md` page is found. Therefore, if `content/blog/_index.md` does not exist, the template will output the section name:
如果没找到 `_index.md` 页面, `.Site.GetPage` 会返回`nil`. 因此，如果`content/blog/_index.md`不存在,
模版会输出区块名称.

```
<h1>{{ with .Site.GetPage "section" "blog" }}{{ .Title }}{{ end }}</h1>
```

Since `blog` has a section index page with front matter at `content/blog/_index.md`, the above code will return the following result:
由于`blog`区块具有索引页面`content/blog/_index.md`并且有前端设置，上面例子会输出如下结果:

```
<h1>My Hugo Blog</h1>
```

If we try the same code with the `events` section, however, Hugo will default to the section title because there is no `content/events/_index.md` from which to pull content and front matter:
如果对`events`区块应用同样代码，由于没有 `content/events/_index.md`,无法读取内容和前端设置,Hugo会使用区块标题的默认值:


```
<h1>{{ with .Site.GetPage "section" "events" }}{{ .Title }}{{ end }}</h1>
```

这个代码会返回如下输出:

```
<h1>Events</h1>
```


[contentorg]: /content-management/organization/
[getpage]: /functions/getpage/
[lists]: /templates/lists/
[lookup]: /templates/lookup-order/
[where]: /functions/where/
[sections]: /content-management/sections/
