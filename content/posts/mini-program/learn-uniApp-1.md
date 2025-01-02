---
title: "UniApp 入门"
date: 2024-03-02T20:46:38+08:00
lastmod: 2024-03-02T20:46:38+08:00
categories: ["Mini Program"]
tags: ["UniApp"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "UniApp 学习笔记"
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

> <https://uniapp.dcloud.net.cn/>

![](https://qiniu.waite.wang/202403262157283.png)

## uni 和 原生小程序开发区别

> 每个页面是一个 .vue 文件，数据绑定及事件处理同 Vue.js 规范：
>
> 1. 属性绑定 src="{{ url }}" 升级成 :src="url"
> 2. 事件绑定 bindtap="eventName" 升级成 @tap="eventName"，支持（）传参
> 3. 支持 Vue 常用指令 v-for、 v-if、v-show、v-model 等

> 温馨提示：调用接口能力，建议前缀 wx 替换为 uni ，养成好习惯，这样支持多端开发。

## 创建项目/ 运行项目

uni-app 支持两种方式创建项目：

1. 通过 HBuilderX 创建
2. 通过命令行创建

### 通过 HBuilderX 创建/ 运行

1. 下载安装 HBuilderX <https://hx.dcloud.net.cn/Tutorial/install/windows>

> HBuilderX是通用的前端开发工具，但为uni-app做了特别强化。

2. 在点击工具栏里的文件 -> 新建 -> 项目（快捷键Ctrl+N）：

![](https://qiniu.waite.wang/202403262144531.png)

uni-app自带的模板有 默认的空项目模板、Hello uni-app 官方组件和API示例，还有一个重要模板是 uni ui项目模板，日常开发推荐使用该模板，已内置大量常用组件。

![](https://qiniu.waite.wang/202403262145716.png)

3. 选择模板后，点击下一步，填写项目名称、项目路径、Appid（小程序）、Appname（小程序名称）等信息，点击完成即可创建项目。

4. 创建完成后，工具 -> 插件安装 -> uni-app编译器

5. 运行项目：点击工具栏里的运行按钮，选择运行到小程序模拟器或者手机端，即可看到效果。

6. 在微信开发者工具里运行：进入hello-uniapp项目，点击工具栏的运行 -> 运行到小程序模拟器 -> 微信开发者工具，即可在微信开发者工具里面体验uni-app。

![](https://qiniu.waite.wang/202403262148740.png)

> 注意：如果是第一次使用，需要先配置小程序ide的相关路径，才能运行成功。如下图，需在输入框输入微信开发者工具的安装路径。

![](https://qiniu.waite.wang/202403262148979.png)

> 注意: 在微信小程序运行需要开启 设置 -> 安全设置 -> 服务端口 -> 开启服务端口, 并且关闭 设置 -> 编译器设置 -> 修改文件时自动保存

这样在 HbuilderX 保存文件后，微信开发者工具会自动刷新。

### 通过命令行创建/ 运行

> <https://uniapp.dcloud.net.cn/quickstart-cli.html>

1. 全局安装 vue-cli`npm install -g @vue/cli`

2. 创建uni-app
    + 使用正式版（对应HBuilderX最新正式版）

    ```
    vue create -p dcloudio/uni-preset-vue my-project
    ```

    + 使用alpha版（对应HBuilderX最新alpha版）

    ```
    vue create -p dcloudio/uni-preset-vue#alpha my-alpha-project
    ```

    + 使用Vue3/Vite版
        + 创建以 javascript 开发的工程（如命令行创建失败，请直接访问 gitee 下载模板）

        ```
        npx degit dcloudio/uni-preset-vue#vite my-vue3-project

        npx degit dcloudio/uni-preset-vue#vite-alpha my-vue3-project
        ```

        + 创建以 typescript 开发的工程（如命令行创建失败，请直接访问 gitee 下载模板）

        ```
        npx degit dcloudio/uni-preset-vue#vite-ts my-vue3-project
        ```

此时，会提示选择项目模板（使用Vue3/Vite版不会提示，目前只支持创建默认模板），初次体验建议选择 hello uni-app 项目模板，如下所示：

![](https://qiniu.waite.wang/202403262211245.png)

> 注意
>
> Vue3/Vite版要求 node 版本^14.18.0 || >=16.0.0
> 如果使用 HBuilderX（3.6.7以下版本）运行 Vue3/Vite 创建的最新的 cli 工程，需要在 HBuilderX 运行配置最底部设置 node路径 为自己本机高版本 node 路径（注意需要重启 HBuilderX 才可以生效）
>
> + HBuilderX Mac 版本菜单栏左上角 HBuilderX->偏好设置->运行配置->node路径
> + HBuilderX Windows 版本菜单栏 工具->设置->运行配置->node路径

国内特殊情况

+ 模板项目存放于 Github，由于国内网络环境问题，可能下载失败。针对此问题可以尝试如下措施：

  + 更换网络重试，比如使用 4g 网络
  + 在设备或路由器的网络设置中增加 DNS（如：8.8.8.8）
  + 在设备中增加固定的 hosts（如：140.82.113.4 github.com）

#### 运行项目

1. 更改项目中 `manifest.json` 中的 `appid` 为自己的小程序appid

2. 进入项目目录，运行命令 `npm run dev:%PLATFORM%`，其中 `%PLATFORM%` 为平台名称，如 `h5`、`mp-weixin`、`mp-alipay`、`mp-baidu`、`mp-toutiao`、`mp-qq`、`quickapp-webview`、`quickapp-webview-union`、`quickapp-webview-huawei`、`quickapp-webview-oppo`、`quickapp-webview-vivo`、`quickapp-webview-xiaomi`、`quickapp-webview-meizu`、`quickapp-webview-leshi`、`quickapp-webview-haier`、`quickapp-webview-samsung`、`quickapp-webview-smartisan`、`quickapp-webview-nubia`、`quickapp-webview-oneplus`、`quickapp-webview-360`、`quickapp-webview-letv`、`quickapp-webview-coolpad`、`quickapp-webview-gionee`、`quickapp-webview-sony`、`quickapp-webview-htc

3. 运行成功后，会在项目目录下生成 `dist` 目录，里面包含了编译后的代码，在微信小程序开发者工具中导入 `dist/dev/mp-weixin` 目录即可查看效果。

## 用 VS Code 编辑 uni-app 项目

为什么要用 VS Code 编辑 uni-app 项目？

1. HBuilderX 是基于 Eclipse 的 IDE，对于一些习惯了 VS Code 的开发者来说，可能不太适应。
2. HbuilderX 对 TS 支持不够友好，而 VS Code 对 TS 支持非常好。

### 安装插件

![](https://qiniu.waite.wang/202403262238212.png)

> 建议勾选以下

![](https://qiniu.waite.wang/202403262241594.png)

### 安装 ts 类型校验

> <https://uni-helper.js.org/uni-app-types>

![](https://qiniu.waite.wang/202403262253552.png)

```bash
npm i -D @types/wechat-miniprogram @uni-helper/uni-app-types
```

``` json
{
  "extends": "@vue/tsconfig/tsconfig.json",
  "compilerOptions": {
    "sourceMap": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "lib": ["esnext", "dom"],
    "types": [
      "@dcloudio/types",
      "@types/wechat-miniprogram",
      "@uni-helper/uni-app-types"
    ]
  },
  "vueCompilerOptions": {
    // 原配置 `experimentalRuntimeMode` 现调整为 `nativeTags`
    "nativeTags": ["block", "component", "template", "slot"]
  },
  "include": ["src/**/*.ts", "src/**/*.d.ts", "src/**/*.tsx", "src/**/*.vue"]
}

```

### json 注释问题

> uni-app 只有在 HBuilderX 中才支持 json 注释，而在 VS Code 中不支持，所以在 VS Code 中编辑 json 文件时，会有报错提示。
> 只有这两个文件支持 json 注释，其他文件不支持。
![](https://qiniu.waite.wang/202403262306083.png)
