---
title: 配置模块
linktitle: 配置模块
description: 本页描述 模块的配置选项.
date: 2019-07-24
categories: [hugo modules]
keywords: [themes, source, organization, directories]
menu:
  docs:
    parent: "modules"
    weight: 10
weight: 10
sections_weight: 10
toc: true
translation_note: 实在没概念 go module
---

## 模块配置: 顶层

{{< code-toggle file="config">}}
[module]
noVendor = ""
proxy = "direct"
noProxy = "none"
private = "*.*"
replacements = ""
{{< /code-toggle >}}


noVendor {{< new-in "0.75.0" >}}
: 可选的Glob模式，当模块发布时与之匹配的模块路径将会被忽略。

A optional Glob pattern matching module paths to skip when vendoring, e.g. "github.com/**"

vendorClosest {{< new-in "0.81.0" >}}
: 启用时，我们会从发布的模块中选择与使用它的模块最接近的模块。默认的行为是选择第一个。请注意对任何模块路径，仍然可能存在一个依赖，所以一旦被使用就不能重新定义。

 When enabled, we will pick the vendored module closest to the module using it. The default behaviour is to pick the first. Note that there can still be only one dependency of a given module path, so once it is in use it cannot be redefined.

proxy
: 定义下载远程模块的代理服务器。默认是`direct`, 意指`git clone`和类似指令。

Defines the proxy server to use to download remote modules. Default is `direct`, which means "git clone" and similar.

noProxy
: 逗号分隔的glob模式匹配路径列表，其中路径不使用上面配置的代理

Comma separated glob list matching paths that should not use the proxy configured above.

private
: 逗号分隔的glob模式匹配路径列表，表内路径应该被认为私有的。

Comma separated glob list matching paths that should be treated as private.

replacements {{< new-in "0.77.0" >}}
: 逗号分隔的模块路径到目录的替换映射列表。比如,`"github.com/bep/myprettytheme -> ../..,github.com/bep/shortcodes -> /some/path`. 这个设置对于临时本地模块开发很有用, 将它设置为OS的环境变量也不错，比如`env HUGO_MODULE_REPLACEMENTS="github.com/bep/myprettytheme -> ../.."`任何相对目录相对于[themesDir](https://gohugo.io/getting-started/configuration/#all-configuration-settings)，并且绝对路径是允许的。

注意上面条目直接映射到Go模块的对应部分。部分设置设定为操作系统环境变量很合适。
比如，设置使用的代理服务器:
Note that the above terms maps directly to their counterparts in Go Modules. Some of these setting may be natural to set as OS environment variables. To set the proxy server to use, as an example:


```
env HUGO_MODULE_PROXY=https://proxy.example.org hugo
```

{{< gomodules-info >}}

## 模块配置: hugo版本

如果模块运行需要特定版本的Hugo, 您可以在 `module` 配置区块中标明，这样用户如果使用了太旧或者太新的hugo版本时，会收到警告。

{{< code-toggle file="config">}}
[module]
[module.hugoVersion]
  min = ""
  max = ""
  extended = false

{{< /code-toggle >}}

上面的每条都可以省略。

min
: 支持的最小Hugo版本, 比如 `0.55.0`

max
: 支持的最大Hugo版本, 比如 `0.55.0`

extended
: 是否需要扩展版本的Hugo

## 模块配置: imports

{{< code-toggle file="config">}}
[module]
[[module.imports]]
  path = "github.com/gohugoio/hugoTestModules1_linux/modh1_2_1v"
  ignoreConfig = false
  ignoreImports = false
  disable = false
[[module.imports]]
  path = "my-shortcodes"
{{< /code-toggle >}}

path
: Can be either a valid Go Module module path, e.g. `github.com/gohugoio/myShortcodes`, or the directory name for the module as stored in your themes folder.
可以是合规的Go模块的模块路径，比如 `github.com/gohugoio/myShortcodes`
或者是模块的目录名称，这个目录也就是保存主题的目录？。


ignoreConfig
:如果启用，任何模块配置文件，比如`config.toml` 都不会加载。注意这个设置也会停止任何暂时的模块依赖的读取。
If enabled, any module configuration file, e.g. `config.toml`, will not be loaded. Note that this will also stop the loading of any transitive module dependencies.

ignoreImports {{< new-in "0.80.0" >}}
: 如果启用, 模块导入不会被执行。
If enabled, module imports will not be followed.

disable
: 设置为`true`将禁用模块，同时保留`go.*`文件中设置的模块信息。
 Set to `true` to disable the module while keeping any version info in the `go.*` files.

{{< gomodules-info >}}


## 模块配置: mounts

{{% note %}}
当Hugo 0.56.0引入`mounts`配置时, 我们注意保存原有的 `staticDir` 和其他配置，确保所有原有的站点能继续工作。但是站点不一定有这两个设置，如果添加了`mounts`部分，那就应该移除原有的旧 `staticDir` 设置。

When the `mounts` config was introduced in Hugo 0.56.0, we were careful to preserve the existing `staticDir` and similar configuration to make sure all existing sites just continued to work. But you should not have both: if you add a `mounts` section you should remove the old `staticDir` etc. settings.
{{% /note %}}

{{% warning %}}
When you add a mount, the default mount for the concerned target root is ignored: be sure to explicitly add it.
添加mount部分的配置，相关的目标根的默认mount就被忽略了: 谨记明确添加这个默认配置。
{{% /warning %}}

**Default mounts**
{{< code-toggle file="config">}}
[module]
[[module.mounts]]
    source="content"
    target="content"
[[module.mounts]]
    source="static"
    target="static"
[[module.mounts]]
    source="layouts"
    target="layouts"
[[module.mounts]]
    source="data"
    target="data"
[[module.mounts]]
    source="assets"
    target="assets"
[[module.mounts]]
    source="i18n"
    target="i18n"
[[module.mounts]]
    source="archetypes"
    target="archetypes"
{{< /code-toggle >}}

source
: 加载的模块的源目录。对于主项目，这可能是项目相关的或者绝对路径，甚至可能是一个符号链接。对于其他模块，必须是项目相关路径。

The source directory of the mount. For the main project, this can be either project-relative or absolute and even a symbolic link. For other modules it must be project-relative.

target
: 模块被hugo虚拟文件系统加载的目标路径。必须以Hugo的组件成分`static`, `content`, `layouts`, `data`, `assets`, `i18n`, or `archetypes`的路径开始，比如`content/blog`.

Where it should be mounted into Hugo's virtual filesystem. It must start with one of Hugo's component folders: `static`, `content`, `layouts`, `data`, `assets`, `i18n`, or `archetypes`. E.g. `content/blog`.

lang
: 语言代码，比如"en". 在多站点模式下，仅仅内容语言相关的部分，以及 `static`静态资源被会加载.

The language code, e.g. "en". Only relevant for `content` mounts, and `static` mounts when in multihost mode.
