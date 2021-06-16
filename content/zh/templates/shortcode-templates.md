---
title: 创建短代码
linktitle: 短代码模板
description: You can extend Hugo's built-in shortcodes by creating your own using the same templating syntax as that for single and list pages.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [templates]
keywords: [shortcodes,templates]
menu:
  docs:
    parent: "templates"
    weight: 100
weight: 100
sections_weight: 100
draft: false
aliases: []
toc: true
---

Shortcodes are a means to consolidate templating into small, reusable snippets that you can embed directly inside of your content. In this sense, you can think of shortcodes as the intermediary between [page and list templates][templates] and [basic content files][].
Shortcodes短代码是加强模板功能的简短的、可重用的代码片段, 您可以在内容内直接嵌入短代码。
某种意义上，可以认为短代理是[页面和list模板][templates] and [基础内容文件][basic content files]中的中间件.

{{% note %}}
Hugo also ships with built-in shortcodes for common use cases. (See [Content Management: Shortcodes](/content-management/shortcodes/).)
Hugo提供了内置的常见用例的短代码.(参考 [内容管理: 短代码](/content-management/shortcodes/).)
{{% /note %}}

## 创建定制短代码 Create Custom Shortcodes

Hugo's built-in shortcodes cover many common, but not all, use cases. Luckily, Hugo provides the ability to easily create custom shortcodes to meet your website's needs.
Hugo的内建短代码覆盖了很多常见-但不是全部-的用例. 幸运的是, Hugo提供了简单创建定制短代码以满足网站需求的能力.

{{< youtube Eu4zSaKOY4A >}}

### 文件位置 File Location

To create a shortcode, place an HTML template in the `layouts/shortcodes` directory of your [source organization][]. Consider the file name carefully since the shortcode name will mirror that of the file but without the `.html` extension. For example, `layouts/shortcodes/myshortcode.html` will be called with either `{{</* myshortcode /*/>}}` or `{{%/* myshortcode /*/%}}` depending on the type of parameters you choose.
为创建短代码模板, 在[代码组织][source organization]的 `layouts/shortcodes`目录创建新的HTML模板.
请仔细考虑文件名称，短代码名会反应文件名称, 但是不包括 `.html` 扩展名. 比如,  `layouts/shortcodes/myshortcode.html` 的代码会以`{{</* myshortcode /*/>}}` or `{{%/* myshortcode /*/%}}`的方式被调用, 依赖于所选择参数类型。


You can organize your shortcodes in subfolders, e.g. in `layouts/shortcodes/boxes`. These shortcodes would then be accessible with their relative path, e.g:
也可以在子目录中组织短代码, 比如,在`layouts/shortcodes/boxes`中. 这些短代码调用是会通过相对目录, 比如:

```
{{</* boxes/square */>}}
```

Note the forward slash.
注意反斜线

### 短代码模板解析查询顺序 Shortcode Template Lookup Order

短代码模板的解析查询顺序简单些,仅仅查询如下两个地方:

1. `/layouts/shortcodes/<SHORTCODE>.html`
2. `/themes/<THEME>/layouts/shortcodes/<SHORTCODE>.html`

### Positional vs Named Parameters

You can create shortcodes using the following types of parameters:
创建短代码可以使用下面的参数类型:

* 位置参数 Positional parameters
* 命名参数 Named parameters
* 位置参数或者命名参数 Positional *or* named parameters (i.e, "flexible")

In shortcodes with positional parameters, the order of the parameters is important. If a shortcode has a single required value (e.g., the `youtube` shortcode below), positional parameters work very well and require less typing from content authors.
带有位置参数的短代码中，参数的顺序很重要。 如果短代码只有一个必填值(例如下面的`youtube`短代码), 位置参数可以很好地工作，并且内容作者的键入次数更少

For more complex layouts with multiple or optional parameters, named parameters work best. While less terse, named parameters require less memorization from a content author and can be added in a shortcode declaration in any order.
对于具有多个或可选参数的更复杂的布局，命名参数最有效. 命名参数虽然不那么简洁，但它们需要内容作者的更少的记忆，并且可以以任何顺序添加到短代码声明中。

Allowing both types of parameters (i.e., a "flexible" shortcode) is useful for complex layouts where you want to set default values that can be easily overridden by users.
对于复杂布局来说, 如果想允许用户重载默认参数值，允许两种类型的参数(比如说适应性很强的短代码)很有帮助,

### 访问参数 Access Parameters

All shortcode parameters can be accessed via the `.Get` method. Whether you pass a key (i.e., string) or a number to the `.Get` method depends on whether you are accessing a named or positional parameter, respectively.
所有短代码可以通过`.Get`方法访问。传递个`.Get`方法的参数是一个键值(字符串)还是一个数值相应的依赖于您在访问命名参数还是位置参数.

To access a parameter by name, use the `.Get` method followed by the named parameter as a quoted string:
通过名称访问参数, 请使用`.Get` 方法, 后面跟随引号括起的命名参数.

```
{{ .Get "class" }}
```

To access a parameter by position, use the `.Get` followed by a numeric position, keeping in mind that positional parameters are zero-indexed:
访问位置参数, 请使用 `.Get`方法, 后面跟随一个表示位置的数值, 请注意位置参数索引从零开始:

```
{{ .Get 0 }}
```

For the second position, you would just use:
对第二个位置, 下面这样访问:

```
{{ .Get 1 }}
```

`with` is great when the output depends on a parameter being set:
`with` 方法很方便，当输出依赖于设置了某参数:

```
{{ with .Get "class"}} class="{{.}}"{{ end }}
```

`.Get` can also be used to check if a parameter has been provided. This is
most helpful when the condition depends on either of the values, or both:
使用`.Get` 方法检查参数是否设置也很有帮助。当条件依赖于某个参数的值时特别有用:

```
{{ if or (.Get "title") (.Get "alt") }} alt="{{ with .Get "alt"}}{{.}}{{else}}{{.Get "title"}}{{end}}"{{ end }}
```

#### 变量 `.Inner`

If a closing shortcode is used, the `.Inner` variable will be populated with all of the content between the opening and closing shortcodes. If a closing shortcode is required, you can check the length of `.Inner` as an indicator of its existence.
当使用了闭合的短代码时, `.Inner`变量被赋予了短代码开启和闭合括号之间的所有内容。 如果闭合短代码是必须的, 可以检查
`.Inner`的长度作为短代码存在的指示.

A shortcode with content declared via the `.Inner` variable can also be declared without the inline content and without the closing shortcode by using the self-closing syntax:
通过 `.Inner`变量定义的短代码也可以不使用内联内容、不使用闭合括号的自闭合模式来声明:

```
{{</* innershortcode /*/>}}
```

#### 变量 `.Params`

The `.Params` variable in shortcodes contains the list parameters passed to shortcode for more complicated use cases. You can also access higher-scoped parameters with the following logic:
更复杂的用例中, 短代码中变量 `.Params` 包含传递给短代码的参数的list. 可以使用下面逻辑访问更高范围层次的参数:

`$.Params`
: these are the parameters passed directly into the shortcode declaration (e.g., a YouTube video ID)
可以访问直接传递给短代码声明的参数(比如youtube视频ID)

`$.Page.Params`
: refers to the page's params; the "page" in this case refers to the content file in which the shortcode is declared (e.g., a `shortcode_color` field in a content's front matter could be accessed via `$.Page.Params.shortcode_color`).
页面参数的引用; 这里的 "page" 引用的是短代码声明所在的内容文件(比如, 页面内容前言设定中 `shortcode_color` 字段可以通过 `$.Page.Params.shortcode_color` 访问)

`$.Page.Site.Params`
: refers to global variables as defined in your [site's configuration file][config].
[站点配置文件][config]中定义的全局变量的引用

#### `.IsNamedParams`

The `.IsNamedParams` variable checks whether the shortcode declaration uses named parameters and returns a boolean value.
变量 `.IsNamedParams` 检查短代码声明是否使用了有名参数，返回一个boolean值.

For example, you could create an `image` shortcode that can take either a `src` named parameter or the first positional parameter, depending on the preference of the content's author. Let's assume the `image` shortcode is called as follows:
比如,创建`image`短代码，接受一个命名参数`src`或者第一个位置参数, 依赖于内容作者的喜好。
假设我们像下面这样调用 `image` 短代码:

```
{{</* image src="images/my-image.jpg"*/>}}
```

You could then include the following as part of your shortcode templating:
然后您可以包含下面代码作为短代码模板的一部分:

```
{{ if .IsNamedParams }}
<img src="{{.Get "src" }}" alt="">
{{ else }}
<img src="{{.Get 0}}" alt="">
{{ end }}
```

See the [example Vimeo shortcode][vimeoexample] below for `.IsNamedParams` in action.
请看下面的使用`.IsNamedParams`参数的[样例Vimeo 短代码][vimeoexample].

{{% warning %}}
While you can create shortcode templates that accept both positional and named parameters, you *cannot* declare shortcodes in content with a mix of parameter types. Therefore, a shortcode declared like `{{</* image src="images/my-image.jpg" "This is my alt text" */>}}` will return an error.
虽然可以创建短代码模板接受位置参数和命名参数, 但是*无法*在混合参数类型的上下文中声明短代码。 因此，短代码声明类似`{{</* image src="images/my-image.jpg" "This is my alt text" */>}}`的声明，会返回错误.
{{% /warning %}}

You can also use the variable `.Page` to access all the normal [page variables][pagevars].
可以使用变量 `.Page`获取所有的普通的页面变量 [page variables][pagevars]

A shortcodes can also be nested. In a nested shortcode, you can access the parent shortcode context with [`.Parent` variable][shortcodesvars]. This can be very useful for inheritance of common shortcode parameters from the root.
短代码可以嵌套。在嵌入的短代码中, 可以通过[`.Parent` 变量][shortcodesvars]访问上一级短代码上下文。这对于从根部继承常见的短代码参数很有帮助.

### 检查短代码是否存在  Checking for Existence

You can check if a specific shortcode is used on a page by calling `.HasShortcode` in that page template, providing the name of the shortcode. This is sometimes useful when you want to include specific scripts or styles in the header that are only used by that shortcode.
可以在页面模板中使用 `.HasShortcode` 参数来检查特定短代码是否在页面内使用。这个特性有时候有些帮助，比如想包含仅仅会被短代码使用的特定脚本或者样式表。

## 定制的短代码例子 Custom Shortcode Examples

The following are examples of the different types of shortcodes you can create via shortcode template files in `/layouts/shortcodes`.
下面是可以在`/layouts/shortcodes`中创建的不同类型的短代码的例子:

### 单-词例子 : `year`

Let's assume you would like to keep mentions of your copyright year current in your content files without having to continually review your markdown. Your goal is to be able to call the shortcode as follows:
假设想使得内容文件的copyright年份维持在当前年份, 而不需要持续的检查您的markdown文件。您的目标是下面这样调用短代码:

```
{{</* year */>}}
```

{{< code file="/layouts/shortcodes/year.html" >}}
{{ now.Format "2006" }}
{{< /code >}}

### 单一位置参数例子Single Positional Example: `youtube`

Embedded videos are a common addition to markdown content that can quickly become unsightly. The following is the code used by [Hugo's built-in YouTube shortcode][youtubeshortcode]:
嵌入的视频在markdown内容中是常见的，不过很块就会使得内容混乱。下面是[Hugo内置的YouTube短代码][youtubeshortcode]的使用方式:

```
{{</* youtube 09jf3ow9jfw */>}}
```

Would load the template at `/layouts/shortcodes/youtube.html`:
会调用`/layouts/shortcodes/youtube.html`的模板:

{{< code file="/layouts/shortcodes/youtube.html" >}}
<div class="embed video-player">
<iframe class="youtube-player" type="text/html" width="640" height="385" src="https://www.youtube.com/embed/{{ index .Params 0 }}" allowfullscreen frameborder="0">
</iframe>
</div>
{{< /code >}}

{{< code file="youtube-embed.html" copy="false" >}}
<div class="embed video-player">
    <iframe class="youtube-player" type="text/html"
        width="640" height="385"
        src="https://www.youtube.com/embed/09jf3ow9jfw"
        allowfullscreen frameborder="0">
    </iframe>
</div>
{{< /code >}}

### 单一命名参数例子 Single Named Example: `image`

Let's say you want to create your own `img` shortcode rather than use Hugo's built-in [`figure` shortcode][figure]. Your goal is to be able to call the shortcode as follows in your content files:
假设您想创建自己的`img`短代码，而不是使用Hugo内建的[`figure`][figure]短代码。目标是像下面这样在内容文件中调用:

{{< code file="content-image.md" >}}
{{</* img src="/media/spf13.jpg" title="Steve Francia" */>}}
{{< /code >}}

You have created the shortcode at `/layouts/shortcodes/img.html`, which loads the following shortcode template:
创建了短代码文件`/layouts/shortcodes/img.html`,调用下面模板:

{{< code file="/layouts/shortcodes/img.html" >}}
<!-- image -->
<figure {{ with .Get "class" }}class="{{.}}"{{ end }}>
    {{ with .Get "link"}}<a href="{{.}}">{{ end }}
        <img src="{{ .Get "src" }}" {{ if or (.Get "alt") (.Get "caption") }}alt="{{ with .Get "alt"}}{{.}}{{else}}{{ .Get "caption" }}{{ end }}"{{ end }} />
    {{ if .Get "link"}}</a>{{ end }}
    {{ if or (or (.Get "title") (.Get "caption")) (.Get "attr")}}
    <figcaption>{{ if isset .Params "title" }}
        <h4>{{ .Get "title" }}</h4>{{ end }}
        {{ if or (.Get "caption") (.Get "attr")}}<p>
        {{ .Get "caption" }}
        {{ with .Get "attrlink"}}<a href="{{.}}"> {{ end }}
            {{ .Get "attr" }}
        {{ if .Get "attrlink"}}</a> {{ end }}
        </p> {{ end }}
    </figcaption>
    {{ end }}
</figure>
<!-- image -->
{{< /code >}}

Would be rendered as:
会这样呈现HTML:

{{< code file="img-output.html" copy="false" >}}
<figure>
    <img src="/media/spf13.jpg"  />
    <figcaption>
        <h4>Steve Francia</h4>
    </figcaption>
</figure>
{{< /code >}}

### 单一的可适应行例子 Single Flexible Example: `vimeo`

```
{{</* vimeo 49718712 */>}}
{{</* vimeo id="49718712" class="flex-video" */>}}
```

会调用 `/layouts/shortcodes/vimeo.html`的模板:

{{< code file="/layouts/shortcodes/vimeo.html" >}}
{{ if .IsNamedParams }}
  <div class="{{ if .Get "class" }}{{ .Get "class" }}{{ else }}vimeo-container{{ end }}">
    <iframe src="https://player.vimeo.com/video/{{ .Get "id" }}" allowfullscreen></iframe>
  </div>
{{ else }}
  <div class="{{ if len .Params | eq 2 }}{{ .Get 1 }}{{ else }}vimeo-container{{ end }}">
    <iframe src="https://player.vimeo.com/video/{{ .Get 0 }}" allowfullscreen></iframe>
  </div>
{{ end }}
{{< /code >}}

Would be rendered as:
HTML呈现如下:

{{< code file="vimeo-iframes.html" copy="false" >}}
<div class="vimeo-container">
  <iframe src="https://player.vimeo.com/video/49718712" allowfullscreen></iframe>
</div>
<div class="flex-video">
  <iframe src="https://player.vimeo.com/video/49718712" allowfullscreen></iframe>
</div>
{{< /code >}}

### 前后闭合的例子  Paired Example: `highlight`

The following is taken from `highlight`, which is a [built-in shortcode][] that ships with Hugo.
下面是从`highlight`截取的，也就是Hugo内建的[built-in shortcode][]

{{< code file="highlight-example.md" >}}
{{</* highlight html */>}}
  <html>
    <body> This HTML </body>
  </html>
{{</* /highlight */>}}
{{< /code >}}

The template for the `highlight` shortcode uses the following code, which is already included in Hugo:
`highlight`短代码使用下面的代码，已经包含在Hugo中:

```
{{ .Get 0 | highlight .Inner  }}
```

The rendered output of the HTML example code block will be as follows:
呈现的HTML输出例子代码块如下:

{{< code file="syntax-highlighted.html" copy="false" >}}
<div class="highlight" style="background: #272822"><pre style="line-height: 125%"><span style="color: #f92672">&lt;html&gt;</span>
    <span style="color: #f92672">&lt;body&gt;</span> This HTML <span style="color: #f92672">&lt;/body&gt;</span>
<span style="color: #f92672">&lt;/html&gt;</span>
</pre></div>
{{< /code >}}

### 嵌套的短代码: 图片库 Nested Shortcode: Image Gallery

Hugo's [`.Parent` shortcode variable][parent] returns a boolean value depending on whether the shortcode in question is called within the context of a *parent* shortcode. This provides an inheritance model for common shortcode parameters.
Hugo的[`.Parent` ][parent]短代码变量返回值依赖于调用的短代码是否在一个*parent*上级短代码的上下文中. 这样提供了通过的短代码参数继承模型.

The following example is contrived but demonstrates the concept. Assume you have a `gallery` shortcode that expects one named `class` parameter:
下面例子是编造的，但是展示了概念。假设有一个`gallery`短代码，期望一个命名为`class`的参数:

{{< code file="layouts/shortcodes/gallery.html" >}}
<div class="{{.Get "class"}}">
  {{.Inner}}
</div>
{{< /code >}}

You also have an `img` shortcode with a single named `src` parameter that you want to call inside of `gallery` and other shortcodes, so that the parent defines the context of each `img`:
另外假设有`img`短代码具有单一命名参数`src`, 你想在`gallery`或者其他短代码中使用，
由此parent定义了每个`img`的上下文:

{{< code file="layouts/shortcodes/img.html" >}}
{{- $src := .Get "src" -}}
{{- with .Parent -}}
  <img src="{{$src}}" class="{{.Get "class"}}-image">
{{- else -}}
  <img src="{{$src}}">
{{- end }}
{{< /code >}}

You can then call your shortcode in your content as follows:
然后可以在内容中如下调用短代码:

```
{{</* gallery class="content-gallery" */>}}
  {{</* img src="/images/one.jpg" */>}}
  {{</* img src="/images/two.jpg" */>}}
{{</* /gallery */>}}
{{</* img src="/images/three.jpg" */>}}
```

This will output the following HTML. Note how the first two `img` shortcodes inherit the `class` value of `content-gallery` set with the call to the parent `gallery`, whereas the third `img` only uses `src`:
输出如下的HTML. 注意头两个`img`短代码继承了父级短代码`gallery`调用而设置的`content-gallery`的`class`值，但是第三个图像仅仅有`src`:

```
<div class="content-gallery">
    <img src="/images/one.jpg" class="content-gallery-image">
    <img src="/images/two.jpg" class="content-gallery-image">
</div>
<img src="/images/three.jpg">
```


## 短代码中错误处理 Error Handling in Shortcodes

Use the [errorf](/functions/errorf) template func and [.Position](/variables/shortcodes/) variable to get useful error messages in shortcodes:
使用[errorf](/functions/errorf)模板函数和[.Position](/variables/shortcodes/)变量来获得有用的短代码错误信息:

```bash
{{ with .Get "name" }}
{{ else }}
{{ errorf "missing value for param 'name': %s" .Position }}
{{ end }}
```

When the above fails, you will see an `ERROR` log similar to the below:
当上面代码有错误时,您会在终端看见如下的`ERROR`日志:


```bash
ERROR 2018/11/07 10:05:55 missing value for param name: "/Users/bep/dev/go/gohugoio/hugo/docs/content/en/variables/shortcodes.md:32:1"
```

## 更多短代码例子 More Shortcode Examples

More shortcode examples can be found in the [shortcodes directory for spf13.com][spfscs] and the [shortcodes directory for the Hugo docs][docsshortcodes].
更多短代码例子可以参考[spf13.com的短代码目录][spfscs] 和 [Hugo 文档的短代码][docsshortcodes].


## 内联的短代码 Inline Shortcodes

Since Hugo 0.52, you can implement your shortcodes inline -- e.g. where you use them in the content file. This can be useful for scripting that you only need in one place.
从Hugo 0.52版本开始, 可以实现内联的短代码 -- 就在内容文件的里面的短代码. 这对于仅仅使用一次的脚本有帮助.

This feature is disabled by default, but can be enabled in your site config:
这个特性默认是关闭的, 但是可以在您的站点配置中启用:

{{< code-toggle file="config">}}
enableInlineShortcodes = true
{{< /code-toggle >}}

It is disabled by default for security reasons. The security model used by Hugo's template handling assumes that template authors are trusted, but that the content files are not, so the templates are injection-safe from malformed input data. But in most situations you have full control over the content, too, and then `enableInlineShortcodes = true` would be considered safe. But it's something to be aware of: It allows ad-hoc [Go Text templates](https://golang.org/pkg/text/template/) to be executed from the content files.
由于安全原因,这个特性默认是禁用的。Hugo的模板处理所用的安全模型假设模板作者是可信任的、到那时内容文件不是，所以模板对于异常的输入信息是插入安全的。但是在很多情景下您也具有对内容的完全控制，这样打开`enableInlineShortcodes = true`参数开关被认为是安全的。不过这还是需要的事项: 这允许特许的[Go Text templates](https://golang.org/pkg/text/template/) 在内容文件内执行。


And once enabled, you can do this in your content files:
启用以后，可以在内容文件中这样做:

 ```go-text-template
 {{</* time.inline */>}}{{ now }}{{</* /time.inline */>}}
 ```

The above will print the current date and time.
上面例子会打印当前日期和是时间.

Note that an inline shortcode's inner content is parsed and executed as a Go text template with the same context as a regular shortcode template.
请注意内联的短代码内容是作为Go语言Text模板解析和执行的，具有和常规短代码模板相同的上下文。

This means that the current page can be accessed via `.Page.Title` etc. This also means that there are no concept of "nested inline shortcodes".
也就意味这当前页面可以通过`.Page.Title`等访问。这也意味着没有"嵌套内联模板"的概念.

The same inline shortcode can be reused later in the same content file, with different params if needed, using the self-closing syntax:
相同的内联短代码模板也可以在相同的内容文件的后面重用,如果有必要使用不同的参数，使用自我闭合的模式.

 ```go-text-template
{{</* time.inline /*/>}}
```


[basic content files]: /content-management/formats/ "请参考文档Hugo如何利用markdown--和其他支持的模式--来创建您网站内容"
[built-in shortcode]: /content-management/shortcodes/ "内置短代码"
[config]: /getting-started/configuration/ "学习更多Hugo的内建配置变量以及如何使用站点配置文件去包括可以在生成的站点中到处可用的全局键值"
[Content Management: Shortcodes]: /content-management/shortcodes/#using-hugo-s-built-in-shortcodes "请参考这部分,如果您对短代码的概念不熟悉或者对如何在内容文件中使用短代码不熟悉."
[source organization]: /getting-started/directory-structure/#directory-structure-explained "学习Hugo新站点建立脚手架以及站点目录结构"
[docsshortcodes]: https://github.com/gohugoio/hugo/tree/master/docs/layouts/shortcodes "参考当前阅读的文档站点的短代码源码目录"
[figure]: /content-management/shortcodes/#figure
[hugosc]: /content-management/shortcodes/#using-hugo-s-built-in-shortcodes
[lookup order]: /templates/lookup-order/ "See the order in which Hugo traverses your template files to decide where and how to render your content at build time"
[pagevars]: /variables/page/ "See which variables you can leverage in your templating for page vs list templates."
[parent]: /variables/shortcodes/
[shortcodesvars]: /variables/shortcodes/ "Certain variables are specific to shortcodes, although most .Page variables can be accessed within your shortcode template."
[spfscs]: https://github.com/spf13/spf13.com/tree/master/layouts/shortcodes "See more examples of shortcodes by visiting the shortcode directory of the source for spf13.com, the blog of Hugo's creator, Steve Francia."
[templates]: /templates/ "The templates section of the Hugo docs."
[vimeoexample]: #single-flexible-example-vimeo
[youtubeshortcode]: /content-management/shortcodes/#youtube "See how to use Hugo's built-in YouTube shortcode."
