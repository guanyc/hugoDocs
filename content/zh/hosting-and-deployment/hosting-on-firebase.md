---
title: Hugo中文文档 在Firebase服务上托管部署Hugo站点
linktitle: 托管在Firebase
description: 可以使用Firebase的免费套餐托管静态站点，同时也可以使用Firebase的 NOSQL API.
date: 2017-03-12
publishdate: 2017-03-12
lastmod: 2017-03-15
categories: [hosting and deployment]
keywords: [hosting,firebase]
authors: [Michel Racic]
menu:
  docs:
    parent: "hosting-and-deployment"
    weight: 20
weight: 20
sections_weight: 20
draft: false
toc: true
aliases: []
---

## 前提条件

1. 拥有 [Firebase][signup]账号. (如果没有, 请先使用Google账号[注册][signup])
2. 完成了快速开始[Quick Start][] 或者已经有了准备部署的Hugo站点.

## Firebase初始化设置

打开[Firebase console][console]管理界面、创建新项目(除非已经有了firebase 项目).
通常需要全局安装开发工具 `firebase-tools` (node.js):

```
npm install -g firebase-tools
```

安装firebase-tools之后使用 `firebase login`登录Firebase, 会打开浏览器,选择账号.
如果登录时选择了错误的账号，请使用`firebase logout`命令退出.


```
firebase login
```
然后, 在Hugo项目的根目录执行 `firebase init` 命令初始化这个Firebase项目:
In the root of your Hugo project, initialize the Firebase project with the `firebase init` command:

```
firebase init
```
此处:

1. 在Feature问题选项中选择 Hosting
2. 选择您设置的项目
3. 接受默认的数据规则文件
4. 接受默认的发布目录`public`
5. 在部署的app是否是单一页面single-page app的回答时选择`No`

## 部署

执行 `firebase deploy` 命令、部署您的Hugo站点、您的站点很快就会启动.

```
hugo && firebase deploy
```



## CI Setup

可以使用下面命令生成部署令牌:


```
firebase login:ci
```

也可以额设置您的CI(比如 [Wercker][]) 并且设置私有变量比如 `$FIREBASE_DEPLOY_TOKEN`.

{{% note %}}
这是一个私有秘密、并不能出现在公开资源库中。
请确认您理解您的CI，确保私钥不会被他人看见.
{{% /note %}}

然后可以在build添加部署、使用上面token:

```
firebase deploy --token $FIREBASE_DEPLOY_TOKEN
```

## 参考

* [Firebase CLI Reference](https://firebase.google.com/docs/cli/#administrative_commands)

[console]: https://console.firebase.google.com
[Quick Start]: /getting-started/quick-start/
[signup]: https://console.firebase.google.com/
[Wercker]: /hosting-and-deployment/deployment-with-wercker/
