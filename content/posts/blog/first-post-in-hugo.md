---
title: "博客迁移小记"
date: 2024-12-28T11:30:03+00:00
categories: ["Blog"]
tags: ["Blog"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "这里是副标题."
canonicalURL: "https://canonical.url/to/page"
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

## 前言

嘿，大家好！我一直觉得拥有一个个人博客是件很酷的事情。它不仅是一个记录学习心得和分享技术经验的地方，更是一个展示自我的平台。在这篇文章中，我想和大家分享一下我在博客迁移过程中的心路历程。从最初的 Hexo 到现在的 Hugo，中间我还尝试了 Typecho、VitePress、WordPress 和 Halo。每一次迁移都让我对博客系统有了更深的理解。

## 博客迁移历程

### Hexo 初体验

最开始，我选择了 Hexo。想想当时，我刚刚接触编程，对很多编程的基础知识都还不是很熟悉，而又想要拥有一个记录自己学习心得和分享技术经验的地方；基于这个想法，我选择了 Hexo。它是一个轻量级的静态博客系统，使用 Markdown 语法编写文章，主题和插件也很丰富。更重要的是，它可以基于 Github Pages 免费部署，这对于我这个穷学生来说，这无疑是一个巨大的诱惑。

然而，随着大陆对 Github 的访问越来越不稳定，以及对 Vercel 的访问限制，我不得不经常折腾去更换部署平台，套 CDN 加速，这让我感觉很麻烦。我开始思考，能不能拥有一个自己的服务器，然后自己部署博客呢？但随着我的文章越来越多，Hexo 的生成速度越来越慢，网站的访问速度动辄就要好几秒钟，这让我感觉很糟糕，于是我开始寻找其他博客系统。

### Typecho 的简约之美

接下来，我转向了 Typecho。它的优点是轻量级，对服务器要求低，而且是 PHP 驱动的，部署起来也很简单。界面简洁，专注写作，真的是很不错。不过，Typecho 的插件和主题市场相对较少，不支持多种类型的主题和插件，对于一些特殊需求根本无法满足。而且一旦遇到了问题，Typecho 的社区支持相对较少，这让我感觉很糟糕。

### WordPress 的全能之选

随着学习以及工作的压力，我越来越不想在博客上花费太多时间，希望博客系统能够尽可能的简单，可以一键式部署，这样我就可以把更多的精力放在写作以及工作、学习上。WordPress 无疑是最成熟的博客系统了。功能强大，插件丰富，主题样式多样，社区支持也很完善。用了一段时间后，我发现在模板方面，WordPress 丰富的模板库几乎全部都是以英语为主的，这导致了在中国大陆使用 WordPress 时，英文模板默认使用的谷歌字体成了站点加载速度的短板。即使有了 GoogleFonts 插件也无济于事，异步加载导致的字体跳变极其的突兀且影响观感。最后一点则是 WordPress 在资源占用方面并不算友好，尤其是以图片为主题的站点，长期使用对于内存是一个考验。综上，在考虑优劣后，我决定去换一个平台继续写作。

### VitePress 的现代化尝试

作为一个前端开发者，VitePress 的现代化构建方案让我很心动。它基于 Vue 3 和 Vite，开发体验非常好，构建速度也很快。不过，它更适合做技术文档，而不是个人博客，所以我又开始寻找新的选择。

### Halo 的新生力量

选择了很久后，最终定下了 HALO 作为新的博客架构。就决定是你了——Halo！

了解到 Halo 是从 1panel 的应用商店中发现的，索性去官网了解了一番，又从 V2EX 上看到了 Halo 官方征集用户需求的帖子，感觉官方很会听取用户意见和建议，相关的问题也反应很迅速。再加上 Halo 设计的调性我十分中意，索性直接下载安装，直接把 WordPress 的数据迁入了 Halo。

在使用 Halo 这几年里，我感受到了 Halo 的强大。从一开始使用的 Butterfly 主题，到 Hao 主题，再到后面追求的 Chirpy 主题，我也成为了 Halo Chirpy 主题的代码贡献者。Halo 无论在部署的便捷性，还是社区的交流，Halo 都给了我很大的帮助。而且从 Halo 1.x 到 2.x 的更新，Halo 的性能得到了很大的提升，插件系统的不断完善以及 Halo 的应用生态的逐渐丰富，让我可以只写内容，其他的事情交给 Halo。甚至于像平台收录，SEO 优化，CDN 加速，这些事情都可以交给 Halo 去处理。

我一直想着，如果不出意外，我应该会一直使用 Halo 作为我的博客架构，直到我不再需要博客。

但自从我更换了我的服务器厂商，2H2G 的内存，让我在跑 Halo 时，内存占用直接飙到 100% 并且直接宕机直到服务器自动关闭我的 Halo 进程，这让我感觉很糟糕（同样是 2H2G 的内存，阿里/腾讯/华为的云服务器，都可以正常运行；但是！京东云---）

所以，我不得不开始寻找新的博客系统。

### Hugo

经过多次迁移，我最终选择了 Hugo。为什么呢？因为它有以下几个优点：

1. **极致的速度**：Hugo 是用 Go 语言编写的，构建速度快得惊人。
2. **简单部署**：它是静态站点，不需要数据库，部署起来非常简单。
3. **维护方便**：使用 Markdown 写作，版本控制也很方便。
4. **高度定制**：支持自定义主题和功能，满足我的各种需求。
5. **安全可靠**：静态网站天然安全，不用担心被攻击。

而对于传统静态博客的部署问题，我也可以使用 Github Actions 来实现自动化部署。

Hugo 的这些优点让我感到非常满意，尤其是在我需要一个快速、稳定且易于维护的博客系统时。它让我可以专注于内容创作，而不必担心技术上的琐事。