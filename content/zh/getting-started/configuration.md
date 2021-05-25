---
title: 配置
linktitle: 配置
description: 配置您的hugo站点.
date: 2013-07-01
publishdate: 2017-01-02
lastmod: 2017-03-05
categories: [getting started,fundamentals]
keywords: [configuration,toml,yaml,json]
menu:
  docs:
    parent: "getting-started"
    weight: 60
weight: 60
sections_weight: 60
draft: false
aliases: [/overview/source-directory/,/overview/configuration/]
toc: true
---


## 配置文件

Hugo使用 `config.toml`, `config.yaml`, 或者 `config.json`(如果在根目录中)作为默认的站点配置文件.

用户可以选择使用命令行`--config`参数开关将默认的配置文件重载为一个或者多个站点配置文件。

例如:

```
hugo --config debugconfig.toml
hugo --config a.toml,b.toml,c.toml
```

{{% note %}}
多站点配置文件可以使用逗号分割的字符串作为`--config` 开关的参数来声明
{{% /note %}}

{{< todo >}}
TODO: distinct config.toml and others (the root object files)
{{< /todo >}}

## 配置目录

使用唯一的站点配置文件之外, 您可以使用`configDir`配置文件目录(默认在 `config/`目录)来维持简单的文件组织和环境特定的设定。

- 目录内每个文件表示一个配置根对象，比如 `params.toml` 来定义 `[Params]`, 使用`menu(s).toml` 来配置 `[Menu]`, 使用`languages.toml` 来定义 `[Languages]` 等

- 每个文件内容必须是顶层对象，比如:

  在 `config.toml` 中:
  ```toml
  [Params]
    foo = "bar"
  ```
  在 `params.toml` 中:
  ```
  foo = "bar"
  ```
- 每个目录包含一组配置文件，包含环境特定的设置
- 文件可以本地化,变成语言特定版本


```
├── config
│   ├── _default
│   │   ├── config.toml
│   │   ├── languages.toml
│   │   ├── menus.en.toml
│   │   ├── menus.zh.toml
│   │   └── params.toml
│   ├── production
│   │   ├── config.toml
│   │   └── params.toml
│   └── staging
│       ├── config.toml
│       └── params.toml
```

考虑上面的配置文件目录结构, 当 运行 `hugo --environment staging`是Hugo将使用`config/_default`目录内的每个设定，然后在这特设定的基础上Merge `staging`内的设定

Considering the structure above, when running `hugo --environment staging`, Hugo will use every settings from `config/_default` and merge `staging`'s on top of those.

{{% note %}}
运行`hugo server`命令的默认环境目录是 __development__， 运行`hugo`命令的默认环境目录是__production__。
{{%/ note %}}

## 全部配置设置

下面列表列出全部hugo定义的变量和括号内它们的默认值。用户可以选择在它们站点配置文件中覆盖这些。

archetypeDir ("archetypes")
: Hugo查找原型文件(内容模板)的目录. {{% module-mounts-note %}}

assetDir ("assets")
: Hugo 查找在[Hugo Pipes](/hugo-pipes/)中使用的资源文件的目录。{{% module-mounts-note %}}

baseURL
: 相对于建站根目录的域名(和路径)比如, https://bep.is/

blackfriday
: 参考 [配置 Blackfriday](/getting-started/configuration-markup#blackfriday)

build
: 参考 [配置构建](#configure-build)

buildDrafts (false)
: 在构建时包含drafts草案内容.

buildExpired  (false)
: 在构建时包含过期内容.

buildFuture (false)
: 在构建时包含未来发布的内容.

caches
: 参考 [配置文件缓存](#configure-file-caches)

canonifyURLs (false)
: 启用可以将相对URL转化为绝对URL

contentDir ("content")
: Hugo读取内容文件的目录 {{% module-mounts-note %}}

dataDir ("data")
: Hugo读取数据文件的目录 {{% module-mounts-note %}}

defaultContentLanguage ("en")
:内容没有语言指示器时所属默认语言

defaultContentLanguageInSubdir (false)
: 输出子目录的默认内容语言,比如 `content/en/`。 站点根目录`/`会被重定向到 `/en/`

disableAliases (false)
: 禁用别名重定向的生成。请注意即使`disableAliases`被设置, 网页别名本身一样保存在网页中。这样的目的是生成`.htaccess`301重定向, netlify的重定向文件或者其他使用定制输出格式的页面

disableHugoGeneratorInject (false)
: Hugo会，默认情况下, 在HTML的头部插入生成器的元标签，仅仅在首页。可以关闭这个功能，但是我们很希望您不关闭，这是我们观察Hugo的受欢迎程度上升的一个好方法.

disableKinds ([])
: 禁用声明*类型*的所有页面, 列表中允许的值包括: `"page"`, `"home"`, `"section"`, `"taxonomy"`, `"term"`, `"RSS"`, `"sitemap"`, `"robotsTXT"`, `"404"`.

disableLiveReload (false)
: 禁用浏览器自动热加载.

disablePathToLower (false)
: 不转化url或路径到小写

enableEmoji (false)
: 对页面内容启用表情符号;参考[表情符号备忘录](https://www.webpagefx.com/tools/emoji-cheat-sheet/).

enableGitInfo (false)
: 对每个页面启用 `.GitInfo`(如果Hugo站点使用Git管理版本)。这样，在构建页面时会使用每个内容文件的最后git提交时间更新生成的页面的Lastmod参数值.

enableInlineShortcodes (false)
: 启用内联短代码支持.  参考 [内联短代码](/templates/shortcode-templates/#inline-shortcodes).

enableMissingTranslationPlaceholders (false)
: 当翻译文本缺失时，显示占位符而不是默认值或者空字符串

enableRobotsTXT (false)
: 启用生成`robots.txt` 文件.

frontmatter

: 参考 [前端设置配置](#configure-front-matter).

footnoteAnchorPrefix ("")
: 脚注锚的前缀

footnoteReturnLinkContents ("")
: 脚注返回链接上显示的文本

googleAnalytics ("")
: Google分析的追踪ID

hasCJKLanguage (false)
: 启用自动检测内容里面包含的中/日/韩语言。这对于中日韩文内容，`.Summary` 和 `.WordCount`函数表现正确有帮助。

imaging
: 参考 [图像处理配置](/content-management/image-processing/#image-processing-config).

languages
: 参考 [语言配置](/content-management/multilingual/#configure-languages).

languageCode ("")
: 站点语言代码。在默认的[RSS 模板](/templates/rss/#configure-rss)中使用, 并且在 [多语言站点](/content-management/multilingual/#configure-multilingual-multihost)中有用.

languageName ("")
: 站点语言名称

disableLanguages
: 参考 [禁用语言](/content-management/multilingual/#disable-a-language)

layoutDir ("layouts")
: Hugo读取布局(模板)的目录

log (false)
: 启用日志

logFile ("")
: 日志文件目录 (如果设置这个, 日志功能自动启用).

markup
: 参考 [配置 Markup](/getting-started/configuration-markup).{{< new-in "0.60.0" >}}

menu
: 参考 [添加非内容条目到菜单](/content-management/menus/#add-non-content-entries-to-a-menu).

minify
: 参考 [Configure Minify](#configure-minify)

module
: 模块配置参见 [模块配置](/hugo-modules/configuration/).{{< new-in "0.56.0" >}}

newContentEditor ("")
: 创建新内容时使用的编辑器

noChmod (false)
: 不同步文件的权限模式位

noTimes (false)
: 不同步文件的修改时间

paginate (10)
: 在分页时[pagination](/templates/pagination/)默认的每页元素数目。

paginatePath ("page")
: 分页时使用的路径(例如，https://example.com/page/2).

permalinks
: 参考 [内容管理](/content-management/urls/#permalinks)相应部分.

pluralizeListTitles (true)
: 复数化列表标题 Pluralize titles in lists.

publishDir ("public")
: hugo写入最后的静态站点(html的文件)的目录.

related
: 参考 [相关内容 ](/content-management/related/#configure-related-content).{{< new-in "0.27" >}}

relativeURLs (false)
: 启用后所有相对URL相对于内容根。注意这不会影响绝对URL。

refLinksErrorLevel ("ERROR")
: 使用 `ref` 和 `relref` 解析网页链接, 当一个链接无法解析时，记录日志的日志级别。可能的值包括`ERROR` (默认值) 或者 `WARNING`. 任何 `ERROR` 错误会导致编译失败。 (`exit -1`)。

refLinksNotFoundURL
:  解析`ref` or `relref`过程中某网页引用无法解析是使用的占位符URL。照原样使用。

rssLimit (unlimited)
: Rss feed内条目最大数量

sectionPagesMenu ("")
: 参考 [" 给疏于打理的博客主的区块菜单 "](/templates/menu-templates/#section-menu-for-lazy-bloggers).

sitemap
: 参考默认 [网站地图配置](/templates/sitemap-template/#configure-sitemapxml).

staticDir ("static")
: 目录或者目录列表，从其内Hugo读取 [静态文件][static-files]. {{% module-mounts-note %}}

summaryLength (70)
: 以词数统计的文本长度显示于 [`.Summary`](/content-management/summaries/#hugo-defined-automatic-summary-splitting)中

taxonomies
: 参考 [配置标签](/content-management/taxonomies#configure-taxonomies).

theme ("")
: 使用的主题 (默认位于 `/themes/THEMENAME/`).

themesDir ("themes")
: Hugo读取主题的目录

timeout (10000)
: 生成网页内容的超时值，以毫秒记(默认长度10秒)。 *注意:* 这个超时值是用于避免递归页面内容的生成。如果你的页面生成很慢的化(比如 由于大图片处理或者远程内容的原因), 您可能想提高这个值。


title ("")
: 站点标题

titleCaseStyle ("AP")
: 参考 [配置标题大小写](#configure-title-case)

uglyURLs (false)
: 启用创建丑URL, 创建形式如 `/filename.html`的URL而不是 `/filename/`.

verbose (false)
: 启用冗余输出

verboseLog (false)
: 启用冗余日志 Enable verbose logging.

watch (false)
: 监测文件系统变化, 有需要时候重建页面


{{% note %}}
如果在类unix机器上开发站点，下面是一些方便快捷的查询配置选项的命令行操作:
```
cd ~/sites/yourhugosite
hugo config | grep emoji
```

输出类似下面

```
enableemoji: true
```
{{% /note %}}

## 配置构建

{{< new-in "0.66.0" >}}

构建 `build` 配置部分包含全局的构建相关的配置选项.


{{< code-toggle file="config">}}
[build]
useResourceCacheWhen="fallback"
writeStats = false
noJSConfigInAssets = false
{{< /code-toggle >}}


useResourceCacheWhen
: 处理PostCSS 和 ToCSS时使用目录`/resources/_gen` 的时刻。有效的值包括 `never`, `always` 和 `fallback`。最后的fallback意指当PostCSS/extended无法获取时会尝试使用缓存。


writeStats {{< new-in "0.69.0" >}}
: When enabled, a file named `hugo_stats.json` will be written to your project root with some aggregated data about the build, e.g. list of HTML entities published to be used to do [CSS pruning](/hugo-pipes/postprocess/#css-purging-with-postcss). If you're only using this for the production build, you should consider placing it below [config/production](/getting-started/configuration/#configuration-directory). It's also worth mentioning that, due to the nature of the partial server builds, new HTML entities will be added when you add or change them while the server is running, but the old values will not be removed until you restart the server or run a regular `hugo` build.

noJSConfigInAssets {{< new-in "0.78.0" >}}
: Turn off writing a `jsconfig.json` into your `/assets` folder with mapping of imports from running [js.Build](https://gohugo.io/hugo-pipes/js). This file is intended to help with intellisense/navigation inside code editors such as [VS Code](https://code.visualstudio.com/). Note that if you do not use `js.Build`, no file will be written.

## 配置服务器

{{< new-in "0.67.0" >}}

仅仅在运行`hugo server`时相关的配置， 可以在开发过程中设置HTTP头信息，这样允许测试内容安全策略和其他点。配置模式和[Netlify的规则](https://docs.netlify.com/routing/headers/#syntax-for-the-netlify-configuration-file)匹配并带有稍微强大一点的[Glob matching](https://github.com/gobwas/glob)。

{{< code-toggle file="config">}}
[server]
[[server.headers]]
for = "/**.html"

[server.headers.values]
X-Frame-Options = "DENY"
X-XSS-Protection = "1; mode=block"
X-Content-Type-Options = "nosniff"
Referrer-Policy = "strict-origin-when-cross-origin"
Content-Security-Policy = "script-src localhost:1313"
{{< /code-toggle >}}

Since this is is "development only", it may make sense to put it below the `development` environment:
由于这仅仅"开发相关"，将这些置于`development`环境下比较合适。


{{< code-toggle file="config/development/server">}}
[[headers]]
for = "/**.html"

[headers.values]
X-Frame-Options = "DENY"
X-XSS-Protection = "1; mode=block"
X-Content-Type-Options = "nosniff"
Referrer-Policy = "strict-origin-when-cross-origin"
Content-Security-Policy = "script-src localhost:1313"
{{< /code-toggle >}}


{{< new-in "0.72.0" >}}

You can also specify simple redirects rules for the server. The syntax is again similar to Netlify's.

Note that a `status` code of 200 will trigger a [URL rewrite](https://docs.netlify.com/routing/redirects/rewrites-proxies/), which is what you want in SPA situations, e.g:

{{< code-toggle file="config/development/server">}}
[[redirects]]
from = "/myspa/**"
to = "/myspa/"
status = 200
force = false
{{< /code-toggle >}}

{{< new-in "0.76.0" >}} Setting `force=true` will make a redirect even if there is existing content in the path. Note that before Hugo 0.76  `force` was the default behaviour, but this is inline with how Netlify does it.

## Configure Title Case

Set `titleCaseStyle` to specify the title style used by the [title](/functions/title/) template function and the automatic section titles in Hugo. It defaults to [AP Stylebook](https://www.apstylebook.com/) for title casing, but you can also set it to `Chicago` or `Go` (every word starts with a capital letter).

## Configuration Environment Variables

HUGO_NUMWORKERMULTIPLIER
: Can be set to increase or reduce the number of workers used in parallel processing in Hugo. If not set, the number of logical CPUs will be used.

## Configuration Lookup Order

Similar to the template [lookup order][], Hugo has a default set of rules for searching for a configuration file in the root of your website's source directory as a default behavior:

1. `./config.toml`
2. `./config.yaml`
3. `./config.json`

In your `config` file, you can direct Hugo as to how you want your website rendered, control your website's menus, and arbitrarily define site-wide parameters specific to your project.


## Example Configuration

The following is a typical example of a configuration file. The values nested under `params:` will populate the [`.Site.Params`][] variable for use in [templates][]:

{{< code-toggle file="config">}}
baseURL: "https://yoursite.example.com/"
title: "My Hugo Site"
footnoteReturnLinkContents: "↩"
permalinks:
  posts: /:year/:month/:title/
params:
  Subtitle: "Hugo is Absurdly Fast!"
  AuthorName: "Jon Doe"
  GitHubUser: "spf13"
  ListOfFoo:
    - "foo1"
    - "foo2"
  SidebarRecentLimit: 5
{{< /code-toggle >}}

## Configure with Environment Variables

In addition to the 3 config options already mentioned, configuration key-values can be defined through operating system environment variables.

For example, the following command will effectively set a website's title on Unix-like systems:

```
$ env HUGO_TITLE="Some Title" hugo
```

This is really useful if you use a service such as Netlify to deploy your site. Look at the Hugo docs [Netlify configuration file](https://github.com/gohugoio/hugoDocs/blob/master/netlify.toml) for an example.

{{% note "Setting Environment Variables" %}}
Names must be prefixed with `HUGO_` and the configuration key must be set in uppercase when setting operating system environment variables.

To set config params, prefix the name with `HUGO_PARAMS_`
{{% /note %}}

{{< new-in "0.79.0" >}} If you are using snake_cased variable names, the above will not work, so since Hugo 0.79.0 Hugo determines the delimiter to use by the first character after `HUGO`. This allows you to define environment variables on the form `HUGOxPARAMSxAPI_KEY=abcdefgh`, using any [allowed](https://stackoverflow.com/questions/2821043/allowed-characters-in-linux-environment-variable-names#:~:text=So%20names%20may%20contain%20any,not%20begin%20with%20a%20digit.) delimiter.

{{< todo >}}
Test and document setting params via JSON env var.
{{< /todo >}}

## Ignore Content Files When Rendering

The following statement inside `./config.toml` will cause Hugo to ignore content files ending with `.foo` and `.boo` when rendering:

```
ignoreFiles = [ "\\.foo$", "\\.boo$" ]
```

The above is a list of regular expressions. Note that the backslash (`\`) character is escaped in this example to keep TOML happy.

## Configure Front Matter

### Configure Dates

Dates are important in Hugo, and you can configure how Hugo assigns dates to your content pages. You do this by adding a `frontmatter` section to your `config.toml`.


The default configuration is:

```toml
[frontmatter]
date = ["date", "publishDate", "lastmod"]
lastmod = [":git", "lastmod", "date", "publishDate"]
publishDate = ["publishDate", "date"]
expiryDate = ["expiryDate"]
```

If you, as an example, have a non-standard date parameter in some of your content, you can override the setting for `date`:

 ```toml
[frontmatter]
date = ["myDate", ":default"]
```

The `:default` is a shortcut to the default settings. The above will set `.Date` to the date value in `myDate` if present, if not we will look in `date`,`publishDate`, `lastmod` and pick the first valid date.

In the list to the right, values starting with ":" are date handlers with a special meaning (see below). The others are just names of date parameters (case insensitive) in your front matter configuration.  Also note that Hugo have some built-in aliases to the above: `lastmod` => `modified`, `publishDate` => `pubdate`, `published` and `expiryDate` => `unpublishdate`. With that, as an example, using `pubDate` as a date in front matter, will, by default, be assigned to `.PublishDate`.

The special date handlers are:


`:fileModTime`
: Fetches the date from the content file's last modification timestamp.

An example:

 ```toml
[frontmatter]
lastmod = ["lastmod", ":fileModTime", ":default"]
```


The above will try first to extract the value for `.Lastmod` starting with the `lastmod` front matter parameter, then the content file's modification timestamp. The last, `:default` should not be needed here, but Hugo will finally look for a valid date in `:git`, `date` and then `publishDate`.


`:filename`
: Fetches the date from the content file's filename. For example, `2018-02-22-mypage.md` will extract the date `2018-02-22`. Also, if `slug` is not set, `mypage` will be used as the value for `.Slug`.

An example:

```toml
[frontmatter]
date  = [":filename", ":default"]
```

The above will try first to extract the value for `.Date` from the filename, then it will look in front matter parameters `date`, `publishDate` and lastly `lastmod`.


`:git`
: This is the Git author date for the last revision of this content file. This will only be set if `--enableGitInfo` is set or `enableGitInfo = true` is set in site config.

## Configure Additional Output Formats

Hugo v0.20 introduced the ability to render your content to multiple output formats (e.g., to JSON, AMP html, or CSV). See [Output Formats][] for information on how to add these values to your Hugo project's configuration file.

## Configure Minify

{{< new-in "0.68.0" >}}

Default configuration:

{{< code-toggle config="minify" />}}

## Configure File Caches

Since Hugo 0.52 you can configure more than just the `cacheDir`. This is the default configuration:

{{< code-toggle >}}
[caches]
[caches.getjson]
dir = ":cacheDir/:project"
maxAge = -1
[caches.getcsv]
dir = ":cacheDir/:project"
maxAge = -1
[caches.images]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.assets]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.modules]
dir = ":cacheDir/modules"
maxAge = -1
{{< /code-toggle >}}

You can override any of these cache settings in your own `config.toml`.

### The keywords explained

`:cacheDir`
: This is the value of the `cacheDir` config option if set (can also be set via OS env variable `HUGO_CACHEDIR`). It will fall back to `/opt/build/cache/hugo_cache/` on Netlify, or a `hugo_cache` directory below the OS temp dir for the others. This means that if you run your builds on Netlify, all caches configured with `:cacheDir` will be saved and restored on the next build. For other CI vendors, please read their documentation. For an CircleCI example, see [this configuration](https://github.com/bep/hugo-sass-test/blob/6c3960a8f4b90e8938228688bc49bdcdd6b2d99e/.circleci/config.yml).

`:project`
: The base directory name of the current Hugo project. This means that, in its default setting, every project will have separated file caches, which means that when you do `hugo --gc` you will not touch files related to other Hugo projects running on the same PC.

`:resourceDir`
: This is the value of the `resourceDir` config option.

maxAge
: This is the duration before a cache entry will be evicted, -1 means forever and 0 effectively turns that particular cache off. Uses Go's `time.Duration`, so valid values are `"10s"` (10 seconds), `"10m"` (10 minutes) and `"10h"` (10 hours).

dir
: The absolute path to where the files for this cache will be stored. Allowed starting placeholders are `:cacheDir` and `:resourceDir` (see above).

## Configuration Format Specs

* [TOML Spec][toml]
* [YAML Spec][yaml]
* [JSON Spec][json]

[`.Site.Params`]: /variables/site/
[directory structure]: /getting-started/directory-structure
[json]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "Specification for JSON, JavaScript Object Notation"
[lookup order]: /templates/lookup-order/
[Output Formats]: /templates/output-formats/
[templates]: /templates/
[toml]: https://github.com/toml-lang/toml
[yaml]: https://yaml.org/spec/
[static-files]: /content-management/static-files/
