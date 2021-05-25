---
title: 内容格式
linktitle: 内容格式
description: HTML 和 Markdown 是Hugo支持的内容格式
date: 2017-01-10
publishdate: 2017-01-10
lastmod: 2017-04-06
categories: [content management]
keywords: [markdown,asciidoc,mmark,pandoc,content format]
menu:
  docs:
    parent: "content-management"
    weight: 20
weight: 20	#rem
draft: false
aliases: [/content/markdown-extras/,/content/supported-formats/,/doc/supported-formats/]
toc: true

---

可以在 `/content`目录下放置任何文件类型，不过Hugo使用`markup`前端(如果已经设定)设定值或者通过文件扩展名(见下面表格中`Markup identifiers`)来决定markup是否需要处理,比如:


* Markdown 转化成HTML
* [Shortcodes](/content-management/shortcodes/) 处理转化
* 应用Layout布局

## 内容格式列表

当前Hugo支持的内容格式:

| 名称  | 标记标志 | 备注 |
| ------------- | ------------- |-------------|
| Goldmark  | md, markdown, goldmark  |注意可以将默认的markdown处理器设置成其他处理器, 请见 [配置Markup](/getting-started/configuration-markup/).{{< new-in "0.60.0" >}} |
| Blackfriday | blackfriday  |Blackfriday 以后最终会被废弃使用.|
|MMark|mmark|Mmark 已经弃用，将会在近期发布版本中移除.|
|Emacs Org-Mode|org| 参考 [go-org](https://github.com/niklasfasching/go-org)|
|AsciiDoc|asciidocext, adoc, ad|需要安装 [Asciidoctor][ascii]|
|RST|rst|需要安装 [RST](http://docutils.sourceforge.net/rst.html)|
|Pandoc|pandoc, pdc|需要安装 [Pandoc](https://www.pandoc.org/)|
|HTML|html, htm| 作为内容文件处理, 解析布局、短代码。 必须设置了front matter。如果没设置，会按原样copy|

标志标记 `markup identifier` 从front matter中设置的`markup`变量获取, 或者从文件扩展名的到. 更多标记相关设定，请参考[配置标记](/getting-started/configuration-markup/).


## 外部帮助程序

上表中一个内容格式需要您的计算机上安装的外部命令的帮助。比如,
对于AsciiDoc文件Hugo会尝试调用`asciidoctor`命令，这意味着
您必须在您的机器上安装相关的工具才能利用这些格式


Hugo调用这些命令时会默认传递合理的缺省参数给这些命令:

- `asciidoctor`: `--no-header-footer -`
- `rst2html`: `--leave-comments --initial-header-level=2`
- `pandoc`: `--mathjax`

{{% warning "Performance of External Helpers" %}}
由于处理额外的文件格式需要使用外部命令, 网站生成效率可能严重依赖于所使用的外部命令的效率。
这个特性还很新颖，欢迎反馈信息。
{{% /warning %}}

### 外部帮助命令  AsciiDoc

在2020年一月[AsciiDoc命令工具](https://github.com/asciidoc/asciidoc) 的实现结束了生命周期，不在受支持.

[AsciiDoc](https://github.com/asciidoc/asciidoc)的实现在2020年一月结束了生命周期
AsciiDoc的开发由[Asciidoctor](https://github.com/asciidoctor)继续。格式当然还是AsciiDoc格式。
请继续使用Asciidoctor的实现。


### 外部帮助命令 Asciidoctor

Asciidoctor社区为AsciiDoc格式提供了广泛的工具集，可以安装作为Hugo的附加工具

[参考Asciidoctor文档获取安装指导](https://asciidoctor.org/docs/install-toolchain/).
请确保所有可选扩展比如`asciidoctor-diagram` 或者 `asciidoctor-html5s` 被安装, 如果有需要。

{{% note %}}
外部命令 `asciidoctor` 要求Hugo输出到_磁盘_的特定目标目录。因此要求运行Hugo时需要提供命令行参数 `--destination`.
{{% /note %}}

有些 [Asciidoctor](https://asciidoctor.org/man/asciidoctor/) 参数可以在Hugo内定制:

参数 | 评论
--- | ---
backend | 如果不知道自己在做什么不要修改这个参数.
doctype | 当前, hugo支持的唯一文档类型是 `article`.
extensions | 可能的扩展包括 `asciidoctor-html5s`, `asciidoctor-bibtex`, `asciidoctor-diagram`, `asciidoctor-interdoc-reftext`, `asciidoctor-katex`, `asciidoctor-latex`, `asciidoctor-mathematical`, `asciidoctor-question`, `asciidoctor-rouge`.
attributes | 可以在AsciiDoc文件中引用的变量参数。这是值名称或者名称-值对的列表。参考 [Asciidoctor's attributes](https://asciidoctor.org/docs/asciidoc-syntax-quick-reference/#attributes-and-substitutions).
noHeaderOrFooter | 输出可嵌入的文档, 删除了除主体文档外一切，如头部、尾部。不要修改这个参数，除非您了解自己在做什么。
safeMode | 安全模式级别 `unsafe`, `safe`, `server` or `secure`. 不要修改这个参数，除非您了解自己在做什么。
sectionNumbers | 自动区块头部进行序数标记 Auto-number section titles.
verbose | 冗余打印处理信息和配置文件检查信息到标准错误输出stderr.
trace | 出错时包含回溯信息.
failureLevel | 引发非零退出码失败的最小日志级别.

Hugo还提供了不直接与Asciidoctor的命令行选项对应的额外的设置选项:

workingFolderCurrent
: 设置Hugo的工作目录为和ASciiDoc处理的文件目录一致，由此[include](https://asciidoctor.org/docs/asciidoc-syntax-quick-reference/#include-files)会在相对目录下工作。这个设置使用了 `asciidoctor`命令行参数`--base-dir` 和 属性 `outdir=`。对于使用[asciidoctor-diagram](https://asciidoctor.org/docs/asciidoctor-diagram/)输出图片,`workingFolderCurrent`必须设置为`true`.

preserveTOC
:默认情况下，Hugo会删除Asciidoctor生成的文章目录,并且通过内置变量[`.TableOfContents`](/content-management/toc/)来提供目录, 以便后期更多配置以及与多个Hugo主题的更好集成。这个选项可以设置成`true`来保留Asciidoctor生成的TOC目录在生成的页面中.

下表是Hugo内和AsciiDoc相关的所有设定以及它们的默认值:

{{< code-toggle config="markup.asciidocExt" />}}

设定扩展和属性的例子:

```
[markup.asciidocExt]
    extensions = ["asciidoctor-html5s", "asciidoctor-diagram"]
    workingFolderCurrent = true
    [markup.asciidocExt.attributes]
        my-base-url = "https://example.com/"
        my-attribute-name = "my value"
```

在复杂的Asciidoctor环境中，有时调试输出外部命令的真实的所有参数会有些帮助。
使用参数`-v`调用Hugo,您会得到类似下面输出:

```
INFO 2019/12/22 09:08:48 Rendering book-as-pdf.adoc with C:\Ruby26-x64\bin\asciidoctor.bat using asciidoc args [--no-header-footer -r asciidoctor-html5s -b html5s -r asciidoctor-diagram --base-dir D:\prototypes\hugo_asciidoc_ddd\docs -a outdir=D:\prototypes\hugo_asciidoc_ddd\build -] ...
```

## 学习 Markdown

学习markdown很简单，学习一会儿就够了。下面的开始学习markdown的优秀资源:

* [Daring Fireball: Markdown, John Gruber (Markdown的创建者)][fireball]
* [Markdown Cheatsheet, Adam Pritchard][mdcheatsheet]
* [Markdown Tutorial (Interactive), Garen Torikian][mdtutorial]
* [The Markdown Guide, Matt Cone][mdguide]

[`emojify` function]: /functions/emojify/
[ascii]: https://asciidoctor.org/
[bfconfig]: /getting-started/configuration/#configuring-blackfriday-rendering
[blackfriday]: https://github.com/russross/blackfriday
[mmark]: https://github.com/miekg/mmark
[config]: /getting-started/configuration/
[developer tools]: /tools/
[emojis]: https://www.webpagefx.com/tools/emoji-cheat-sheet/
[fireball]: https://daringfireball.net/projects/markdown/
[gfmtasks]: https://guides.github.com/features/mastering-markdown/#syntax
[helperssource]: https://github.com/gohugoio/hugo/blob/77c60a3440806067109347d04eb5368b65ea0fe8/helpers/general.go#L65
[hl]: /content-management/syntax-highlighting/
[hlsc]: /content-management/shortcodes/#highlight
[hugocss]: /css/style.css
[ietf]: https://tools.ietf.org/html/
[mathjaxdocs]: https://docs.mathjax.org/en/latest/
[mdcheatsheet]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
[mdguide]: https://www.markdownguide.org/
[mdtutorial]: https://www.markdowntutorial.com/
[Miek Gieben's website]: https://miek.nl/2016/march/05/mmark-syntax-document/
[mmark]: https://github.com/mmarkdown/mmark
[org]: https://orgmode.org/
[pandoc]: https://www.pandoc.org/
[Pygments]: https://pygments.org/
[rest]: https://docutils.sourceforge.io/rst.html
[sc]: /content-management/shortcodes/
[sct]: /templates/shortcode-templates/
