---
title : "页面包 Page Bundles"
description : "使用页面包Page Bundles组织内容"
date : 2018-01-24T13:09:00-05:00
linktitle : "页面包"
keywords : ["page", "bundle", "leaf", "branch"]
categories : ["content management"]
toc : true
menu :
  docs:
    identifier : "page-bundles"
    parent : "content-management"
    weight : 11
---

页面包是组织[Page Resources](/content-management/page-resources/)的方式

一个页面包可能是:

- 叶子包Leaf Bundle (叶子意味着没有子元素)
- 分支包Branch Bundle (主页、区块、标签条目、标签列表)

|                                     | 叶子包                                                    | 分支包                                                                                                                                                                                                      |
|-------------------------------------|----------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  |
| 用法                       | 单一页面的内容和附件的集合         |  区块页面的附件的集合(主页、区块、标签页、标签条目列表)                                                                                                                    |
| Index文件名                | `index.md` [^fn:1]             | `_index.md` [^fn:1]                                                                                                                                                                                                |
| 允许的资源                 | 页面和非页面(比如图片、pdf、等)类型 | 仅仅非页面类型(如图片、pdf文档等)                                                                                                                                                                       |
| 资源所在位置?               |在页子包目录下的任何目录层次内 | 仅在分支包**所在**目录, 即包含`_index.md` ([参考 ref](https://discourse.gohugo.io/t/question-about-content-folder-structure/11822/4?u=kaushalmodi))的目录 |
| 布局类型                  | `single`                   | `列表`                                                                                   |
| 允许嵌套            | 不允许在下面包含更多嵌套的包          | 允许下面包含嵌套更多叶子包和分支包                                                                                                      |
| 举例        | `content/posts/my-post/index.md`       | `content/posts/_index.md`                    |
| 引用非索引页资源... | 可以作为页面资源访问        | 只能作为常规页面访问|


## 叶子包 {#leaf-bundles}

 _叶子包_ 是指在`content/`目录中任何层次的目录, 改目录必须包含一个 **`index.md`** 文件.

### 叶子包目录/文件组织例子 {#examples-of-leaf-bundle-organization}

```text
content/
├── about
│   ├── index.md
├── posts
│   ├── my-post
│   │   ├── content1.md
│   │   ├── content2.md
│   │   ├── image1.jpg
│   │   ├── image2.png
│   │   └── index.md
│   └── my-other-post
│       └── index.md
│
└── another-section
    ├── ..
    └── not-a-leaf-bundle
        ├── ..
        └── another-leaf-bundle
            └── index.md
```

上面`/content`目录中，包含了4个叶子包:

about
: 这个包在根部(直接在`content`目录下面)仅有一个索引文件 `index.md`.

my-post
: 这个包有`index.md`文件, 另外两个markdown内容文件和两张图片文件。

image1
: 此图片作为 `my-post` 页面资源 仅仅在`my-post/index.md`的资源内可以访问


image2
: 此图片作为 `my-post` 页面资源 仅仅在`my-post/index.md`的资源内可以访问
This image is a page resource of `my-post`
    and only available in `my-post/index.md` resources.

my-other-post
: 这个叶子包仅仅具有 `index.md`.

another-leaf-bundle
: 这个叶子包嵌套在多个目录下面。页仅仅具有一个 `index.md`.

{{% note %}}
创建叶子包所在的层次深度并没有关系，只要不是在另一个 **叶子** 包下面即可.
{{% /note %}}


### 无名包 Headless Bundle {#headless-bundle}

无名包是配置成不会在任何地点发布的包:
-   没有 `Permalink`, 不会在 `public/`目录中生成HTML文件
-   不会成为 `.Site.RegularPages`等变量的一部分.

但是您可以通过 `.Site.GetPage`访问它. 下面是一个列子:

```go-html-template
{{ $headless := .Site.GetPage "/some-headless-bundle" }}
{{ $reusablePages := $headless.Resources.Match "author*" }}
<h2>Authors</h2>
{{ range $reusablePages }}
    <h3>{{ .Title }}</h3>
    {{ .Content }}
{{ end }}
```

_这个例子中, 我们假设`some-headless-bundle` 是一个无名包, 它包含一个或者多个 **页面** 资源,这写资源的`.Name` 名称和`"author*"`匹配._


上面例子的解释:

1. 获得 `some-headless-bundle` 页面 "对象".
2. 对这个 *页面包* 使用 `.Resources.Match` 获得匹配`"author*"`的资源的列表切片
3. 对资源的*列表切片*循环, 输出它们的 `.Title` 和 `.Content`.

---

叶子包可以通过在front matter(在文件`index.md`中)添加如下配置来设置:

```toml
headless = true
```

使用无名包的用例有很多,比如:

-   共享的媒体库
-   可重用的页面内容小块 "snippets"


## 分支包 {#branch-bundles}

A _Branch Bundle_ is any directory at any hierarchy within the
`content/` directory, that contains at least an **`_index.md`** file.

_分支包_ 是指在`content/`目录中任何层次的任何目录, 该目录必须包含至少一个 **`_index.md`** 文件.


这个 `_index.md` 也可以直接置于`content/`目录下

{{% note %}}
这里`md` (markdown) 仅仅是作为一个例子使用。
您可以使用任何文件类型作为内容资源，只要它是Hugo能够识别的内容类型.
{{% /note %}}


### 分支包组织的例子 {#examples-of-branch-bundle-organization}

```text
content/
├── branch-bundle-1
│   ├── branch-content1.md
│   ├── branch-content2.md
│   ├── image1.jpg
│   ├── image2.png
│   └── _index.md
└── branch-bundle-2
    ├── _index.md
    └── a-leaf-bundle
        └── index.md
```

上面例子 `content/` 目录中, 有两个分支包(还有一个叶子包):

`branch-bundle-1`
: 这个分支包具有 `_index.md`, 另外两个Markdown内容文件和两个图片文件.

`branch-bundle-2`
: 这个分支包具有 `_index.md` 和一个内嵌的叶子包.

{{% note %}}
创建分支包所在的目录层次深度没有关系
{{% /note %}}

[^fn:1]: 这个 `.md` 扩展仅仅作为示例。扩展也可以是 `.html`, `.json` 或者任何有效的 MIME 类型.
