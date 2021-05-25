---
title: 快速开始
linktitle: 快速开始
description: 应用美丽的Ananke模板创建新的Hugo站点.
date: 2013-07-01
publishdate: 2013-07-01
categories: [快速开始, getting started]
keywords: [ quick start,usage]
authors: [Shekhar Gulati, Ryan Watters]
menu:
  docs:
    parent: "getting-started"
    weight: 10
weight: 10
sections_weight: 10
draft: false
aliases: [/quickstart/,/overview/quickstart/]
toc: true
---

{{% note %}}
本快速开始例子中使用 `macOS` 操作系统. 在其他操作系统中安装Hugo的指令，请参考
[安装](/getting-started/installing)

运行本教程前建议您已经[安装Git](https://git-scm.com/downloads)工具

其他途径，比如Hugo相关书籍和教程视频请见[外部学习资源](/getting-started/external-learning-resources/)
{{% /note %}}

## 第一步: 安装 Hugo

{{% note %}}
`macOS`操作系统的的包管理工具`Homebrew` 和 `MacPorts`, 可以相应的从[brew.sh](https://brew.sh/) 或者 [macports.org](https://www.macports.org/)安装。 使用windows的安装过程请见[安装](/getting-started/installing)中关于windows部分文档。
{{% /note %}}

```bash
brew install hugo
# 或者
port install hugo
```

运行如下命令验证安装:

```bash
hugo version
```

{{< asciicast ItACREbFgvJ0HjnSNeTknxWy9 >}}

## 第二步: 建新站点

```bash
hugo new site quickstart
```

上面命令会创建一个新目录`quickstart`, 并在其内创建了新的Hugo站点

{{< asciicast 3mf1JGaN0AX0Z7j5kLGl3hSh8 >}}

## 第三步: 添加Theme主题

[主题列表](https://themes.gohugo.io/) 内有许多主题可以考量. 这里使用的是漂亮的 [Ananke 主题](https://themes.gohugo.io/gohugo-theme-ananke/).

首先从Github下载主题文件并添加在新建站点的`themes`目录中:

```bash
cd quickstart
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

*不使用git的用户:*
   - 如果没有安装git，可以从下面链接下载主题的最新版本档案:
       https://github.com/budparr/gohugo-theme-ananke/archive/master.zip
   - 解压缩zip文件获得一个 "gohugo-theme-ananke-master" 目录
   - 重命名目录为"ananke", 然后移动这个目录到 "themes/" 目录下

然后，在站点配置文件中添加主题设置:

```bash
echo theme = \"ananke\" >> config.toml
```

{{< asciicast 7naKerRYUGVPj8kiDmdh5k5h9 >}}

## 第四步: 添加内容

可以选择手工创建内容文件(比如 `content/<CATEGORY>/<FILE>.<FORMAT>`)并在文件中添加元数据(metadata), 不过hugo有更好的方式，能够使用`new`命令来为您做一些事情(比如添加标题和日期):

```
hugo new posts/my-first-post.md
```

{{< asciicast eUojYCfRTZvkEiqc52fUsJRBR >}}

有需要的话可以编辑新建的文件内容，它的开头部分是类似这样的:

```markdown
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---

```

{{% note %}}
草稿不会被编译部署;当编辑好草稿时,更新文件头部的  
草稿状态为`draft: false`. 更多信息见 [这里](/getting-started/usage/#draft-future-and-expired-content).
{{% /note %}}

## 第五步: 启动Hugo服务器

现在,启动服务器了, 这里(译注: 使用命令行参数 -D)启用了[草稿](/getting-started/usage/#draft-future-and-expired-content):

{{< asciicast BvJBsF6egk9c163bMsObhuNXj >}}

```
▶ hugo server -D

                   | EN
+------------------+----+
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  3
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
Watching for changes in /Users/bep/quickstart/{content,data,layouts,static,themes}
Watching for config changes in /Users/bep/quickstart/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

**浏览器里导航到[http://localhost:1313/](http://localhost:1313/)查看您的新站点.**

随意编辑或者添加新内容，然后简单的刷新浏览器可以很快看见内容的变化(可能需要强制刷新浏览器,类似Ctrl-R的快捷键一般会起作用).

## 第六步: 自定义主题

新站点看起来已经很不错了，现在您想在公开发布站点前对其做些小调整

###    站点配置

在文本编辑器中打开 `config.toml` 文件:

```
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```

将 `title` 替换为一段更符合您站点的文本。同时，如果您已经拥有了域名, 请修改 `baseURL` 为您的域名. 注意这个域名设置值在运行本地开发机器时并不起作用。

{{% note %}}
**提示:** 尝试在服务器运行时对站点配置文件或者站点的其他文件做些修改，在浏览器上会马上看见这些变化，或许您可能需要 [清空缓存](https://kb.iu.edu/d/ahic).
{{% /note %}}

关于ananke主题的配置选项, 请参考[ananke主题站点](https://github.com/budparr/gohugo-theme-ananke).

**更多主题配置信息，参见[定制主题](/themes/customizing/).**

### 第七部: 编译站点静态页面

很轻松, 调用下面命令:

```
hugo -D
```

编译输出的默认目录是 `./public/`(使用命令行参数`-d`/`--destination`可以更改输出目录, 或者在配置文件中设置 `publishdir` 来设置)
