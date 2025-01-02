---
title: "微信小程序入门"
date: 2023-03-02T20:52:46+08:00
lastmod: 2023-03-02T20:52:46+08:00
categories: ["Mini Program"]
tags: ["Wechat Mini Program"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "微信小程序学习笔记"
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

## 微信小程序入门

### 微信小程序介绍

#### 微信小程序介绍

​ 微信小程序，简称小程序，是一种不需要下载安装即可使用的应用，它实现了应用”触手可及”的梦想，用户扫一扫或搜一下即可打开应用。

​ 说明：

* 小程序是需要下载的，小程序的占用大小很小，感觉不到下载
* 目前大小限制2M （最终开发的小程序打包压缩后的大小），如果超过2M,就得做分包上传.之后再合并
* 进入小程序后继续网络请求数据

#### 小程序特点

微信小程序的特点：

* 免安装
* 接近原生（IOS，Android ）的app操作基于微信开发。使用wx提供的api开发
* 必须在微信里面使用

#### 小程序的优缺点

* ==方便快捷，即用即走==
* ==速度快、不占内存==
* 安全稳定、保密性强
* 功能丰富，场景丰富
* ==开发成本低、维护简便==
* ==开发周期比较短==
* 体验好

#### 小程序开发需求

* 不注册可以开发小程序(不能发布)
* 注册小程序
  * 企业注册(公司内部人员注册好了，给一个APPID)
  * 个人注册

#### 微信小程序的注册

微信公众平台：<https://mp.weixin.qq.com/>

### 开发工具

开发工具下载地址：<https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html>

![image-20220122170116175](https://qiniu.waite.wang/202401192149328.png)

#### 开发者工具的使用

 ![image-20220122201456358](https://qiniu.waite.wang/202401192150079.png)

![image-20220608183526635](https://qiniu.waite.wang/202401192150318.png)

> 以下就是微信小程序开发工具的界面，主要有：微信小程序模拟器、项目目录、代码编写区域、控制台。

### 项目目录结构介绍

![image-20240119215440525](https://qiniu.waite.wang/202401192154095.png)

#### pages目录

​ pages目录下放的就是小程序中的各个页面。

​ 在pages中创建页面的时候，会出现4个文件：

* xxx.js：页面相关的js代码可以写在这里
* xxx.wxml：这个就是页面文件，相当于我们之前的HTML，所以页面结构内容写在这里
* xxx.wxss：页面的样式内容，相当于之前的css，所以页面相关的样式可以写在这里
* xxx.json：页面有关的配置，比如页面导航栏的背景色、内容等等

比如： ![10](https://qiniu.waite.wang/202401192151123.png)

#### app.js文件

​ app.js文件是整个项目的一个==总体配置==。里面包含了项目运行==生命周期的回调函数==。

#### app.json文件

​ 小程序根目录下的 `app.json` 文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。

小程序根目录下的 `app.json` 文件用来对微信小程序进行全局配置。文件内容为一个 JSON 对象，有以下属性：

<https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html>

![](https://qiniu.waite.wang/202401192155179.png)

#### app.wxss文件

* app.wxss 文件是微信小程序项目的==全局样式表==，它可以应用到所有的wxml文件中。
* 微信小程序中使用 rpx 作为长度单位。1rpx = 1/750 屏幕宽度。也就是屏幕宽度等于 750rpx。
* px 也可以使用，表示的是设备独立像素。
* 建议使用长度单位 rpx。它自动做了适配。

#### project.config.json文件

​ `project.config.json`文件是小程序项目的配置文件（如开发工具的外观配置），一般不需要修改，我们目前就改一个地方：

![11](https://qiniu.waite.wang/202401192153939.png)

`"checkSiteMap":false` 作用是==控制台不要有一些没用的警告==。

#### Sitemap.json

<https://developers.weixin.qq.com/miniprogram/dev/framework/sitemap.html>

搜索功能文件，指定哪些页面可以被搜索，可被配置。是在搜索小程序的时候，指定哪些页面允许被搜索到。

## 小程序配置项

### 全局配置

> <https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#entryPagePath>

小程序根目录下的 `app.json` 文件用来对微信小程序进行全局配置。文件内容为一个 JSON 对象，有以下属性：

| 属性                                                         | 类型            | 必填 | 描述                                                         | 最低版本                                                     |
| :----------------------------------------------------------- | :-------------- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [entryPagePath](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#entryPagePath) | string          | 否   | 小程序默认启动首页                                           |                                                              |
| [pages](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#pages) | string[]        | 是   | 页面路径列表                                                 |                                                              |
| [window](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#window) | Object          | 否   | 全局的默认窗口表现                                           |                                                              |
| [tabBar](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#tabBar) | Object          | 否   | 底部 `tab` 栏的表现                                          |                                                              |
| [networkTimeout](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#networkTimeout) | Object          | 否   | 网络超时时间                                                 |                                                              |
| [debug](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#debug) | boolean         | 否   | 是否开启 debug 模式，默认关闭                                |                                                              |
| [functionalPages](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#functionalPages) | boolean         | 否   | 是否启用插件功能页，默认关闭                                 | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [subpackages](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#subpackages) | Object[]        | 否   | 分包结构配置                                                 | [1.7.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [workers](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#workers) | string          | 否   | `Worker` 代码放置的目录                                      | [1.9.90](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [requiredBackgroundModes](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#requiredBackgroundModes) | string[]        | 否   | 需要在后台使用的能力，如「音乐播放」                         |                                                              |
| [requiredPrivateInfos](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#requiredPrivateInfos) | string[]        | 否   | 调用的地理位置相关隐私接口                                   |                                                              |
| [plugins](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#plugins) | Object          | 否   | 使用到的插件                                                 | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [preloadRule](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#preloadRule) | Object          | 否   | 分包预下载规则                                               | [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [resizable](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#resizable) | boolean         | 否   | PC 小程序是否支持用户任意改变窗口大小（包括最大化窗口）；iPad 小程序是否支持屏幕旋转。默认关闭 | [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [usingComponents](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#usingComponents) | Object          | 否   | 全局[自定义组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)配置 | 开发者工具 1.02.1810190                                      |
| [permission](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#permission) | Object          | 否   | 小程序接口权限相关设置                                       | 微信客户端 7.0.0                                             |
| [sitemapLocation](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#sitemapLocation) | string          | 是   | 指明 sitemap.json 的位置                                     |                                                              |
| [style](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#style) | string          | 否   | 指定使用升级后的weui样式                                     | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [useExtendedLib](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#useextendedlib) | Object          | 否   | 指定需要引用的扩展库                                         | [2.2.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [entranceDeclare](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#entranceDeclare) | Object          | 否   | 微信消息用小程序打开                                         | 微信客户端 7.0.9                                             |
| [darkmode](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#darkmode) | boolean         | 否   | 小程序支持 DarkMode                                          | [2.11.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [themeLocation](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#themeLocation) | string          | 否   | 指明 theme.json 的位置，darkmode为true为必填                 | 开发者工具 1.03.2004271                                      |
| [lazyCodeLoading](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#lazyCodeLoading) | string          | 否   | 配置自定义组件代码按需注入                                   | [2.11.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [singlePage](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#singlePage) | Object          | 否   | 单页模式相关配置                                             | [2.12.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| supportedMaterials                                           | Object          | 否   | [聊天素材小程序打开](https://developers.weixin.qq.com/miniprogram/dev/framework/material/support_material.html)相关配置 | [2.14.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| serviceProviderTicket                                        | string          | 否   | [定制化型服务商](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/operation/thirdparty/customized_service_platform_guidelines.html)票据 |                                                              |
| [embeddedAppIdList](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#embeddedAppIdList) | string[]        | 否   | 半屏小程序 appId                                             | [2.20.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [halfPage](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#halfPage) | Object          | 否   | 视频号直播半屏场景设置                                       | [2.18.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [debugOptions](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#debugOptions) | Object          | 否   | 调试相关配置                                                 | [2.22.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [enablePassiveEvent](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#enablePassiveEvent) | Object或boolean | 否   | touch 事件监听是否为 passive                                 | [2.24.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [resolveAlias](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#resolveAlias) | Object          | 否   | 自定义模块映射规则                                           |                                                              |
| [renderer](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#renderer) | string          | 否   | 全局默认的渲染后端                                           | [2.30.4](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [rendererOptions](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#rendererOptions) | Object          | 否   | 渲染后端选项                                                 | [2.31.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| componentFramework                                           | string          | 否   | 组件框架，详见[相关文档](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/glass-easel/migration.html) | [2.30.4](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| miniApp                                                      | Object          | 否   | 多端模式场景接入身份管理服务时开启小程序授权页相关配置，详见[相关文档](https://dev.weixin.qq.com/docs/framework/getting_started/auth.html#_4、开启小程序授权页) |                                                              |
| static                                                       | Object          | 否   | 正常情况下默认所有资源文件都被打包发布到所有平台，可以通过 static 字段配置特定每个目录/文件只能发布到特定的平台(多端场景) [相关文档](https://dev.weixin.qq.com/docs/framework/guideline/devtools/condition-compile.html#资源) |                                                              |
| convertRpxToVw                                               | boolean         | 否   | 配置是否将 rpx 单位转换为 vw 单位，开启后能修复某些 rpx 下的精度问题 | [3.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

> 以下简单列举了一部分, 其他未列举可以在官方文档中查看

#### entryPagePath

指定小程序的默认启动路径（首页），常见情景是从微信聊天列表页下拉启动、小程序列表启动等。如果不填，将默认为 `pages` 列表的第一项。不支持带页面路径参数。

#### pages

用于指定小程序由哪些页面组成，每一项都对应一个页面的 路径（含文件名） 信息。文件名不需要写文件后缀，框架会自动去寻找对应位置的 `.json`, `.js`, `.wxml`, `.wxss` 四个文件进行处理。

未指定 `entryPagePath` 时，数组的第一项代表小程序的初始页面（首页）。

**小程序中新增/减少页面，都需要对 pages 数组进行修改。**

则需要在 app.json 中写

```json
{
  "pages": ["pages/index/index", "pages/logs/logs"]
}
```

#### window

用于设置小程序的状态栏、导航条、标题、窗口背景色。

| 属性                                                         | 类型     | 默认值   | 描述                                                         | 最低版本                                                     |
| :----------------------------------------------------------- | :------- | :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| navigationBarBackgroundColor                                 | HexColor | #000000  | 导航栏背景颜色，如 `#000000`                                 |                                                              |
| navigationBarTextStyle                                       | string   | white    | 导航栏标题、状态栏颜色，仅支持 `black` / `white`             |                                                              |
| navigationBarTitleText                                       | string   |          | 导航栏标题文字内容                                           |                                                              |
| navigationStyle                                              | string   | default  | 导航栏样式，仅支持以下值： `default` 默认样式 `custom` 自定义导航栏，只保留右上角胶囊按钮。参见注 2。 | iOS/Android 微信客户端 6.6.0，Windows 微信客户端不支持       |
| homeButton                                                   | boolean  | default  | 在非首页、非页面栈最底层页面或非tabbar内页面中的导航栏展示home键 | 微信客户端 8.0.24                                            |
| backgroundColor                                              | HexColor | #ffffff  | 窗口的背景色                                                 |                                                              |
| backgroundTextStyle                                          | string   | dark     | 下拉 loading 的样式，仅支持 `dark` / `light`                 |                                                              |
| backgroundColorTop                                           | string   | #ffffff  | 顶部窗口的背景色，仅 iOS 支持                                | 微信客户端 6.5.16                                            |
| backgroundColorBottom                                        | string   | #ffffff  | 底部窗口的背景色，仅 iOS 支持                                | 微信客户端 6.5.16                                            |
| enablePullDownRefresh                                        | boolean  | false    | 是否开启全局的下拉刷新。 详见 [Page.onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onpulldownrefresh) |                                                              |
| onReachBottomDistance                                        | number   | 50       | 页面上拉触底事件触发时距页面底部距离，单位为 px。 详见 [Page.onReachBottom](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onreachbottom) |                                                              |
| pageOrientation                                              | string   | portrait | 屏幕旋转设置，支持 `auto` / `portrait` / `landscape` 详见 [响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html) | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) (auto) / [2.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) (landscape) |
| [restartStrategy](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html#restartStrategy) | string   | homePage | 重新启动策略配置                                             | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| initialRenderingCache                                        | string   |          | 页面[初始渲染缓存](https://developers.weixin.qq.com/miniprogram/dev/framework/view/initial-rendering-cache.html)配置，支持 `static` / `dynamic` | [2.11.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| visualEffectInBackground                                     | string   | none     | 切入系统后台时，隐藏页面内容，保护用户隐私。支持 `hidden` / `none` | [2.15.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| handleWebviewPreload                                         | string   | static   | 控制[预加载下个页面的时机](https://developers.weixin.qq.com/miniprogram/dev/framework/performance/tips/runtime_nav.html#_2-4-控制预加载下个页面的时机)。支持 `static` / `manual` / `auto` | [2.15.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

如：

```json
{
  "window": {
    "navigationBarBackgroundColor": "#ffffff",
    "navigationBarTextStyle": "black",
    "navigationBarTitleText": "微信接口功能演示",
    "backgroundColor": "#eeeeee",
    "backgroundTextStyle": "light"
  }
}
```

![img](https://qiniu.waite.wang/202401201548556.jpeg)

#### tabBar

如果小程序是一个多 tab 应用（客户端窗口的底部或顶部有 tab 栏可以切换页面），可以通过 tabBar 配置项指定 tab 栏的表现，以及 tab 切换时显示的对应页面。

| 属性            | 类型     | 必填 | 默认值 | 描述                                                         | 最低版本                                                     |
| :-------------- | :------- | :--- | :----- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| color           | HexColor | 是   |        | tab 上的文字默认颜色，仅支持十六进制颜色                     |                                                              |
| selectedColor   | HexColor | 是   |        | tab 上的文字选中时的颜色，仅支持十六进制颜色                 |                                                              |
| backgroundColor | HexColor | 是   |        | tab 的背景色，仅支持十六进制颜色                             |                                                              |
| borderStyle     | string   | 否   | black  | tabbar 上边框的颜色， 仅支持 `black` / `white`               |                                                              |
| list            | Array    | 是   |        | tab 的列表，详见 `list` 属性说明，最少 2 个、最多 5 个 tab   |                                                              |
| position        | string   | 否   | bottom | tabBar 的位置，仅支持 `bottom` / `top`                       |                                                              |
| custom          | boolean  | 否   | false  | 自定义 tabBar，见[详情](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/custom-tabbar.html) | [2.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

其中 list 接受一个数组，**只能配置最少 2 个、最多 5 个 tab**。tab 按数组的顺序排序，每个项都是一个对象，其属性值如下：

| 属性             | 类型   | 必填 | 说明                                                         |
| :--------------- | :----- | :--- | :----------------------------------------------------------- |
| pagePath         | string | 是   | 页面路径，必须在 pages 中先定义                              |
| text             | string | 是   | tab 上按钮文字                                               |
| iconPath         | string | 否   | 图片路径，icon 大小限制为 40kb，建议尺寸为 81px * 81px，不支持网络图片。 **当 `position` 为 `top` 时，不显示 icon。** |
| selectedIconPath | string | 否   | 选中时的图片路径，icon 大小限制为 40kb，建议尺寸为 81px * 81px，不支持网络图片。 **当 `position` 为 `top` 时，不显示 icon。** |

![img](https://qiniu.waite.wang/202401201556765.png)

> 以下是一个简单示例

```json
"tabBar": {
 "color": "#00A95E",
 "selectedColor": "#FA8072",
 "backgroundColor": "#f5f5f5",
 "borderStyle": "black",
 "list": [
  {
   "pagePath": "pages/index/index",
   "text": "Index",
   "iconPath": "icon/_home.png",
   "selectedIconPath": "icon/home.png"
  },
  {
   "pagePath": "pages/logs/logs",
   "text": "Logs",
   "iconPath": "icon/_search.png",
   "selectedIconPath": "icon/search.png"
  },
  {
   "pagePath": "pages/demo/demo",
   "text": "Demo",
   "iconPath": "icon/_videocamera.png",
   "selectedIconPath": "icon/videocamera.png"
  }
 ]
}
```

![image-20240120160151572](https://qiniu.waite.wang/202401201601641.png)

#### networkTimeout

各类网络请求的超时时间，单位均为毫秒。

| 属性          | 类型   | 必填 | 默认值 | 说明                                                         |
| :------------ | :----- | :--- | :----- | :----------------------------------------------------------- |
| request       | number | 否   | 60000  | [wx.request](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html) 的超时时间，单位：毫秒。 |
| connectSocket | number | 否   | 60000  | [wx.connectSocket](https://developers.weixin.qq.com/miniprogram/dev/api/network/websocket/wx.connectSocket.html) 的超时时间，单位：毫秒。 |
| uploadFile    | number | 否   | 60000  | [wx.uploadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/upload/wx.uploadFile.html) 的超时时间，单位：毫秒。 |
| downloadFile  | number | 否   | 60000  | [wx.downloadFile](https://developers.weixin.qq.com/miniprogram/dev/api/network/download/wx.downloadFile.html) 的超时时间，单位：毫秒 |

#### debug

可以在开发者工具中开启 `debug` 模式，在开发者工具的控制台面板，调试信息以 `info` 的形式给出，其信息有 Page 的注册，页面路由，数据更新，事件触发等。可以帮助开发者快速定位一些常见的问题。

### 页面配置

app.json 中的部分配置，也支持对单个页面进行配置，可以在页面对应的 `.json` 文件来对本页面的表现进行配置。

页面中配置项在当前页面会覆盖 `app.json` 中相同的配置项（样式相关的配置项属于 `app.json` 中的 `window` 属性，但这里不需要额外指定 `window` 字段），具体的取值和含义可参考[全局配置文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html)中说明。

文件内容为一个 JSON 对象，有以下属性：

| 属性                         | 类型            | 默认值    | 描述                                                         | 最低版本                                                     |
| :--------------------------- | :-------------- | :-------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| navigationBarBackgroundColor | HexColor        | #000000   | 导航栏背景颜色，如 `#000000`                                 |                                                              |
| navigationBarTextStyle       | string          | white     | 导航栏标题、状态栏颜色，仅支持 `black` / `white`             |                                                              |
| navigationBarTitleText       | string          |           | 导航栏标题文字内容                                           |                                                              |
| navigationStyle              | string          | default   | 导航栏样式，仅支持以下值： `default` 默认样式 `custom` 自定义导航栏，只保留右上角胶囊按钮。 | iOS/Android 微信客户端 7.0.0，Windows 微信客户端不支持       |
| homeButton                   | boolean         | false     | 在非首页、非页面栈最底层页面或非tabbar内页面中的导航栏展示home键 | 微信客户端 8.0.24                                            |
| backgroundColor              | HexColor        | #ffffff   | 窗口的背景色                                                 |                                                              |
| backgroundColorContent       | HexColor        | #RRGGBBAA | 页面容器背景色，[点击查看设置背景色详情](https://developers.weixin.qq.com/miniprogram/dev/framework/runtime/skyline/custom-route.html#设置页面透明) |                                                              |
| backgroundTextStyle          | string          | dark      | 下拉 loading 的样式，仅支持 `dark` / `light`                 |                                                              |
| backgroundColorTop           | string          | #ffffff   | 顶部窗口的背景色，仅 iOS 支持                                | 微信客户端 6.5.16                                            |
| backgroundColorBottom        | string          | #ffffff   | 底部窗口的背景色，仅 iOS 支持                                | 微信客户端 6.5.16                                            |
| enablePullDownRefresh        | boolean         | false     | 是否开启当前页面下拉刷新。 详见 [Page.onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onpulldownrefresh) |                                                              |
| onReachBottomDistance        | number          | 50        | 页面上拉触底事件触发时距页面底部距离，单位为px。 详见 [Page.onReachBottom](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html#onreachbottom) |                                                              |
| pageOrientation              | string          | portrait  | 屏幕旋转设置，支持 `auto` / `portrait` / `landscape` 详见 [响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html) | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) (auto) / [2.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) (landscape) |
| disableScroll                | boolean         | false     | 设置为 `true` 则页面整体不能上下滚动。 只在页面配置中有效，无法在 `app.json` 中设置 |                                                              |
| usingComponents              | Object          | 否        | 页面[自定义组件](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)配置 | [1.6.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| initialRenderingCache        | string          |           | 页面[初始渲染缓存](https://developers.weixin.qq.com/miniprogram/dev/framework/view/initial-rendering-cache.html)配置，支持 `static` / `dynamic` | [2.11.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| style                        | string          | default   | 启用新版的组件样式                                           | [2.10.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| singlePage                   | Object          | 否        | 单页模式相关配置                                             | [2.12.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| restartStrategy              | string          | homePage  | 重新启动策略配置                                             | [2.8.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| handleWebviewPreload         | string          | static    | 控制[预加载下个页面的时机](https://developers.weixin.qq.com/miniprogram/dev/framework/performance/tips/runtime_nav.html#_2-4-控制预加载下个页面的时机)。支持 `static` / `manual` / `auto` | [2.15.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| visualEffectInBackground     | string          | 否        | 切入系统后台时，隐藏页面内容，保护用户隐私。支持 `hidden` / `none`，若对页面单独设置则会覆盖全局的配置，详见 [全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html) | [2.15.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| enablePassiveEvent           | Object或boolean | 否        | 事件监听是否为 passive，若对页面单独设置则会覆盖全局的配置，详见 [全局配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html) | [2.24.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| renderer                     | string          | 否        | 渲染后端                                                     | [2.30.4](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| rendererOptions              | Object          | 否        | 渲染后端选项，详情[相关文档](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/app.html) | [3.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| componentFramework           | string          | 否        | 组件框架，详情[相关文档](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/glass-easel/migration.html) | [2.30.4](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

* 注：并不是所有 `app.json` 中的配置都可以在页面覆盖或单独指定，仅限于本文档包含的选项。
* 注：iOS/Android 客户端 7.0.0 以下版本，`navigationStyle` 只在 `app.json` 中生效。

![image-20240120160819935](https://qiniu.waite.wang/202401201608866.png)

### WXSS

WXSS (WeiXin Style Sheets)是一套样式语言，用于描述 WXML 的组件样式。

WXSS 用来决定 WXML 的组件应该怎么显示。

为了适应广大的前端开发者，WXSS 具有 CSS 大部分特性。同时为了更适合开发微信小程序，WXSS 对 CSS 进行了扩充以及修改。

与 CSS 相比，WXSS 扩展的特性有：

* 尺寸单位
* 样式导入

#### 尺寸单位

* rpx（responsive pixel）: 可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

| 设备         | rpx换算px (屏幕宽度/750) | px换算rpx (750/屏幕宽度) |
| :----------- | :----------------------- | :----------------------- |
| iPhone5      | 1rpx = 0.42px            | 1px = 2.34rpx            |
| iPhone6      | 1rpx = 0.5px             | 1px = 2rpx               |
| iPhone6 Plus | 1rpx = 0.552px           | 1px = 1.81rpx            |

**建议：** 开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。

**注意：** 在较小的屏幕上不可避免的会有一些毛刺，请在开发时尽量避免这种情况。

#### 样式导入

使用`@import`语句可以导入外联样式表，`@import`后跟需要导入的外联样式表的相对路径，用`;`表示语句结束。

**示例代码：**

```less
/** common.wxss **/
.small-p {
  padding:5px;
}
/** app.wxss **/
@import "common.wxss";
.middle-p {
  padding:15px;
}
```

#### 内联样式

框架组件上支持使用 style、class 属性来控制组件的样式。

* style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。

```html
<view style="color:{{color}};" />
```

* class：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上`.`，样式类名之间用空格分隔。

```html
<view class="normal_view" />
```

#### 选择器

目前支持的选择器有：

| 选择器           | 样例             | 样例描述                                       |
| :--------------- | :--------------- | :--------------------------------------------- |
| .class           | `.intro`         | 选择所有拥有 class="intro" 的组件              |
| #id              | `#firstname`     | 选择拥有 id="firstname" 的组件                 |
| element          | `view`           | 选择所有 view 组件                             |
| element, element | `view, checkbox` | 选择所有文档的 view 组件和所有的 checkbox 组件 |
| ::after          | `view::after`    | 在 view 组件后边插入内容                       |
| ::before         | `view::before`   | 在 view 组件前边插入内容                       |

#### 全局样式与局部样式

定义在 app.wxss 中的样式为全局样式，作用于每一个页面。在 page 的 wxss 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 app.wxss 中相同的选择器。

## 组件

> <https://developers.weixin.qq.com/miniprogram/dev/component/>

html 中有 `div、span、ul、li 、img`,  而小程序上面所有的标签都没有，只有组件,  微信小程序中的组件就相当于之前HTML中的标签。但是小程序中的组件除了包裹功能，还具有样式和 js 功能。

![image-20240120161419501](https://qiniu.waite.wang/202401201614793.png)

> 以下会介绍一些常用的标签, 剩下的可以在官方文档中查看

### 视图/基础组件

#### View

​ 视图容器，view组件就相当于之前HTML中的div标签。

| 属性                   | 类型    | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :--------------------- | :------ | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| hover-class            | string  | none   | 否   | 指定按下去的样式类。当 `hover-class="none"` 时，没有点击态效果 | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| hover-stop-propagation | boolean | false  | 否   | 指定是否阻止本节点的祖先节点出现点击态                       | [1.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| hover-start-time       | number  | 50     | 否   | 按住后多久出现点击态，单位毫秒                               | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| hover-stay-time        | number  | 400    | 否   | 手指松开后点击态保留时间，单位毫秒                           | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

```html
<view class="wrapper" hover-class="wrapper-hover" hover-stay-time="1000">我的第一个微信程序</view>
```

```css
.wrapper{
 width: 100%;
 height: 100rpx;
 text-align: center;
 line-height: 100rpx;
 background-color: skyblue;
}

.wrapper-hover {
 background-color: pink;
}
```

> 以下是一个 Flex 布局的例子

```html
<view class="container">
  <view class="page-body">
    <view class="page-section">
      <view class="page-section-title">
        <text>flex-direction: row</text>
        <text>横向布局</text>
      </view>
      <view class="page-section-spacing">
        <view class="flex-wrp" style="flex-direction:row;">
          <view class="flex-item demo-text-1"></view>
          <view class="flex-item demo-text-2"></view>
          <view class="flex-item demo-text-3"></view>
        </view>
      </view>
    </view>
    <view class="page-section">
      <view class="page-section-title">
        <text>flex-direction: column</text>
        <text>纵向布局</text>
      </view>
      <view class="flex-wrp" style="flex-direction:column;">
        <view class="flex-item flex-item-V demo-text-1"></view>
        <view class="flex-item flex-item-V demo-text-2"></view>
        <view class="flex-item flex-item-V demo-text-3"></view>
      </view>
    </view>
  </view>
</view>
```

```css
.container {
 margin-top: 80px;
}

.flex-wrp {
 display: flex;
}

.flex-item {
 width: 200rpx;
 height: 300rpx;
 font-size: 26rpx;
}

.flex-item-V {
 margin: 0 auto;
 width: 300rpx;
 height: 200rpx;
}

.page-section-title text {
 line-height: 36px;
}

.demo-text-1 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #1AAD19;
 color: #FFFFFF;
 font-size: 36rpx;
}

.demo-text-1:before {
 content: 'A';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}

.demo-text-2 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #2782D7;
 color: #FFFFFF;
 font-size: 36rpx;
}

.demo-text-2:before {
 content: 'B';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}

.demo-text-3 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #F1F1F1;
 color: #353535;
 font-size: 36rpx;
}

.demo-text-3:before {
 content: 'C';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}
```

![image-20240120162353536](https://qiniu.waite.wang/202401201623482.png)

#### text

​ 文本组件，相当于HTML中的span标签。

```html
<text>啦啦啦啦</text>
```

#### swiper

> <https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html#%E6%B8%B2%E6%9F%93%E6%A8%A1%E5%BC%8F%E6%95%88%E6%9E%9C%E6%BC%94%E7%A4%BA>

滑块视图容器。其中只可放置`swiper-item`组件，否则会导致未定义的行为。也就是说swiper内部只能放swiper-item组件，而swiper-item组件中就可以随便放其它组件及内容了。

```html
<swiper class="banner">
  <swiper-item>item1</swiper-item>
  <swiper-item>item2</swiper-item>
  <swiper-item>item3</swiper-item>
</swiper>
```

```css
.banner{
    height: 80rpx;
    text-align: center;
    line-height: 80rpx;
}
```

可以看到swiper组件有轮播图的效果。而且它有默认的高度(150px)。

|      | 属性                   | 类型    | 默认值            | 必填 | 说明                                                  | 最低版本                                                     |
| :--- | :--------------------- | :------ | :---------------- | :--- | :---------------------------------------------------- | ------------------------------------------------------------ |
|      | indicator-dots         | boolean | false             | 否   | 是否显示面板指示点                                    | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | indicator-color        | color   | rgba(0, 0, 0, .3) | 否   | 指示点颜色                                            | [1.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | indicator-active-color | color   | #000000           | 否   | 当前选中的指示点颜色                                  | [1.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | autoplay               | boolean | false             | 否   | 是否自动切换                                          | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | current                | number  | 0                 | 否   | 当前所在滑块的 index                                  | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | interval               | number  | 5000              | 否   | 自动切换时间间隔                                      | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | duration               | number  | 500               | 否   | 滑动动画时长                                          | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | circular               | boolean | false             | 否   | 是否采用衔接滑动                                      | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | vertical               | boolean | false             | 否   | 滑动方向是否为纵向                                    | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | display-multiple-items | number  | 1                 | 否   | 同时显示的滑块数量                                    | [1.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | previous-margin        | string  | "0px"             | 否   | 前边距，可用于露出前一项的一小部分，接受 px 和 rpx 值 | [1.9.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

> 以下是一个简单的轮播图效果

```javascript
const food = [
 'https://img1.baidu.com/it/u=3033226715,2238019049&fm=253&fmt=auto&app=138&f=JPEG?w=800&h=500',
 'https://img1.baidu.com/it/u=1948650034,2409824853&fm=253&fmt=auto&app=120&f=JPEG?w=1200&h=675',
 'https://img2.baidu.com/it/u=3016274568,4110305242&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=313',
 'https://img2.baidu.com/it/u=2364493189,83457107&fm=253&fmt=auto&app=138&f=JPEG?w=800&h=500'
]

Page({
 data: {
  food
 },
})
```

```html
<view style="height: 400rpx;">
 <swiper class="banner" indicator-dots>
  <swiper-item wx:for="{{food}}" wx:key="*this">
   <image class="img" src="{{item}}" mode="aspectFill" />
  </swiper-item>
 </swiper>
</view>
```

```css
.banner {
 height: 100%;
 width: 100%;
 text-align: center;
 line-height: 80rpx;
}
```

> 当然, 在 Skyline 渲染模式下, 他也会有不同的表现, 具体可以看
>
> <https://developers.weixin.qq.com/miniprogram/dev/component/swiper.html#%E6%8C%87%E7%A4%BA%E5%99%A8%E6%95%88%E6%9E%9C%E6%BC%94%E7%A4%BA>

#### scroll-view

> <https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html>

可滚动视图区域。使用竖向滚动时，需要给[scroll-view](https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html)一个固定高度，通过 WXSS 设置 height。组件属性的长度单位默认为px，[2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入单位(rpx/px)。

1. 横向滚动需打开 enable-flex 以兼容 WebView，如 <scroll-view scroll-x enable-flex style="flex-direction: row;"/>
2. 滚动条的长度是预估的，若直接子节点的高度差别较大，则滚动条长度可能会不准确
3. 使用 `worklet` 函数需要开启开发者工具 "将 JS 编译成 ES5" 或 "编译 worklet 函数" 选项。

> 以下我会给出 纵向滚动 以及 横向滚动 两个效果, 具体属性可以在官方文档中查看

```html
<view class="container">
  <view class="page-body">
    <view class="page-section">
      <view class="page-section-title">
        <text>Vertical Scroll</text>
        <text>纵向滚动</text>
      </view>
      <view class="page-section-spacing">
        <scroll-view type="list" scroll-y="true" style="height: 300rpx;">
          <view id="demo1" class="scroll-view-item demo-text-1"></view>
          <view id="demo2"  class="scroll-view-item demo-text-2"></view>
          <view id="demo3" class="scroll-view-item demo-text-3"></view>
        </scroll-view>
      </view>
    </view>
    <view class="page-section">
      <view class="page-section-title">
        <text>Horizontal Scroll</text>
        <text>横向滚动</text>
      </view>
      <view class="page-section-spacing">
        <scroll-view type="list" class="scroll-view_H"  scroll-x="true" bindscroll="scroll" style="width: 100%;height: 300rpx;">
          <view id="demo1" class="scroll-view-item_H demo-text-1"></view>
          <view id="demo2"  class="scroll-view-item_H demo-text-2"></view>
          <view id="demo3" class="scroll-view-item_H demo-text-3"></view>
        </scroll-view>
      </view>
    </view>
  </view>
</view>
```

```javascript
Page({
  tapMove() {
    this.setData({
      scrollTop: this.data.scrollTop + 10
    })
  }
})
```

```css
.container {
 margin-top: 80px;
}

.page-section-spacing {
 margin-top: 60rpx;
}

.scroll-view_H {
 white-space: nowrap;
}

.scroll-view-item {
 height: 300rpx;
}

.scroll-view-item_H {
 display: inline-block;
 width: 100%;
 height: 300rpx;
}

.demo-text-1 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #1AAD19;
 color: #FFFFFF;
 font-size: 36rpx;
}

.demo-text-1::before {
 content: 'A';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}

.demo-text-2 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #2782D7;
 color: #FFFFFF;
 font-size: 36rpx;
}

.demo-text-2::before {
 content: 'B';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}

.demo-text-3 {
 position: relative;
 align-items: center;
 justify-content: center;
 background-color: #F1F1F1;
 color: #353535;
 font-size: 36rpx;
}

.demo-text-3::before {
 content: 'C';
 position: absolute;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
}
```

> <https://developers.weixin.qq.com/miniprogram/dev/component/scroll-view.html#scroll-view-%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%8B%E6%8B%89%E5%88%B7%E6%96%B0%E7%A4%BA%E4%BE%8B>

> 以下是横向滚动案例

```html
<!-- scroll-view -->
<scroll-view class="myScroll" scroll-x>
 <view class="myScroll.row">1</view>
 <view class="row">2</view>
 <view class="row">3</view>
 <view class="row">4</view>
 <view class="row">5</view>
 <view class="row">6</view>
 <view class="row">7</view>
 <view class="row">8</view>
</scroll-view>
```

```css
.myScroll {
 width: 100%;
 height: 220rpx;
 background: #eee;
 /* 使文本在不换行的情况下继续显示 */
 white-space: nowrap;
}

.myScroll .row {
 width: 220rpx;
 height: 220rpx;
 background: burlywood;
 margin-right: 20rpx;
 display: inline-block;
}

.myScroll .row:last-child {
 margin-right: 0;
}
```

#### icon

​ icon组件就是在页面可以显示一个图标。

```html
<icon type="success"></icon>
<icon type="success" size="50"></icon>
<icon type="warn"></icon>
```

| 属性  | 类型          | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :---- | :------------ | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| type  | string        |        | 是   | icon的类型，有效值：success, success_no_circle, info, warn, waiting, cancel, download, search, clear | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| size  | number/string | 23     | 否   | icon的大小，单位默认为px，[2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入单位(rpx/px)，[2.21.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入其余单位(rem 等)。 | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| color | string        |        | 否   | icon的颜色，同css的color                                     | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

#### progress 进度条

进度条。组件属性的长度单位默认为px，[2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)起支持传入单位(rpx/px)。

| 属性            | 类型          | 默认值    | 必填 | 说明                                                    | 最低版本                                                     |
| :-------------- | :------------ | :-------- | :--- | :------------------------------------------------------ | :----------------------------------------------------------- |
| percent         | number        |           | 否   | 百分比0~100                                             | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| show-info       | boolean       | false     | 否   | 在进度条右侧显示百分比                                  | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| border-radius   | number/string | 0         | 否   | 圆角大小                                                | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| font-size       | number/string | 16        | 否   | 右侧百分比字体大小                                      | [2.3.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| stroke-width    | number/string | 6         | 否   | 进度条线的宽度                                          | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| color           | string        | #09BB07   | 否   | 进度条颜色（请使用activeColor）                         | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| activeColor     | string        | #09BB07   | 否   | 已选择的进度条的颜色                                    | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| backgroundColor | string        | #EBEBEB   | 否   | 未选择的进度条的颜色                                    | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| active          | boolean       | false     | 否   | 进度条从左往右的动画                                    | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| active-mode     | string        | backwards | 否   | backwards: 动画从头播；forwards：动画从上次结束点接着播 | [1.7.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| duration        | number        | 30        | 否   | 进度增加1%所需毫秒数                                    | [2.8.2](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| bindactiveend   | eventhandle   |           | 否   | 动画完成事件                                            | [2.4.1](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

```html
<progress percent="20" show-info />
<progress percent="40" stroke-width="12" />
<progress percent="60" color="pink" />
<progress percent="80" active />
```

![image-20240121004604493](https://qiniu.waite.wang/202401210046684.png)

### 表单组件

> 具体可以在官方文档查看

#### Button

> <https://developers.weixin.qq.com/miniprogram/dev/component/button.html>
>
> 具体属性看官方文档, 可以通过 `open-type` 配置分享, 打开交流窗口等等功能

```html
<button size="default">按钮1</button>
<button size="mini">按钮2</button>
<button size="mini" type="primary">按钮3</button>
<button size="mini" type="warn">按钮3</button>
<button size="mini" type="primary" open-type="share">按钮4</button>
```

![image-20240121215332766](https://qiniu.waite.wang/202401212153960.png)

### 导航组件

#### navigator

> <https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html>
>
> navigator组件相当于HTML的超链接标签。

```html
<!-- 
  navigator组件相当于HTML的超链接标签。
    target属性：在哪个目标上发生跳转，默认self-当前小程序；
    url属性：当前小程序内的跳转链接
    open-type属性：指定跳转方式，默认是navigate
      navigate：保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面。
      redirect：关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面。
      switchTab：跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
      reLaunch：关闭所有页面，打开到应用内的某个页面
      navigateBack：关闭当前页面，返回上一页面或多级页面。
 -->
<navigator target="self" url="../about/about" open-type="switchTab">点我跳转</navigator>
```

```html
<!-- 导航 
跳转到页面，都不写后缀名
1、open-type='navigate' 跳转方式:
保留当前页面，跳转应用内的某个页面，但不跳转tabber页面
2、open-type="redirect 关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面。 没返回，有返回首页
3、open-type="switchTab"  跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
4、reLaunch 关闭所有页面，打开到应用内的某个页面
5、navigateBack 关闭当前页面，返回上一页面或多级页面。标签的话，就是返回上一页。用方法的话，可以指定返回多少级
6、exit	退出小程序，`target="miniProgram"`时生效 需要用真机去测

-->
<navigator url="../detail/detail">进入详情页</navigator>
<navigator url="../detail/detail" open-type="redirect">redirect 进入详情页</navigator>
<navigator url="../index/index" open-type="switchTab">跳转到首页</navigator>
<!-- 还可以调tabBar -->
<navigator url="../detail/detail" open-type="reLaunch">reLaunch 到详情页</navigator>
<navigator open-type="exit" target="miniProgram">退出小程序</navigator>
```

### 媒体组件

#### image

> <https://developers.weixin.qq.com/miniprogram/dev/component/image.html>

为了保证大小, 可以使用裁剪或者缩放调整大小

|      | 属性                   | 类型    | 默认值      | 必填 | 说明                                                         | 最低版本                                                     |
| :--- | :--------------------- | :------ | :---------- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
|      | show-menu-by-longpress | boolean | false       | 否   | 长按图片显示发送给朋友、收藏、保存图片、搜一搜、打开名片/前往群聊/打开小程序（若图片中包含对应二维码或小程序码）的菜单。 | [2.7.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
|      | mode                   | string  | scaleToFill | 否   | 图片裁剪、缩放的模式                                         | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

|              |                                                              |                                                              |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 合法值       | 说明                                                         | 最低版本                                                     |
| scaleToFill  | 缩放模式，不保持纵横比缩放图片，使图片的宽高完全拉伸至填满 image 元素 |                                                              |
| aspectFit    | 缩放模式，保持纵横比缩放图片，使图片的长边能完全显示出来。也就是说，可以完整地将图片显示出来。 |                                                              |
| aspectFill   | 缩放模式，保持纵横比缩放图片，只保证图片的短边能完全显示出来。也就是说，图片通常只在水平或垂直方向是完整的，另一个方向将会发生截取。 |                                                              |
| widthFix     | 缩放模式，宽度不变，高度自动变化，保持原图宽高比不变         |                                                              |
| heightFix    | 缩放模式，高度不变，宽度自动变化，保持原图宽高比不变         | [2.10.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| top          | 裁剪模式，不缩放图片，只显示图片的顶部区域。仅 Webview 支持。 |                                                              |
| bottom       | 裁剪模式，不缩放图片，只显示图片的底部区域。仅 Webview 支持。 |                                                              |
| center       | 裁剪模式，不缩放图片，只显示图片的中间区域。仅 Webview 支持。 |                                                              |
| left         | 裁剪模式，不缩放图片，只显示图片的左边区域。仅 Webview 支持。 |                                                              |
| right        | 裁剪模式，不缩放图片，只显示图片的右边区域。仅 Webview 支持。 |                                                              |
| top left     | 裁剪模式，不缩放图片，只显示图片的左上边区域。仅 Webview 支持。 |                                                              |
| top right    | 裁剪模式，不缩放图片，只显示图片的右上边区域。仅 Webview 支持。 |                                                              |
| bottom left  | 裁剪模式，不缩放图片，只显示图片的左下边区域。仅 Webview 支持。 |                                                              |
| bottom right | 裁剪模式，不缩放图片，只显示图片的右下边区域。仅 Webview 支持。 |                                                              |

> 可以在 <https://developers.weixin.qq.com/miniprogram/dev/component/image.html#%E7%A4%BA%E4%BE%8B%E4%BB%A3%E7%A0%81> 查看示例代码

### 地图组件

> <https://developers.weixin.qq.com/miniprogram/dev/component/map.html>

```html
<!-- 
  latitude：维度
  longitude：经度
 -->
<map longitude="115" latitude="39"></map>
```

## 框架

​ 小程序依赖于微信客户端提供的环境--宿主环境，小程序借助这个宿注环境提供的功能，可以实现网页无法实现的功能。让小程序更接近原生的app体验。

小程序开发框架的目标是通过尽可能简单、高效的方式让开发者可以在微信中开发具有原生 APP 体验的服务。

![](https://qiniu.waite.wang/202401222127287.png)

​ 首先，我们来简单了解下小程序的运行环境。整个小程序框架系统分为两部分：**[逻辑层](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/)**（App Service）和 **[视图层](https://developers.weixin.qq.com/miniprogram/dev/framework/view/)**（View）。小程序提供了自己的视图层描述语言 `WXML` 和 `WXSS`，以及基于 `JavaScript` 的逻辑层框架，并在视图层与逻辑层间提供了数据传输和事件系统，让开发者能够专注于数据与逻辑。

​ 小程序的渲染层和逻辑层分别由2个线程管理：渲染层的界面使用了WebView 进行渲染；逻辑层采用JsCore线程运行JS脚本。一个小程序存在多个界面，所以渲染层存在多个WebView线程，这两个线程的通信会经由微信客户端（下文中也会采用Native来代指微信客户端）做中转，逻辑层发送网络请求也经由Native转发，小程序的通信模型下图所示。

![](https://qiniu.waite.wang/202401222127418.png)

### 响应的数据绑定

框架的核心是一个响应的数据绑定系统，可以让数据与视图非常简单地保持同步。当做数据修改的时候，只需要在逻辑层修改数据，视图层就会做相应的更新。

```html
<!-- This is our View -->
<view> Hello {{name}}! </view>
<button bindtap="changeName"> Click me! </button>
```

```javascript
// This is our App Service.
// This is our data.
var helloData = {
  name: 'Weixin'
}

// Register a Page.
Page({
  data: helloData,
  changeName: function(e) {
    // sent data change to view
    this.setData({
      name: 'MINA'
    })
  }
})
```

* 开发者通过框架将逻辑层数据中的 `name` 与视图层的 `name` 进行了绑定，所以在页面一打开的时候会显示 `Hello Weixin!`；
* 当点击按钮的时候，视图层会发送 `changeName` 的事件给逻辑层，逻辑层找到并执行对应的事件处理函数；
* 回调函数触发后，逻辑层执行 `setData` 的操作，将 `data` 中的 `name` 从 `Weixin` 变为 `MINA`，因为该数据和视图层已经绑定了，从而视图层会自动改变为 `Hello MINA!`。

### 逻辑层

小程序开发框架的逻辑层使用 `JavaScript` 引擎为小程序提供开发 `JavaScript` 代码的运行环境以及微信小程序的特有功能。

逻辑层将数据进行处理后发送给视图层，同时接受视图层的事件反馈。

开发者写的所有代码最终将会打包成一份 `JavaScript` 文件，并在小程序启动的时候运行，直到小程序销毁。这一行为类似 [ServiceWorker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)，所以逻辑层也称之为 App Service。

在 `JavaScript` 的基础上，我们增加了一些功能，以方便小程序的开发：

* 增加 `App` 和 `Page` 方法，进行[程序注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html)和[页面注册](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html)。
* 增加 `getApp` 和 `getCurrentPages` 方法，分别用来获取 `App` 实例和当前页面栈。
* 提供丰富的 [API](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html)，如微信用户数据，扫一扫，支付等微信特有能力。
* 提供[模块化](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html#模块化)能力，每个页面有独立的[作用域](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/module.html#文件作用域)。

**注意：小程序框架的逻辑层并非运行在浏览器中，因此 `JavaScript` 在 web 中一些能力都无法使用，如 `window`，`document` 等。**

#### 注册小程序

每个小程序都需要在 `app.js` 中调用 `App` 方法注册小程序实例，绑定生命周期回调函数、错误监听和页面不存在监听函数等。

```js
// app.js
App({
  onLaunch (options) {
    // Do something initial when launch.
  },
  onShow (options) {
    // Do something when show.
  },
  onHide () {
    // Do something when hide.
  },
  onError (msg) {
    console.log(msg)
  },
  globalData: 'I am global data'
}
```

| 属性                                                         | 类型     | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :----------------------------------------------------------- | :------- | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [onLaunch](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onLaunch-Object-object) | function |        | 否   | 生命周期回调——监听小程序初始化。                             |                                                              |
| [onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onShow-Object-object) | function |        | 否   | 生命周期回调——监听小程序启动或切前台。                       |                                                              |
| [onHide](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onHide) | function |        | 否   | 生命周期回调——监听小程序切后台。                             |                                                              |
| [onError](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onError-String-error) | function |        | 否   | 错误监听函数。                                               |                                                              |
| [onPageNotFound](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onPageNotFound-Object-object) | function |        | 否   | 页面不存在监听函数。                                         | [1.9.90](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onUnhandledRejection](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onUnhandledRejection-Object-object) | function |        | 否   | 未处理的 Promise 拒绝事件监听函数。                          | [2.10.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onThemeChange](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onThemeChange-Object-object) | function |        | 否   | 监听系统主题变化                                             | [2.11.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| 其他                                                         | any      |        | 否   | 开发者可以添加任意的函数或数据变量到 `Object` 参数中，用 `this` 可以访问 |                                                              |

整个小程序只有一个 App 实例，是全部页面共享的。开发者可以通过 `getApp` 方法获取到全局唯一的 App 实例，获取App上的数据或调用开发者注册在 `App` 上的函数。

```js
// xxx.js
const appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

#### 注册页面

对于小程序中的每个页面，都需要在页面对应的 `js` 文件中进行注册，指定页面的初始数据、生命周期回调、事件处理函数等。

##### 使用 Page 构造器注册页

简单的页面可以使用 `Page()` 进行构造。

**代码示例：**

```js
//index.js
Page({
  data: {
    text: "This is page data."
  },
  onLoad: function(options) {
    // 页面创建时执行
  },
  onShow: function() {
    // 页面出现在前台时执行
  },
  onReady: function() {
    // 页面首次渲染完毕时执行
  },
  onHide: function() {
    // 页面从前台变为后台时执行
  },
  onUnload: function() {
    // 页面销毁时执行
  },
  onPullDownRefresh: function() {
    // 触发下拉刷新时执行
  },
  onReachBottom: function() {
    // 页面触底时执行
  },
  onShareAppMessage: function () {
    // 页面被用户分享时执行
  },
  onPageScroll: function() {
    // 页面滚动时执行
  },
  onResize: function() {
    // 页面尺寸变化时执行
  },
  onTabItemTap(item) {
    // tab 点击时执行
    console.log(item.index)
    console.log(item.pagePath)
    console.log(item.text)
  },
  // 事件响应函数
  viewTap: function() {
    this.setData({
      text: 'Set some data for updating view.'
    }, function() {
      // this is setData callback
    })
  },
  // 自由数据
  customData: {
    hi: 'MINA'
  }
})
```

详细的参数含义和使用请参考 [Page 参考文档](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html)

#### 页面生命周期

![img](https://qiniu.waite.wang/202401222133494.png)

#### 模块化

可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module.html) 或者 `exports` 才能对外暴露接口。

注意：

* `exports` 是 [`module.exports`](https://developers.weixin.qq.com/miniprogram/dev/reference/api/module.html) 的一个引用，因此在模块里边随意更改 `exports` 的指向会造成未知的错误。所以更推荐开发者采用 `module.exports` 来暴露模块接口，除非你已经清晰知道这两者的关系。
* 小程序目前不支持直接引入 `node_modules` , 开发者需要使用到 `node_modules` 时候建议拷贝出相关的代码到小程序的目录中，或者使用小程序支持的 [npm](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html) 功能。

```javascript
// common.js
function sayHello(name) {
  console.log(`Hello ${name} !`)
}
function sayGoodbye(name) {
  console.log(`Goodbye ${name} !`)
}

module.exports.sayHello = sayHello
exports.sayGoodbye = sayGoodbye
```

在需要使用这些模块的文件中，使用 `require` 将公共代码引入

```javascript
var common = require('common.js')
Page({
  helloMINA: function() {
    common.sayHello('MINA')
  },
  goodbyeMINA: function() {
    common.sayGoodbye('MINA')
  }
})
```

#### 文件作用域

在 JavaScript 文件中声明的变量和函数只在该文件中有效；不同的文件中可以声明相同名字的变量和函数，不会互相影响。

通过全局函数 `getApp` 可以获取全局的应用实例，如果需要全局的数据可以在 `App()` 中设置，如：

```javascript
// app.js
App({
  globalData: 1
})
// a.js
// The localValue can only be used in file a.js.
var localValue = 'a'
// Get the app instance.
var app = getApp()
// Get the global data and change it.
app.globalData++
// b.js
// You can redefine localValue in file b.js, without interference with the localValue in a.js.
var localValue = 'b'
// If a.js it run before b.js, now the globalData shoule be 2.
console.log(getApp().globalData)
```

#### API

> 详细见 <https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html#API>

### 视图层

#### WXML

##### 数据绑定

```html
<!--wxml-->
<view> {{message}} </view>
// page.js
Page({
  data: {
    message: 'Hello MINA!'
  }
})
```

##### 列表渲染

```html
<!--wxml-->
<view wx:for="{{array}}"> {{item}} </view>
// page.js
Page({
  data: {
    array: [1, 2, 3, 4, 5]
  }
})
```

##### 条件渲染

```html
<!--wxml-->
<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
<view wx:elif="{{view == 'APP'}}"> APP </view>
<view wx:else="{{view == 'MINA'}}"> MINA </view>
// page.js
Page({
  data: {
    view: 'MINA'
  }
})
```

##### 模板

```html
<!--wxml-->
<template name="staffName">
  <view>
    FirstName: {{firstName}}, LastName: {{lastName}}
  </view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
<template is="staffName" data="{{...staffB}}"></template>
<template is="staffName" data="{{...staffC}}"></template>
// page.js
Page({
  data: {
    staffA: {firstName: 'Hulk', lastName: 'Hu'},
    staffB: {firstName: 'Shang', lastName: 'You'},
    staffC: {firstName: 'Gideon', lastName: 'Lin'}
  }
})
```

##### 三目运算符

```vue
//<view class="{{flag?'red':'green'}}">是否加样式</view>
data: {
    flag: false,
},
```

```javascript
// Register a Page.
Page({
 data: helloData,
 changeName: function (e) {
  // sent data change to view
  console.log(this.data)
  this.data.name = this.data.name === 'Weixin' ? 'xxx' : 'Weixin'
  this.setData({
   name: this.data.name
  })
 }
})
```

> 具体的能力以及使用方式在以下章节查看：
>
> [数据绑定](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/data.html)、[列表渲染](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/list.html)、[条件渲染](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/conditional.html)、[模板](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/template.html)、[引用](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/import.html)

#### WXS

WXS（WeiXin Script）是内联在 WXML 中的脚本段。通过 WXS 可以在模版中内联少量处理脚本，丰富模板的数据预处理能力。另外， WXS 还可以用来编写简单的 [WXS 事件响应函数](https://developers.weixin.qq.com/miniprogram/dev/framework/view/interactive-animation.html)。

从语法上看， WXS 类似于有少量限制的 JavaScript 。要完整了解 WXS 语法，请参考[WXS 语法参考](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/)。

以下是一些使用 WXS 的简单示例。

##### 页面渲染

```html
<!--wxml-->
<wxs module="m1">
var msg = "hello world";

module.exports.message = msg;
</wxs>

<view> {{m1.message}} </view>
```

##### 数据处理

```html
// page.js
Page({
  data: {
    array: [1, 2, 3, 4, 5, 1, 2, 3, 4]
  }
})
<!--wxml-->
<!-- 下面的 getMax 函数，接受一个数组，且返回数组中最大的元素的值 -->
<wxs module="m1">
var getMax = function(array) {
  var max = undefined;
  for (var i = 0; i < array.length; ++i) {
    max = max === undefined ?
      array[i] :
      (max >= array[i] ? max : array[i]);
  }
  return max;
}

module.exports.getMax = getMax;
</wxs>

<!-- 调用 wxs 里面的 getMax 函数，参数为 page.js 里面的 array -->
<view> {{m1.getMax(array)}} </view>
```

#### 简易双向绑定

在 WXML 中，普通的属性的绑定是单向的。例如：

```html
<input value="{{value}}" />
```

如果使用 `this.setData({ value: 'leaf' })` 来更新 `value` ，`this.data.value` 和输入框的中显示的值都会被更新为 `leaf` ；但如果用户修改了输入框里的值，却不会同时改变 `this.data.value` 。

如果需要在用户输入的同时改变 `this.data.value` ，需要借助简易双向绑定机制。此时，可以在对应项目之前加入 `model:` 前缀：

如果使用 `this.setData({ value: 'leaf' })` 来更新 `value` ，`this.data.value` 和输入框的中显示的值都会被更新为 `leaf` ；但如果用户修改了输入框里的值，却不会同时改变 `this.data.value` 。

如果需要在用户输入的同时改变 `this.data.value` ，需要借助简易双向绑定机制。此时，可以在对应项目之前加入 `model:` 前缀：

```html
<input model:value="{{value}}" />
```

这样，如果输入框的值被改变了， `this.data.value` 也会同时改变。同时， WXML 中所有绑定了 `value` 的位置也会被一同更新， [数据监听器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/observer.html) 也会被正常触发。

[在开发者工具中预览效果](https://developers.weixin.qq.com/s/8jXvobmV7vcj)

用于双向绑定的表达式有如下限制：

1. 只能是一个单一字段的绑定，如

```js
<input model:value="值为 {{value}}" />
<input model:value="{{ a + b }}" />
```

都是非法的；

1. 目前，尚不能 data 路径，如

```js
<input model:value="{{ a.b }}" />
```

这样的表达式目前暂不支持。

> 关于微信小程序警告“Do not have handler in component: pages/xxx/xxx. “的解决方法

通过资料查询和微信开发者社区询问，原因是没有绑定bindinput方法，因此我们可以为表单绑定一个空的方法，来解决这个警告

```html
<input model:value="{{name}}" bindinput="textCallback"/>

<!-- js -->
Page({
 data: helloData,
 changeName: function (e) {
  // sent data change to view
  console.log(this.data)
  this.data.name = this.data.name === 'Weixin' ? 'xxx' : 'Weixin'
  this.setData({
   name: this.data.name
  })
 },
 textCallback: function () {}
})
```

##### 在自定义组件中传递双向绑定

双向绑定同样可以使用在自定义组件上。如下的自定义组件：

```js
// custom-component.js
Component({
  properties: {
    myValue: String
  }
})
<!-- custom-component.wxml -->
<input model:value="{{myValue}}" />
```

这个自定义组件将自身的 `myValue` 属性双向绑定到了组件内输入框的 `value` 属性上。这样，如果页面这样使用这个组件：

```html
<custom-component model:my-value="{{pageValue}}" />
```

当输入框的值变更时，自定义组件的 `myValue` 属性会同时变更，这样，页面的 `this.data.pageValue` 也会同时变更，页面 WXML 中所有绑定了 `pageValue` 的位置也会被一同更新。

##### 在自定义组件中触发双向绑定更新

自定义组件还可以自己触发双向绑定更新，做法就是：使用 setData 设置自身的属性。例如：

```js
// custom-component.js
Component({
  properties: {
    myValue: String
  },
  methods: {
    update: function() {
      // 更新 myValue
      this.setData({
        myValue: 'leaf'
      })
    }
  }
})
```

如果页面这样使用这个组件：

```html
<custom-component model:my-value="{{pageValue}}" />
```

当组件使用 `setData` 更新 `myValue` 时，页面的 `this.data.pageValue` 也会同时变更，页面 WXML 中所有绑定了 `pageValue` 的位置也会被一同更新。

#### 响应显示区域变化

> 具体看 <https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html>

从小程序基础库版本 [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始，小程序在手机上支持屏幕旋转。使小程序中的页面支持屏幕旋转的方法是：在 `app.json` 的 `window` 段中设置 `"pageOrientation": "auto"` ，或在页面 json 文件中配置 `"pageOrientation": "auto"` 。

以下是在单个页面 json 文件中启用屏幕旋转的示例。

**代码示例：**

```json
{
  "pageOrientation": "auto"
}
```

如果页面添加了上述声明，则在屏幕旋转时，这个页面将随之旋转，显示区域尺寸也会随着屏幕旋转而变化。

从小程序基础库版本 [2.5.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 开始， `pageOrientation` 还可以被设置为 `landscape` ，表示固定为横屏显示。

#### 动画

> <https://developers.weixin.qq.com/miniprogram/dev/framework/view/animation.html>

#### 初始渲染缓存

小程序页面的初始化分为两个部分。

* 逻辑层初始化：载入必需的小程序代码、初始化页面 this 对象（也包括它涉及到的所有自定义组件的 this 对象）、将相关数据发送给视图层。
* 视图层初始化：载入必需的小程序代码，然后等待逻辑层初始化完毕并接收逻辑层发送的数据，最后渲染页面。

在启动页面时，尤其是小程序冷启动、进入第一个页面时，逻辑层初始化的时间较长。在页面初始化过程中，用户将看到小程序的标准载入画面（冷启动时）或可能看到轻微的白屏现象（页面跳转过程中）。

启用初始渲染缓存，可以使视图层不需要等待逻辑层初始化完毕，而直接提前将页面初始 data 的渲染结果展示给用户，这可以使得页面对用户可见的时间大大提前。它的工作原理如下：

* 在小程序页面第一次被打开后，将页面初始数据渲染结果记录下来，写入一个持久化的缓存区域（缓存可长时间保留，但可能因为小程序更新、基础库更新、储存空间回收等原因被清除）；
* 在这个页面被第二次打开时，检查缓存中是否还存有这个页面上一次初始数据的渲染结果，如果有，就直接将渲染结果展示出来；
* 如果展示了缓存中的渲染结果，这个页面暂时还不能响应用户事件，等到逻辑层初始化完毕后才能响应用户事件。

利用初始渲染缓存，可以：

* 快速展示出页面中永远不会变的部分，如导航栏；
* 预先展示一个骨架页，提升用户体验；
* 展示自定义的加载提示；
* 提前展示广告，等等。

##### 支持的组件

在初始渲染缓存阶段中，复杂组件不能被展示或不能响应交互。

目前支持的内置组件：

* `<view />`
* `<text />`
* `<button />`
* `<image />`
* `<scroll-view />`
* `<rich-text />`

自定义组件本身可以被展示（但它们里面用到的内置组件也遵循上述限制）。

##### 静态初始渲染缓存

若想启用初始渲染缓存，最简单的方法是在页面的 `json` 文件中添加配置项 `"initialRenderingCache": "static"` ：

```json
{
  "initialRenderingCache": "static"
}
```

如果想要对所有页面启用，可以在 `app.json` 的 `window` 配置段中添加这个配置：

```json
{
  "window": {
    "initialRenderingCache": "static"
  }
}
```

添加这个配置项之后，在手机中预览小程序首页，然后杀死小程序再次进入，就会通过初始渲染缓存来渲染首页。

注意：这种情况下，初始渲染缓存记录的是页面 data 应用在页面 WXML 上的结果，不包含任何 setData 的结果。

例如，如果想要在页面中展示出“正在加载”几个字，这几个字受到 `loading` 数据字段控制：

```html
<view wx:if="{{loading}}">正在加载</view>
```

这种情况下， `loading` 应当在 `data` 中指定为 `true` ，如：

```js
// 正确的做法
Page({
  data: {
    loading: true
  }
})
```

而不能通过 `setData` 将 `loading` 置为 `true` ：

```js
// 错误的做法！不要这么做！
Page({
  data: {},
  onLoad: function() {
    this.setData({
      loading: true
    })
  }
})
```

换而言之，这种做法只包含页面 `data` 的渲染结果，即页面的纯静态成分。

##### 在初始渲染缓存中添加动态内容

有些场景中，只是页面 `data` 的渲染结果会比较局限。有时会想要额外展示一些可变的内容，如展示的广告图片 URL 等。

这种情况下可以使用“动态”初始渲染缓存的方式。首先，配置 `"initialRenderingCache": "dynamic"` ：

```json
{
  "initialRenderingCache": "dynamic"
}
```

此时，初始渲染缓存不会被自动启用，还需要在页面中调用 `this.setInitialRenderingCache(dynamicData)` 才能启用。其中， `dynamicData` 是一组数据，与 `data` 一起参与页面 WXML 渲染。

```js
Page({
  data: {
    loading: true
  },
  onReady: function() {
    this.setInitialRenderingCache({
      loadingHint: '正在加载' // 这一部分数据将被应用于界面上，相当于在初始 data 基础上额外进行一次 setData
    })
  }
})
<view wx:if="{{loading}}">{{loadingHint}}</view>
```

从原理上说，在动态生成初始渲染缓存的方式下，页面会在后台使用动态数据重新渲染一次，因而开销相对较大。因而要尽量避免频繁调用 `this.setInitialRenderingCache` ，如果在一个页面内多次调用，仅最后一次调用生效。

注意：

* `this.setInitialRenderingCache` 调用时机不能早于 `Page` 的 `onReady` 或 `Component` 的 `ready` 生命周期，否则可能对性能有负面影响。
* 如果想禁用初始渲染缓存，调用 `this.setInitialRenderingCache(null)` 。

## 自定义 tabBar

> 参考 <https://developers.weixin.qq.com/miniprogram/dev/framework/ability/custom-tabbar.html>

### 自定义tabBar

自定义 tabBar 可以让开发者更加灵活地设置 tabBar 样式，以满足更多个性化的场景。

在自定义 tabBar 模式下

为了保证低版本兼容以及区分哪些页面是 tab 页，tabBar 的相关配置项需完整声明，但这些字段不会作用于自定义 tabBar 的渲染。
此时需要开发者提供一个自定义组件来渲染 tabBar，所有 tabBar 的样式都由该自定义组件渲染。推荐用 fixed 在底部的 cover-view + cover-image 组件渲染样式，以保证 tabBar 层级相对较高。
与 tabBar 样式相关的接口，如 wx.setTabBarItem 等将失效。
每个 tab 页下的自定义 tabBar 组件实例是不同的，可通过自定义组件下的 getTabBar 接口，获取当前页面的自定义 tabBar 组件实例。
注意：如需实现 tab 选中态，要在当前页面下，通过 getTabBar 接口获取组件实例，并调用 setData 更新选中态。可参考底部的代码示例。

### 使用

1. 配置信息

+ 在 app.json 中的 tabBar 项指定 custom 字段，同时其余 tabBar 相关配置也补充完整。
* 所有 tab 页的 json 里需声明 usingComponents 项，也可以在 app.json 全局开启。

``` json
{
  "tabBar": {
    "custom": true,
    "color": "#000000",
    "selectedColor": "#000000",
    "backgroundColor": "#000000",
    "list": [{
      "pagePath": "page/component/index",
      "text": "组件"
    }, {
      "pagePath": "page/API/index",
      "text": "接口"
    }]
  },
  "usingComponents": {}
}
```

2. 添加 tabBar 代码文件

在代码根目录下添加入口文件:

custom-tab-bar/index.js
custom-tab-bar/index.json
custom-tab-bar/index.wxml
custom-tab-bar/index.wxss
3. 编写 tabBar 代码

用自定义组件的方式编写即可，该自定义组件完全接管 tabBar 的渲染。另外，自定义组件新增 getTabBar 接口，可获取当前页面下的自定义 tabBar 组件实例

以下是一个简单的自定义 tabBar 组件示例：

![](https://qiniu.waite.wang/202403262041168.png)

``` html
<view class="tabbar">
 <view class="tabbar-item {{ select === index ? 'tabbar-select' : '' }}" wx:for="{{ list }}" wx:key="index" data-page="{{ item.pagePath }}" data-index="{{ index }}" data-type="{{ item.type }}" bindtap="selectPage">
  <block wx:if="{{ item.type === 0 }}">
   <image class="tabbar-image" src="{{ select === index ?  item.selectedIconPath : item.iconPath }}"></image>
   <text class="tabbar-text">{{ item.text }}</text>
  </block>
  <block wx:else>
   <view class="publish">
    <image class="tabbar-image" src="../images/add.png"></image>
   </view>
  </block>
 </view>
</view>
```

``` js
Component({
 data: {
  select: 0,
  list: [
   {
    iconPath: "/images/index.png",
    pagePath: "/pages/index/index",
    selectedIconPath: "/images/index_fill.png",
    text: "首页",
    type: 0
   },
   {
    iconPath: "/images/classify.png",
    pagePath: "/pages/classify/classify",
    selectedIconPath: "/images/classify_fill.png",
    text: "分类",
    type: 0
   },
   {
    type: 1,
    pagePath: "/pages/publish/publish",
   },
   {
    iconPath: "/images/collection.png",
    pagePath: "/pages/collection/collection",
    selectedIconPath: "/images/collection_fill.png",
    text: "收藏夹",
    type: 0,
   },
   {
    iconPath: "/images/me.png",
    pagePath: "/pages/me/me",
    selectedIconPath: "/images/me_fill.png",
    text: "我的",
    type: 0
   }
  ]
 },

 methods: {
  selectPage(e) {
   const { index, page, type } = e.currentTarget.dataset
   if (index !== this.data.select && type === 0) {
    wx.switchTab({
     url: page,
    })
   }
   if (type === 1) {
    wx.navigateTo({
     url: '../../pages/publish/publish',
    })
   }
   this.setData({
        select: index
      })
  }
 }
})
```

``` css
.tabbar {
 width: 100%;
 height: 80rpx;
 display: flex;
 background-color: #fff;
 position: fixed;
 bottom: 0;
 padding-bottom: env(safe-area-inset-bottom);
 padding-top: 10rpx;
 z-index: 9999;
}

.tabbar-item {
 flex: 1;
 display: flex;
 flex-direction: column;
 justify-content: center;
 align-items: center;
}

.tabbar-item .tabbar-image {
 width: 40rpx;
 height: 40rpx;
}

.tabbar-item .tabbar-text {
 font-size: 26rpx;
 margin-top: 10rpx;
}

.tabbar-item .publish {
 padding: 8px;
 background-color: #B2A4EC;
 border-radius: 50%;
 display: flex;
 align-items: center;
 justify-content: center;
 margin-bottom: auto;
}

.tabbar-select {
 color: #B2A4EC;
}
```

### skyline 模式
