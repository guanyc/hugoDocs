---
title: 数据模板 Data Templates
linktitle: 数据模板
description: 在Hugo内建的变量外, 您可以在模板或者短代码中声明自己的定制数据,可以从本地数据源或者远程动态数据源获取数据 In addition to Hugo's built-in variables, you can specify your own custom data in templates or shortcodes that pull from both local and dynamic sources.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-03-12
categories: [templates]
keywords: [data,dynamic,csv,json,toml,yaml]
menu:
  docs:
    parent: "templates"
    weight: 80
weight: 80
sections_weight: 80
draft: false
aliases: [/extras/datafiles/,/extras/datadrivencontent/,/doc/datafiles/]
toc: true
---

<!-- begin data files -->

Hugo supports loading data from YAML, JSON, and TOML files located in the `data` directory in the root of your Hugo project.
Hugo支持从位于hugo项目根目录下的**data**目录的YAML、JSON或者TOML文件中读取数据

{{< youtube FyPgSuwIMWQ >}}

## 数据目录 The Data Folder

The `data` folder is where you can store additional data for Hugo to use when generating your site. Data files aren't used to generate standalone pages; rather, they're meant to be supplemental to content files. This feature can extend the content in case your front matter fields grow out of control. Or perhaps you want to show a larger dataset in a template (see example below). In both cases, it's a good idea to outsource the data in their own files.
`data`目录是存储hugo 创建站点时hugo使用的其他数据的目录。Data数据文件不用于生成单独页面,他们是内容文件的补充。这个特性在前言设定字段增长太多失去控制时，可以优雅的扩展内容文件。或者可能您想在模板中显示非常大的数据集(参考下面). 这两种情形中，将数据保存在它们自己的文件中都是好主意.

These files must be YAML, JSON, or TOML files (using the `.yml`, `.yaml`, `.json`, or `.toml` extension). The data will be accessible as a `map` in the `.Site.Data` variable.
这些文件必须是YAML、JSON、或者TOML格式的文件(使用`.yml`, `.yaml`, `.json` 或者 `.toml`文件扩展名).
数据可以作为`map`通过`.Site.Data`变量访问.

## 主题中使用数据文件 Data Files in Themes

Data Files can also be used in [Hugo themes][themes] but note that theme data files follow the same logic as other template files in the [Hugo lookup order][lookup] (i.e., given two files with the same name and relative path, the file in the root project `data` directory will override the file in the `themes/<THEME>/data` directory).
数据文件也可以使用在[Hugo Themes][themes]中, 但是请注意主题的数据文件遵循和其他模板数据的[Hugo lookup order][lookup]解析查询顺序相同的逻辑(比如,假设两个文件具有相同的名称和相对路径,在项目根目录的`data`目录中的文件会具有较高优先级、重载`themes/<THEME>/data`目录中的数据文件).

Therefore, theme authors should take care to not include data files that could be easily overwritten by a user who decides to [customize a theme][customize]. For theme-specific data items that shouldn't be overridden, it can be wise to prefix the folder structure with a namespace; e.g. `mytheme/data/<THEME>/somekey/...`. To check if any such duplicate exists, run hugo with the `-v` flag.
因此、主题作者需要注意不要包含可能会很容易被决定[定制主题][customize]的用户重载的数据文件。对于主题特定的数据单元不应该被重载的，最好使用目录结构的前缀作为命名空间;比如`mytheme/data/<THEME>/somekey...`. 为检查是否包含这样的重复数据文件,请使用`-v`标志运行Hugo. 

The keys in the map created with data templates from data files will be a dot-chained set of `path`, `filename`, and `key` in file (if applicable).
从数据文件中读取的数据模板中创建的字典键是`path`、`filename`、和文件中`key`(如果可以提取的)的点`.`链.

This is best explained with an example:
使用例子解释最好:

## 例子 Example: Jaco Pastorius' Solo Discography

[Jaco Pastorius](https://en.wikipedia.org/wiki/Jaco_Pastorius_discography) was a great bass player, but his solo discography is short enough to use as an example. [John Patitucci](https://en.wikipedia.org/wiki/John_Patitucci) is another bass giant.
[Jaco Pastorius](https://en.wikipedia.org/wiki/Jaco_Pastorius_discography) 是一名伟大的贝斯手, 但是他的个人唱片很短可以作为例子使用. [John Patitucci](https://en.wikipedia.org/wiki/John_Patitucci) 是另一名贝斯巨匠.

The example below is a bit contrived, but it illustrates the flexibility of data Files. This example uses TOML as its file format with the two following data files:
下面的例子编造的有些牵强,但是它很好的展示了数据文件的可适应性. 这个例子使用TOML作为文件格式的下面的两个数据文件:

* `data/jazz/bass/jacopastorius.toml`
* `data/jazz/bass/johnpatitucci.toml`

`jacopastorius.toml` contains the content below. `johnpatitucci.toml` contains a similar list:
文件`jacopastorius.toml` 包含内容如下, 文件`johnpatitucci.toml` 包含类似的列表:

```
discography = [
"1974 – Modern American Music … Period! The Criteria Sessions",
"1974 – Jaco",
"1976 - Jaco Pastorius",
"1981 - Word of Mouth",
"1981 - The Birthday Concert (released in 1995)",
"1982 - Twins I & II (released in 1999)",
"1983 - Invitation",
"1986 - Broadway Blues (released in 1998)",
"1986 - Honestly Solo Live (released in 1990)",
"1986 - Live In Italy (released in 1991)",
"1986 - Heavy'n Jazz (released in 1992)",
"1991 - Live In New York City, Volumes 1-7.",
"1999 - Rare Collection (compilation)",
"2003 - Punk Jazz: The Jaco Pastorius Anthology (compilation)",
"2007 - The Essential Jaco Pastorius (compilation)"
]
```

The list of bass players can be accessed via `.Site.Data.jazz.bass`, a single bass player by adding the filename without the suffix, e.g. `.Site.Data.jazz.bass.jacopastorius`.
贝斯手的列表可以通过变量 `.Site.Data.jazz.bass`访问, 单独的贝斯手可以通过在变量后面加上文件名后缀访问, 如`.Site.Data.jazz.bass.jacopastorius`.

You can now render the list of recordings for all the bass players in a template:
现在可以访问模板中所有贝斯手的唱片列表:

```
{{ range $.Site.Data.jazz.bass }}
   {{ partial "artist.html" . }}
{{ end }}
```

And then in the `partials/artist.html`:

然后在文件`partials/artist.html`中:

```
<ul>
{{ range .discography }}
  <li>{{ . }}</li>
{{ end }}
</ul>
```

Discover a new favorite bass player? Just add another `.toml` file in the same directory.
发现了另一位喜欢的贝斯手? 只要在相同目录中添加另外一个`toml`文件.

## 例子:访问数据文件的有名值 Example: Accessing Named Values in a Data File

Assume you have the following data structure in your `User0123.[yml|toml|json]` data file located directly in `data/`:
假设您在目录`data/`中的数据文件`User0123.[yml|toml|json]` 中具有如下数据结i: 

{{< code-toggle file="User0123" >}}
Name: User0123
"Short Description": "He is a **jolly good** fellow."
Achievements:
  - "Can create a Key, Value list from Data File"
  - "Learns Hugo"
  - "Reads documentation"
{{</ code-toggle >}}

You can use the following code to render the `Short Description` in your layout::
可以使用下面代码在布局中呈现`Short Description`: 

```
<div>Short Description of {{.Site.Data.User0123.Name}}: <p>{{ index .Site.Data.User0123 "Short Description" | markdownify }}</p></div>
```

Note the use of the [`markdownify` template function][markdownify]. This will send the description through the Blackfriday Markdown rendering engine.
注意 [`markdownify` 模板函数][markdownify]的使用. 这会将描述文本发送给markdown呈现引擎.

<!-- begin "Data-drive Content" page -->

## Data-Driven Content 数据驱动的内容

In addition to the [data files](/extras/datafiles/) feature, Hugo also has a "data-driven content" feature, which lets you load any [JSON](https://www.json.org/) or [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) file from nearly any resource.

在 [数据文件](/extras/datafiles/) 特性之外, Hugo 也支持"数据驱动内容" 的特性,此特性使得您可以从几乎任何资源处加载任何[JSON](https://www.json.org/) 或者 [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) 文件. 数据驱动的内容当前包含两个函数 `getJSON` 和 `getCSV`, 当然这两个函数在所有模板文件中都可用.

## 实现细节 Implementation details

### 对URL调用函数 Call the Functions with a URL

在模板中, 可以这样调用函数:
In your template, call the functions like this:

```
{{ $dataJ := getJSON "url" }}
{{ $dataC := getCSV "separator" "url" }}
```

If you use a prefix or postfix for the URL, the functions accept [variadic arguments][variadic]:
若果URL使用了后缀或者前缀, 函数接受可变参数 [variadic arguments][variadic]:

```
{{ $dataJ := getJSON "url prefix" "arg1" "arg2" "arg n" }}
{{ $dataC := getCSV  "separator" "url prefix" "arg1" "arg2" "arg n" }}
```

The separator for `getCSV` must be put in the first position and can only be one character long.
函数`getCSV` 中使用的分隔符 必须是第一个位置上的参数并且仅有一个字符长.

All passed arguments will be joined to the final URL:
所有传递的参数会joined连接在一起构建最终的URL:

```
{{ $urlPre := "https://api.github.com" }}
{{ $gistJ := getJSON $urlPre "/users/GITHUB_USERNAME/gists" }}
```

这个会在内部解析成如下的地址:

```
{{ $gistJ := getJSON "https://api.github.com/users/GITHUB_USERNAME/gists" }}
```

Finally, you can range over an array. This example will output the
first 5 gists for a GitHub user:
最后, 可以迭代数组。这个例子会输出Github用户的前5个代码段:


```
<ul>
  {{ $urlPre := "https://api.github.com" }}
  {{ $gistJ := getJSON $urlPre "/users/GITHUB_USERNAME/gists" }}
  {{ range first 5 $gistJ }}
    {{ if .public }}
      <li><a href="{{ .html_url }}" target="_blank">{{ .description }}</a></li>
    {{ end }}
  {{ end }}
</ul>
```

### CSV文件例子 Example for CSV files

For `getCSV`, the one-character-long separator must be placed in the first position followed by the URL. The following is an example of creating an HTML table in a [partial template][partials] from a published CSV:
对于 `getCSV`, 一个字符长的分隔符必须放在跟随URL后面的第一个参数的位置. 下面是从发布的CSV构建的[部分模板][partials]中创建HTML表格的例子:


{{< code file="layouts/partials/get-csv.html" >}}
  <table>
    <thead>
      <tr>
      <th>Name</th>
      <th>Position</th>
      <th>Salary</th>
      </tr>
    </thead>
    <tbody>
    {{ $url := "https://example.com/finance/employee-salaries.csv" }}
    {{ $sep := "," }}
    {{ range $i, $r := getCSV $sep $url }}
      <tr>
        <td>{{ index $r 0 }}</td>
        <td>{{ index $r 1 }}</td>
        <td>{{ index $r 2 }}</td>
      </tr>
    {{ end }}
    </tbody>
  </table>
{{< /code >}}

The expression `{{index $r number}}` must be used to output the nth-column from the current row.
注意必须使用表达式 `{{index $r number}}` 来输出当前行的第n个列.

### 缓存URLS Cache URLs

Each downloaded URL will be cached in the default folder `$TMPDIR/hugo_cache/`. The variable `$TMPDIR` will be resolved to your system-dependent temporary directory.
每个下载的URL资源会被缓存在默认的目录`$TMPDIR/hugo_cache/`. 变量 `$TMPDIR` 解析为依赖于操作系统的临时目录.

With the command-line flag `--cacheDir`, you can specify any folder on your system as a caching directory.
使用命令行参数 `--cacheDir`, 您可以指定任何系统任何目录作为缓存目录.

You can also set `cacheDir` in the [main configuration file][config].
也可以在配置文件中 [main configuration file][config]设置`cacheDir`.

If you don't like caching at all, you can fully disable caching with the command line flag `--ignoreCache`.
如果根本就不喜欢缓存, 可以通过设置命令行参数`--ignoreCache` 来完全禁用缓存.

### 使用REST URLS时验证 Authentication When Using REST URLs

Currently, you can only use those authentication methods that can be put into an URL. [OAuth][] and other authentication methods are not implemented.
当前, 仅仅可以填入到URL的认证方法得到支持。 [OAuth][] 和其他认证方法未实现.


## 加载本地文件 Load Local files

To load local files with `getJSON` and `getCSV`, the source files must reside within Hugo's working directory. The file extension does not matter, but the content does.
为通过`getJSON` 和 `getCSV`加载本地文件, 源文件必须位于Hugo的工作目录中。文件的扩展名无所谓，文件内容要符合要求. 

It applies the same output logic as above in [Call the Functions with a URL](#call-the-functions-with-a-url).
加载本地文件的输出逻辑同上面调用URL的输出一样。

{{% note %}}
The local CSV files to be loaded using `getCSV` must be located **outside** of the `data` directory.
想要加载的本地CSV文件必须位于`data`目录的**外部**
{{% /note %}}

## 数据文件热加载 LiveReload with Data Files

There is no chance to trigger a [LiveReload][] when the content of a URL changes. However, when a *local* file changes (i.e., `data/*` and `themes/<THEME>/data/*`), a LiveReload will be triggered. Symlinks are not supported. Note too that because downloading of data takes a while, Hugo stops processing your Markdown files until the data download has completed.
当URL的内容变化时, Hugo没有机会去触发[LiveReload][] 热加载。不过, 当*本地* 文件变化时(比如`data/*` and `themes/<THEME>/data/*`), 会触发热加载. 符号链接不支持热加载. 也请注意下载数据需要一小段时间, Hugo会停止处理markdown文件直到数据下载完毕.

{{% warning "URL Data and LiveReload" %}}
If you change any local file and the LiveReload is triggered, Hugo will read the data-driven (URL) content from the cache. If you have disabled the cache (i.e., by running the server with `hugo server --ignoreCache`), Hugo will re-download the content every time LiveReload triggers. This can create *huge* traffic. You may reach API limits quickly.
如果您改变了本地文件并且触发了热加载, Hugo会从缓存读取数据驱动(URL)的内容. 如果您选择禁用了缓存(比如，运行服务器使用 `hugo server --ignoreCache` )
Hugo会在每次热加载触发时重新下载内容. 这会导致*巨大的*流量. 导致您可能超过API的限制.
- Hugo will re-download the content every time LiveReload triggers. This can create *huge* traffic. You may reach API limits quickly.
{{% /warning %}}

## 数据驱动内容例子 Examples of Data-driven Content

- Photo gallery JSON powered: [https://github.com/pcdummy/hugo-lightslider-example](https://github.com/pcdummy/hugo-lightslider-example)
- GitHub Starred Repositories [in a post](https://github.com/SchumacherFM/blog-cs/blob/master/content%2Fposts%2Fgithub-starred.md) using data-driven content in a [custom short code](https://github.com/SchumacherFM/blog-cs/blob/master/layouts%2Fshortcodes%2FghStarred.html).

## 数据格式规范 Specs for Data Formats

* [TOML Spec][toml]
* [YAML Spec][yaml]
* [JSON Spec][json]
* [CSV Spec][csv]

[config]: /getting-started/configuration/
[csv]: https://tools.ietf.org/html/rfc4180
[customize]: /themes/customizing/
[json]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "Specification for JSON, JavaScript Object Notation"
[LiveReload]: /getting-started/usage/#livereload
[lookup]: /templates/lookup-order/
[markdownify]: /functions/markdownify/
[OAuth]: https://en.wikipedia.org/wiki/OAuth
[partials]: /templates/partials/
[themes]: /themes/
[toml]: https://github.com/toml-lang/toml
[variadic]: https://en.wikipedia.org/wiki/Variadic_function
[vars]: /variables/
[yaml]: https://yaml.org/spec/
