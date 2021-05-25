---
title: 菜单模板
linktitle: 菜单模板
description: 菜单对于内容管理是强大又简单的特性，又易于操作来满足您设计需要.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [lists,sections,menus]
menu:
  docs:
    title: "如何在模板中使用菜单"
    parent: "templates"
    weight: 130
weight: 130
sections_weight: 130
draft: false
aliases: [/templates/menus/]
toc: false
---

Hugo makes no assumptions about how your rendered HTML will be
structured. Instead, it provides all of the functions you will need to be
able to build your menu however you want.
Hugo没有对站点呈现的HTML结构做任何假设, 它仅仅提供了您需要的构建菜单的所有功能.

The following is an example:
下面是菜单模板例子:

{{< code file="layouts/partials/sidebar.html" download="sidebar.html" >}}
<!-- sidebar start -->
<aside>
    <ul>
        {{ $currentPage := . }}
        {{ range .Site.Menus.main }}
            {{ if .HasChildren }}
                <li class="{{ if $currentPage.HasMenuCurrent "main" . }}active{{ end }}">
                    <a href="#">
                        {{ .Pre }}
                        <span>{{ .Name }}</span>
                    </a>
                </li>
                <ul class="sub-menu">
                    {{ range .Children }}
                        <li class="{{ if $currentPage.IsMenuCurrent "main" . }}active{{ end }}">
                            <a href="{{ .URL }}">{{ .Name }}</a>
                        </li>
                    {{ end }}
                </ul>
            {{ else }}
                <li>
                    <a href="{{ .URL }}">
                        {{ .Pre }}
                        <span>{{ .Name }}</span>
                    </a>
                </li>
            {{ end }}
        {{ end }}
        <li>
            <a href="#" target="_blank">Hardcoded Link 1</a>
        </li>
        <li>
            <a href="#" target="_blank">Hardcoded Link 2</a>
        </li>
    </ul>
</aside>
{{< /code >}}

{{% note "`absLangURL` and `relLangURL`" %}}
Use the [`absLangURL`](/functions/abslangurl) or [`relLangURL`](/functions/rellangurl) functions if your theme makes use of the [multilingual feature](/content-management/multilingual/). In contrast to `absURL` and `relURL`, these two functions add the correct language prefix to the url.

如果您的主题使用 [多语言特性](/content-management/multilingual/) 请使用[`absLangURL`](/functions/abslangurl) 或者 [`relLangURL`](/functions/rellangurl) 函数.和函数 `absURL` and `relURL`相比，这两个函数在URL的前面添加了正确的语言前缀.
{{% /note %}}

## 疏于打理的博客作者的区块菜单 Section Menu for Lazy Bloggers

为启动这个菜单, 在站点`config`中配置`sectionPagesMenu`:

```
sectionPagesMenu = "main"
```

The menu name can be anything, but take a note of what it is.
菜单名称随意,但是要记住这个名称.

This will create a menu with all the sections as menu items and all the sections' pages as "shadow-members". The _shadow_ implies that the pages isn't represented by a menu-item themselves, but this enables you to create a top-level menu like this:
这会创建一个菜单，所有的区块作为菜单的子项目, 所有的区块的页面作为"shadow-members". 
此处_shadow_意味着页面本身并没有作为菜单单元表现, 但是这允许您创建一个顶层的菜单, 类似下面这个: 
```
<nav class="sidebar-nav">
    {{ $currentPage := . }}
    {{ range .Site.Menus.main }}
    <a class="sidebar-nav-item{{if or ($currentPage.IsMenuCurrent "main" .) ($currentPage.HasMenuCurrent "main" .) }} active{{end}}" href="{{ .URL }}" title="{{ .Title }}">{{ .Name }}</a>
    {{ end }}
</nav>
```

In the above, the menu item is marked as active if on the current section's list page or on a page in that section.
上面,如果菜单单元位于当前区块的list页面或者在区块中某页面上,此单元会标记为active。


## 站点配置菜单 Site Config menus

The above is all that's needed. But if you want custom menu items, e.g. changing weight, name, or link title attribute, you can define them manually in the site config file:
上面的菜单已经足够。但是如果需要定制菜单单元,比如,改变菜单项目的顺序、名称、或者链接的标题属性等, 您可以在站点配置文件中手工定制菜单 
 
{{< code-toggle file="config" >}}
[[menu.main]]
    name = "This is the blog section"
    title = "blog section"
    weight = -110
    identifier = "blog"
    url = "/blog/"
{{</ code-toggle >}}

{{% note %}}
请注意标志符 `identifier` *必须* 和区块名称匹配.
{{% /note %}}

## 在页面前端设置中设置菜单条目 Menu Entries from the Page's front matter

It's also possible to create menu entries from the page (i.e. the `.md`-file).
从页面(`.md`文件)中创建菜单条目也是可能的.

Here is a `yaml` example:
这是`yaml`例子:

```
---
title: Menu Templates
linktitle: Menu Templates
menu:
  docs:
    title: "how to use menus in templates"
    parent: "templates"
    weight: 130
---
...
```

{{% note %}}
可以定义多个菜单.也不需要是一个复杂的值, `menu`可以是一个字符串、一个字符串数组、或者像上例中的复杂值的数组 
You can define more than one menu. It also doesn't have to be a complex value,
`menu` can also be a string, an array of strings, or an array of complex values
like in the example above.
{{% /note %}}

### 在菜单中使用.Page变量  Using .Page in Menus

If you use the front matter method of defining menu entries, you'll get access to the `.Page` variable.
This allows to use every variable that's reachable from the [page variable](/variables/page/).
如果使用前端设置中定义菜单条目的方法, 那么可以访问`.Page`变量. 这样允许您使用可以从 [page variable](/variables/page/)访问的每个变量.

This variable is only set when the menu entry is defined in the page's front matter.
Menu entries from the site config don't know anything about `.Page`.
这个变量仅在菜单条目是在页面前端设置中定义的时候设置。在站点配置文件中定义的菜单条目对`.Page`一无所知.

That's why you have to use the go template's `with` keyword or something similar in your templating language.
所以需要使用go模板的`with`关键字或者您模版语言中其他类似的功能.

Here's an example:
这是一个例子:

```
<nav class="sidebar-nav">
  {{ range .Site.Menus.main }}
    <a href="{{ .URL }}" title="{{ .Title }}">
      {{- .Name -}}
      {{- with .Page -}}
        <span class="date">
        {{- dateFormat " (2006-01-02)" .Date -}}
        </span>
      {{- end -}}
    </a>
  {{ end }}
</nav>
```

## 在菜单中使用.Params Using .Params in Menus

User-defined content on menu items are accessible via `.Params`.
菜单单元上用户定义的内容可以通过`.params`访问:

Here's an example:
这是例子:


```
<nav class="sidebar-nav">
  {{ range .Site.Menus.main }}
    <a href="{{ .URL }}" title="{{ .Title }}" class="{{ with .Params.class }}{{ . }}{{ end }}">
      {{- .Name -}}
    </a>
  {{ end }}
</nav>
```

{{% note %}}
With Menu-level .Params they can easily exist on one menu item but not another. It's recommended to access them gracefully using the [with function](/functions/with).
对于菜单界别的.Params参数, 可能在某个菜单单元上存在, 但是在另外一个菜单单元上就不存在.建议通过[with function](/functions/with)函数优雅的访问这些参数.
{{% /note %}}
