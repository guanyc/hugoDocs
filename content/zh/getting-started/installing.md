---
title: 安装Hugo
linktitle: 安装Hugo
description: 在macOS, Windows, Linux, OpenBSD, FreeBSD等操作系统,以及在Go语言编译器工具链可以运行的系统上安装Hugo
symmary: 在macOS, Windows, Linux, OpenBSD, FreeBSD等操作系统,以及在Go语言编译器工具链可以运行的系统上安装Hugo
date: 2016-11-01
publishdate: 2016-11-01
lastmod: 2018-01-02
categories: [getting started,fundamentals]
authors: ["Michael Henderson"]
keywords: [install,pc,windows,linux,macos,binary,tarball]
menu:
  docs:
    parent: "getting-started"
    weight: 30
weight: 30
sections_weight: 30
draft: false
aliases: [/tutorials/installing-on-windows/,/tutorials/installing-on-mac/,/overview/installing/,/getting-started/install,/install/]
toc: true
---


{{% note %}}
社区内很多关于“Hugo是用Go语言编写的”的谈论，但是享受Hugo的快乐并不需要下载安装Go语言。运用好预编译的二进制文件就妥了.
{{% /note %}}


Hugo 是使用[Go语言](https://golang.org/) 编写的，支持多个平台。最新版本见 [Hugo 分发版本][releases].


当前，Hugo对下列系统提供了预编译的二进制文件:

* macOS (Darwin) for x64, i386, and ARM architectures
* Windows
* Linux
* OpenBSD
* FreeBSD

Hugo may also be compiled from source wherever the Go toolchain can run; e.g., on other operating systems such as DragonFly BSD, OpenBSD, Plan&nbsp;9, Solaris, and others. See <https://golang.org/doc/install/source> for the full set of supported combinations of target operating systems and compilation architectures.

在Go工具链可以运行的系统上，可以从源代码编译Hugo, 比如，其他操作系统例如DragonFly BSD, OpenBSD, Plan&nbsp;9, Solaris等。关于受支持的目标操作系统和编译架构的全部组合的信息，请参考 <https://golang.org/doc/install/source>


## 快速安装

### 二进制安装  (跨平台)

从[Hugo版本]处下载适合您系统的版本。下载以后，二进制文件可以在任何目录运行。不需要安装在全局目录。这在共享主机或者您没有特权账户的系统上变现良好。

理想情况下，您可以在系统的 `PATH` 环境变量中的某目录安装hugo，`/usr/local/bin` 是可能形最大的目录.

### Docker


我们没有提供Docker的官方hugo景象，不过我们推荐链接中的最新的docker-hugo发行版:https://hub.docker.com/r/klakegg/hugo/

### Homebrew (macOS)

在macOS系统中使用[Homebrew][brew], 使用下面命令安装Hugo:

{{< code file="install-with-homebrew.sh" >}}
brew install hugo
{{< /code >}}

更多详细解释，请阅读下面的macOS和Windows系统安装部分。

### MacPorts (macOS)

在macOS系统中使用[MacPorts][macports], 使用下面命令安装Hugo:

{{< code file="install-with-macports.sh" >}}
port install hugo
{{< /code >}}

### Homebrew (Linux)

在Linux系统中使用[Homebrew][linuxbrew], 使用下面命令安装Hugo:

{{< code file="install-with-linuxbrew.sh" >}}
brew install hugo
{{< /code >}}

在linux系统中安装Homebrew的指南见[Linuxbrew站点][linuxbrew].

### Chocolatey (Windows)

If you are on a Windows machine and use [Chocolatey][] for package management, you can install Hugo with the following one-liner:

在windows系统中使用[Chocolatey][]管理包，使用下面命令安装Hugo:

{{< code file="install-with-chocolatey.ps1" >}}
choco install hugo -confirm
{{< /code >}}

或者使用下面命令下载包含 Sass/SCSS 处理能力的`hugo-extended`扩展版本:

{{< code file="install-extended-with-chocolatey.ps1" >}}
choco install hugo-extended -confirm
{{< /code >}}

### Scoop (Windows)


在windows系统中使用[Scoop][]管理包，使用下面命令安装Hugo:


```bash
scoop install hugo
```

或者通过下面命令下载hugo-extended 扩展版本:

```bash
scoop install hugo-extended
```

### 源代码

#### 必备工具

* [Git][installgit]
* [Go 语言(至少要求 Go语言 1.11版本)](https://golang.org/dl/)

#### 从Github获取源代码

Since Hugo 0.48, Hugo uses the Go Modules support built into Go 1.11 to build. The easiest way to get started is to clone Hugo in a directory outside of the GOPATH, as in the following example:

从Hugo 0.48版本开始, Hugo 使用了Go语言1.11版本提供的Go语言Modules模块技术来编译构建。
最简单的方式是像下面这样开始，在GOPATH环境变量包含目录以外的某个目录中克隆出来hugo源代码:

{{< code file="from-gh.sh" >}}
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install --tags extended
{{< /code >}}

如果不需要Sass/Scss支持，请删除上面命令中的`--tags extended`部分.

{{% note %}}
If you are a Windows user, substitute the `$HOME` environment variable above with `%USERPROFILE%`.
如果是Windows用户,请使用`%USERPROFILE%`环境变量替换命令行中 `$HOME` 环境变量
{{% /note %}}

## macOS

### 假设条件

1. 您会打开macOS的命令行
2. 您运行的是64位的现代Mac
3. 您将使用 `~/Sites` 目录作为站点的启动点。(使用`~/Sites` 目录是出于示例目的。如果您足够熟悉命令行和文件系统，那么按照指导进行您应该没有问题.)


### 选择安装方法

There are three ways to install Hugo on your Mac
有三种方法在Mac上安装Hugo

1. 使用包管理器,比如[Homebrew][brew] (`brew`) 或者 [MacPorts][macports] (`port`)
2. 下载分发的版本 (如 tarball文件)
3. 从源代码构建


There is no "best" way to install Hugo on your Mac. You should use the method that works best for your use case.

在mac上安装hugo没有最好的方式，请选择适合您案例的最佳方式

#### 优点和缺点

前面提到的方法都有各自的优点和缺点

1. **使用包管理器** 是最简单的方法，需要的工作量也最小。缺点到不是很严重, 下载来的包默认来源于最新的发布版本，会缺少错误修复直到下一个发布版本为止(除非在使用homebrew安装Hugo时应用了 `--HEAD` 参数), 版本发布由于团队协作的原因可能会拖后几天。无论如何，使用包管理器安装Hugo是推荐的方法，可以获得稳定的、广泛使用的源，而且包管理器表现不错还容易更新。

2. **Tarball文件** 下载tarball压缩包并从压缩包安装也容易，虽然比包管理器方式安装要求的命令行指令技能会多一些。包内容更新也很容易,有新版本是在操作一遍即可。这样系统可以很灵活的包含多个版本。如果不使用`brew`, 压缩包/二进制文件是一个好选项。


3. **从源码构建.** 从源码构建需要做的工作最多。从源码构建的好处是，可以在版本发布前添加特性或者修复错误。麻烦的地方就是可能需要更多时间来管理代码编译的定制工作。从源码构建的复杂性可以管理，但是比上面两种选项需要更多时间。

{{% note %}}
由于从源码编译对具有更多命令行经验的老手来说更有吸引力, 本文会更多关注通过Homebrew和压缩文件包来安装Hugo
{{% /note %}}

### 使用Brew安装Hugo

{{< youtube WvhCGlLcrF8 >}}

#### 第一步: (如果尚未安装) 安装 `brew`,

Go to the `brew` website, <https://brew.sh/>, and follow the directions there. The most important step is the installation from the command line:

访问`brew`网站 <https://brew.sh/>,按照文档指示。最重要步骤是在终端命令行中安装:

{{< code file="install-brew.sh" >}}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{{< /code >}}

#### 第二步: 运行 `brew` 命令安装 `hugo`

使用 `brew` 安装hugo如下一行命令，很简单:

{{< code file="install-brew.sh" >}}
brew install hugo
{{< /code >}}

如果Homebrew 运行正常, 终端里会看到类似下面的输出:


```
==> Downloading https://homebrew.bintray.com/bottles/hugo-0.21.sierra.bottle.tar.gz
######################################################################### 100.0%
==> Pouring hugo-0.21.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/hugo/0.21: 32 files, 17.4MB
```

{{% note "Installing the Latest Hugo with Brew" %}}
如果需要绝对最新的开发中版本，请将 `brew install hugo` 替换为 `brew install hugo --HEAD`
{{% /note %}}

`brew` 应该更新了系统path环境变量，使其包含了hugo命令。
打开新的终端运行下面命令来确认安装Hugo成功。

```
$ # 显示hugo命令的目录
which hugo
/usr/local/bin/hugo

# 显示安装的Hugo 版本
ls -l $( which hugo )
lrwxr-xr-x  1 mdhender admin  30 Mar 28 22:19 /usr/local/bin/hugo -> ../Cellar/hugo/0.13_1/bin/hugo

# 验证hugo运行正常
hugo version
Hugo Static Site Generator v0.13 BuildDate: 2015-03-09T21:34:47-05:00
```

### 从压缩包安装Hugo

#### Step 1: Decide on the location

从压缩包安装，您必须决定是在`/usr/local/bin`还是在您的home目录安装。在对待这个问题上有三种阵营:

1. 在 `/usr/local/bin`安装hugo，这样系统所有用户都有访问hugo的权限。这是一个好主意，因为这是可执行文件的相当标准的目录。不妥的地方在于，可能需要提权才能将软件安装在哪里。

2. 在 `~/bin` 目录中安装Hugo，只有您可以运行它。这个主意不错，安装很简单，维护也很简单，并且不需要提权。不好地方就是只有你能运行Hugo。如果站点有其他用户，他们不得不维护自己的备份。这可能会导致不同用户运行不同版本。当让，对于想体验不同版本来说也很方便。

3. 在您的`Sites`目录中安装hugo，如果仅仅构建一个站点，这是个不坏的主意。所有的文件都在一个地方。如果需要测试新版本，可以copy备份整个站点然后更新Hugo命令。


选择三个方案中每个都会运行良好。为简单起见，下面主要讨论第二个方案。

#### 第二步: 下载Tarball压缩包

1. 在浏览器中访问 <https://github.com/gohugoio/hugo/releases> (译注: 请使用github支持的浏览器及其相应版本，某些浏览器的老版本已经不受支持)

2. 向下找到当前版本，找到对应的"latest Release"(Find the current release by scrolling down and looking for the green tag that reads "Latest Release.")

3.  下载Mac的最新版本压缩包。压缩包文件名称类似 `hugo_X.Y_osx-64bit.tgz`, 这里 `X.YY` 是版本号

4. 压缩包会默认下载在您的`~/Downloads` 目录，如果选择使用了不同的目录，在下面的命令中需要相应的改变。

#### 第三步: 确认下载

确认下载的压缩包在下载过程中没有损坏:

```
tar tvf ~/Downloads/hugo_X.Y_osx-64bit.tgz
-rwxrwxrwx  0 0      0           0 Feb 22 04:02 hugo_X.Y_osx-64bit/hugo_X.Y_osx-64bit.tgz
-rwxrwxrwx  0 0      0           0 Feb 22 03:24 hugo_X.Y_osx-64bit/README.md
-rwxrwxrwx  0 0      0           0 Jan 30 18:48 hugo_X.Y_osx-64bit/LICENSE.md
```

以 `.md` 结尾的markdown文件是Hugo的文档。其它文件是可执行文件。

#### 第四步: 在`bin` 目录中安装Hugo

```
# 如果有必要, 新建目录
mkdir -p ~/bin

# 进入新建目录，将新建目录作为工作目录
cd ~/bin

# 解压压缩包 extract the tarball
tar -xvzf ~/Downloads/hugo_X.Y_osx-64bit.tgz
Archive:  hugo_X.Y_osx-64bit.tgz
  x ./
  x ./hugo
  x ./LICENSE.md
  x ./README.md

# 验证hugo可以运行
./hugo version
Hugo Static Site Generator v0.13 BuildDate: 2015-02-22T04:02:30-06:00
```

You may need to add your bin directory to your `PATH` environment variable. The `which` command will check for us. If it can find `hugo`, it will print the full path to it. Otherwise, it will not print anything.

可能有需要将您的命令目录加入到您的 `PATH` 环境变量中. 命令`which`会为我们检查是否有Hugo。如果它发现了`hugo`,它会打印hugo的全路径，否则，它不打印任何内容

```
# 检查hugo是否在路径中
which hugo
/Users/USERNAME/bin/hugo
```

If `hugo` is not in your `PATH`:

如果 `hugo` 没在您的 `PATH`环境变量中:

1. 确定您的默认shell(zsh 或者是 bash)
Determine your default shell (zsh or bash).

   ```
   echo $SHELL
   ```

2. 编辑脚本档案 Edit your profile.

   如果默认shell是zsh:

   ```
   nano ~/.zprofile
   ```

   如果默认shell是bash:

   ```
   nano ~/.bash_profile
   ```

3. 添加一行，将 `$HOME/bin` 添加到已经存在的 `PATH`环境变量中.

   ```
   export PATH=$PATH:$HOME/bin
   ```

4. 按键 Control-X 保存文件, 然后按 Y键.

5. 关闭命令行终端，再打开一个终端以确认对您环境变量变化的更改。再次运行`which hugo`命令曲确认变化。

完成这些，Hugo已经安装成功

### 在Mac上从源码构建

如果想从源码编译Hugo, 那么需要安装 Go语言(Golang). 可以从[从Go语言网站直接安装](https://golang.org/dl/) 或者通过Homebrew使用如下命令:

```
brew install go
```

#### 第一步: 获取源代码


如果想获得某特定版本的Hugo源代码，可以去 <https://github.com/gohugoio/hugo/releases>
下载目标版本的源码。如果想包含最新的变化(可能包含bugs)，克隆Hugo 资料库到本地:


```
git clone https://github.com/gohugoio/hugo
```


直接从Hugo资源库克隆代码是一把双刃刀，得到好处同时也带来缺陷。使用Hugo的最最新的版本，你的开发可以接受Hugo的最新特性，同时也带来最新的缺陷。欢迎反馈。如果在最新版本中发现bug, [请在Github上提交一个问题](https://github.com/gohugoio/hugo/issues/new).


#### 第二步: 编译

创建目录作为您存储源码的工作目录,然后获取Hugo的依赖:

```
mkdir -p src/github.com/gohugoio
ln -sf $(pwd) src/github.com/gohugoio/hugo

go get
```

上面命令会获取依赖包的最新版本,如果Hugo编译失败，远影可能是依赖包的开发者引入了巨大的变化。

在你正确配置目录以后，可以用下面语句编译Hugo:

```
go build -o hugo main.go
```

然后将 `Hugo`可执行文件置于系统环境变量中，现在您可以开始使用Hugo了。

## Windows系统

下面视频旨在成为在Windows PC上安装Hugo的完整指导

{{< youtube G7umPCU-8xc >}}

### 假设

1. 您使用 `C:\Hugo\Sites` 目录作为新项目的起始点.
2. 您使用 `C:\Hugo\bin`目录作为存储hugo可执行文件的目录


### 配置目录

您需要存储Hugo可执行文件，[content][]和产生的Hugo站点文件的目录:

1. 打开Windows 浏览器
2. 创建 `C:\Hugo`目录, 假设您想Hugo置于您的C盘，当然可以在任何地方
3. 在Hugo目录下创建子目录: `C:\Hugo\bin`
4. 在Hugo目录下创建另一个子目录: `C:\Hugo\Sites`

### 技术用户

1. 从[Hugo Releases][releases]下载最新的Hugo可执行性文件压缩包.
2. 解压缩所有内容到你的 `..\Hugo\bin` 目录.
3. 在 PowerShell 或者您喜欢的命令行洁面中, 跳转目录到`C:\Hugo\bin`, 运行命令 `set PATH=%PATH%;C:\Hugo\bin`将 `C:\Hugo\bin`(或者您hugo.exe文件所在位置), 将`hugo.exe` 可执行文件添加到您的系统变量中。如果重新启动后，hugo不起作用，您可能需要作为系统管理员来运行这个命令。

### 非技术用户

1. 打开浏览器转至 [Hugo发布版本][releases] 页面.
2. 最新的版本在网页上面, 滚动到版本发布信息的底部下载链接部分, 都是ZIP文件
3. 找到windows文件在底部附近(按字母顺序，所以windows在后面), 根据你的操作系统是32位还是64位选择相应的32位或者64位发布版本。(如果不清楚自己系统位数, [请看这里](https://esupport.trendmicro.com/en-us/home/pages/technical-support/1038680.aspx).)
4. 下载好文件后, 将压缩文件移动到您的 `C:\Hugo\bin` 目录.
5. 双击ZIP文件解压缩，确保解压缩文件放在相同的 `C:\Hugo\bin`目录-windows默认会这样做, 除非您会选择其他地方解压缩文件
6. 现在您应该有下面三个文件: hugo可执行文件 (`hugo.exe`)、许可`LICENSE` 和自述文件`README.md`.

接下来您需要将Hugo添加到Windows的路径环境变量设置中:

#### Windows 10用户设置方法:

* 右键点击 **开始** 按钮.
* 点击 **系统**.
* 点击左侧 **高级系统设置**.
* 点击底部 **环境变量...** 按钮
* 在用户变量部分, 找到以大写字母**PATH**开头的那行.
* 双击**PATH**.
* 点击 **新建...** 按钮
* 在目录输入框中输入 `hugo.exe` 的解压缩目录, 也就是 `C:\Hugo\bin`, 如果您是按照上面的指示安装的。*这个环境变量是Hugo所在的目录而不是二进制文件本身.*   输入完毕 按 <kbd>Enter</kbd> 键.
* 在每个窗口点击OK退出设置.

{{% note "Path Editor in Windows 10"%}}
Windows 10的 路径编辑器是在大约[2015年11月更新](https://blogs.windows.com/windowsexperience/2015/11/12/first-major-update-for-windows-10-available-today/)中添加的。您系统内应该有这个或者以后的更新, 上面步骤才会工作。您可以通过点击<i class="fa fa-windows">&nbsp;Start button → Settings → System → About</i>来查看自己的Windows 10版本。也请参考[这里](https://www.howtogeek.com/236195/how-to-find-out-which-build-and-version-of-windows-10-you-have/)获得更新信息。
{{% /note %}}

####  Windows 7 和 8.x 用户:

Windows 7和8.1没有包换Windows 10中包含的简单的路径环境变量编辑器, 建议这些系统的非技术用户安装免费的第三方路径变量编辑器,比如like [Windows Environment Variables Editor][Windows Environment Variables Editor] 或者[Path Editor](https://patheditor2.codeplex.com/)
).

### 验证可执行文件 Verify the Executable

运行几个命令确认可执行文件已经就绪，然后从构建一个例子站点开始.

#### 1. 打开命令提示符

在提示符界面，输入`hugo help` 然后按<kbd>Enter</kbd> 键。会看见如下面这样开始的输出:

```
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at https://gohugo.io/.
```

如果看见这样的输出, 恭喜！安装已经完成了。如果没有看见，那么仔细检查在环境变量中输入的是正确的`hugo.exe`文件所在的目录。 如果还是没有正确，请查询[Hugo discussion forum][forum]，看看是否有其他人已经解决了我们的文同。如果还没有，在"support"类目下留个note-请确保包含您的命令 和 您的输出。

At the prompt, change your directory to the `Sites` directory.
在命令行，变化目录到您的站点`Sites`目录
```
C:\Program Files> cd C:\Hugo\Sites
C:\Hugo\Sites>
```

#### 2. 运行命令

运行命令生成新站点，此处使用了`example.com`作为站点的名字

```
C:\Hugo\Sites> hugo new site example.com
```
现在您应该看见目录`C:\Hugo\Sites\example.com`。 进入目录，列表显示内容。会看见同下面类似的输出:

```
C:\Hugo\Sites> cd example.com
C:\Hugo\Sites\example.com> dir
Directory of C:\hugo\sites\example.com

04/13/2015  10:44 PM    <DIR>          .
04/13/2015  10:44 PM    <DIR>          ..
04/13/2015  10:44 PM    <DIR>          archetypes
04/13/2015  10:44 PM                83 config.toml
04/13/2015  10:44 PM    <DIR>          content
04/13/2015  10:44 PM    <DIR>          data
04/13/2015  10:44 PM    <DIR>          layouts
04/13/2015  10:44 PM    <DIR>          static
               1 File(s)             83 bytes
               7 Dir(s)   6,273,331,200 bytes free
```

###  Windows安装故障解决

[@dhersam][] 创建了常见问题解决方案的视频(英语):

{{< youtube c8fJIRNChmU >}}

## Linux安装

### 通过Snap安装

In any of the [Linux distributions that support snaps][snaps], you may install the "extended" Sass/SCSS version with this command:
在任何[支持snap的Linux发行版][snaps]上, 您可以通过如下命令安装extended扩展(带Sass/Scss处理)能力的hugo:

    snap install hugo --channel=extended

安装不含扩展能力的Hugo:

    snap install hugo

在这两个版本之间切换使用 `snap refresh hugo --channel=extended` 或者 `snap refresh hugo --channel=stable`.

{{% note %}}
通过Snap安装的hugo仅仅能够写位于用户主目录以及gvfs加载的用户所有的目录---这是由于Snap的限制和安全模型所致。更多信息请见[in this related GitHub issue](https://github.com/gohugoio/hugo/issues/3143).
{{% /note %}}

### Debian and Ubuntu

[@anthonyfok](https://github.com/anthonyfok) 和 [Debian Go Packaging Team](https://go-team.pages.debian.net/) 团队的朋友们维护着官方的Hugo [Debian 发布版本](https://packages.debian.org/hugo), 通过[Ubuntu](https://packages.ubuntu.com/hugo)分发,可以通过 `apt-get`安装:


    sudo apt-get install hugo

通过这个命令安装的内容依赖于您Debian/Ubuntu版本。在Ubuntu Bionic版本(18.04)这个安装的是非扩展的版本，没有Sass/Scss支持。在Ubuntu disco(19.04), 命令安装的是带有Sass/Scss扩展版本的Hugo

不推荐这个安装选项，原因是Debian和Ubuntu的Linux包管理器中包含的hugo版本经常会晚几个版本. 详情请见[这里](https://github.com/gcushen/hugo-academic/issues/703)

### Arch Linux

可以通过Arch Linux[社区](https://www.archlinux.org/packages/community/x86_64/hugo/) 资源库安装Hugo。 对于Arch Linux派生版本也一样

```
sudo pacman -Syu hugo
```

### Fedora, Red Hat and CentOS


Fedora维护着[官方Hugo版本](https://apps.fedoraproject.org/packages/hugo)，可以通过下面命令安装:

    sudo dnf install hugo

最新版本，由[@daftaupe](https://github.com/daftaupe)维护的在Fedora公司的Hugo版本值得推荐:
* <https://copr.fedorainfracloud.org/coprs/daftaupe/hugo/>

请参考 [Hugo 论坛相关讨论][redhatforum].

### Solus

Solus 在包资源库包含Hugo, 通过下面命令安装:

```
sudo eopkg install hugo
```

## OpenBSD

OpenBSD 提供了hugo包，通过 `pkg_add`安装:

    doas pkg_add hugo


## 更新 Hugo

更新Hugo很简单， 下载并替换原有的可执行文件就可以了，或者如果使用homebrew执行`brew upgrade hugo`


## 后续
现在已经安装Hugo，接下累可以阅读[快速开始文档][quickstart], 探索文档的其余部分。 如果有问题请访问 [Hugo Discussion Forum][forum]访问Hugo社区咨询。

[brew]: https://brew.sh/
[macports]: https://www.macports.org/
[Chocolatey]: https://chocolatey.org/
[content]: /content-management/
[@dhersam]: https://github.com/dhersam
[forum]: https://discourse.gohugo.io
[mage]: https://github.com/magefile/mage
[dep]: https://github.com/golang/dep
[highlight shortcode]: /content-management/shortcodes/#highlight
[installgit]: https://git-scm.com/
[installgo]: https://golang.org/dl/
[linuxbrew]: https://docs.brew.sh/Homebrew-on-Linux
[Path Editor]: https://patheditor2.codeplex.com/
[pygments]: https://pygments.org
[quickstart]: /getting-started/quick-start/
[redhatforum]: https://discourse.gohugo.io/t/solved-fedora-copr-repository-out-of-service/2491
[releases]: https://github.com/gohugoio/hugo/releases
[Scoop]: https://scoop.sh/
[snaps]: https://snapcraft.io/docs/installing-snapd
[windowsarch]: https://esupport.trendmicro.com/en-us/home/pages/technical-support/1038680.aspx
[Windows Environment Variables Editor]: http://eveditor.com/
