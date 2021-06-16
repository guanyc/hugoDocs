---
title: 基础模板和区块
linktitle: 基础模板
description: 基础模板和区块构造允许您定义主模板的外壳(页面的镶嵌结构).
godocref: https://golang.org/pkg/text/template/#example_Template_block
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates,fundamentals]
keywords: [blocks,base]
menu:
  docs:
    parent: "templates"
    weight: 20
weight: 20
sections_weight: 20
draft: false
aliases: [/templates/blocks/,/templates/base-templates-and-blocks/]
toc: true
---

`区块`关键词允许您定义页面的一个或者多个主模板的外壳,然后在需要时填入或者重载.

{{< youtube QVOMCYitLEc >}}

## 基础模板查询顺序

{{< new-in "0.63.0" >}} 从Hugo版本V0.63开始, 基础模板的查询顺序紧随其适用的模板. (比如 `_default/list.html`).

参考 [模板查询顺序](/templates/lookup-order/) 获得更多细节和例子的信息.

## 定义基础模板

下面例子定义了一个简单的基础模板`_default/baseof.html`. 作为一个缺省模板, 它是所有页面呈现的外壳，除非重新指明了接近模板查询顺序开始的另一个`*baseof.html`.

{{< code file="layouts/_default/baseof.html" download="baseof.html" >}}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ block "title" . }}
      <!-- 区块可以包含缺省内容  -->
      {{ .Site.Title }}
    {{ end }}</title>
  </head>
  <body>
    <!-- 所有模板都分享的代码, 比如一个头部部分 -->

    {{ block "main" . }}
      <!-- 不同模板开始不同的页面的部分  -->
    {{ end }}

    {{ block "footer" . }}
    <!-- 更多共享代码， 可能是页脚，但是也可以被重载如果有需要-->
    {{ end }}
  </body>
</html>
{{< /code >}}

## 重载基础模板

基于上面的基础模板, 可以定义[默认的list模板][hugolists]. 默认的list模板从基础模板中继承所有定义在 `"main"` 之前的代码, 然后实现自己的`"main"` 区块:

{{< code file="layouts/_default/list.html" download="list.html" >}}
{{ define "main" }}
  <h1>Posts</h1>
  {{ range .Pages }}
    <article>
      <h2>{{ .Title }}</h2>
      {{ .Content }}
    </article>
  {{ end }}
{{ end }}
{{< /code >}}

这样将我们的 "main"(基本为空白)区块内容替换为对list模板有意义的内容。这里, 我们并没有定义`"title"` 区块, 所以从基础模板中的内容在lists中仍然不变.

{{% warning %}}
在区块定义外面的代码 *能够* 破坏布局. 即使是HTML注释，也是如此:

```
<!-- 看起来有害的HTML注释..可能会在构建时破坏布局 -->
{{ define "main" }}
...your code here
{{ end }}
```
[更多信息请见 Hugo 论坛讨论.](https://discourse.gohugo.io/t/baseof-html-block-templates-and-list-types-results-in-empty-pages/5612/6)
{{% /warning %}}

下面[默认的单页模板][singletemplate]例子中显示如何重载基础模板中的 `"main"` 和 `"title"`区块, 并且应用了针对[默认的单页模板][singletemplate]特别的代码:

{{< code file="layouts/_default/single.html" download="single.html" >}}
{{ define "title" }}
  <!-- 这里覆盖了 baseof.html 中的默认值 "{{.Site.Title}}"-在一开始的例子中的 -->
  {{ .Title }} &ndash; {{ .Site.Title }}
{{ end }}
{{ define "main" }}
  <h1>{{ .Title }}</h1>
  {{ .Content }}
{{ end }}
{{< /code >}}

[hugolists]: /templates/lists
[lookup]: /templates/lookup-order/
[rendering the section]: /templates/section-templates/
[singletemplate]: /templates/single-page-templates/
