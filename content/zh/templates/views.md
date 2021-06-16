---
title: 视图模板
# linktitle: Content Views
description: Hugo可以展示内容的不同视图. 这在list视图和摘要视图中非常有用.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [views]
menu:
  docs:
    parent: "templates"
    weight: 70
weight: 70
sections_weight: 70
draft: false
aliases: []
toc: true
---

These alternative **content views** are especially useful in [list templates][lists].
这些可选择的 **内容视图**在[list templates][lists]中特别有用.

下面是一些内容视图的常见用例:

* 在首页显示所有类型的内容,但是仅仅显示受限的[summary views][summaries]..
* 在[taxonomy list page][taxonomylists]中显示内容符号列表.
  这里使用视图直截了当, 每个内容类型的呈现工作由内容本身代理完成.

## 创建视图 Create a Content View

To create a new view, create a template in each of your different content type directories with the view name. The following example contains an "li" view and a "summary" view for the `posts` and `project` content types. As you can see, these sit next to the [single content view][single] template, `single.html`. You can even provide a specific view for a given type and continue to use the `_default/single.html` for the primary view.
为创建视图，在每个不同的内容类型目录中创建一个以视图为名的模板。下面的例子中 `posts` 和 `project` 内容类型各自包含了"li" 视图 和 "summary" 视图. 您可能注意到，这两个文件位于[单独视图][single] 模板`single.html`的旁边--您甚至可以为特定类型创建特定的视图，对主要的视图继续使用 `_default/single.html`。

```
  ▾ layouts/
    ▾ posts/
        li.html
        single.html
        summary.html
    ▾ project/
        li.html
        single.html
        summary.html
```

Hugo also has support for a default content template to be used in the event that a specific content view template has not been provided for that type. Content views can also be defined in the `_default` directory and will work the same as list and single templates who eventually trickle down to the `_default` directory as a matter of the lookup order.
如果特定类型的特定的内容模板未提供的话, Hugo也支持使用默认的内容模板. 视图也可以定义在`_default` 目录，同列表模板和单独模板一样，在模板解析查询顺序的最后会定位到`_default`目录使用视图模板。

```
▾ layouts/
  ▾ _default/
      li.html
      single.html
      summary.html
```

## 那个模板会会被选中呈现?

下面是视图的解析查询顺序:

1. `/layouts/<TYPE>/<VIEW>.html`
2. `/layouts/_default/<VIEW>.html`
3. `/themes/<THEME>/layouts/<TYPE>/<VIEW>.html`
4. `/themes/<THEME>/layouts/_default/<VIEW>.html`

## 例子: List中的视图

下面例子展示了如何在[list templates][lists]中使用视图.

### `list.html`

In this example, `.Render` is passed into the template to call the [render function][render]. `.Render` is a special function that instructs content to render itself with the view template provided as the first argument. In this case, the template is going to render the `summary.html` view that follows:
本例中的模板中使用了 `.Render` 来调用[render function][render]. `.Render`是一个特殊函数, 指示内容使用函数第一个参数提供的视图模板来显示自己. 此处, 模板会呈现如下的内容的`summary.html`:

{{< code file="layouts/_default/list.html" download="list.html" >}}
<main id="main">
  <div>
  <h1 id="title">{{ .Title }}</h1>
  {{ range .Pages }}
    {{ .Render "summary"}}
  {{ end }}
  </div>
</main>
{{< /code >}}

### `summary.html`

Hugo会传递整个page对象给下面的 `summary.html`视图模板(参考[Page Variables][pagevars]中全部变量列表)

{{< code file="layouts/_default/summary.html" download="summary.html" >}}
<article class="post">
  <header>
    <h2><a href='{{ .Permalink }}'> {{ .Title }}</a> </h2>
    <div class="post-meta">{{ .Date.Format "Mon, Jan 2, 2006" }} - {{ .FuzzyWordCount }} Words </div>
  </header>
  {{ .Summary }}
  <footer>
  <a href='{{ .Permalink }}'><nobr>Read more →</nobr></a>
  </footer>
</article>
{{< /code >}}

### `li.html`

Continuing on the previous example, we can change our render function to use a smaller `li.html` view by changing the argument in the call to the `.Render` function (i.e., `{{ .Render "li" }}`).
继续上面例子, 我们可以改变调用.render函数的提供的参数来改变呈现的函数, 使用一个小型的`li.html` 视图(比如`{{ .Render "li" }}`).

{{< code file="layouts/_default/li.html" download="li.html" >}}
<li>
  <a href="{{ .Permalink }}">{{ .Title }}</a>
  <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
</li>
{{< /code >}}

[lists]: /templates/lists/
[lookup]: /templates/lookup-order/
[pagevars]: /variables/page/
[render]: /functions/render/
[single]: /templates/single-page-templates/
[spf]: https://spf13.com
[spfsourceli]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/li.html
[spfsourcesection]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/section.html
[spfsourcesummary]: https://github.com/spf13/spf13.com/blob/master/layouts/_default/summary.html
[summaries]: /content-management/summaries/
[taxonomylists]: /templates/taxonomy-templates/
