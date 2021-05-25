---
title: 使用Hugo模块
linktitle: 使用模块
description: 使用模块构建管理您的站点.
date: 2019-07-24
categories: [hugo modules]
keywords: [install, themes, source, organization, directories,usage,modules]
menu:
  docs:
    parent: "modules"
    weight: 20
weight: 20
sections_weight: 20
draft: false
aliases: [/themes/usage/,/themes/installing/,/installing-and-using-themes/]
toc: true
---

## 前提条件

{{< gomodules-info >}}



## 初始化新模块 Initialize a New Module

Use `hugo mod init` to initialize a new Hugo Module. If it fails to guess the module path, you must provide it as an argument, e.g.:

使用 `hugo mod init` 命令初始化新Hugo模块。如果hugo检测不到模块目录, 必须通过参数提供, 比如:

```bash
hugo mod init github.com/gohugoio/myShortcodes
```

也请参考[CLI Doc](/commands/hugo_mod_init/).

## 使用主题模块 Use a Module for a Theme
The easiest way to use a Module for a theme is to import it in the config.
最简单的使用主题模块的方式是在配置文件中引入

1. Initialize the hugo module system: `hugo mod init github.com/<your_user>/<your_project>`
2. Import the theme in your `config.toml`:

1. 初始化hugo模块系统: `hugo mod init github.com/<your_user>/<your_project>`
2. 在配置文件中 `config.toml`引入主题:

```toml
[module]
  [[module.imports]]
    path = "github.com/spf13/hyde"
```

## 更新模块

Modules will be downloaded and added when you add them as imports to your configuration, see [Module Imports](/hugo-modules/configuration/#module-config-imports).

当您在配置文件添加模块配置引入后, 模块会被下载并添加，参考[Module Imports](/hugo-modules/configuration/#module-config-imports)


更新或者管理版本，请使用`hugo mod get`

例子如下:

### 更新所有模块

```bash
hugo mod get -u
```

### 递归更新所有模块

{{< new-in "0.65.0" >}}

```bash
hugo mod get -u ./...
```

### 更新一个模块

```bash
hugo mod get -u github.com/gohugoio/myShortcodes
```
### 更新指定版本

```bash
hugo mod get github.com/gohugoio/myShortcodes@v1.0.7
```

同时请参考 [CLI Doc](/commands/hugo_mod_get/).

## 修改和测试模块变化

项目中引入模块进行本地开发的一个方法是替换指令为源代码在`go.md`中的本地目录。
One way to do local development of a module imported in a project is to add a replace directive to a local directory with the source in `go.mod`:

```bash
replace github.com/bep/hugotestmods/mypartials => /Users/bep/hugotestmods/mypartials
```

如果运行着 `hugo server` , 配置会被重新加载,  `/Users/bep/hugotestmods/mypartials` 目录会被添加到检测目录列表中.

If you have the `hugo server` running, the configuration will be reloaded and `/Users/bep/hugotestmods/mypartials` put on the watch list.

注意从版本v.0.77.0开始，您可以使用模块配置[`replacements`](https://gohugo.io/hugo-modules/configuration/#module-config-top-level) option. {{< new-in "0.77.0" >}}

## 打印依赖图 Print Dependency Graph


Use `hugo mod graph` from the relevant module directory and it will print the dependency graph, including vendoring, module replacement or disabled status. 在相关模块目录中使用`hugo mod graph`命令，会打印输出依赖图，包括vendoring，模块替换或者禁用状态。

E.g.:

```
hugo mod graph

github.com/bep/my-modular-site github.com/bep/hugotestmods/mymounts@v1.2.0
github.com/bep/my-modular-site github.com/bep/hugotestmods/mypartials@v1.0.7
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myassets@v1.0.4
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myv2@v1.0.0
DISABLED github.com/bep/my-modular-site github.com/spf13/hyde@v0.0.0-20190427180251-e36f5799b396
github.com/bep/my-modular-site github.com/bep/hugo-fresh@v1.0.1
github.com/bep/my-modular-site in-themesdir

```

也请参考 [CLI Doc](/commands/hugo_mod_graph/).

## Vendor Your Modules

`hugo mod vendor` will write all the module dependencies to a `_vendor` folder, which will then be used for all subsequent builds.

Note that:

* You can run `hugo mod vendor` on any level in the module tree.
* Vendoring will not store modules stored in your `themes` folder.
* Most commands accept a `--ignoreVendorPaths` flag, which will then not use the vendored modules in `_vendor` for the module paths matching the [Glob](https://github.com/gobwas/glob) pattern given. Note that before Hugo 0.75 this flag was named `--ignoreVendor` and was a "all or nothing". {{< new-in "0.75.0" >}}

Also see the [CLI Doc](/commands/hugo_mod_vendor/).


## Tidy go.mod, go.sum

Run `hugo mod tidy` to remove unused entries in `go.mod` and `go.sum`.

Also see the [CLI Doc](/commands/hugo_mod_clean/).

## Clean Module Cache

Run `hugo mod clean` to delete the entire modules cache.

Note that you can also configure the `modules` cache with a `maxAge`, see [File Caches](/hugo-modules/configuration/#configure-file-caches).



Also see the [CLI Doc](/commands/hugo_mod_clean/).
