---
title: 基本用法
linktitle: 基本用法
description: Hugo的命令行功能完整、易于使用, 即使对于很少使用命令行的用户来说也是如此.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [开始使用]
keywords: [usage,livereload,command line,flags]
menu:
  docs:
    parent: "getting-started"
    weight: 40
weight: 40
sections_weight: 40
draft: false
aliases: [/overview/usage/,/extras/livereload/,/doc/usage/,/usage/]
toc: true
---

下面是开发Hugo站点时您最常用命令的描述。参考[命令行参考][commands]获取Hugo命令行的全面介绍

## 测试安装

在[安装 Hugo][install]以后, 请确认它在系统`PATH`环境变量中。使用 `help` 指令测试Hugo已经被正确安装。

```
hugo help
```

可以看到, 界面输出类似下面这样:



```
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at https://gohugo.io/.

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  check       Contains some verification checks
  config      Print the site configuration
  convert     Convert your content to different formats
  env         Print Hugo version and environment info
  gen         A collection of several useful generators.
  help        Help about any command
  import      Import your site from others.
  list        Listing out various types of content
  new         Create new content for your site
  server      A high performance webserver
  version     Print the version number of Hugo

Flags:
  -b, --baseURL string         hostname (and path) to the root, e.g. https://spf13.com/
  -D, --buildDrafts            include content marked as draft
  -E, --buildExpired           include expired content
  -F, --buildFuture            include content with publishdate in the future
      --cacheDir string        filesystem path to cache directory. Defaults: $TMPDIR/hugo_cache/
      --cleanDestinationDir    remove files from destination not found in static directories
      --config string          config file (default is path/config.yaml|json|toml)
      --configDir string       config dir (default "config")
  -c, --contentDir string      filesystem path to content directory
      --debug                  debug output
  -d, --destination string     filesystem path to write files to
      --disableKinds strings   disable different kind of pages (home, RSS etc.)
      --enableGitInfo          add Git revision, date and author info to the pages
  -e, --environment string     build environment
      --forceSyncStatic        copy all files when static is changed.
      --gc                     enable to run some cleanup tasks (remove unused cache files) after the build
  -h, --help                   help for hugo
      --i18n-warnings          print missing translations
      --ignoreCache            ignores the cache directory
  -l, --layoutDir string       filesystem path to layout directory
      --log                    enable Logging
      --logFile string         log File path (if set, logging enabled automatically)
      --minify                 minify any supported output format (HTML, XML etc.)
      --noChmod                don't sync permission mode of files
      --noTimes                don't sync modification time of files
      --path-warnings          print warnings on duplicate target paths etc.
      --quiet                  build in quiet mode
      --renderToMemory         render to memory (only useful for benchmark testing)
  -s, --source string          filesystem path to read files relative from
      --templateMetrics        display metrics about template executions
      --templateMetricsHints   calculate some improvement hints when combined with --templateMetrics
  -t, --theme strings          themes to use (located in /themes/THEMENAME/)
      --themesDir string       filesystem path to themes directory
      --trace file             write trace to file (not useful in general)
  -v, --verbose                verbose output
      --verboseLog             verbose logging
  -w, --watch                  watch filesystem for changes and recreate as needed

Use "hugo [command] --help" for more information about a command.
```

(译注，上面的输出在下面重复并译成中文)
```
hugo is the main command, used to build your Hugo site.
hugo是主要的命令，使用它来构建您的Hugo站点

Hugo是快速灵活的静态站点生成器
由spf13和朋友们用爱构建，使用Go语言

完整的文档在这里  https://gohugo.io/.

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  check       包含一些验证检查
  config      打印站点配置信息
  convert     转化内容为其他不同模式
  env         打印Hugo版本和环境信息
  gen         几个有帮助的生成器的集合
  help        关于命令的帮助
  import      从它处导入你的站点
  list        不同类型内容的列表
  new         为站点生成新内容
  server      高效能web服务器
  version     打印Hugo版本号

Flags:
  -b, --baseURL string         对应于站点的域名(和路径),比如 https://spf13.com/
  -D, --buildDrafts            包含标记为草案的内容
  -E, --buildExpired           包含标记为过期的内容
  -F, --buildFuture            包含标记发布日期为未来的的内容
      --cacheDir string        缓存目录 默认是: $TMPDIR/hugo_cache/
      --cleanDestinationDir    从目标目录删除不在static目录中内容
      --config string          配置文件(默认是 path/config.yaml|json|toml)
      --configDir string       配置目录 (默认是 "config")
  -c, --contentDir string      内容目录路径
      --debug                  输出调试信息
  -d, --destination string     输出文件的文件系统路径
      --disableKinds strings   禁用其他不同格式的页面(home, RSS 等)
      --enableGitInfo          为页面添加Git修订版本、日期和作者等信息
  -e, --environment string     构建环境
      --forceSyncStatic        当static文件有变化时copy所有文件
      --gc                     构建后执行一些清理工作 (移除未用的缓存文件)
  -h, --help                   hugo的帮助
      --i18n-warnings          显示丢失的翻译
      --ignoreCache            忽略缓存
  -l, --layoutDir string       layout布局目录
      --log                    启用Log日志
      --logFile string         日志文件路径 (如果设置此选项,自动启动日志)
      --minify                 对支持的输出内容格式最小化(如HTML, XML 等)
      --noChmod                不同步文件的许可模式
      --noTimes                不同步文件的修改时间
      --path-warnings          目标路径重复是打印警告信息
      --quiet                  在安静模式构建
      --renderToMemory         显示在内存中(仅仅在性能测试中有用)
  -s, --source string          读取文件的相对文件系统路径
      --templateMetrics        显示模板执行的指标
      --templateMetricsHints   计算一些改善的提示，当和 --templateMetrics一起使用时
  -t, --theme strings          要使用的主题(位于 /themes/THEMENAME/)
      --themesDir string       主题路径的文件系统路径
      --trace file             写跟踪信息的文件(一般来说无用)
  -v, --verbose                显示冗长的输出
      --verboseLog             记录冗长的日志
  -w, --watch                  监控文件系统变化,并且在必要时重新创建网站文件

使用 "hugo [command] --help" 获得某个命令的更多信息.
```



##  `hugo` 命令

The most common usage is probably to run `hugo` with your current directory being the input directory.

最常用的命令大概是把当前目录也就是输入目录时运行 `hugo`

这个命令会默认在 `public/` 目录中生成您的网站，当然您可以通过改变[站点配置][config]中的 `publishDir` 选项来配置这个输出目录。

`hugo` 命令在`public/`目录生成您的网站，已经准备好部署到您的web服务器上:


```
hugo
0 draft content
0 future content
99 pages created
0 paginator pages created
16 tags created
0 groups created
in 90 ms
```

## 草案、未来和过期内容 Draft, Future, and Expired Content

Hugo允许您在网站内容的[前端设定][front matter]中设置文档的 `draft`, `publishdate` 甚至 `expirydate` 字段。默认情况下，Hugo不会发布下面内容:

1. `publishdate` 发布日期值设定在未来的内容
2. `draft: true` 草案状态设置为真的内容
3. `expirydate` 过期日期值设置为过去某事件的内容

这三个可以在本地开发和部署编译时通过对 `hugo` 和 `hugo server` 分别加如下参数来重新设定, 或者在[配置文件][config]中设定对应(不包含 `--`)域的boolean值：

1. `--buildFuture`
2. `--buildDrafts`
3. `--buildExpired`

## 热加载

Hugo内置 [热加载](https://github.com/livereload/livereload-js) 功能. 不需要安装额外包。常见方式是在开发站点时用 `hugo server` 命令使用Hugo作为服务器并监听文件内容变化:

```
hugo server
0 draft content
0 future content
99 pages created
0 paginator pages created
16 tags created
0 groups created
in 120 ms
Watching for changes in /Users/yourname/sites/yourhugosite/{data,content,layouts,static}
Serving pages from /Users/yourname/sites/yourhugosite/public
Web Server is available at http://localhost:1313/
Press Ctrl+C to stop
```

这运行一个全功能的web服务器并且同时监控您文件系统中您的[项目组织][dirs]内下面目录内的增删改变:

* `/static/*`
* `/content/*`
* `/data/*`
* `/i18n/*`
* `/layouts/*`
* `/themes/<CURRENT-THEME>/*`
* `config`

当您做出变化时，Hugo会同时构建站点并提供内容服务。一旦构建站点完毕，热加载会通知浏览器默默的重新加载页面。

通常Hugo构建都很快以至于您觉察不到这些变化,除非您直接看着浏览器中的站点内容。这意味在第二屏幕或者当前屏幕的另一半中打开站点，您可以看见最新的站点内容, 完全没必要离开您的文本编辑器。

{{% note "Closing `</body>` Tag"%}}
Hugo在模板的 `</body>` 前插入了热加载的 `<script>`脚本, 如果这个脚本没有，热加载将不起作用
{{% /note %}}

### 禁用热加载

热加载在Hugo生成的页面中插入了Javascript脚本, 脚本在浏览器的web通讯客户端和Hugo web通讯服务器之间建立了联系。

热加载对开发来说非常有用。不过一些用户可能在正式产品中使用 `hugo server` 来即时显示更新的内容。下面方法可以简单禁用热加载 However, some Hugo users may use `hugo server` in production to instantly display updated content. The following methods make it easy to disable LiveReload:

```
hugo server --watch=false
```

Or...

```
hugo server --disableLiveReload
```
相应的通过在`config.toml` 或者 `config.yml`配置文件中添加 键-值 对， 第二个标志可以省略，

```
disableLiveReload = true
```

```
disableLiveReload: true
```

##  部署站点

After running `hugo server` for local web development, you need to do a final `hugo` run *without the `server` part of the command* to rebuild your site. You may then deploy your site by copying the `public/` directory to your production web server.

在本地开发运行 `hugo server` 命令后, 需要最后运行 `hugo` 命令, 这个命令 *并不包含 `server` 部分*，来重新构建您的站点， 然后可以通过copy `public/` 目录到您的生产web服务器来部署您的站点。

由于Hugo生成的是静态站点，您的站点可以在任何地方的任何服务器上托管运行。参考[托管和部署][hosting]部分学习社区贡献的托管和自动部署的方法。

{{% warning "Generated Files are **NOT** Removed on Site Build" %}}
运行`hugo`命令 *并没有* 删除以前构建生成的文件 。这意味在运行`hugo`命令前, 您可能需要删除 `public/` 目录(或者您通过命令行参数或者配置文件声明的发布目录)。如果您没有删除这些文件，那么可能冒着错误文件(比如，草案或者未来的文件)遗留在生成的站点内的危险
{{% /warning %}}


[commands]: /commands/
[config]: /getting-started/configuration/
[dirs]: /getting-started/directory-structure/
[front matter]: /content-management/front-matter/
[hosting]: /hosting-and-deployment/
[install]: /getting-started/installing/
