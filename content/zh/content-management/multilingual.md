---
title: 多语言模式和国际化
linktitle: 多语言模式 和 国际化i18n
description: Hugo支持多语言并行的网站创建.
date: 2017-01-10
publishdate: 2017-01-10
lastmod: 2017-01-10
categories: [content management]
keywords: [multilingual,i18n, internationalization]
menu:
  docs:
    parent: "content-management"
    weight: 150
weight: 150	#rem
draft: false
aliases: [/content/multilingual/,/tutorials/create-a-multilingual-site/]
toc: true
---

You should define the available languages in a `languages` section in your site configuration.
在站点配置文件中定义一个 `languages` 部分, 定义可用的语言.

> 也请参考 [Hugo Multilingual Part 1: Content translation](https://regisphilibert.com/blog/2018/08/hugo-multilingual-part-1-managing-content-translation/)

## 配置语言 Configure Languages

下面是多语言Hugo站点的站点配置文件的例子:

{{< code-toggle file="config" >}}
defaultContentLanguage = "en"
copyright = "Everything is mine"

[params]
[params.navigation]
help  = "Help"

[languages]
[languages.en]
title = "My blog"
weight = 1
[languages.en.params]
linkedin = "https://linkedin.com/whoever"

[languages.fr]
title = "Mon blogue"
weight = 2
[languages.fr.params]
linkedin = "https://linkedin.com/fr/whoever"
[languages.fr.params.navigation]
help  = "Aide"

[languages.ar]
title = "مدونتي"
weight = 2
languagedirection = "rtl"

[languages.pt-pt]
title = "O meu blog"
weight = 3
{{< /code-toggle >}}

Anything not defined in a `languages` block will fall back to the global value for that key (e.g., `copyright` for the English `en` language). This also works for `params`, as demonstrated with `help` above: You will get the value `Aide` in French and `Help` in all the languages without this parameter set.
任何不在`languages`配置块内定义的键会退回使用键的全局值(如英语`en`中的`copyright`). 这个规则同样适用于`Params`, 像例子中`help`展示的那样: 在法语中`help`的值是`Aide`,在其他没有设置这个参数的语言中为`Help`.

With the configuration above, all content, sitemap, RSS feeds, paginations,
and taxonomy pages will be rendered below `/` in English (your default content language) and then below `/fr` in French.
基于上面配置, 所有内容、sitemap、RSS feeds、导航和标签页在英语中会呈现在`/`路径下, 在法语中会在`/fr`路径下.

When working with front matter `Params` in [single page templates][singles], omit the `params` in the key for the translation.
在[single page templates][singles]单页模板中处理前言设定的`Params`时, 请忽略翻译键中的`params`.

`defaultContentLanguage` sets the project's default language. If not set, the default language will be `en`.
`defaultContentLanguage`设置项目的缺省语言。如果不设置, 默认语言是`en`.

If the default language needs to be rendered below its own language code (`/en`) like the others, set `defaultContentLanguageInSubdir: true`.
如果默认语言需要同其他语言一样呈现在自己的语言代码(`/en`)下面, 请设置`defaultContentLanguageInSubdir:true`

Only the obvious non-global options can be overridden per language. Examples of global options are `baseURL`, `buildDrafts`, etc.
仅仅不是明显的全局的选项可以在每个语言中重载. 全局选项包括 `baseURL`, `buildDrafts`等.

**请注意:** 使用小写的语言代码,即使在使用地区语言时也是如此(比如. 使用 pt-pt 代替 pt-PT).
Currently Hugo language internals lowercase language codes, which can cause conflicts with settings like `defaultContentLanguage` which are not lowercased. Please track the evolution of this issue in [Hugo repository issue tracker](https://github.com/gohugoio/hugo/issues/7344)


### 禁用语言 Disable a Language

You can disable one or more languages. This can be useful when working on a new translation.
可以禁用一个或多个语言. 这在翻译的时候很有用.

```toml
disableLanguages = ["fr", "ja"]
```

Note that you cannot disable the default content language.
注意并不能禁用默认的内容语言

We kept this as a standalone setting to make it easier to set via [OS environment](/getting-started/configuration/#configure-with-environment-variables):
为在[OS environment](/getting-started/configuration/#configure-with-environment-variables)
操作系统环境中设置简单，我们维持禁用语言的独立设置方式.

```bash
HUGO_DISABLELANGUAGES="fr ja" hugo
```
如果已经在`config.toml`中禁用了几种语言，可以在开发时这样设置启用它们:

```bash
HUGO_DISABLELANGUAGES=" " hugo server
```


### 配置多语言多站点 Configure Multilingual Multihost

From **Hugo 0.31** we support multiple languages in a multihost configuration. See [this issue](https://github.com/gohugoio/hugo/issues/4027) for details.
从Hugo 0.31开始 hugo支持多语言多站点配置. 更多细节请参考[this issue](https://github.com/gohugoio/hugo/issues/4027)

This means that you can now configure a `baseURL` per `language`:
这意味着您可以给每个语言配置一个`baseURL`


> If a `baseURL` is set on the `language` level, then all languages must have one and they must all be different.
如果在 `language` 层级上设置了`baseURL`, 那么所有语言都必须设置并且不能相同:

Example:

{{< code-toggle file="config" >}}
[languages]
[languages.fr]
baseURL = "https://example.fr"
languageName = "Français"
weight = 1
title = "En Français"

[languages.en]
baseURL = "https://example.com"
languageName = "English"
weight = 2
title = "In English"
{{</ code-toggle >}}

With the above, the two sites will be generated into `public` with their own root:
使用上面设置, 会在public目录下生成具有自己根目录的两个站点:

```bash
public
├── en
└── fr
```

**All URLs (i.e `.Permalink` etc.) will be generated from that root. So the English home page above will have its `.Permalink` set to `https://example.com/`.**
**所有URLs (i.e `.Permalink` etc.) 会相对于根生成. 因此English首页的`.Permalink` 会被设置成  `https://example.com/`**


When you run `hugo server` we will start multiple HTTP servers. You will typically see something like this in the console:
当我们使用 `hugo server` 命令时，Hugo会启动多个HTTP服务器. 在命令行界面您会看到类似的输出:

```bash
Web Server is available at 127.0.0.1:1313 (bind address 127.0.0.1)
Web Server is available at 127.0.0.1:1314 (bind address 127.0.0.1)
Press Ctrl+C to stop
```

Live reload and `--navigateToChanged` between the servers work as expected.
热加载和服务器间的 `--navigateToChanged` 如预期工作。

### Taxonomies and Blackfriday
标签分类 和 Blackfriday设置

Taxonomies and [Blackfriday configuration][config] can also be set per language:
Taxonomies and [Blackfriday configuration][config]也能够分语言设置:

{{< code-toggle file="config" >}}
[Taxonomies]
tag = "tags"

[blackfriday]
angledQuotes = true
hrefTargetBlank = true

[languages]
[languages.en]
weight = 1
title = "English"
[languages.en.blackfriday]
angledQuotes = false

[languages.fr]
weight = 2
title = "Français"
[languages.fr.Taxonomies]
plaque = "plaques"
{{</ code-toggle >}}

## 内容翻译 Translate Your Content

There are two ways to manage your content translations. Both ensure each page is assigned a language and is linked to its counterpart translations.
管理内容翻译有两种方式. 每种方式都保证了每个页面被赋予了语言并且被链接到其他翻译版本。

### 按文件名称翻译 Translation by filename

考虑下面例子:

1. `/content/about.en.md`
2. `/content/about.fr.md`

第一个文件是英语的,有链接到法语。
第二个文件是法语的,有链接到英语。

Their language is __assigned__ according to the language code added as a __suffix to the filename__.
文件的语言值通过文件名的后缀来__赋予__.

By having the same **path and base filename**, the content pieces are __linked__ together as translated pages.
通过具有相同的路径和基础文件名称, 多语言内容文件和他们的翻译页面链接在一起.

{{< note >}}
If a file has no language code, it will be assigned the default language.
如果文件没有语言代码, 它会被赋予默认的语言代码。
{{</ note >}}

### 按内容目录翻译 Translation by content directory

This system uses different content directories for each of the languages. Each language's content directory is set using the `contentDir` param.
这个系统对每个语言使用不同的内容目录。每个语言的内容目录通过`contentDir` 参数设定。

{{< code-toggle file="config" >}}

languages:
  en:
    weight: 10
    languageName: "English"
    contentDir: "content/english"
  fr:
    weight: 20
    languageName: "Français"
    contentDir: "content/french"

{{< /code-toggle >}}

The value of `contentDir` can be any valid path -- even absolute path references. The only restriction is that the content directories cannot overlap.
 `contentDir` 的值可以是任何有效滤镜 -- 即使是绝对路径引用也行. 唯一的限制是内容目录不能有重叠.

Considering the following example in conjunction with the configuration above:
结合上面的配置，考虑下面例子:

1. `/content/english/about.md`
2. `/content/french/about.md`

第一个文件是英语的,有链接到第二个法语的。
第二个文件是法语的,有链接到第一个英语的。


Their language is __assigned__ according to the content directory they are __placed__ in.
文件的语言根据文件__所在__目录被__赋予__.  

By having the same **path and basename** (relative to their language content directory), the content pieces are __linked__ together as translated pages.
通过具有相同的路径和基础文件名称(相对于他们语言内容目录), 多语言内容文件和他们的翻译页面__链接__在一起.


### 忽略默认链接 Bypassing default linking.

Any pages sharing the same `translationKey` set in front matter will be linked as translated pages regardless of basename or location.
任何页面如果在前言设定中设置了相同的 `translationKey` 键值, 他们会被链接在一起，并不考虑他们的基础名称或者位置。


考虑下面例子:

1. `/content/about-us.en.md`
2. `/content/om.nn.md`
3. `/content/presentation/a-propos.fr.md`

```yaml
# 在所有三个页面都设置
translationKey: "about"
```

By setting the `translationKey` front matter param to `about` in all three pages, they will be __linked__ as translated pages.
因为在所有三个页面的前言设定都设置了 `translationKey`的相同值, 他们会作为翻译页面__链接__在一起。


### 永久链接本地化 Localizing permalinks

Because paths and filenames are used to handle linking, all translated pages will share the same URL (apart from the language subdirectory).
因为路径和文件名称用于处理链接, 所有的翻译页面会共享相同的URL(语言子目录除外)

To localize the URLs, the [`slug`]({{< ref "/content-management/organization/index.md#slug" >}}) or [`url`]({{< ref "/content-management/organization/index.md#url" >}}) front matter param can be set in any of the non-default language file.
需要本地化URLs, 前言设定的[`slug`]({{< ref "/content-management/organization/index.md#slug" >}}) or [`url`]({{< ref "/content-management/organization/index.md#url" >}})参数可以在非默认语言文件中设定。

For example, a French translation (`content/about.fr.md`) can have its own localized slug.
一个例子, 法国翻译版本(`content/about.fr.md`)可以具有它自己的本地化slug:

{{< code-toggle >}}
Title: A Propos
slug: "a-propos"
{{< /code-toggle >}}


At render, Hugo will build both `/about/` and `/fr/a-propos/` while maintaining their translation linking.
呈现时, Hugo会创建两个`/about/` 和 `/fr/a-propos/`, 同时维持他们的翻译链接.

{{% note %}}
如果使用`url`, 记得也要包含语言部分:`/fr/compagnie/a-propos/`.
{{%/ note %}}

### 页面包 Page Bundles

To avoid the burden of having to duplicate files, each Page Bundle inherits the resources of its linked translated pages' bundles except for the content files (markdown files, html files etc...).
为避免文件重复的负担,每个页面包继承了它链接的翻译页面包的资源文件，除了内容文件(markdown, html文件等)

Therefore, from within a template, the page will have access to the files from all linked pages' bundles.
因此，在模板内,页面可以访问所有链接的页面包的资源。

If, across the linked bundles, two or more files share the same basename, only one will be included and chosen as follows:
如果在链接的多个包内, 有两个或者多个文件具有相同的基础名称, 仅仅有一个会被包含，选择顺序如下:

* 当前语言包的文件, 如果存在.
* 遍历按照语言`Weight`排序的包时第一个遇到的文件.

{{% note %}}
页面包资源遵循内容文件同样的语言赋值逻辑. 通过文件名称(`image.jpg`, `image.fr.jpg`)和通过目录(`english/about/header.jpg`, `french/about/header.jpg`).
{{%/ note %}}

## 引用翻译内容 Reference the Translated Content

To create a list of links to translated content, use a template similar to the following:
创建到达翻译内容的链接列表, 使用类似下面的模板:

{{< code file="layouts/partials/i18nlist.html" >}}
{{ if .IsTranslated }}
<h4>{{ i18n "translations" }}</h4>
<ul>
    {{ range .Translations }}
    <li>
        <a href="{{ .Permalink }}">{{ .Lang }}: {{ .Title }}{{ if .IsPage }} ({{ i18n "wordCount" . }}){{ end }}</a>
    </li>
    {{ end }}
</ul>
{{ end }}
{{< /code >}}

The above can be put in a `partial` (i.e., inside `layouts/partials/`) and included in any template, whether a [single content page][contenttemplate] or the [homepage][]. It will not print anything if there are no translations for a given page.
上面代码可以放在一个`partial`模板中(在 `layouts/partials/`目录中), 可以被任何模板包含，一个[独立内容页][contenttemplate] 或者 [首页][homepage]模板。如果特定页面没有任何翻译页面, Hugo不会打印任何内容.

上面使用了[`i18n` function][i18func], 下节介绍.

### 列出所有语言 List All Available Languages

`.AllTranslations` on a `Page` can be used to list all translations, including the page itself. On the home page it can be used to build a language navigator:
 可以使用`Page` 页面变量`.AllTranslations` 获得所有翻译, 包括页面本身。
在首页可以用来构建语言导航器.


{{< code file="layouts/partials/allLanguages.html" >}}
<ul>
{{ range $.Site.Home.AllTranslations }}
<li><a href="{{ .Permalink }}">{{ .Language.LanguageName }}</a></li>
{{ end }}
</ul>
{{< /code >}}

## 字符串翻译 Translation of Strings

Hugo uses [go-i18n][] to support string translations. [See the project's source repository][go-i18n-source] to find tools that will help you manage your translation workflows.
Hugo使用 [go-i18n][] 支持字符串翻译. [参考项目的代码库][go-i18n-source] 获得可以帮助你管理翻译工作流的工具信息.

Translations are collected from the `themes/<THEME>/i18n/` folder (built into the theme), as well as translations present in `i18n/` at the root of your project. In the `i18n`, the translations will be merged and take precedence over what is in the theme folder. Language files should be named according to [RFC 5646][] with names such as `en-US.toml`, `fr.toml`, etc.
翻译的字符串从`themes/<THEME>/i18n/`(集成进主题中)目录汇总, 还有在项目根的`i18n`目录中的字符串翻译。在i18n目录的字符串翻译会被合并，优先于主题目录中的翻译. 语言文件应该按照 [RFC 5646][]命名, 文件名称类似 `en-US.toml`, `fr.toml`等.

{{% note %}}
从Hugo 0.31开始，不需要使用使用有效的语言代码. 可以是任何东西, 参考 https://github.com/gohugoio/hugo/issues/3564
{{% /note %}}

### Query basic translation

在模板内, 这样使用 `i18n` 函数:

```
{{ i18n "home" }}
```

The function will search for the `"home"` id from `i18n/en-US.toml` file
函数会在`i18n/en-US.toml`文件中查询id  `"home"`:

```
[home]
other = "Home"
```

The result will be

```
Home
```


###  查询带变量的字符串 Query a flexible translation with variables

经常会想在翻译字符串中使用页面变量. 可以在调用`i18n`的时候传送`.`上下文变量给函数。:

```
{{ i18n "wordCount" . }}
```

函数会将 `.` 上下文传递给 `i18n/en-US.toml` 中的`"wordCount"` id:

```
[wordCount]
other = "This article has {{ .WordCount }} words."
```

Assume `.WordCount` in the context has value is 101. The result will be:
假设上下文的`.WordCount` 具有值101. 输出结果如下:

```
This article has 101 words.
```

### 查询单数/复数翻译 Query a singular/plural translation

In order to meet singular/plural requirement, you must pass a dictionary (map) with a numeric `.Count` property to the `i18n` function. The below example uses `.ReadingTime` variable which has a built-in `.Count` property.

```
{{ i18n "readingTime" .ReadingTime }}
```

The function will read `.Count` from `.ReadingTime` and evaluate where the number is singular (`one`) or plural (`other`). After that, it will pass to `readingTime` id in `i18n/en-US.toml` file:

```
[readingTime]
one = "One minute to read"
other = "{{.Count}} minutes to read"
```

Assume `.ReadingTime.Count` in the context has value of 525600. The result will be:

```
525600 minutes to read
```

If `.ReadingTime.Count` in the context has value is 1. The result is:

```
One minutes to read
```

In case you need to pass custom data: (`(dict "Count" 25)` is minimum requirement)

```
{{ i18n "readingTime" (dict "Count" 25 "FirstArgument" true "SecondArgument" false "Etc" "so on, so far") }}
```


## 定制日期 Customize Dates

At the time of this writing, Go does not yet have support for internationalized locales for dates, but if you do some work, you can simulate it. For example, if you want to use French month names, you can add a data file like ``data/mois.yaml`` with this content:
在撰写本文档时,Go语言并没有支持日期的国际化语言环境, 不过通过一些简单工作，可以模拟类似效果. 比如, 想使用法国的月份名称, 您可以添加一个数据文件如 ``data/mois.yaml``具有以下内容:

~~~yaml
1: "janvier"
2: "février"
3: "mars"
4: "avril"
5: "mai"
6: "juin"
7: "juillet"
8: "août"
9: "septembre"
10: "octobre"
11: "novembre"
12: "décembre"
~~~

...then index the non-English date names in your templates like so:
然后在模板中引用非英语日期名称，类似这样:

~~~html
<time class="post-date" datetime="{{ .Date.Format `2006-01-02T15:04:05Z07:00` | safeHTML }}">
  Article publié le {{ .Date.Day }} {{ index $.Site.Data.mois (printf "%d" .Date.Month) }} {{ .Date.Year }} (dernière modification le {{ .Lastmod.Day }} {{ index $.Site.Data.mois (printf "%d" .Lastmod.Month) }} {{ .Lastmod.Year }})
</time>
~~~

This technique extracts the day, month and year by specifying ``.Date.Day``, ``.Date.Month``, and ``.Date.Year``, and uses the month number as a key, when indexing the month name data file.

通过声明 ``.Date.Day``, ``.Date.Month``, and ``.Date.Year``, 此方法提取了日、月份、年份. 使用月份数字作为Key,
从数据文件索引月份名称.

## 菜单 Menus

You can define your menus for each language independently. Creating multilingual menus works just like [creating regular menus][menus], except they're defined in language-specific blocks in the configuration file:
可以独立为每个语言定义菜单. 创建多语言菜单和[创建普通菜单][menus],
除了他们是定义在配置文件的语言特定区块内:

```
defaultContentLanguage = "en"

[languages.en]
weight = 0
languageName = "English"

[[languages.en.menu.main]]
url    = "/"
name   = "Home"
weight = 0


[languages.de]
weight = 10
languageName = "Deutsch"

[[languages.de.menu.main]]
url    = "/"
name   = "Startseite"
weight = 0
```

The rendering of the main navigation works as usual. `.Site.Menus` will just contain the menu in the current language. Note that `absLangURL` below will link to the correct locale of your website. Without it, menu entries in all languages would link to the English version, since it's the default content language that resides in the root directory.

主导航的呈现同往常一样. `.Site.Menus`仅包含当前语言的菜单。
注意, 下面的`absLangURL`会链接到网站正确语言的链接. 如果不使用这个函数, 所有语言中的菜单条目都会指向英文版本, 这是由于默认内容语言位于根目录.

```
<ul>
    {{- $currentPage := . -}}
    {{ range .Site.Menus.main -}}
    <li class="{{ if $currentPage.IsMenuCurrent "main" . }}active{{ end }}">
        <a href="{{ .URL | absLangURL }}">{{ .Name }}</a>
    </li>
    {{- end }}
</ul>

```

## Missing Translations

If a string does not have a translation for the current language, Hugo will use the value from the default language. If no default value is set, an empty string will be shown.
如果字符串在当前语言没有翻译,Hugo会使用默认语言的值. 如果没有缺省值, 会显示空字符串.

While translating a Hugo website, it can be handy to have a visual indicator of missing translations. The [`enableMissingTranslationPlaceholders` configuration option][config] will flag all untranslated strings with the placeholder `[i18n] identifier`, where `identifier` is the id of the missing translation.
当您翻译hugo站点是，能够又一个缺失翻译的视觉提示会很好。
设置[`enableMissingTranslationPlaceholders` configuration option][config]配置选项会在所有未翻译字符串的位置标记出来, 填入占位符`[i18n] identifier`，这个`identifier`是未翻译的字符串的Id.


{{% note %}}
注意hugo会生成缺失翻译的占位符。这对生产环境来说不合适.
{{% /note %}}

For merging of content from other languages (i.e. missing content translations), see [lang.Merge](/functions/lang.merge/).
对于从其他语言并入内容(缺少内容翻译),参考[lang.Merge](/functions/lang.merge/)

To track down missing translation strings, run Hugo with the `--i18n-warnings` flag:
未跟踪缺失翻译字符串,可以在运行hugo命令时使用 `--i18n-warnings` 标记:

```
 hugo --i18n-warnings | grep i18n
i18n|MISSING_TRANSLATION|en|wordCount
```

## 多语言的主题支持 Multilingual Themes support

To support Multilingual mode in your themes, some considerations must be taken for the URLs in the templates. If there is more than one language, URLs must meet the following criteria:

* Come from the built-in `.Permalink` or `.RelPermalink`
* Be constructed with the [`relLangURL` template function][rellangurl] or the [`absLangURL` template function][abslangurl] **OR** be prefixed with `{{ .LanguagePrefix }}`

If there is more than one language defined, the `LanguagePrefix` variable will equal `/en` (or whatever your `CurrentLanguage` is). If not enabled, it will be an empty string (and is therefore harmless for single-language Hugo websites).

[abslangurl]: /functions/abslangurl
[config]: /getting-started/configuration/
[contenttemplate]: /templates/single-page-templates/
[go-i18n-source]: https://github.com/nicksnyder/go-i18n
[go-i18n]: https://github.com/nicksnyder/go-i18n
[homepage]: /templates/homepage/
[i18func]: /functions/i18n/
[menus]: /content-management/menus/
[rellangurl]: /functions/rellangurl
[RFC 5646]: https://tools.ietf.org/html/rfc5646
[singles]: /templates/single-page-templates/
