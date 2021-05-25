---
title: Hugo模板导引
linktitle: 导引
description: Hugo使用Go 语言的 `html/template` 和 `text/template` 模板库作为Hugo模板的基础.
godocref: https://golang.org/pkg/html/template/
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-25
categories: [templates,fundamentals]
keywords: [go]
menu:
  docs:
    parent: "templates"
    weight: 10
weight: 10
sections_weight: 10
draft: false
aliases: [/layouts/introduction/,/layout/introduction/, /templates/go-templates/]
toc: true
---

{{% note %}}
下面是Go模板的入门介绍。深入介绍请参考
Go模板的官方文档[Go docs](https://golang.org/pkg/text/template/).
{{% /note %}}

Go模板提供了一个极端简单的模板语言, Go模板的信仰是只有最基本的逻辑应该出现在模板或视图中.

## 基本模式

Go模板是HTML文件，添加了[变量 variables][variables] 和 [函数 functions][functions]
Go模板的变量和函数在`{{ }}`访问

### 访问预定义变量

_预定义变量_ 可以是在当前范围内(如后面部分[Variables]({{< relref
"#variables" >}}例子中的`.Title`))已经存在的变量,
或者是定制变量(如后面同样部分的`$address` 例子)

```go-html-template
{{ .Title }}
{{ $address }}
```

函数的多个参数以空格分隔, 一般模式如:

```
{{ FUNCTION ARG1 ARG2 .. }}
```

下面的例子调用了函数`add`, 使用了输入`1`和`2`:

```go-html-template
{{ add 1 2 }}
```

#### 方法和字段通过点表示法访问

访问页面内容前端设定[front matter][]中定义的页面参数`bar`:

```go-html-template
{{ .Params.bar }}
```

#### 括号可以用来组合项目单元在一起

```go-html-template
{{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}
```

## 变量 Variables {#variables}

每个Go模板具有一个数据对象。在Hugo中，每个模板被传递了一个`Page`对象.
下面例子中，`.Title`是可以在 [`Page` variable][pagevars]页面变量中访问的元素中的一个.

以`页面`为模板的默认范围,`Title`元素在当前上下文中(`.` -- "the **dot**")
可以简单的使用点前缀表示法访问(`.Title`):


```go-html-template
<title>{{ .Title }}</title>
```

参数值也可以保存在定制变量中, 后面可以引用:

{{% note %}}
定制的变量需要使用 `$` 前缀.
{{% /note %}}

```go-html-template
{{ $address := "123 Main St." }}
{{ $address }}
```

{{% warning %}}
Hugo V0.47以及更早版本, 在If条件语句和类似语句内定义的变量对外面不可见。
更多信息参考[https://github.com/golang/go/issues/10608](https://github.com/golang/go/issues/10608).

对这个问题，Hugo创建了解决方法---使用[Scratch](/functions/scratch).
{{% /warning %}}

Hugo V0.48版本以后, 变量可以使用`=`操作符号重新定义.(`=` 是Go语言1.11中出现的新特性).


下面例子只会在Hugo新版本中工作. 例子在主页会打印输出 "Var is Hugo Home",
在所有其他页面打印输出 "Var is Hugo Page"

```go-html-template
{{ $var := "Hugo Page" }}
{{ if .IsHome }}
    {{ $var = "Hugo Home" }}
{{ end }}
Var is {{ $var }}
```

## 函数

Go Templates only ship with a few basic functions but also provide a mechanism for applications to extend the original set.
Go语言模板仅仅自带了几个基础函数, 但是给应用提供了扩展原始函数集合的机制。

[Hugo 模板函数][functions] 特别提供了针对构建网络站点的额外的功能.
通过使用名称调用函数, 后面跟随几个空格符分隔的需要的参数。
模板函数无法添加，除非重新编译Hugo.



### 例子1: 加法

```go-html-template
{{ add 1 2 }}
<!-- prints 3 -->
```

### 例子2: 比较数字大小

```go-html-template
{{ lt 1 2 }}
<!-- prints true (i.e., since 1 is less than 2) -->
```

注意两个例子都使用了Go模板的[数学函数][math functions].

{{% note "Additional Boolean Operators" %}}
在Hugo文档列出的之外，更多的boolean操作符号请参考[Go模板文档](https://golang.org/pkg/text/template/#hdr-Functions).
{{% /note %}}

## 包含 Includes

当一个模板包含另一个模板时, 需要传递另一个模板需要访问的数据.

{{% note %}}
传递当前上下文的的数据，请记住在结尾加上一个dot **.**  .
{{% /note %}}

Hugo内模板所在位置总是从 `layouts/` 目录开始.

### 部分函数 Partial

The [`partial`][partials] function is used to include *partial* templates using
the syntax `{{ partial "<PATH>/<PARTIAL>.<EXTENSION>" . }}`.
函数 [`partial`][partials] 用于包含 *部分 partial* 模板,
调用格式是 `{{ partial "<PATH>/<PARTIAL>.<EXTENSION>" . }}`

例如, 包含一个部分模板 `layouts/partials/header.html` :

```go-html-template
{{ partial "header.html" . }}
```

### 模板函数 Template

The `template` function was used to include *partial* templates
in much older Hugo versions. Now it's useful only for calling
[*internal* templates][internal_templates]. The syntax is `{{ template
"_internal/<TEMPLATE>.<EXTENSION>" . }}`.
在很旧的Hugo版本中，`template`函数用于包含 *部分partial* templates.
现在仅仅用于调用[*内部模板internal* templates][internal_templates].
调用形式是`{{ template "_internal/<TEMPLATE>.<EXTENSION>" . }}`.

{{% note %}}
The available **internal** templates can be found
[here](https://github.com/gohugoio/hugo/tree/master/tpl/tplimpl/embedded/templates).

可以调用的 **内部** 模板,请参考[这里](https://github.com/gohugoio/hugo/tree/master/tpl/tplimpl/embedded/templates).

{{% /note %}}

包含内部 `opengraph.html`模板的例子:

```go-html-template
{{ template "_internal/opengraph.html" . }}
```

## 逻辑函数

Go 模板提供了最基本的迭代和条件逻辑.

### 迭代

Go 模板重度使用'range'函数迭代 _map_, _array_, or _slice_.
下面是如何使用`range`的几个不同例子.

#### 例子1: 使用上下文 (`.`)

```go-html-template
{{ range $array }}
    {{ . }} <!-- dot 点 表示数组 $array 中一个元素-->
{{ end }}
```

#### 例子2: 为数组元素值声明一个变量

```go-html-template
{{ range $elem_val := $array }}
    {{ $elem_val }}
{{ end }}
```

#### 例子3: 为数组元素的索引和值声明变量名称

对于数组或者数组切片, 第一个声明的变量会映射到每个元素的索引.

```go-html-template
{{ range $elem_index, $elem_val := $array }}
   {{ $elem_index }} -- {{ $elem_val }}
{{ end }}
```

#### 例子4: 为Map元素的键 _和_ 值声明变量名称

对于字典Map，第一个声明的变量会映射到map元素的键.

```go-html-template
{{ range $elem_key, $elem_val := $map }}
   {{ $elem_key }} -- {{ $elem_val }}
{{ end }}
```

#### 例子5: 空 _map_, _array_, 或者 _slice_的逻辑条件判断.

如果传递给range的 _map_, _array_, 或者 _slice_ 为空, 那么else语句会被执行。

```go-html-template
{{ range $array }}
    {{ . }}
{{else}}
    <!-- 这里仅仅在$array是空时 会执行 -->
{{ end }}
```

### 条件逻辑 Conditionals

`if`, `else`, `with`, `or`, and `and` provide the framework for handling conditional logic in Go Templates. Like `range`, each statement is closed with an `{{ end }}`.
Go语言模板中的`if`, `else`, `with`, `or`, and `and`提供了处理条件逻辑的框架.
类似`range`, 每个语句使用{{ end }}闭合。

Go模板中认为下列值为 **false**:

- `false` (boolean)
- 0 (integer)
- 任何零长度的数组、切片、字典、和字符串

#### 例子1: `with`

使用`with`编写"如果一些事情存在, 这样做" 之类的语句很常见.

{{% note %}}
`with` 重新绑定了上下文 `.` 在它的范围内(同`range`中一样).
{{% /note %}}

It skips the block if the variable is absent, or if it evaluates to
"false" as explained above.
如果变量不存在，或者评估为上面解释中的"false", 那么会跳过代码块

```go-html-template
{{ with .Params.title }}
    <h4>{{ . }}</h4>
{{ end }}
```

#### 例子2: `with` .. `else`

下面的代码片段中，如果内容的前端设定设置了 "description"，就使用 "description"的值，
否则使用默认的 `.Summary` [页面变量][pagevars]:


```go-html-template
{{ with .Param "description" }}
    {{ . }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

See the [`.Param` function][param].

#### 例子3: `if`

编写处理 `with` 逻辑的另一个种选择(更冗长)是使用`if`. 这里，`.`并未被重新绑定.

下面是用`if` 重写的 "例子1":

```go-html-template
{{ if isset .Params "title" }}
    <h4>{{ index .Params "title" }}</h4>
{{ end }}
```

#### 例子4: `if` .. `else`

Below example is "Example 2" rewritten using `if` .. `else`, and using
[`isset` function][isset] + `.Params` variable (different from the
[`.Param` **function**][param]) instead:
下面的例子是使用`if` .. `else`重写的 “例子 2”，此处替换成了[`isset` 函数][isset] + `.Params` 变量 (和[`.Param` **函数**][param]不同)

```go-html-template
{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

#### 例子5: `if` .. `else if` .. `else`

和 `with` 不同, `if` 语句也可以包含 `else if` 分支.

```go-html-template
{{ if (isset .Params "description") }}
    {{ index .Params "description" }}
{{ else if (isset .Params "summary") }}
    {{ index .Params "summary" }}
{{ else }}
    {{ .Summary }}
{{ end }}
```

#### 例子6: `and` 和 `or`

```go-html-template
{{ if (and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")) }}
```

## 管道 Pipes

Go模板的最强大组件之一是以堆栈形式一个接一个调用方法的能力. 这通过使用pipes实现。管道概念来自于Unix管道，
概念很简单: 每个管道的输出成为接下来的管道的输入。

Go模板的非常简单的格式，管道是构建方法调用链的基础。
管道的一个限制是仅仅能作用于一个简单值并且它会成为管道下一个方法的最后一个参数.

下面几个简单例子有助于解释管道用法.

### 例子 1: `shuffle`

下面两个例子功能上是相同的:

```go-html-template
{{ shuffle (seq 1 5) }}
```


```go-html-template
{{ (seq 1 5) | shuffle }}
```

### 例子 2: `index`

The following accesses the page parameter called "disqus_url" and escapes the HTML. This example also uses the [`index` function](/functions/index-function/), which is built into Go Templates:
下面的代码访问页面参数 "disqus_url" 并转义为HTML. 例子中也使用了[`index` 函数](/functions/index-function/), 这是Go模板的内置函数:

```go-html-template
{{ index .Params "disqus_url" | html }}
```

### 例子 3: `or` 和 `isset`

```go-html-template
{{ if or (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr") }}
Stuff Here
{{ end }}
```

可以重写成如下管道形式:

```go-html-template
{{ if isset .Params "caption" | or isset .Params "title" | or isset .Params "attr" }}
Stuff Here
{{ end }}
```

### 例子 4: IE浏览器的条件注释 Internet Explorer Conditional Comments {#ie-conditional-comments}

By default, Go Templates remove HTML comments from output. This has the unfortunate side effect of removing Internet Explorer conditional comments. As a workaround, use something like this:
缺省情况下, Go模板会删除输出中的HTML注释. 这个会导致删除IE浏览器的条件注释的不幸的负效应.
作为解决方法，使用类似下面的方法:

```go-html-template
{{ "<!--[if lt IE 9]>" | safeHTML }}
  <script src="html5shiv.js"></script>
{{ "<![endif]-->" | safeHTML }}
```

或者,您可以使用反点号(`` ` ``)标记IE 条件 注释, 避免了注释内对每个双引号转义的繁杂的工作,如
Go文档中text/template部分的[例子中](https://golang.org/pkg/text/template/#hdr-Examples)一样:

```go-html-template
{{ `<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7"><![endif]-->` | safeHTML }}
```

## 上下文 Context (aka "the dot") {#the-dot}

The most easily overlooked concept to understand about Go Templates is
that `{{ . }}` always refers to the **current context**.
理解Go模版中最容易被简单忽视的概念是,`{{ . }}` 总是指向 **当前上下文current context**.

- 在最顶层的模板中, 这指的是它可以获取的数据集合.
- 在循环迭代中, 它具有循环中当前item的值。 `{{ . }}` 当前不在指向完整页面所有的数据集合.


如果需要在循环内部访问页面级别的数据(比如，在前端设定的页面参数)，
可以通过如下方式的一种来实现:

### 1. 定义一个上下文独立的变量

下面例子展示了如何定义一个独立于上下文的变量.

{{< code file="tags-range-with-page-variable.html" >}}
{{ $title := .Site.Title }}
<ul>
{{ range .Params.tags }}
    <li>
        <a href="/tags/{{ . | urlize }}">{{ . }}</a>
        - {{ $title }}
    </li>
{{ end }}
</ul>
{{< /code >}}

{{% note %}}
Notice how once we have entered the loop (i.e. `range`), the value of `{{ . }}` has changed. We have defined a variable outside of the loop (`{{$title}}`) that we've assigned a value so that we have access to the value from within the loop as well.
注意当我们进入循环部分(`range`)时,  `{{ . }}`的值发生了改变. 我们在循环外面定义了一个变量 (`{{$title}}`)
并且赋值给它, 所以在循环内部我们一样可以访问变量的值.
{{% /note %}}

### 2. 使用 `$.` 访问全局上下文

`$`在模版中具有特殊的重要性. 缺省情况下`.` ("the dot")的最初的值被赋予`$`. 这是一个[Go语言文本/模板的有文档的特性][dotdoc]。这意味着您可以在任何地点访问全局上下文. 下面例子重写了前面的代码块，不过现在是从全局上下文获取`.Site.Title`:

{{< code file="range-through-tags-w-global.html" >}}
<ul>
{{ range .Params.tags }}
  <li>
    <a href="/tags/{{ . | urlize }}">{{ . }}</a>
            - {{ $.Site.Title }}
  </li>
{{ end }}
</ul>
{{< /code >}}

{{% warning "Don't Redefine the Dot" %}}
`$` 的内置魔法会停止工作，如果有人不小心重定义了这个特殊字符. 比如 `{{ $ := .Site }}`. *不要这样做.*
当然，您可以从这个错误中恢复, 您可以在全局上下文中使用 `{{ $ := . }}` 来恢复`$`为它的默认值,
{{% /warning %}}

## 空白 Whitespace

Go语言1.6包含了从Go tag标签的任何一边删除空白的能力,
只需要在对应的`{{` 或者 `}}` 分隔符旁边包含一个连字符号 (`-`) 和跟随的空格.

下面列子中，Go模版会包含换行符和水平tag键在HTML输出中:

```go-html-template
<div>
  {{ .Title }}
</div>
```

输出:

```html
<div>
  Hello, World!
</div>
```

在下面利用 `-` 会删除围绕 `.Title` 多余的空白, 删除换行符:

```go-html-template
<div>
  {{- .Title -}}
</div>
```

输出:

```html
<div>Hello, World!</div>
```

Go语言认为下面字符是 _空白_ :

* <kbd>space</kbd>  空格键
* horizontal <kbd>tab</kbd>   水平tab键
* carriage <kbd>return</kbd>  会车
* newline                     换行

## 注释

In order to keep your templates organized and share information throughout your team, you may want to add comments to your templates. There are two ways to do that with Hugo.
为了在团队内保持模板的良好结构, 并且分享信息, 需要在模板中添加注释. 在Hugo中有两种方式.

### Go 模板注释 Go Templates comments

Go Templates support `{{/*` and `*/}}` to open and close a comment block. Nothing within that block will be rendered.
Go模板支持使用 `{{/*` 和 `*/}}` 来开启关闭注释代码块。在注释块内一切不会被输出.

比如:

```go-html-template
Bonsoir, {{/* {{ add 0 + 2 }} */}}Eliott.
```

会输出 `Bonsoir, Eliott.`,  并且会忽略注释块内的格式错误 (`add 0 + 2`).

### HTML 注释

If you need to produce HTML comments from your templates, take a look at the [Internet Explorer conditional comments](#ie-conditional-comments) example. If you need variables to construct such HTML comments, just pipe `printf` to `safeHTML`. For example:
如果需要从模板生成HTML注释，请参考前面的[Internet Explorer 条件注释](#ie-conditional-comments) 例子.
如果需要变量来构建类似HTML注释, 使用 `printf` 到 `safeHTML` 管道即可. 比如:

```go-html-template
{{ printf "<!-- Our website is named: %s -->" .Site.Title | safeHTML }}
```

#### 包含Go模板的HTML注释

默认情况下HTML注释会被剥离，但是注释内容依然会被求值。这意味着即使HTML注释永远不会生成任何内容到最终HTML页面,
在代码内的注释仍然可能使得构建过程失败.

{{% note %}}
**不要** 试图用HTML注释来注释包含Go Template的代码.
{{% /note %}}

```go-html-template
<!-- {{ $author := "Emma Goldman" }} was a great woman. -->
{{ $author }}
```

The templating engine will strip the content within the HTML comment, but will first evaluate any Go Template code if present within. So the above example will render `Emma Goldman`, as the `$author` variable got evaluated in the HTML comment. But the build would have failed if that code in the HTML comment had an error.
模板引擎会删除HTML注释的内容，但是会首先评估注释内包含的任何Go模板代码。上面例子会生成 `Emma Goldman`,
因为 `$author` 变量在HTML注释内获得值。不过如果HTML注释中代码包含错误会导致构建失败.

## Hugo参数 Hugo Parameters

Hugo provides the option of passing values to your template layer through your [site configuration][config] (i.e. for site-wide values) or through the metadata of each specific piece of content (i.e. the [front matter][]). You can define any values of any type and use them however you want in your templates, as long as the values are supported by the [front matter format]({{< ref "front-matter.md#front-matter-formats" >}}).
Hugo提供了从[站点配置文件][config] (对于站点范围的值)或者从每个特定内容的元数据([front matter][])来传递值给模板层的选项。只要[前端格式]({{< ref "front-matter.md#front-matter-formats" >}})支持, 您可以定义任何类型的参数，在您的模板中以任何方式使用。

## 使用内容(`Page`)参数

You can provide variables to be used by templates in individual content's [front matter][].
可以通过单独内容的 [front matter][]提供变量给模板使用.

An example of this is used in the Hugo docs. Most of the pages benefit from having the table of contents provided, but sometimes the table of contents doesn't make a lot of sense. We've defined a `notoc` variable in our front matter that will prevent a table of contents from rendering when specifically set to `true`.
Hugo Doc中使用了这样的例子。大部分Hugo Doc页面从提供的目录中获益，但是有时候文档目录并没有太大用。
我们在前端设置中定义了 `notoc` 变量, 当这个值特别设置为`true`时，会阻止内容目录的生成。

使用front matter(YAML)的例子:

```
---
title: Roadmap
lastmod: 2017-03-05
date: 2013-11-18
notoc: true
---
```

下面是可以在`toc.html` [partial template][partials]中使用的相应的代码:

{{< code file="layouts/partials/toc.html" download="toc.html" >}}
{{ if not .Params.notoc }}
<aside>
  <header>
    <a href="#{{.Title | urlize}}">
    <h3>{{.Title}}</h3>
    </a>
  </header>
  {{.TableOfContents}}
</aside>
<a href="#" id="toc-toggle"></a>
{{ end }}
{{< /code >}}

对于没有明确声明的页面的默认行为是包含TOC. 模板检查确认页面前端设置的`notoc:`值没有被设置为`true`.

## 使用站点配置参数

You can arbitrarily define as many site-level parameters as you want in your [site's configuration file][config]. These parameters are globally available in your templates.
可以在[站点配置文件][config]中任意定义站点界别的参数，想定义多少就可以定义多少。这些参数在页面模板中全局可以访问.

比如, 声明如下参数:

{{< code-toggle file="config" >}}
params:
  copyrighthtml: "Copyright &#xA9; 2017 John Doe. All Rights Reserved."
  twitteruser: "spf13"
  sidebarrecentlimit: 5
{{< /code >}}

在页脚布局中, 你可能想仅当提供`copyrighthtml`参数时声明一个<footer>元素。如果参数已经提供，
您需要声明通过[`safeHTML` function][safehtml]函数可以安全使用的字符串,这样HTML实体不需要再次转义。
这样您可以在每个元月一日更新顶层配置文件，而不是在摸板中查询特定字符串，如此更新站点更容易些。

```go-html-template
{{ if .Site.Params.copyrighthtml }}
    <footer>
        <div class="text-center">{{.Site.Params.CopyrightHTML | safeHTML}}</div>
    </footer>
{{ end }}
```

An alternative way of writing the "`if`" and then referencing the same value is to use [`with`][with] instead. `with` rebinds the context (`.`) within its scope and skips the block if the variable is absent:
上面例子使用"if"然后引用同样的值，另一个替代方式是使用[`with`][with].
`with`在其上下文中重新绑定(`.`)符号， 如果变量不存在，代码块将会被忽略。

{{< code file="layouts/partials/twitter.html" >}}
{{ with .Site.Params.twitteruser }}
    <div>
        <a href="https://twitter.com/{{.}}" rel="author">
        <img src="/images/twitter.png" width="48" height="48" title="Twitter: {{.}}" alt="Twitter"></a>
    </div>
{{ end }}
{{< /code >}}

最后， 您可以从模板布局中提取"magic constants". 下面例子使用了[`first`][first] 函数, 以及 [`.RelPermalink`][relpermalink] 页面变量和 [`.Site.Pages`][sitevars] 站点变量.


```go-html-template
<nav>
  <h1>Recent Posts</h1>
  <ul>
  {{- range first .Site.Params.SidebarRecentLimit .Site.Pages -}}
      <li><a href="{{.RelPermalink}}">{{.Title}}</a></li>
  {{- end -}}
  </ul>
</nav>
```

## 例子: 仅仅显示未来的事件

Go allows you to do more than what's shown here. Using Hugo's [`where` function][where] and Go built-ins, we can list only the items from `content/events/` whose date (set in a content file's [front matter][]) is in the future. The following is an example [partial template][partials]:

Go可以让您做的比这里说的更多。使用Hugo的[`where`函数][where]和Go语言的内建函数, 我们可以从内容块 `content/events/` 中仅仅列出日期(在内容文件的[前端设定][front matter]中设定)在未来的条目。
下面例子是一个[部分模板partial template][partials]的例子:

{{< code file="layouts/partials/upcoming-events.html" download="upcoming-events.html" >}}
<h4>Upcoming Events</h4>
<ul class="upcoming-events">
{{ range where .Pages.ByDate "Section" "events" }}
    {{ if ge .Date.Unix now.Unix }}
        <li>
        <!-- add span for event type -->
          <span>{{ .Type | title }} —</span>
          {{ .Title }} on
        <!-- add span for event date -->
          <span>{{ .Date.Format "2 January at 3:04pm" }}</span>
          at {{ .Params.place }}
        </li>
    {{ end }}
{{ end }}
</ul>
{{< /code >}}


[`where` function]: /functions/where/
[config]: /getting-started/configuration/
[dotdoc]: https://golang.org/pkg/text/template/#hdr-Variables
[first]: /functions/first/
[front matter]: /content-management/front-matter/
[functions]: /functions/ "See the full list of Hugo's templating functions with a quick start reference guide and basic and advanced examples."
[Go html/template]: https://golang.org/pkg/html/template/ "Godocs references for Go's html templating"
[gohtmltemplate]: https://golang.org/pkg/html/template/ "Godocs references for Go's html templating"
[index]: /functions/index-function/
[math functions]: /functions/math/
[partials]: /templates/partials/ "Link to the partial templates page inside of the templating section of the Hugo docs"
[internal_templates]: /templates/internal/
[relpermalink]: /variables/page/
[safehtml]: /functions/safehtml/
[sitevars]: /variables/site/
[pagevars]: /variables/page/
[variables]: /variables/ "See the full extent of page-, site-, and other variables that Hugo make available to you in your templates."
[where]: /functions/where/
[with]: /functions/with/
[godocsindex]: https://golang.org/pkg/text/template/ "Godocs page for index function"
[param]: /functions/param/
[isset]: /functions/isset/
