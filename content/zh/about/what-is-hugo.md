---
title: Hugo简介
linktitle: Hugo简介
description: Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again. Hugo是一个快速的、现代静态网站生成器、使用Go语言编写、设计宗旨是使得网站创建再次变得有趣.
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
layout: single
menu:
  docs:
    parent: "about"
    weight: 10
weight: 10
sections_weight: 10
draft: false
aliases: [/overview/introduction/,/about/why-i-built-hugo/]
toc: true
---

Hugo is a general-purpose website framework. Technically speaking, Hugo is a [static site generator][]. Unlike systems that dynamically build a page with each visitor request, Hugo builds pages when you create or update your content. Since websites are viewed far more often than they are edited, Hugo is designed to provide an optimal viewing experience for your website's end users and an ideal writing experience for website authors.
Hugo是一个常见的网站框架. 从技术上来说, Hugo是一个[static site generator][]静态网站生成器. 与每个用户访问是动态构建页面的系统不同,Hugo在您创建或者更新内容的时候构建网页. 由于网站被访问的次数远远多于它们被编辑的次数,因此Hugo设计的宗旨是提供给网站访问者最佳访问体验、同时给网站作者提供理想的写作经验.

Websites built with Hugo are extremely fast and secure. Hugo sites can be hosted anywhere, including [Netlify][], [Heroku][], [GoDaddy][], [DreamHost][], [GitHub Pages][], [GitLab Pages][], [Surge][], [Aerobatic][], [Firebase][], [Google Cloud Storage][], [Amazon S3][], [Rackspace][], [Azure][], and [CloudFront][] and work well with CDNs. Hugo sites run without the need for a database or dependencies on expensive runtimes like Ruby, Python, or PHP.
使用Hugo构建的网站及其快速安全. Hugo站点可以托管在任何服务上, 包括[Netlify][], [Heroku][], [GoDaddy][], [DreamHost][], [GitHub Pages][], [GitLab Pages][], [Surge][], [Aerobatic][], [Firebase][], [Google Cloud Storage][], [Amazon S3][], [Rackspace][], [Azure][], and [CloudFront][] 并且与CDN服务配合良好. HUgo站点运行不需要数据库、不依赖于昂贵的运行时如ruby, Python,或者PHP.

We think of Hugo as the ideal website creation tool with nearly instant build times, able to rebuild whenever a change is made.
我们认为Hugo是理想的网站创建工具、具有几乎瞬时的构建时间、可以在任何网站变化的时刻重建.

## How Fast is Hugo? Hugo有多快?

{{< youtube "CdiDYZ51a2o" >}}

## What Does Hugo Do? HUgo的工作是什么?

In technical terms, Hugo takes a source directory of files and templates and uses these as input to create a complete website.
以技术术语来说, Hugo接受文件和模板的源目录作为输入来创建完整的网站.

## Who Should Use Hugo? 谁需要Hugo？

Hugo is for people that prefer writing in a text editor over a browser.
Hugo适合于对比浏览器更偏爱文本编辑器的工作者.

Hugo is for people who want to hand code their own website without worrying about setting up complicated runtimes, dependencies and databases.
HUgo适合于希望手工敲代码构建他们自己网站的作者、而无需担心设置复杂的运行环境、依赖的软件和数据库。

Hugo is for people building a blog, a company site, a portfolio site, documentation, a single landing page, or a website with thousands of pages.
HUgo适合于这些人、构建博客、创建公司站点、档案站点、文档站点、一个单独的登录页或者具有好几千页面的网站的工作者.

[@spf13]: https://twitter.com/spf13
[Aerobatic]: https://www.aerobatic.com/
[Amazon S3]: https://aws.amazon.com/s3/
[Azure]: https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website
[CloudFront]: https://aws.amazon.com/cloudfront/ "Amazon CloudFront"
[DreamHost]: https://www.dreamhost.com/
[Firebase]: https://firebase.google.com/docs/hosting/ "Firebase static hosting"
[GitHub Pages]: https://pages.github.com/
[GitLab Pages]: https://about.gitlab.com/features/pages/
[Go language]: https://golang.org/
[GoDaddy]: https://www.godaddy.com/ "GoDaddy.com Hosting"
[Google Cloud Storage]: https://cloud.google.com/storage/
[Heroku]: https://www.heroku.com/
[Jekyll]: https://jekyllrb.com/
[Middleman]: https://middlemanapp.com/
[Nanoc]: https://nanoc.ws/
[Netlify]: https://netlify.com
[Rackspace]: https://www.rackspace.com/cloud/files
[Surge]: https://surge.sh
[contributing to it]: https://github.com/gohugoio/hugo
[rackspace]: https://www.rackspace.com/openstack/public/files
[static site generator]: /about/benefits/
