---
title: 菜单
linktitle: 菜单
description: Hugo拥有一个简单强大的菜单系统
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-03-31
categories: [content management]
keywords: [menus]
draft: false
menu:
  docs:
    parent: "content-management"
    weight: 120
weight: 120	#rem
aliases: [/extras/menus/]
toc: true
---

{{% note "Lazy Blogger"%}}
If all you want is a simple menu for your sections, see the ["Section Menu for Lazy Bloggers" in Menu Templates](/templates/menu-templates/#section-menu-for-lazy-bloggers).
如果您全部所需的是一个简单的菜单，请看[菜单模板中"给疏于打理的博客的区块菜单"](/templates/menu-templates/#section-menu-for-lazy-bloggers)
{{% /note %}}

使用hugo菜单您可以:

* 在一个或者多个菜单中添加条目内容
* 无限深度的嵌套菜单
* 创建不绑定于任何内容的菜单项目
* 突出显示活动的菜单项目(以及活动的分支)

## Hugo菜单是?

**菜单** 是菜单条目的数组,这个数组可以通过[`.Site.Menus` 站点变量][sitevars]使用名称来访问。比如，可以通过 `.Site.Menus.main`访问网站的`main` 菜单.

{{% note "Menus on Multilingual Sites" %}}
如果使用了[多语言支持特性multilingual feature](/content-management/multilingual/), 可以定义语言独立的菜单
{{% /note %}}

参考 [Menu Entry Properties][me-props] 获得关于全部与菜单条目有关的变量和函数.

## 菜单添加内容

Hugo允许您通过内容的[front matter](/content-management/front-matter/)来给菜单添加内容.

### 简单方式

如果您全部所需是添加菜单条目，这个简易方式做的非常不错.

#### 在单一菜单中

```
---
menu: "main"
---
```

#### 在多个菜单中

```
---
menu: ["main", "footer"]
---
```

#### 高级设置高级菜单
```
---
menu:
  docs:
    parent: 'extras'
    weight: 20
---
```

## 菜单中添加非内容条目

You can also add entries to menus that aren’t attached to a piece of content. This takes place in your Hugo project's [`config` file][config].
也可以在菜单中添加没有绑定到内容的条目。这在Hugo项目的[`配置` 文件][config]中添加。

下面是从一个配置文件中抽取的代码片段举例:
Here’s an example snippet pulled from a configuration file:

{{< code-toggle file="config" >}}
[[menu.main]]
    name = "about hugo"
    pre = "<i class='fa fa-heart'></i>"
    weight = -110
    identifier = "about"
    url = "/about/"
[[menu.main]]
    name = "getting started"
    pre = "<i class='fa fa-road'></i>"
    post = "<span class='alert'>New!</span>"
    weight = -100
    url = "/getting-started/"
{{< /code-toggle >}}

{{% note %}}
The URLs must be relative to the context root. If the `baseURL` is `https://example.com/mysite/`, then the URLs in the menu must not include the context root `mysite`. Using an absolute URL will override the baseURL. If the value used for `URL` in the above example is `https://subdomain.example.com/`, the output will be `https://subdomain.example.com`.
菜单中URLs必须相对与上下文的根。如果`baseURL`是`https://example.com/mysite/`,
那么在菜单中的URL必须不包含上下文的根`mysite`. 使用绝对URL会覆盖baseURL.
如果上例中使用的`URL`是`https://subdomain.example.com/`, 那么输出是`https://subdomain.example.com`
{{% /note %}}

## 菜单嵌套

菜单的所有嵌套通过`parent`节点.


The parent of an entry should be the identifier of another entry. The identifier should be unique (within a menu).
菜单条目的父节点应该是另一个条目的标志符号identifier. 标志符号应该唯一(在一个菜单内).

下面的顺序用来确定标志符:

`.Name > .LinkTitle > .Title`

上面的`.Title` 将被使用除非`.LinkTitle`存在, 以此类推。
实践中, `.Name` 和 `.Identifier`只用来构建菜单结构关系，因此不会被显示.

上面例子中, 菜单的顶层元素在[site `config` file][config]中定义。
所有的内容条目通过`.Parent`和在这些顶层条目中的一个绑定。

## 参数

可以通过`params`域给菜单条目添加用户定义内容.

常见的用例是定义一个参数添加css类给特定菜单条目.

{{< code-toggle file="config" >}}
[[menu.main]]
    name = "about hugo"
    pre = "<i class='fa fa-heart'></i>"
    weight = -110
    identifier = "about"
    url = "/about/"
    [menu.main.params]
      class = "highlight-menu-item"
{{</ code-toggle >}}


## 菜单显示 Render Menus

参考 [Menu Templates](/templates/menu-templates/)获得如何在模板中显示网站菜单的信息.

[config]: /getting-started/configuration/
[multilingual]: /content-management/multilingual/
[sitevars]: /variables/
[me-props]: /variables/menus/
