---
title: "图像处理"
description: "图片页面资源可以调整大小和剪裁."
date: 2018-01-24T13:10:00-05:00
linktitle: "图像处理"
categories: ["content management"]
keywords: [resources, images]
weight: 4004
draft: false
toc: true
menu:
  docs:
    parent: "content-management"
    weight: 32
---

## 图片页面资源

`图片`是 [页面资源]({{< relref "/content-management/page-resources" >}}),
下面列出来的处理方法对在静态资源 `/static`目录中的图片不起作用。

打印[页面包]({{< relref "/content-management/organization#page-bundles" >}})中所有图片:

```go-html-template
{{ with .Resources.ByType "image" }}
{{ range . }}
{{ .RelPermalink }}
{{ end }}
{{ end }}

```

## 图片资源
`image` 资源也可以从 [全局资源global resource]({{< relref "/hugo-pipes/introduction#from-file-to-resource" >}})获取。

```go-html-template
{{- $image := resources.Get "images/logo.jpg" -}}
```

## 图像处理方法

图片`image`资源实现了   `Resize`, `Fit`, `Fill`, and `Filter` 方法，
每个方法返回一个应用指定的面积和处理选项变换后的图片.

{{% note %}}
图像变换时图像元数据 (EXIF, IPTC, XMP, etc.) 并没有保留.
对_original_原图像使用[`Exif`](#exif) 方法提取JEPG或者TIFF图像的EXIF元数据.
{{% /note %}}

### 缩放 Resize

缩放图片到指定宽度和高度.

```go
// Resize to a width of 600px and preserve ratio
{{ $image := $resource.Resize "600x" }}

// Resize to a height of 400px and preserve ratio
{{ $image := $resource.Resize "x400" }}

// Resize to a width 600px and a height of 400px
{{ $image := $resource.Resize "600x400" }}
```

### 适配 Fit

缩小图片到适合给定的面积并保留长宽比。高度和宽度都是需要的参数。
Scale down the image to fit the given dimensions while maintaining aspect ratio. Both height and width are required.

```go
{{ $image := $resource.Fit "600x400" }}
```

### 填充 Fill

缩放和剪切图片到指定面积。要求指定高度和宽度。
Resize and crop the image to match the given dimensions. Both height and width are required.

```go
{{ $image := $resource.Fill "600x400" }}
```

### 滤镜 Filter

对图片应用一个或多个滤镜。参考 [Image Filters](/functions/images/#image-filters) 获得滤镜完整列表.


```go-html-template
{{ $img = $img.Filter (images.GaussianBlur 6) (images.Pixelate 8) }}
```

上面代码也可以使用更具函数化的风格的管道来写:

```go-html-template
{{ $img = $img | images.Filter (images.GaussianBlur 6) (images.Pixelate 8) }}
```

滤镜应用是按照给定的顺序.

有时候可以创建滤镜链，然后在需要时使用，这很有帮助。
Sometimes it can be useful to create the filter chain once and then reuse it:

```go-html-template
{{ $filters := slice  (images.GaussianBlur 6) (images.Pixelate 8) }}
{{ $img1 = $img1.Filter $filters }}
{{ $img2 = $img2.Filter $filters }}
```

### Exif

提供图片的[Exif](https://en.wikipedia.org/wiki/Exif)对象和元数据

注意仅仅支持JPEG和TIFF图像，所以建议使用 `with`关键字将访问语句包裹起来，如下列:

```go-html-template
{{ with $img.Exif }}
Date: {{ .Date }}
Lat/Long: {{ .Lat}}/{{ .Long }}
Tags:
{{ range $k, $v := .Tags }}
TAG: {{ $k }}: {{ $v }}
{{ end }}
{{ end }}
```

或者单独的访问EXIF信息通过.访问符号，如:

```go-html-template
{{ with $src.Exif }}
  <ul>
      {{ with .Date }}<li>Date: {{ .Format "January 02, 2006" }}</li>{{ end }}
      {{ with .Tags.ApertureValue }}<li>Aperture: {{ lang.NumFmt 2 . }}</li>{{ end }}
      {{ with .Tags.BrightnessValue }}<li>Brightness: {{ lang.NumFmt 2 . }}</li>{{ end }}
      {{ with .Tags.ExposureTime }}<li>Exposure Time: {{ . }}</li>{{ end }}
      {{ with .Tags.FNumber }}<li>F Number: {{ . }}</li>{{ end }}
      {{ with .Tags.FocalLength }}<li>Focal Length: {{ . }}</li>{{ end }}
      {{ with .Tags.ISOSpeedRatings }}<li>ISO Speed Ratings: {{ . }}</li>{{ end }}
      {{ with .Tags.LensModel }}<li>Lens Model: {{ . }}</li>{{ end }}
  </ul>
{{ end }}
```

有些域需要使用[`lang.NumFmt`]({{< relref "functions/numfmt" >}}) 函数进行格式化，避免显示类似输出 `Aperture: 2.278934289`, 格式化的输出应该这样 `Aperture: 2.28`.

#### Exif 域

Date
: 照片拍摄日期/时间 "photo taken" date/time

Lat
: 照片拍摄地点的纬度  "photo taken where", GPS latitude

Long
: 照片拍摄地点的经度 "photo taken where", GPS longitude

参考 [图像处理配置](#image-processing-config) 获取Exif应包含域的如何进行配置的信息。for how to configure what gets included in Exif.

## 图像处理选项

在面积 (如 `600x400`)外, Hugo还支持一系列额外的图像选项.

### Background Color 背景色

The background color to fill into the transparency layer. This is mostly useful when converting to a format that does not support transparency, e.g. `JPEG`.
填充透明图层的背景颜色。这对于将转化为的不支持透明的图像格式，比如`JPEG`非常有用。

可以用`#`号开头的3位或者6位的16进制字符串来设置背景色.

```go
{{ $image.Resize "600x jpg #b31280" }}
```

颜色代码、请参考 https://www.google.com/search?q=color+picker

**注意** 您也需要设置默认使用的背景色，参考 [图像处理配置](#image-processing-config).

### JPEG质量 JPEG Quality

仅与JPEG图像相关。值从1到100包含边界，越高数值越清晰。默认是75
Only relevant for JPEG images, values 1 to 100 inclusive, higher is better. Default is 75.

```go
{{ $image.Resize "600x q50" }}
```

### 旋转 Rotate

Rotates an image by the given angle counter-clockwise. The rotation will be performed first to get the dimensions correct. The main use of this is to be able to manually correct for [EXIF orientation](https://github.com/golang/go/issues/4341) of JPEG images.

将图像以指定角度逆时针旋转。旋转操作会先进行来获得正确的面积。这个参数主要用处是手工调整JPEG图片的 [EXIF orientation](https://github.com/golang/go/issues/4341) .

```go
{{ $image.Resize "600x r90" }}
```

### 锚点 Anchor

Only relevant for the `Fill` method. This is useful for thumbnail generation where the main motive is located in, say, the left corner.
仅仅对`Fill`方法有用.这对于生成缩略图有用，比如主要动机位于,比如说左边角。

Valid values are `Smart`, `Center`, `TopLeft`, `Top`, `TopRight`, `Left`, `Right`, `BottomLeft`, `Bottom`, `BottomRight`.

有效值包括 `Smart`, `Center`, `TopLeft`, `Top`, `TopRight`, `Left`, `Right`, `BottomLeft`, `Bottom`, `BottomRight`.

Default value is `Smart`, which uses [Smartcrop](https://github.com/muesli/smartcrop) to determine the best crop.
默认值为`Smart`, 它使用[Smartcrop](https://github.com/muesli/smartcrop)来决定最佳的剪裁.

```go
{{ $image.Fill "300x200 BottomLeft" }}
```

### 重采样滤镜 Resample Filter

Filter used in resizing. Default is `Box`, a simple and fast resampling filter appropriate for downscaling.
缩放时候使用的滤镜。默认是`Box`, 一个简单快速的重新采样滤镜，适合缩小采样规模。

可选值例子有: `Box`, `NearestNeighbor`, `Linear`, `Gaussian`.

参考 https://github.com/disintegration/imaging.
如果有限选择快速处理而不是图片质量，这是一个可以测试的选项。
If you want to trade quality for faster processing, this may be a option to test.

```go
{{ $image.Resize "600x400 Gaussian" }}
```

### 目标格式 Target Format

By default the images is encoded in the source format, but you can set the target format as an option.

默认情况下，变化后图片和原图片是一种格式, 但是作为一种选择也可以设置目标图片的格式

有效的值包括 `jpg`, `png`, `tif`, `bmp` 和 `gif`.

```go
{{ $image.Resize "600x jpg" }}
```

## 图像处理例子 Image Processing Examples

_例子中使用的日落图片版权为[Bjørn Erik Pedersen](https://commons.wikimedia.org/wiki/User:Bep) (Creative Commons Attribution-Share Alike 4.0 International license)_


{{< imgproc sunset Resize "300x" />}}

{{< imgproc sunset Fill "90x120 left" />}}

{{< imgproc sunset Fill "90x120 right" />}}

{{< imgproc sunset Fit "90x90" />}}

{{< imgproc sunset Resize "300x q10" />}}

这是上面例子中使用的短代码:

{{< code file="layouts/shortcodes/imgproc.html" >}}
{{< readfile file="layouts/shortcodes/imgproc.html" >}}  
{{< /code >}}

这样使用短代码

```go-html-template
{{</* imgproc sunset Resize "300x" /*/>}}
```

{{% note %}}
**Tip:** Note the self-closing shortcode syntax above. The `imgproc` shortcode can be called both with and without **inner content**.
{{% /note %}}

## 图像处理配置

可以在配置文件中`config.toml`以默认图像处理选项配置 `imaging`区块:

```toml
[imaging]
# 图片缩放使用的缺省的重采样滤镜. 默认是Box,
# 缩小图片时适用的简单快速平均滤镜.
# 参考 https://github.com/disintegration/imaging
resampleFilter = "box"

# 缺省的JPEG质量设置值. 默认是75.
quality = 75

# 剪切图片时适用的锚
# Anchor used when cropping pictures.
# Default is "smart" which does Smart Cropping, using https://github.com/muesli/smartcrop
# Smart Cropping is content aware and tries to find the best crop for each image.
# Valid values are Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
anchor = "smart"

# 缺省背景色.
# 如果目标格式支持,Hugo会保留透明度,
# 对JPEG会适用下面的缺省值.
# 3位或者6位的16进制颜色字符串.
# 参考 https://www.google.com/search?q=color+picker
bgColor = "#ffffff"

[imaging.exif]
# Regexp matching the fields you want to Exclude from the (massive) set of Exif info
# available. As we cache this info to disk, this is for performance and
# disk space reasons more than anything.
# If you want it all, put ".*" in this config setting.
# Note that if neither this or ExcludeFields is set, Hugo will return a small
# default set.
includeFields = ""

# Regexp matching the Exif fields you want to exclude. This may be easier to use
# than IncludeFields above, depending on what you want.
excludeFields = ""

# Hugo extracts the "photo taken" date/time into .Date by default.
# Set this to true to turn it off.
disableDate = false

# Hugo extracts the "photo taken where" (GPS latitude and longitude) into
# .Long and .Lat. Set this to true to turn it off.
disableLatLong = false


```

## Smart Cropping of Images

By default, Hugo will use [Smartcrop](https://github.com/muesli/smartcrop), a library created by [muesli](https://github.com/muesli), when cropping images with `.Fill`. You can set the anchor point manually, but in most cases the smart option will make a good choice. And we will work with the library author to improve this in the future.

An example using the sunset image from above:

{{< imgproc sunset Fill "200x200 smart" />}}

## Image Processing Performance Consideration

Processed images are stored below `<project-dir>/resources` (can be set with `resourceDir` config setting). This folder is deliberately placed in the project, as it is recommended to check these into source control as part of the project. These images are not "Hugo fast" to generate, but once generated they can be reused.

If you change your image settings (e.g. size), remove or rename images etc., you will end up with unused images taking up space and cluttering your project.

To clean up, run:

```bash
hugo --gc
```

{{% note %}}
**GC** is short for **Garbage Collection**.
{{% /note %}}
