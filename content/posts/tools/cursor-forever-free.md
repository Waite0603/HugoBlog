---
title: "Cursor 无限续杯方案"
date: 2025-01-12T20:55:23+08:00
lastmod: 2025-01-25T20:55:23+08:00
categories: ["Blog"]
tags: ["Blog"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "Cursor 无限续杯."
disableHLJS: true
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

> 参考地址：<https://github.com/chengazhen/cursor-auto-free>

在本篇文章开始之前，确保你已经安装好了 Cursor[https://www.cursor.com/]

本篇文章主要介绍[cursor-auto-free](https://github.com/chengazhen/cursor-auto-free)这个项目的使用。

## 源代码构建或者下载执行文件

我们可以通过 `git clone` 命令来下载源代码，或者直接在 Release 页面下载已经构建好的执行文件。

然后我们根据项目配置要求，配置变量

```bash
需要使用 cloudflare 域名邮箱，请自行搜索如何使用 cloudflare 域名邮箱，请自行搜索如何使用。
（非常重要） 需要使用 temp-mail.plus 邮箱，请自行搜索如何使用 temp-mail.plus 邮箱。
将 cloudflare 的域名邮箱转发到 temp-mail.plus 邮箱。
下载 .env.example 文件到程序所在根目录，并重命名为 .env 文件。
修改 .env 文件中的配置。
```

下文将告诉你如何配置这些变量。

## 配置变量

使用Cloudflare的前提是要有一个域名。

有了域名以后，我们就把它托管到Cloudflare上。我们先登录一下Cloudflare，没有账号就注册一个。右上角点击添加站点，输入你的域名，然后点击继续。这里有一些付费的，我们都不要，直接找下面这个免费的，点击继续。

![](https://qiniu.waite.wang/202501122103640.png)

一路下一步。这步是重点，更改名称服务器。找到这两个名称服务器。

![](https://qiniu.waite.wang/202501122104630.png)

我们需要去刚才买域名的网站更改一下DNS地址r。就把这里改一下，把原来的全部删掉，然后填上刚才网站给我分配的这两个DNS，点击保存。

![](https://qiniu.waite.wang/202501122105730.png)

我们再回到Cloudflare，点击立即检查，等待一段时间后，显示成功或者当cloudflare 发送成功邮件到你的邮箱，就表示成功了。

在验证成功后，我们就可以使用 Cloudflare 的域名邮箱了。

在任务栏，点击电子邮件，启用电子邮件，根据提示下一步即可。

成功开通后，点击路由规则，这里有一个catch-all地址，我们点击右侧的编辑，然后点击添加规则，选择转发到，然后填上我们的 [temp-mail.plus](https://tempmail.plus/zh/) 邮箱地址，点击保存。

这时我们在 temp-mail.plus 邮箱中就可以收到 Cloudflare 的验证邮件，点击验证即可。

当显示以下时表示验证成功，且可以正常使用。

![](https://qiniu.waite.wang/202501122114363.png)

这时我们把我们域名邮箱和 temp-mail.plus 邮箱前缀填入到 `.env` 文件中，运行 `cursor-pro-keep-alive.py`/ 可执行文件 即可。

![](https://qiniu.waite.wang/202501122116314.png)

## 运行中可能遇到的问题

![](https://qiniu.waite.wang/202501122118375.png)

这是因为 Google 浏览器不正常安装导致的，如果确定已经安装了 Google 浏览器，更改如下代码即可。

> 更新：工具更新了相关的配置路径（只限于 Win）
> 在 `.env` 文件下添加

```
# 使用其他浏览器(如Edge), 下面替换成你的实际浏览器安装路径
# BROWSER_PATH='C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe'
```
## 关于禁用 Cursor 的自动更新

> 最新版工具已经集成，不需要往下看了


> 注意以下实现的时间点是 2025-01-25 21:50:00
>
> 未来的更改版本的破解可能性关注[go-cursor-help](https://github.com/yuaotian/go-cursor-help)

Cursor 的自动更新功能，会自动更新到最新版本，在 0.45 版本后，Cursor 的机器码检测机制已经更新，所以需要禁用自动更新。

### 在Curosr编辑器中关闭Cursor自动更新

通过点击右上角的Cursor的设置按钮，进入到Cursor的功能设置页面。

![](https://qiniu.waite.wang/202501252148161.png)

![](https://qiniu.waite.wang/202501252148299.png)

![](https://qiniu.waite.wang/202501252149876.png)

> 如果你只是进行了以上的步骤，那么你只是表面禁止了Curosr的自动更新，实际上当你关闭Cursor编辑器后，它还是会自动检测是否有新版本更新，并且会进行自动更新

### 在Cursor相关目录下，禁止Cursor自动更新

#### 删除Cursor的更新缓存文件夹

以Windows为例，Cursor更新缓存文件夹路径为`C:\Users\{你的用户名}\AppData\Local\cursor-updater`
找到这个cursor-updater文件夹，直接把整个文件夹删除掉，建议先将整个文件夹打包压缩，进行备份操作，我这里的压缩包取名为cursor-updater-bak, 在这个目录下，新建一个文件，去掉后缀名，取名为cursor-updater（这里主要是占位文件）

#### 禁止Cursor自动更新

- 进入Cursor安装目录，windows位置为`C:\Users\{你的用户名}\AppData\Local\Cursor`
- 进入目录中的resources文件夹, 把 resources 目录里的 app-update.yml 改名为 app-update.yml.bak。之后新建文本文档, 将新文件名称改为 app-update.yml
- 选中app-update.yml文件，鼠标右键 -> 属性，勾选"只读"，点击确定

> 通过完成上面的设置，恭喜你，已经成功禁止了Cursor的自动更新！
