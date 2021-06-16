---
title: 短代码
linktitle: 短代码
description: 短代码是位于内容文件中、简单的代码段，调用内置或者定制的模板.
godocref:
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2019-11-07
menu:
  docs:
    parent: "content-management"
    weight: 35
weight: 35	#rem
categories: [content management]
keywords: [markdown,content,shortcodes]
draft: false
aliases: [/extras/shortcodes/]
testparam: "Hugo Rocks!"
toc: true
---

## 短代码是什么？

Hugo的设计者们中意Markdown，内容格式简单，但是有时Markdown会有不足。
内容作者经常会被逼着添加在Markdown内容中添加原生HTML(比如，video，`<iframe>`等)。
我们认为这与Markdown格式的漂亮简单冲突。

Hugo创建了**短代码**来解决这些限制.


短代码是内容文件中的简单的代码段，hugo会使用预定义的模板来呈现它。
注意短代码在模板文件中不会工作。
如果您在模板中需要短代码提供的插入代码之类的功能，
很多时候您想要的是[部分模板][partials]。

在美化Markdown之外，短代码可以在任何时间更新以反应新的类、技巧或标准。在站点生成的时间点，Hugo短代码会简单合并您的变化，避免了可能的复杂查询替换操作。

## 使用短代码

{{< youtube 2xkNJL4gJ9E >}}

在内容文件中，调用短代码的形式是`{{%/* shortcodename parameters */%}}`. 短代码参数是空格分隔的，内部有空格的参数需要使用引号括起来.

短代码声明中第一个词总是短代码的名称。参数跟随名称。依赖于短代码的定义方式，参数可以是有名参数、位置参数、或者都是，虽然在单独一次调用中不能混合参数类型。有名参数的格式以HTML的格式 `name="value"`为模型.

有些短代码使用或者要求结束的短代码。又和HTML一样，开始和结束的短代码要匹配(仅仅名称)，结束的短代码声明前面附加了斜线

下面是配对短代码的两个例子:

```
{{%/* mdshortcode */%}}Stuff to `process` in the *center*.{{%/* /mdshortcode */%}}
```

```
{{</* highlight go */>}} A bunch of code here {{</* /highlight */>}}
```

上面例子使用了两个不同的分隔符号，区别在于第一个使用了`%`字符作为分隔符号而第二个使用了 `<>` 字符。

### 使用原生字符串参数

{{< new-in "0.64.1" >}}
可以给短代码传递多行字符串作为参数，多行字符串需要使用原生字符串:

```
{{</*  myshortcode `This is some <b>HTML</b>,
and a new line with a "quoted string".` */>}}
```

### 短代码内包含Markdown 标记

从Hugo 0.55 版本开始我们改变了`%`分隔符的工作方式。 使用`%`作为最外层分隔符的短代码将会在传递给内容呈现器时完全呈现出来，意味着它们能够成为生成的目录表或者脚注等的一部分。

如果需要旧版本的工作方式，请在短代码模板的开始部分添加下面的代码:

```
{{ $_hugo_config := `{ "version": 1 }` }}
```


### 短代码内不包含Markdown

鼓号`<`标明短代码的内部内容*不需要*进一步呈现。没有Markdown的短代码经常会包含内部HTML代码。

```
{{</* myshortcode */>}}<p>Hello <strong>World!</strong></p>{{</* /myshortcode */>}}
```

### 嵌套的短代码

您可以在短代码里面调用其他短代码，需要创建内部短代码的模板，内部短代码可以使用`.Parent` 变量.
`.Parent` 允许您检查短代码被调用的上下文。参考 [短代码模板][sctemps].

## 使用Hugo内置的短代码

Hugo 附带了一系列的预定义的代表非常普遍用法的短代码。提供这些短代码给内容作者带来便利，
同时保持markdown内容整洁。

### 图像 `figure`

`figure` 是markdown中图像格式的扩展，markdown中图像不支持更语意化的[HTML5 `<figure>` element][figureelement].

`figure`短代码可以使用下面的具名参数:

src
: 于展示的图像的URL

link
: 如果需要对图像添加链接，目标URL.

target
: 如果`link`参数已经设置，URL可选的 `target` 属性

rel
: 如果`link`参数已经设置, 对于URL的可选属性

alt
: 如果image无法显示，替换显示的文本.

title
: 图片标题.

caption
: 图片说明. 在caption值内的markdown会被呈现输出

class
: HTML 标签 `figure` 的`class`属性.

height
: 图片高度 `height` attribute of the image.

width
: 图片宽度 `width` attribute of the image.

attr
: 图片归因文字。`attr`值内的markdown标记会被呈现输出

attrlink
: 如果attr属性需要添加链接，这是目的URL.

####  `figure` 例子输入

{{< code file="figure-input-example.md" >}}
{{</* figure src="/media/spf13.jpg" title="Steve Francia" */>}}
{{< /code >}}

#### `figure` 输出

{{< output file="figure-output-example.html" >}}
<figure>
  <img src="/media/spf13.jpg"  />
  <figcaption>
      <h4>Steve Francia</h4>
  </figcaption>
</figure>
{{< /output >}}

### `gist` GitHub代码片段

博客在写文章时经常会包含Github代码片段。假设我们想使用这个[这个URL所在的gist][examplegist]

```
https://gist.github.com/spf13/7896402
```

我们可以使用URL中获取的用户名和gist id，嵌入代码片段在我们的内容中:

```
{{</* gist spf13 7896402 */>}}
```

#### 例子 `gist` 输入

如果gist包含几个文件，您想引用其中一个，您可以传递文件名称(带引号的)作为可选的第三个参数:

{{< code file="gist-input.md" >}}
{{</* gist spf13 7896402 "img.html" */>}}
{{< /code >}}

#### 例子 `gist` 输出

{{< output file="gist-output.html" >}}
{{< gist spf13 7896402 >}}
{{< /output >}}

#### 例子 `gist` 展示

为展示Hugo的短代码特性的神奇效率, 我们在本页嵌入了`spf13` `gist` 例子。下面模拟了用户访问您站点的体验。很自然的、最后输出显示和您的央视表和包围的标记一致.

{{< gist spf13 7896402 >}}

### 高亮 `highlight`

这个短代码会将提供的源码转化为模式加亮的HTML. 更多请参考[highlighting](/tools/syntax-highlighting/).
`highlight`接受一个要求的`language`参数，并且需要匹配的结束的短代码.

####  `highlight` 例子 输入

{{< code file="content/tutorials/learn-html.md" >}}
{{</* highlight html */>}}
<section id="main">
  <div>
   <h1 id="title">{{ .Title }}</h1>
    {{ range .Pages }}
        {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
{{</* /highlight */>}}
{{< /code >}}

####  `highlight` 例子输出

上面的 `highlight` 短代码例子会生成下面的HTML当站点呈现的时候:

{{< output file="tutorials/learn-html/index.html" >}}
<span style="color: #f92672">&lt;section</span> <span style="color: #a6e22e">id=</span><span style="color: #e6db74">&quot;main&quot;</span><span style="color: #f92672">&gt;</span>
  <span style="color: #f92672">&lt;div&gt;</span>
   <span style="color: #f92672">&lt;h1</span> <span style="color: #a6e22e">id=</span><span style="color: #e6db74">&quot;title&quot;</span><span style="color: #f92672">&gt;</span>{{ .Title }}<span style="color: #f92672">&lt;/h1&gt;</span>
    {{ range .Pages }}
        {{ .Render &quot;summary&quot;}}
    {{ end }}
  <span style="color: #f92672">&lt;/div&gt;</span>
<span style="color: #f92672">&lt;/section&gt;</span>
{{< /output >}}

{{% note "More on Syntax Highlighting" %}}
为您站点添加形式高亮代码块的更多选项，请参考[Syntax Highlighting in Developer Tools](/tools/syntax-highlighting/).
{{% /note %}}

### `instagram` 照片

想嵌入[Instagram][]的照片,仅仅需要照片的ID. 可以从URL判断出Instagram的照片ID.


```
https://www.instagram.com/p/BWNjjyYFxVx/
```

#### Example `instagram` 输入

{{< code file="instagram-input.md" >}}
{{</* instagram BWNjjyYFxVx */>}}
{{< /code >}}

提供了可以隐藏caption的选项:

{{< code file="instagram-input-hide-caption.md" >}}
{{</* instagram BWNjjyYFxVx hidecaption */>}}
{{< /code >}}

#### Example `instagram` 输出


前面例子添加了 `hidecaption`选项, 下面的HTML会添加到您站点输出的标记中:

{{< output file="instagram-hide-caption-output.html" >}}
{{< instagram BWNjjyYFxVx hidecaption >}}
{{< /output >}}

#### Example `instagram` 显示

应用上面使用了`hidecaption`参数的`instagram`短代码例子，下面例子模拟了网站访问者的访问网站展示的经验。
自然，最后展示和您的样式表和周围标记一致。

{{< instagram BWNjjyYFxVx hidecaption >}}



{{% note %}}
The `instagram`-shortcode refers an endpoint of Instagram's API, that's deprecated since October 24th, 2020. Thus, no images can be fetched from this API endpoint, resulting in an error when the `instagram`-shortcode is used. For more information please have a look at GitHub issue [#7879](https://github.com/gohugoio/hugo/issues/7879).
{{% /note %}}

### `param` 参数

从当前页面的前言设定的变量 `Page's` 参数集合中获得某个值，如果没有得到，缺省使用站点的参数值。
如果在两处都没有发现指定键的参数值，会在日志中记录`ERROR`信息.

```bash
{{</* param testparam */>}}
```

`testparam`是页面在前言设定中设置的值为`Hugo Rocks!`的参数键，上面的输出是这样:

{{< param testparam >}}

访问深层嵌套的参数，使用"."模式, 比如:

```bash
{{</* param "my.nested.param" */>}}
```

### 永久链接 和 相对链接 `ref` and `relref`

这两个短代码通过查询页面的相对路径和逻辑名称返回发现的页面的永久链接(`ref`)或者相对链接(`relref`).


`ref` 和 `relref`也可以为Hugo产生的头链接生成片段链接。

{{% note "More on Cross References" %}}
关于`ref` 和 `relref`的更多扩展描述，请看[cross references](/content-management/cross-references/) 文档.
{{% /note %}}

`ref` 和 `relref`接受一个必须的_引用_参数，用引号包围并且在参数位置第一个位置.

####  `ref` and `relref` 输入举例

```
[Neat]({{</* ref "blog/neat.md" */>}})
[Who]({{</* relref "about.md#who" */>}})
```

####  `ref` and `relref` 输出举例

这里假设标准的Hugo漂亮URL开关打开.

```
<a href="/blog/neat">Neat</a>
<a href="/about/#who:c28654c202e73453784cfd2c5ab356c0">Who</a>
```

### `tweet`

想在blog文章中包含tweet？您所需全部是tweet的URL:

```
https://twitter.com/spf13/status/877500564405444608
```

####  `tweet` 输入举例

Pass the tweet's ID from the URL as a parameter to the `tweet` shortcode:

{{< code file="example-tweet-input.md" >}}
{{</* tweet 877500564405444608 */>}}
{{< /code >}}

####  `tweet` 输出举例

上面例子，添加到您网站的HTMl标记输出如下:

{{< output file="example-tweet-output.html" >}}
{{< tweet 877500564405444608 >}}
{{< /output >}}

####  `tweet` 展示举例

使用前面的`tweet`短代码例子，下面例子模拟了网站访问者的访问网站展示的经验。
自然，最后展示和您的样式表和周围标记一致.


{{< tweet 877500564405444608 >}}

### `vimeo`

从[Vimeo][]添加视频来[YouTube输入短代码][YouTube Input shortcode]一样
```
https://vimeo.com/channels/staffpicks/146022717
```

#### Example `vimeo` 输入举例

从视频的URL中解析出ID，传递给`vimeo`短代码做参数:

{{< code file="example-vimeo-input.md" >}}
{{</* vimeo 146022717 */>}}
{{< /code >}}

#### Example `vimeo` 输出举例

Using the preceding `vimeo` example, the following HTML will be added to your rendered website's markup:
使用前面`vimeo`例子，下面的HTML会添加到呈现站点的标记中:

{{< output file="example-vimeo-output.html" >}}
{{< vimeo 146022717 >}}
{{< /output >}}

{{% tip %}}
如果需要进一步定制youtube或者vimeo输出的样式，在调用短代码时候添加一个有名参数`class`.
这个新的`class`参数会被添加在包围`<iframe>`的`<div>`元素上,同时删除内联的样式.
注意您也会可能需要调用`id`作为有名参数。您可以给Vimeo视频一个描述性的`title`参数.

```
{{</* vimeo id="146022717" class="my-vimeo-wrapper-class" title="My vimeo video" */>}}
```
{{% /tip %}}

#### Example `vimeo` 展示举例

使用前面的`vimeo`短代码例子，下面例子模拟了网站访问者的访问网站展示经验。
自然，最后展示和您的样式表和周围标记一致.


{{< vimeo 146022717 >}}

### `youtube` 油管

`youtube` 短代码嵌入一个[YouTube videos][]的响应式视频播放器。仅仅需要视频ID作为参数, 比如:

```
https://www.youtube.com/watch?v=w7Ft2ymGmfc
```


#### Example `youtube` 输入举例

copy视频ID，就是视频URL中跟随在`v=`后面的内容，传递给`youtube`短代码:

{{< code file="example-youtube-input.md" >}}
{{</* youtube w7Ft2ymGmfc */>}}
{{< /code >}}

Furthermore, you can automatically start playback of the embedded video by setting the `autoplay` parameter to `true`. Remember that you can't mix named and unnamed parameters, so you'll need to assign the yet unnamed video id to the parameter `id`:
此外、可以通过设置`autoplay` 参数为 `true`开始嵌入视频的自动重播. 请记住您不能混合有名和无名参数,
所以您需要将原来未命名的视频id赋值给有名参数 `id`:

{{< code file="example-youtube-input-with-autoplay.md" >}}
{{</* youtube id="w7Ft2ymGmfc" autoplay="true" */>}}
{{< /code >}}

For [accessibility reasons](https://dequeuniversity.com/tips/provide-iframe-titles), it's best to provide a title for your YouTube video.  You  can do this using the shortcode by providing a `title` parameter. If no title is provided, a default of "YouTube Video" will be used.
出于[可访问性原因](https://dequeuniversity.com/tips/provide-iframe-titles), 最好给youtube视频提供一个标题. 可以通过给短代码提供一个`title`参数来实现. 如果没提供标题，会使用默认标题"YouTube Video".

{{< code file="example-youtube-input-with-title.md" >}}
{{</* youtube id="w7Ft2ymGmfc" title="A New Hugo Site in Under Two Minutes" */>}}
{{< /code >}}


#### Example `youtube` 输出举例

使用前面`youtube`例子，下面的HTML会添加到站点输出的标记中:


{{< code file="example-youtube-output.html" >}}
{{< youtube id="w7Ft2ymGmfc" autoplay="true" >}}
{{< /code >}}

#### Example `youtube` 展示举例

使用前面的`youtube`短代码例子(不包含 `autoplay="true"`)，下面例子模拟了网站访问者的访问网站展示经验。
自然，最后展示和您的样式表和周围标记一致. 这个视频也包含在 [快速入门文档][quickstart]中.


{{< youtube w7Ft2ymGmfc >}}

## 隐私配置

如何配置Hugo 站点满足欧盟新的隐私规范，请参考[Hugo and the GDPR][]

## 创建定制的短代码

To learn more about creating custom shortcodes, see the [shortcode template documentation][].
学习创建定制短代码，请参考[短代码模板][shortcode template documentation]文档.

[`figure` shortcode]: #figure
[contentmanagementsection]: /content-management/formats/
[examplegist]: https://gist.github.com/spf13/7896402
[figureelement]: https://html5doctor.com/the-figure-figcaption-elements/ "An article from HTML5 doctor discussing the fig and figcaption elements."
[Hugo and the GDPR]: /about/hugo-and-gdpr/
[Instagram]: https://www.instagram.com/
[pagevariables]: /variables/page/
[partials]: /templates/partials/
[Pygments]: https://pygments.org/
[quickstart]: /getting-started/quick-start/
[sctemps]: /templates/shortcode-templates/
[scvars]: /variables/shortcodes/
[shortcode template documentation]: /templates/shortcode-templates/
[templatessection]: /templates/
[Vimeo]: https://vimeo.com/
[YouTube Videos]: https://www.youtube.com/
[YouTube Input shortcode]: #youtube
