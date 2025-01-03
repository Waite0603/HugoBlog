---
title: "系统学习 Vue -- 3-脚手架学习"
date: 2023-03-11T21:05:54+08:00
lastmod: 2023-03-22T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue"]
author: "Waite Wang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
description: "Vue 学习笔记-Vue脚手架学习"
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

## VueCLI

> <https://cli.vuejs.org/zh/>

什么是Vue脚手架？

+ 我们前面学习了如何通过webpack配置Vue的开发环境，但是在真实开发中我们不可能每一个项目从头来完成 所有的webpack配置，这样显示开发的效率会大大的降低；
+ 所以在真实开发中，我们通常会使用脚手架来创建一个项目，Vue的项目我们使用的就是Vue的脚手架；
+ 脚手架其实是建筑工程中的一个概念，在我们软件工程中也会将一些帮助我们搭建项目的工具称之为脚手架；
+ 我们可以通过CLI选择项目的配置和创建出我们的项目；
+ Vue CLI已经内置了webpack相关的配置，我们不需要从零来配置；

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统，提供：

+ 通过 `@vue/cli` 实现的交互式的项目脚手架。
+ 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。
+ 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  + 可升级；
  + 基于 webpack 构建，并带有合理的默认配置；
  + 可以通过项目内的配置文件进行配置；
  + 可以通过插件进行扩展。
+ 一个丰富的官方插件集合，集成了前端生态中最好的工具。
+ 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

### Vue CLI 安装

> Node 版本要求
>
> Vue CLI 4.x 需要 [Node.js](https://nodejs.org/) v8.9 或更高版本 (推荐 v10 以上)。你可以使用 [n](https://github.com/tj/n)，[nvm](https://github.com/creationix/nvm) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows) 在同一台电脑中管理多个 Node 版本。

+ 可以使用下列任一命令安装这个新的包：

```less
npm install -g @vue/cli
## OR
yarn global add @vue/cli
```

+ 安装之后，你就可以在命令行中访问 `vue` 命令。你可以通过简单运行 `vue`，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。你还可以用这个命令来检查其版本是否正确：

```less
vue --version
```

+ 如需升级全局的 Vue CLI 包，请运行：

```less
npm update -g @vue/cli

## 或者
yarn global upgrade --latest @vue/cli
```

+ 上面列出来的命令是用于升级全局的 Vue CLI。如需升级项目中的 Vue CLI 相关模块（以 `@vue/cli-plugin-` 或 `vue-cli-plugin-` 开头），请在项目目录下运行 `vue upgrade`：

```
用法： upgrade [options] [plugin-name]

（试用）升级 Vue CLI 服务及插件

选项：
-t, --to <version>    升级 <plugin-name> 到指定的版本
-f, --from <version>  跳过本地版本检测，默认插件是从此处指定的版本升级上来
-r, --registry <url>  使用指定的 registry 地址安装依赖
--all                 升级所有的插件
--next                检查插件新版本时，包括 alpha/beta/rc 版本在内
-h, --help            输出帮助内容
```

### 创建一个项目

运行以下命令来创建一个新项目：

```less
vue create hello-world
```

1. 选择预设
   + Default ([Vue 3] babel, eslint): 选择 Vue3 版本, 并且默认选择 babel, eslint
   + Default ([Vue 2] babel, eslint): 选择 Vue2 版本, 并且默认选择 babel, eslint
   + Manually select features: 手动选择需要获取的特性

![image-20231122103332793](https://qiniu.waite.wang/202311221033285.png)

2. 我们选择 `Manually select features`
   + Babel: Babel是一个JavaScript编译器，用于将新版本的JavaScript代码转换为向后兼容的旧版本，以便在不同浏览器和环境中运行。
   + TypeScript: TypeScript是一种静态类型的JavaScript超集，它添加了类型注解和其他特性，以提供更强大的开发工具和更可靠的代码。
   + Progressive Web App (PWA) Support: PWA是一种使用现代Web技术构建的应用程序，可以在各种设备和平台上提供类似原生应用的体验。
   + Router: Vue Router是Vue.js官方提供的路由管理器，用于实现单页面应用程序（SPA）中的导航和路由功能。
   + Vuex: Vuex是Vue.js官方提供的状态管理库，用于在大型应用程序中管理和共享状态。
   + CSS Pre-processors: CSS预处理器（如Sass、Less）允许您在编写CSS时使用变量、嵌套规则、函数等高级功能，以提高样式表的可维护性和可重用性。
   + Linter / Formatter: 代码检查工具（如ESLint）用于强制执行一致的代码风格和检测潜在的错误。代码格式化工具（如Prettier）可自动格式化代码，使其符合统一的样式规范。
   + Unit Testing: 单元测试是一种软件测试方法，用于验证应用程序中各个部分的功能是否按预期工作。
   + E2E Testing: 端到端（End-to-End）测试是一种测试方法，用于模拟用户在应用程序中执行的实际操作，以验证整个应用程序的功能和流程。

![image-20231122103618450](https://qiniu.waite.wang/202311221036769.png)

3. 选择 Vue 版本

![image-20231122103654515](https://qiniu.waite.wang/202311221036975.png)

4. Pick a linter / formatter config: 选择的代码检查和格式化配置
   + ESLint with error prevention only：仅使用ESLint进行错误检查，不应用其他格式化规则。
   + ESLint + Airbnb config：使用ESLint进行错误检查，并应用Airbnb JavaScript风格指南的格式化规则。
   + ESLint + Standard config：使用ESLint进行错误检查，并应用JavaScript Standard风格的格式化规则。
   + ESLint + Prettier：使用ESLint进行错误检查，并与Prettier代码格式化工具集成，以实现一致的代码样式
5. Pick additional lint features: 可供选择的附加代码检查功能
   + Lint on save：在保存文件时运行代码检查，以及在编辑器中进行实时错误检测和提示。
   + Lint and fix on commit：在提交代码时运行代码检查，并自动修复一些常见的问题，以确保提交的代码符合规范。
6. Where do you prefer placing config for Babel, ESLint, etc.? 配置文件位置选择
   + In dedicated config files：将Babel、ESLint等配置信息分别保存在它们各自的配置文件中（如.babelrc、.eslintrc等。
   + In package.json：将Babel、ESLint等配置信息保存在package.json文件的特定字段中，如babel、eslintConfig等。

7. Save this as a preset for future projects? (y/N) 这是一个保存预设供以后使用的选项，如果您希望将当前配置保存为预设，以便在将来的项目中重用，可以选择“y”，否则选择“N”。

> 您还可以使用图形界面使用以下`vue ui`命令创建和管理项目：

```less
vue ui
```

![界面预览](https://cli.vuejs.org/ui-new-project.png)

### 项目结构

```
|-node_modules       -- 所有的项目依赖包都放在这个目录下
|-public             -- 公共文件夹
---|favicon.ico      -- 网站的显示图标
---|index.html       -- 入口的html文件
|-src                -- 源文件目录，编写的代码基本都在这个目录下
---|assets           -- 放置静态文件的目录，比如logo.png就放在这里
---|components       -- Vue的组件文件，自定义的组件都会放到这
---|App.vue          -- 根组件，这个在Vue2中也有
---|main.ts          -- 入口文件，因为采用了TypeScript所以是ts结尾
---|shims-vue.d.ts   -- 类文件(也叫定义文件)，因为.vue结尾的文件在ts中不认可，所以要有定义文件
|-.browserslistrc    -- 在不同前端工具之间公用目标浏览器和node版本的配置文件，作用是设置兼容性
|-.eslintrc.js       -- Eslint的配置文件，不用作过多介绍
|-.gitignore         -- 用来配置那些文件不归git管理
|-package.json       -- 命令配置和包管理文件
|-README.md          -- 项目的说明文件，使用markdown语法进行编写
|-tsconfig.json      -- 关于TypeScript的配置文件
|-yarn.lock          -- 使用yarn后自动生成的文件，由Yarn管理，安装yarn包时的重要信息存储到yarn.lock文件中
```

> 在一个 Vue CLI 项目中，`@vue/cli-service` 安装了一个名为 `vue-cli-service` 的命令。你可以在 npm scripts 中以 `vue-cli-service`、或者从终端中以 `./node_modules/.bin/vue-cli-service` 访问这个命令。

```json
"scripts": {
  "serve": "vue-cli-service serve",
  "build": "vue-cli-service build",
  "lint": "vue-cli-service lint"
}
```

你可以通过 npm 或 Yarn 调用这些 script：

```less
npm run serve
## OR
yarn serve
```

如果你可以使用 [npx](https://github.com/npm/npx) (最新版的 npm 应该已经自带)，也可以直接这样调用命令：

```less
npx vue-cli-service serve
```

### vue-cli-service serve

```
用法：vue-cli-service serve [options] [entry]

选项：

  --open    在服务器启动时打开浏览器
  --copy    在服务器启动时将 URL 复制到剪切版
  --mode    指定环境模式 (默认值：development)
  --host    指定 host (默认值：0.0.0.0)
  --port    指定 port (默认值：8080)
  --https   使用 https (默认值：false)
```

`vue-cli-service serve` 命令会启动一个开发服务器 (基于 [webpack-dev-server](https://github.com/webpack/webpack-dev-server)) 并附带开箱即用的模块热重载 (Hot-Module-Replacement)。

除了通过命令行参数，你也可以使用 `vue.config.js` 里的 [devServer](https://cli.vuejs.org/zh/config/#devserver) 字段配置开发服务器。

命令行参数 `[entry]` 将被指定为唯一入口 (默认值：`src/main.js`，TypeScript 项目则为 `src/main.ts`)，而非额外的追加入口。尝试使用 `[entry]` 覆盖 `config.pages` 中的 `entry` 将可能引发错误。

### [vue-cli-service build](https://cli.vuejs.org/zh/guide/cli-service.html#vue-cli-service-build)

```
用法：vue-cli-service build [options] [entry|pattern]

选项：

  --mode        指定环境模式 (默认值：production)
  --dest        指定输出目录 (默认值：dist)
  --modern      面向现代浏览器带自动回退地构建应用
  --target      app | lib | wc | wc-async (默认值：app)
  --name        库或 Web Components 模式下的名字 (默认值：package.json 中的 "name" 字段或入口文件名)
  --no-clean    在构建项目之前不清除目标目录的内容
  --report      生成 report.html 以帮助分析包内容
  --report-json 生成 report.json 以帮助分析包内容
  --watch       监听文件变化
```

`vue-cli-service build` 会在 `dist/` 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好的缓存而做的自动的 vendor chunk splitting。它的 chunk manifest 会内联在 HTML 里。

这里还有一些有用的命令参数：

+ `--modern` 使用[现代模式](https://cli.vuejs.org/zh/guide/browser-compatibility.html#现代模式)构建应用，为现代浏览器交付原生支持的 ES2015 代码，并生成一个兼容老浏览器的包用来自动回退。
+ `--target` 允许你将项目中的任何组件以一个库或 Web Components 组件的方式进行构建。更多细节请查阅[构建目标](https://cli.vuejs.org/zh/guide/build-targets.html)。
+ `--report` 和 `--report-json` 会根据构建统计生成报告，它会帮助你分析包中包含的模块们的大小。

### vue-cli-service inspect

```
用法：vue-cli-service inspect [options] [...paths]

选项：

  --mode    指定环境模式 (默认值：development)
```

如果你想要设置 inspect 命令的选项，可以在命令后面添加对应的参数。例如，如果你想要查看生产环境的 webpack 配置，可以运行以下命令：`vue-cli-service inspect --mode production`

你也可以通过在 vue.config.js 文件中配置 configureWebpack 选项来修改 webpack 配置。这个选项允许你返回一个对象或函数来修改默认的 webpack 配置。例如：

```javascript
module.exports = {
  configureWebpack: {
    plugins: [
      // 添加一个插件
    ]
  }
}
```

> 你可以使用 `vue-cli-service inspect` 来审查一个 Vue CLI 项目的 webpack config。更多细节请查阅[审查 webpack config](https://cli.vuejs.org/zh/guide/webpack.html#审查项目的-webpack-config)。

## Webpack

### 认识webpack

+ 事实上随着前端的快速发展，目前前端的开发已经变的越来越复杂了：
  + 比如开发过程中我们需要通过模块化的方式来开发；
  + 比如也会使用一些高级的特性来加快我们的开发效率或者安全性，比如通过ES6+、TypeScript开发脚本逻辑， 通过sass、less等方式来编写 css 样式代码；
  + 比如开发过程中，我们还希望实时的监听文件的变化来并且反映到浏览器上，提高开发的效率；
  + 比如开发完成后我们还需要将代码进行压缩、合并以及其他相关的优化；
+ 但是对于很多的前端开发者来说，并不需要思考这些问题，日常的开发中根本就没有面临这些问题：

  + 这是因为目前前端开发我们通常都会直接使用三大框架来开发：Vue、React、Angular；
  + 但是事实上，这三大框架的创建过程我们都是借助于脚手架（CLI）的；
  + 事实上Vue-CLI、create-react-app、Angular-CLI都是基于webpack来帮助我们支持模块化、less、 TypeScript、打包优化等的；

### Webpack到底是什么呢？

> <https://webpack.docschina.org/>

> 本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，然后将你项目中所需的每一个模块组合成一个或多个 *bundles*，它们均为静态资源，用于展示你的内容。
>
> + 打包bundler：webpack可以将帮助我们进行打包，所以它是一个打包工具
> + 模块化module：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等；

![image-20231102102926580](https://qiniu.waite.wang/202311021029845.png)

### Vue项目加载的文件有哪些呢？

+ JavaScript的打包：
  + 将ES6转换成ES5的语法；
  + TypeScript的处理，将其转换成JavaScript；
+ Css的处理：
  + CSS文件模块的加载、提取；
  + Less、Sass等预处理器的处理；
+ 资源文件img、font：
  + 图片img文件的加载；
  + 字体font文件的加载；
+ HTML资源的处理：
  + 打包HTML资源文件；
+ 处理vue项目的SFC文件.vue文件

### Webpack的使用

+ webpack的官方文档是[webpack](https://webpack.js.org/)
  + webpack的中文官方文档是[webpack](https://webpack.docschina.org/)
+ Webpack的运行是依赖Node环境的，所以我们电脑上必须有Node环境
  + 所以我们需要先安装Node.js，并且同时会安装npm；
  + Node官方网站：[Node.js](https://nodejs.org/)

#### Webpack的安装

+ webpack的安装目前分为两个：webpack、webpack-cli
+ 执行webpack命令，会执行node_modules下的.bin目录下的webpack；
+ webpack在执行时是依赖webpack-cli的，如果没有安装就会报错；
+ 而webpack-cli中代码执行时，才是真正利用webpack进行编译和打包的过程；
+ 所以在安装webpack时，我们需要同时安装webpack-cli（第三方的脚手架事实上是没有使用webpack-cli的，而是类似于自 己的vue-service-cli的东西）

![img](https://qiniu.waite.wang/202311051542731.png)

#### Webpack的默认打包

> ES6、CommonJS等模块化语法在浏览器中是不被直接识别的，但是通过使用Webpack的打包功能，可以将这些语法转换为浏览器可以识别的语法。Webpack可以将多个模块打包成一个或多个浏览器可识别的文件，使得在浏览器中可以正常运行这些模块化代码。

+ 我们可以通过webpack进行打包，之后运行打包之后的代码

  + 在目录下直接执行 webpack 命令

```
webpack
```

+ 生成一个 dist 文件夹，里面存放一个main.js的文件，就是我们打包之后的文件：

  + 这个文件中的代码被压缩和丑化了；
  + 另外我们发现代码中依然存在ES6的语法，比如箭头函数、const等，这是因为默认情况下webpack并不清楚我们打包后的文 件是否需要转成ES5之前的语法，后续我们需要通过babel来进行转换和设置；

+ 我们发现是可以正常进行打包的，但是有一个问题，webpack是如何确定我们的入口的呢？

  + 事实上，当我们运行webpack时，webpack会查找当前目录下的 src/index.js作为入口；
  + 所以，如果当前项目中没有存在 src/index.js 文件，那么会报错；

+ 当然，我们也可以通过配置来指定入口和出口

```css
npx webpack --entry ./src/main.js --output-path ./build
```

#### webpack 局部安装

1. 版本控制：通过在项目中局部安装Webpack，可以确保每个项目使用的Webpack版本是一致的，避免不同项目之间的版本冲突。
2. 简化部署：将Webpack安装在项目的本地目录中，可以简化项目的部署过程。只需要将整个项目目录复制到其他环境中，不需要再次安装全局的Webpack。
3. 隔离环境：每个项目可以拥有自己独立的Webpack配置和插件，不会受到其他项目的影响。这样可以更灵活地根据项目需求进行定制和配置。
4. 可移植性：通过局部安装Webpack，可以将整个项目打包成一个可移植的文件夹，方便在不同环境中进行迁移和共享。

+ 第一步：创建package.json文件，用于管理项目的信息、库依赖等

```csharp
npm init
```

+ 第二步：安装局部的webpack

```csharp
npm install webpack webpack-cli --save-dev
// -D(--save-dev) 开发依赖
```

+ 第三步：使用局部的webpack

```undefined
npx webpack
```

+ 第四步：在 package.json 中创建scripts脚本，执行脚本打包即可

```coffeescript
## package
"scripts": {
    "build": "webpack"
}
## cmd
npm run build
```

![image-20231105235229201](https://qiniu.waite.wang/202311052352789.png)

### Webpack配置

#### Webpack 配置文件

+ 在通常情况下，webpack需要打包的项目是非常复杂的，并且我们需要一系列的配置来满足要求，默认配置必然 是不可以的。
+ 我们可以在根目录下创建一个webpack.config.js文件，来作为webpack的配置文件：

```javascript
const path = require('path');

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js"
  }
}
```

+ 继续执行webpack命令，依然可以正常打包

```coffeescript
npm run build
```

#### 指定配置文件

+ 但是如果我们的配置文件并不是webpack.config.js的名字，而是其他的名字呢？

  + 比如我们将webpack.config.js修改成了 wk.config.js；
  + 这个时候我们可以通过 --config 来指定对应的配置文件；

```
webpack --config wk.config.js
```

+ 但是每次这样执行命令来对源码进行编译，会非常繁琐，所以我们可以在package.json中增加一个新的脚本：

```
"build": "webpack --config wk.config.js"
```

+ 之后我们执行 npm run build来打包即可。

### Webpack的依赖

Webpack通过以下步骤对项目进行打包：

1. 配置：创建一个Webpack配置文件（通常命名为webpack.config.js），在其中定义打包的入口文件、输出文件的路径、加载器和插件等。
2. 入口：在配置文件中指定打包的入口文件，可以是单个文件或多个文件,  从入口开始，会生成一个 依赖关系图，这个依赖关系图会包含应用程序中所需的所有模块（比如 js 文件、css 文件、图片、字体等), 然后遍历图结构，打包一个个模块（根据文件的不同使用不同的loader来解析）；
3. 加载器：根据需要，配置加载器来处理不同类型的文件。例如，使用babel-loader来转换ES6+代码，使用 css-loader 和 style-loader 来处理CSS文件。
4. 插件：根据需要，配置插件来执行额外的任务。例如，使用html-webpack-plugin生成HTML文件，使用mini-css-extract-plugin提取CSS文件。
5. 输出：在配置文件中指定打包输出的路径和文件名。
6. 运行：在命令行中运行webpack命令，Webpack将根据配置文件进行打包，并生成输出文件。

在运行Webpack命令时，可以使用不同的参数和选项来控制打包的行为。例如，使用--mode参数指定打包的模式（开发模式或生产模式），使用--watch选项启用监听模式等。

![img](https://qiniu.waite.wang/202311060009812.png)

### loader的使用

> <https://webpack.docschina.org/loaders/>
>
> 在 Webpack5 以后会采用 asset module type 来替代 loader

> 在Webpack中，Loader是用于对不同类型的文件进行转换和处理的模块。它们作为Webpack的一部分，用于在打包过程中对文件进行预处理。
>
> Loader可以将不同类型的文件（如JavaScript、CSS、图片等）转换为模块，以便在应用程序中使用。它们可以执行各种任务，例如将ES6+代码转换为ES5语法、处理CSS文件中的样式、压缩和优化图像等。
>
> Loader通常以链式调用的方式使用，可以根据需要配置多个Loader来处理文件。每个Loader都会对文件进行一次转换，并将转换后的结果传递给下一个Loader，直到最后一个Loader将最终的结果返回给Webpack进行打包。
>
> Loader的配置是通过Webpack的配置文件（通常是webpack.config.js）中的module.rules字段来完成。在这个字段中，可以指定不同类型文件的匹配规则和对应的Loader。

#### css-loader 的使用

+ 对于加载css文件来说，我们需要一个可以读取css文件的loader；
+ 这个loader最常用的是css-loader；
+ css-loader的安装：

```
npm install css-loader -D
npm install css-loader --save-dev
```

+ 如何使用这个loader来加载css文件呢？有三种方式：
  + 内联方式；
  + CLI方式（webpack5中不再使用）；
  + 配置方式；

1. 内联方式：内联方式使用较少，因为不方便管理；

   + 在引入的样式前加上使用的loader，并且使用 ! 分割；

```css
import "css-loader!../css/style.css"
```

2. 配置方式

+ 配置方式表示的意思是在我们的webpack.config.js文件中写明配置信息：
  + module.rules中允许我们配置多个loader（因为我们也会继续使用其他的loader，来完成其他文件的加载)
  + 这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个Loader有一个全局的概览
+ module.rules的配置如下：
  + rules属性对应的值是一个数组：[Rule]
  + 数组中存放的是一个个的Rule，Rule是一个对象，对象中可以设置多个属性：
    + test属性：用于对 resource（资源）进行匹配的，通常会设置成正则表达式；
    + use属性：对应的值时一个数组：[UseEntry]
      + UseEntry是一个对象，可以通过对象的属性来设置一些其他属性
        + loader：必须有一个 loader属性，对应的值是一个字符串；
        + options：可选的属性，值是一个字符串或者对象，值会被传入到loader中；
        + query：目前已经使用options来替代；
      + 传递字符串（如：use: [ 'style-loader' ]）是 loader 属性的简写方式（如：use: [ { loader: 'style-loader'} ]）；
    + loader属性： Rule.use: [ { loader } ] 的简写

```css
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  }
};
```

#### style-loader

+ 我们已经可以通过css-loader来加载css文件了

  + 但是你会发现这个css在我们的代码中并没有生效（页面没有效果）。

+ 因为css-loader只是负责将.css文件进行解析，并不会将解析之后的css插入到页面中；

+ 如果我们希望再完成插入style的操作，那么我们还需要另外一个loader，就是style-loader；

+ 安装style-loader：

```
npm install style-loader -D
```

+ 那么我们应该如何使用style-loader：
  + 在配置文件中，添加style-loader；
  + 注意：因为loader的执行顺序是从右向左（或者说从下到上，或者说从后到前的），所以我们需要将style-loader写到 css-loader 的前面；

```javascript
use: ['style-loader', 'css-loader']
```

#### Less工具处理

+ 我们可以使用less工具来完成它的编译转换：

```coffeescript
npm install less -D
```

+ 执行如下命令：

```cobol
npm install less -D npx lessc ./src/css/title.less title.css
```

##### less-loader处理

+ 但是在项目中我们会编写大量的css，它们如何可以自动转换呢？

  + 这个时候我们就可以使用less-loader，来自动使用less工具转换less到css；

```coffeescript
npm install less-loader -D
```

```json
{
    test: /\.less$/,
    use: [
        "style-loader",
        "css-loader",
        "less-loader"
    ] 
}
```

#### postcss-loader(认识)

+ 什么是PostCSS呢？
  + PostCSS是一个通过JavaScript来转换样式的工具；
  + 这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置； p但是实现这些功能，我们需要借助于PostCSS对应的插件；
+ 如何使用PostCSS呢？主要就是两个步骤：
  + 第一步：查找PostCSS在构建工具中的扩展，比如webpack中的postcss-loader；
  + 第二步：选择可以添加你需要的PostCSS相关的插件；

##### 命令行使用postcss

+ 安装一下它们：postcss、postcss-cli

```coffeescript
npm install postcss postcss-cli -D
```

+ 我们编写一个需要添加前缀的css：
  + [Autoprefixer CSS online](https://autoprefixer.github.io/)
  + 我们可以在上面的网站中查询一些添加css属性的样式；
  + 在Webpack中，PostCSS通常用于对CSS文件进行预处理和后处理。您可以使用PostCSS来编写现代CSS语法，然后使用各种插件对其进行处理，例如自动添加CSS前缀、压缩CSS等。

##### 插件autoprefixer

+ 因为我们需要添加前缀，所以要安装autoprefixer：

```coffeescript
npm install autoprefixer -D
```

+ 直接使用使用postcss工具，并且制定使用autoprefixer

```cobol
npx postcss --use autoprefixer -o end.css ./src/css/style.css
```

> 转化的结果如下

```css
.title {
  user-select: none;
}

.title {
  -webkit-user-select: none;
     -moz-user-select: none;
      -ms-user-select: none;
          user-select: none;
}
/*## sourceMappingURL=data:application/json;base64,eyJ2ZXJzaW9uIjozLCJzb3VyY2VzIjpbInRlc3QuY3NzIl0sIm5hbWVzIjpbXSwibWFwcGluZ3MiOiJBQUFBO0VBQ0UseUJBQWlCO0tBQWpCLHNCQUFpQjtNQUFqQixxQkFBaUI7VUFBakIsaUJBQWlCO0FBQ25CIiwiZmlsZSI6ImRlbW8uY3NzIiwic291cmNlc0NvbnRlbnQiOlsiLnRpdGxlIHtcbiAgdXNlci1zZWxlY3Q6IG5vbmU7XG59Il19 */
```

##### postcss-loader

+ 真实开发中我们必然不会直接使用命令行工具来对css进行处理，而是可以借助于构建工具：

  + 在webpack中使用postcss就是使用postcss-loader来处理的；

+ 我们来安装postcss-loader：

```coffeescript
npm install postcss-loader -D
```

+ 因为postcss需要有对应的插件才会起效果，所以我们需要配置它的plugin；

```css
use: [
 {
  loading: "postcss-loader",
  options: {
   postcssOptions: {
    plugins: [
     require('autoprefixer')
    ] 
   }
  }
 }
]
```

+ 我们可以将 postcss 配置分离, 根目录下新建 postcss.config.js

```javascript
module.exports = {
  plugins: [
    require("postcss-preset-env")
  ]
}
```

+ 这样在 webpack.config 中只需要 添加 `postcss-loader` 即可

##### postcss-preset-env

> `postcss-preset-env` 是一个 PostCSS 插件，它允许您使用最新的 CSS 特性，同时它会根据您的目标环境自动添加必要的回退。
>
> 这个插件包含了 autoprefixer（自动添加浏览器前缀），cssnano（压缩 CSS），以及一些其他的 PostCSS 插件。这意味着您可以在 CSS 中使用最新的特性，例如 CSS Grid，CSS Variables，等等，而不需要担心兼容性问题。
>
> 在您的 `postcss.config.js` 文件中，您已经将 `postcss-preset-env` 添加为一个插件，这意味着当您运行 PostCSS 时，它将使用 `postcss-preset-env` 来处理您的 CSS 文件。

+ 事实上，在配置postcss-loader时，我们配置插件并不需要使用autoprefixer。

+ 我们可以使用另外一个插件：postcss-preset-env

  + postcss-preset-env也是一个postcss的插件；
  + 它可以帮助我们将一些现代的CSS特性，转成大多数浏览器认识的CSS，并且会根据目标浏览器或者运行时环境 添加所需的polyfill；
  + 也包括会自动帮助我们添加 autoprefixer（所以相当于已经内置了 autoprefixer）；

+ 首先，我们需要安装 postcss-preset-env：

```coffeescript
npm install postcss-preset-env -D
```

+ 之后，我们直接修改掉之前的 autoprefixer 即可;

#### file-loader

+ 要处理jpg、png等格式的图片，我们也需要有对应的loader：file-loader

  + file-loader的作用就是帮助我们处理import/require()方式引入的一个文件资源，并且会将它放到我们输出的文件夹中；
  + 当然我们待会儿可以学习如何修改它的名字和所在文件夹；

+ 安装file-loader：

```coffeescript
npm install file-loader -D
```

+ 配置处理图片的Rule：

```json
{
  test: /\.(jpe?g|png|gif|svg)$/,
  type: "asset",
  generator: {
    filename: "img/[name]_[hash:6][ext]"
  },
  parser: {
    dataUrlCondition: {
      maxSize: 100 * 1024
    }
  }
}
```

#### url-loader

+ url-loader和file-loader的工作方式是相似的，但是可以将较小的文件，转成base64的URI。

+ 安装url-loader：

```coffeescript
url-loader npm install url-loader -D 
```

+ 显示结果是一样的，并且图片可以正常显示；

![image-20231111211324041](https://qiniu.waite.wang/202311112113242.png)

+ 但是在dist文件夹中，我们会看不到图片文件：
  + 这是因为我的两张图片的大小分别是38kb和295kb；
  + 默认情况下url-loader会将所有的图片文件转成base64编码

```javascript
{
  test: /\.(jpe?g|png|gif|svg)$/,
  use: {
    loader: "url-loader",
    options: {
      // outputPath: "img",
      name: "img/[name]_[hash:6].[ext]",
      limit: 100 * 1024
    }
  }
}
```

##### url-loader的limit

+ 但是开发中我们往往是小的图片需要转换，但是大的图片直接使用图片即可
  + 这是因为小的图片转换base64之后可以和页面一起被请求，减少不必要的请求过程；
  + 而大的图片也进行转换，反而会影响页面的请求速度；
+ 那么，我们如何可以限制哪些大小的图片转换和不转换呢？
  + url-loader有一个options属性limit，可以用于设置转换的限制；
  + 下面的代码38kb的图片会进行base64编码，而295kb的不会；

#### 字体文件加载

```javascript
{
  test: /\.(eot|ttf|woff2?)$/,
  use: {
    loader: "file-loader",
    options: {
      // outputPath: "font",
      name: "font/[name]_[hash:6].[ext]"
    }
  }
},
{
  test: /\.(eot|ttf|woff2?)$/,
  type: "asset/resource",
  generator: {
    filename: "font/[name]_[hash:6][ext]"
  }
}
```

### 文件命名规则

+ 有时候我们处理后的文件名称按照一定的规则进行显示：
  + 比如保留原来的文件名、扩展名，同时为了防止重复，包含一个hash值等；
+ 这个时候我们可以使用PlaceHolders来完成，webpack给我们提供了大量的PlaceHolders来显示不同的内容：
  + <https://webpack.js.org/loaders/file-loader/#placeholders>
+ 我们这里介绍几个最常用的placeholder：
  + [ext]： 处理文件的扩展名；
  + [name]：处理文件的名称；
  + [hash]：文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值（32个十六进制）；
  + [contentHash]：在file-loader中和[hash]结果是一致的（在webpack的一些其他地方不一样，后面会讲到）；
  + [`hash:<length>`]：截图hash的长度，默认32个字符太长了；
  + [path]：文件相对于webpack配置文件的路径；

#### 设置文件名称

+ 那么我们可以按照如下的格式编写：
+ 这个也是vue的写法；

![img](https://qiniu.waite.wang/202311061505056.png)

#### 设置文件的存放路径

+ 刚才通过 img/ 已经设置了文件夹，这个也是vue、react脚手架中常见的设置方式
  + 其实按照这种设置方式就可以了；
  + 当然我们也可以通过outputPath来设置输出的文件夹；

![img](https://qiniu.waite.wang/202311061506379.png)

### asset module type

+ 我们当前使用的webpack版本是webpack5：
  + 在webpack5之前，加载这些资源我们需要使用一些loader，比如raw-loader 、url-loader、file-loader；
  + 在webpack5开始，我们可以直接使用资源模块类型（asset module type），来替代上面的这些loader；
+ 资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：
  + asset/resource 发送一个单独的文件并导出 URL。之前通过使用 file-loader 实现；
  + asset/inline 导出一个资源的 data URI。之前通过使用 url-loader 实现；
  + asset/source 导出资源的源代码。之前通过使用 raw-loader 实现；
  + asset 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 url-loader，并且配置资源体积限制实现；

#### asset module type的使用

+ 比如加载图片，我们可以使用下面的方式：

```javascript
{
  test: /\.(jpe?g|png|gif|svg)$/,
  type: "asset",
  // 自定义文件的输出路径和文件名
  generator: {
    filename: "img/[name]_[hash:6][ext]"
  },
  parser: {
    dataUrlCondition: {
      maxSize: 100 * 1024
    }
  }
}
```

### Plugin

+ Loader是用于特定的模块类型进行转换；
+ Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等；

![image-20231112151429444](https://qiniu.waite.wang/202311121514761.png)

#### CleanWebpackPlugin

+ 每次修改了一些配置，重新打包时，都需要手动删除dist文件夹：
+ 我们可以借助于一个插件来帮助我们完成，这个插件就是CleanWebpackPlugin；
+ 首先，我们先安装这个插件：

```less
 npm install clean-webpack-plugin -D
```

+ 之后在插件中配置：

```javascript
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
  plugins: [
    new CleanWebpackPlugin();
  ]
};
```

#### HtmlWebpackPlugin

+ 我们的HTML文件是编写在根目录下的，而最终打包的dist文件夹中是没有index.html文件的。
+ 在进行项目部署的时，必然也是需要有对应的入口文件index.html；
+ 所以我们也需要对index.html进行打包处理；

+ 对HTML进行打包处理我们可以使用另外一个插件：HtmlWebpackPlugin；

```less
npm install html-webpack-plugin -D
```

##### 自定义HTML模板

+ 如果我们想在自己的模块中加入一些比较特别的内容：
  + 比如添加一个noscript标签，在用户的JavaScript被关闭时，给予响应的提示；
  + 比如在开发vue或者react项目时，我们需要一个可以挂载后续组件的根标签

+ 这个我们需要一个属于自己的index.html模块：

> public/index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

+ 上面的代码中，会有一些类似这样的语法<% 变量 %>，这个是EJS模块填充数据的方式。
  + 在配置 HtmlWebpackPlugin 时，我们可以添加如下配置：
    + template：指定我们要使用的模块所在的路径；
    + title：在进行 htmlWebpackPlugin.options.title 读取时，就会读到该信息；

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  plugins: [
      new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    })
  ]
}; 
```

#### DefinePlugin

> <https://github.com/jantimon/html-webpack-plugin>

+ 因为在模板中使用了 `BASE_URL`, 但是我们并没有设置过这个常量值，所以会出现没有定义的错误 - `BASE_URL is not defined`
+ 这个时候我们可以使用DefinePlugin插件
+ DefinePlugin允许在编译时创建配置的全局常量，是一个webpack内置的插件（不需要单独安装）：

```javascript
const { DefinePlugin } = require("webpack");

module.exports = {
  plugins: [
      new DefinePlugin({
      BASE_URL: "'./'"
    })
  ]
}; 
```

#### CopyWebpackPlugin

+ `CopyWebpackPlugin` 是一个用于 webpack 的插件，它的主要功能是将单个文件或整个目录复制到构建目录。

  这个插件在以下情况下非常有用：

  1. 当你有一些静态资源（例如图片、字体或公共库）需要包含在你的构建中，但是这些资源并不需要通过 webpack 处理时。
  2. 当你需要将一些文件复制到构建目录，以便在部署应用程序时使用

+ 安装CopyWebpackPlugin插件：

```less
npm install copy-webpack-plugin -D
```

+ 接下来配置CopyWebpackPlugin即可：
  + 复制的规则在patterns中设置；
  + from：设置从哪一个源中开始复制；
  + to：复制到的位置，可以省略，会默认复制到打包的目录下；
  + globOptions：设置一些额外的选项，其中可以编写需要忽略的文件：
    + .DS_Store：mac目录下回自动生成的一个文件；
    + index.html：也不需要复制，因为我们已经通过HtmlWebpackPlugin完成了index.html的生成；

```javascript
const CopyWebpackPlugin = require('copy-webpack-plugin');

new CopyWebpackPlugin({
  patterns: [
    {
      from: "public",
      to: "./",
      globOptions: {
        ignore: [
          "**/index.html"
        ]
      }
    }
  ]
})
```

### Mode配置列表

```javascript
module.exports = {
  // 设置模式
  // development 开发阶段, 会设置development
  // production 准备打包上线的时候, 设置production
  mode: "development",
  // 设置source-map, 建立js映射文件, 方便调试代码和错误
  devtool: "source-map"
  }
```

## Vite

> <https://cn.vitejs.dev/guide/>

### 什么是 Vite

+ Vite（法语意为 "快速的"，发音 `/vit/`，发音同 "veet"）是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：
  + 一个开发服务器(dev server)，它基于 [原生 ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 提供了 [丰富的内建功能](https://cn.vitejs.dev/guide/features.html)，如速度快到惊人的 [模块热更新（HMR）](https://cn.vitejs.dev/guide/features.html#hot-module-replacement)。
  + 一套构建指令，它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。
+ Vite 是一种具有明确建议的工具，具备合理的默认设置。您可以在 [功能指南](https://cn.vitejs.dev/guide/features.html) 中了解 Vite 的各种可能性。通过 [插件](https://cn.vitejs.dev/guide/using-plugins.html)，Vite 支持与其他框架或工具的集成。如有需要，您可以通过 [配置部分](https://cn.vitejs.dev/config/) 自定义适应你的项目。
+ Vite 还提供了强大的扩展性，可通过其 [插件 API](https://cn.vitejs.dev/guide/api-plugin.html) 和 [JavaScript API](https://cn.vitejs.dev/guide/api-javascript.html) 进行扩展，并提供完整的类型支持。
+ Vite作为一个基于浏览器原生ESM的构建工具，它省略了开发环境的打包过程，利用浏览器去解析imports，在服务端按需编译返回。同时，在开发环境拥有速度快到惊人的模块热更新，且热更新的速度不会随着模块增多而变慢。因此，使用Vite进行开发，至少会比Webpack快10倍左右。
+ 你可以在 [为什么选 Vite](https://cn.vitejs.dev/guide/why.html) 部分深入了解该项目的设计理念。

#### Vite的主要特性

+ Instant Server Start —— 即时服务启动
+ Lightning Fast HMR —— 闪电般快速的热更新
+ Rich Features —— 丰富的功能
+ Optimized Build —— 经过优化的构建
+ Universal Plugin Interface —— 通用的Plugin接口
+ Fully Typed APIs —— 类型齐全的API

#### 主流构建工具对比

**Browserify**

预编译模块化方案（文件打包工具）

+ Browserify基于流方式干净灵活
+ 遵循commonJS规范打包JS
+ 可引入插件打包CSS等其他资源（非原生能力）

**Gulp**

+ 基于流的自动化构建工具（工程化）
+ 配置复杂度高，偏向编程式，需要定义task处理构建
+ 支持监听读写文件
+ 可搭配Browserify等模块化工具来使用

**Parcel**

+ 极速打包（工程化：极速0配置）
+ 零配置，但造成了配置不灵活，内置常见场景的构建方案及其依赖，无需再次安装（babel等）
+ 以html入口，自动检测和打包依赖
+ 不支持SourceMap
+ 无法Tree-shaking

**Webpack**

+ 预编译模块化方案（工程化：大而全）
+ 通过配置文件达到一站式配置
+ loader进行资源转换，功能全面（css+js+icon+front）
+ 插件丰富，灵活扩展
+ 社群庞大
+ 大型项目构建慢

**Rollup**

+ 基于ES6打包（模块打包工具）
+ Tree-shaking
+ 打包文件小且干净，执行效率更高
+ 更专注于JS打包

**Snowpack**

+ 基于ESM运行时编译（工程化：ESM运行时）
+ 无需递归循环依赖组装依赖树
+ 默认输出单独的构建模块（未打包），可选择不同打包器（webpack、rollup等）

**Vite**

+ 基于ESM运行时打包
+ 借鉴了Snowpack
+ 生产环境使用Rollup，集成度更高，相比Snowpack支持多页面、库模式、动态导入自动polyfill等

### 为什么使用 Vite

#### **开发环境⚡️速度的提升**

1. 使用JS开发的工具通常需要很长的时间才能启动开发服务器，且这个启动时间与代码量、代码复杂度正相关。即使使用HMR，文件修改后的效果也要几秒钟才能在浏览器中反应出来，代表如Webpack。那么Vite是如何解决如Webpack这样的构建工具一样，在复杂、多模块项目开发中启动慢、HMR慢的问题呢？
2. 我们详细对比了开发环境中的Vite和Webpack，发现主要有如下不同：

| Webpack                               | Vite                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| 先打包生成bundle，再启动开发服务器    | 先启动开发服务器，利用新一代浏览器的ESM能力，无需打包，直接请求所需模块并实时编译 |
| HMR时需要把改动模块及相关依赖全部编译 | HMR时只需让浏览器重新请求该模块，同时利用浏览器的缓存（源码模块协商缓存，依赖模块强缓存）来优化请求 |
| 内存高效利用                          | -                                                            |

3. 因此，针对开发环境中的启动慢问题，Vite开发环境冷启动无需打包，无需分析模块之间的依赖，同时也无需在启动开发服务器前进行编译，启动时还会使用 **esbuild** 来进行预构建。而Webpack 启动后会做一堆事情，经历一条很长的编译打包链条，从入口开始需要逐步经历语法解析、依赖收集、代码转译、打包合并、代码优化，最终将高版本的、离散的源码编译打包成低版本、高兼容性的产物代码;
4. 针对HMR慢，即使只有很小的改动，Webpack依然需要构建完整的模块依赖图，并根据依赖图来进行转换。而Vite利用了ESM和浏览器缓存技术，更新速度与项目复杂度无关。可以看到，如Snowpack、Vite这类面相非打包的构建工具，在开发环境启动时只需要启动两个Server，一个用于页面加载，一个用于HMR的Websocket。当浏览器发出原生的ESM请求，Server收到请求只需要编译当前文件后返回给浏览器，不需要管理依赖。

![image-20231122171817220](https://qiniu.waite.wang/202311221718913.png)

![image-20231122171839049](https://qiniu.waite.wang/202311221718035.png)

#### **使用简单，开箱即用**

相比Webpack需要对entry、loader、plugin等进行诸多配置，Vite的使用可谓是相当简单了。只需执行初始化命令，就可以得到一个预设好的开发环境，开箱即获得一堆功能，包括：CSS预处理、html预处理、异步加载、分包、压缩、HMR等。他使用复杂度介于Parcel和Webpack的中间，只是暴露了极少数的配置项和plugin接口，既不会像Parcel一样配置不灵活，又不会像Webpack一样需要了解庞大的loader、plugin生态，灵活适中、复杂度适中。适合前端新手。

### Vite 的安装与使用

#### 安装

> Vite 需要 [Node.js](https://nodejs.org/en/) 版本 18+，20+。然而，有些模板需要依赖更高的 Node 版本才能正常运行，当你的包管理器发出警告时，请注意升级你的 Node 版本。

+ 首先，我们安装一下vite工具：

  ```
  npm install vite –g ## 全局安装 
  npm install vite –D ## 局部安装
  12
  ```

+ 通过vite来启动项目：

  ```
  npx vite
  ```

#### Vite构建Vue3项目

使用 NPM:

```css
npm create vite@latest
```

使用 Yarn:

```less
yarn create vite
```

```
Need to install the following packages:
  create-vite@4.2.0
Ok to proceed? (y) y
✔ Project name: … vite-vue3
? Select a framework: › - Use arrow-keys. Return to submit.
    Vanilla     （如果你要使用Vue2可以用这个方式，然后再自己配置一下vite-plugin-vue2插件）
 ❯  Vue         （默认是Vue3项目）
    React       （React项目）
    Preact      （比React项目轻量级的Preact项目）
    Lit
    Svelte
    Others
```

因为我们要构建Vue3的项目，所以这里我们选择Vue就行了，然后下一步选择开发语言

```
✔ Project name: … vite-vue3
✔ Select a framework: › Vue
? Select a variant: › - Use arrow-keys. Return to submit.
❯   JavaScript
    TypeScript
    Customize with create-vue ↗
    Nuxt ↗
```

cd 进项目路径, `npm install`, 然后 `npm run dev` 运行即可

![](https://qiniu.waite.wang/7C797674-06CF-4E87-B344-63990EF519B6.jpg)

### Vite 支持

#### Css 支持

+ vite可以直接支持css的处理

  + 直接导入css即可；

+ vite可以直接支持css预处理器，比如less

  + 直接导入less；
  + 之后安装less编译器；

  ```less
   npm install less -D
  ```

  + vite直接支持postcss的转换：
  + 只需要安装postcss，并且配置 postcss.config.js 的配置文件即可；

  ```less
  npm install postcss postcss-preset-env -D
  
  // 在项目中创建 postcss.config.ts
  const postcssPresetEnv = require('postcss-preset-env');
  
  module.exports = {
    // 安装了一个预设 预设就是相当于最佳实践，已经帮你安装好了很多东西
    plugin: [postcssPresetEnv(/* pluginOptions */)]
  }
  ```

  ##### 全局导入

  ```javascript
  // vite.config.js
  
  import { defineConfig } from 'vite';
  
  export default defineConfig({
    // 其他配置项...
    css: {
      preprocessorOptions: {
        scss: {
          additionalData: `
            @import "@/style/globalVar.scss";
          `,
        },
      },
    },
  });
  ```

  ##### 别名

  ```javascript
  // vite.config.js
  
  import { defineConfig } from 'vite';
  import vue from '@vitejs/plugin-vue';
  
  export default defineConfig({
    // 其他配置项...
    resolve: {
      alias: {
        '@': '/src'
      }
    },
    plugins: [vue()]
  });
  ```

#### Ts 支持

+ vite对TypeScript是原生支持的，它会直接使用ESBuild来完成编译：
  + 只需要直接导入即可；
+ 如果我们查看浏览器中的请求，会发现请求的依然是ts的代码：
  + 这是因为vite中的服务器Connect会对我们的请求进行转发；
  + 获取ts编译后的代码，给浏览器返回，浏览器可以直接进行解析；

#### vue支持

+ vite对vue提供第一优先级支持：

  + Vue 3 单文件组件支持：@vitejs/plugin-vue
  + Vue 3 JSX 支持：@vitejs/plugin-vue-jsx
  + Vue 2 支持：underfin/vite-plugin-vue2

+ 安装支持vue的插件：

  ```less
   npm install @vitejs/plugin-vue -D
  ```

+ 在vite.config.js中配置插件

```javascript
const vue = require('@vitejs/plugin-vue')

module.exports = {
  plugins: [
    vue()
  ]
}
```

### Vite 原理

#### ESM&esbuild

##### ESM

在ES6没有出现之前，随着js代码日益膨胀，往往会对资源模块化来提效，这也就出现了多个模块化方案。如CommonJS常用于服务端，AMD、CMD规范常用在客户端。ES6出现后，紧接着出现了ESM。ESM是浏览器支持的一种模块化方案，允许在浏览器实现模块化。

+ CommonJS：模块同步，如Browserify会对代码进行解析，整理出代码中的所有模块依赖关系，然后把nodejs的模块编译成浏览器可用的模块，相关的模块代码都打包在一起，形成一个完整的JS文件，这个文件中不会存在 require 这类的模块化语法，变成可以在浏览器中运行的普通JS，运行时加载
+ AMD：模块异步，依赖前置，是requireJS在推广过程中对模块定义的规范化产出，加载完依赖后立即执行依赖模块，依赖加载成功后执行回调
+ CMD：模块异步，延迟执行，是seaJS在推广过程中对模块定义的规范化产出，就近依赖，先加载所有依赖模块，运行时才执行require内容，按顺序执行

与CommonJS、AMD不同，ESM的对外接口只是一种静态定义，为编译时加载，遇到模块加载命令import，就会生成一个只读引用。等脚本真正执行时，再根据这个只读引用，到被加载的那个模块内取值。由于ESM编译时就能确定模块的依赖关系，因此能够只包含要运行的代码，可以显著减少文件体积，降低浏览器压力。

由于ESM是一个比较新的模块化方案，目前其浏览器能力支持如下：

![img](https://qiniu.waite.wang/202311221721568.png)

可以看到，除了IE、Opera等，新一代浏览器中绝大部分都已支持。

接下来以Vite创建的模板为例，看一下ESM的解析过程：

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>

<script>
  import HelloWorld from './components/HelloWorld.vue'
  export default {
    name: 'App',
    components: {
      HelloWorld
    }
  }
</script>
```

+ 当浏览器解析 import HelloWorld from './components/HelloWorld.vue' 时，会向当前域名发送一个请求获取对应的资源（ESM支持解析相对路径）
+ 浏览器下载对应的文件，然后解析成模块记录。接下来会进行实例化，为模块分配内存，然后按照导入、导出语句建立模块和内存的映射关系。最后，运行上述代码，把内存空间填充为真实的值。

##### esbuild

+ ESBuild的特点：
  + 超快的构建速度，并且不需要缓存；
  + 支持ES6和CommonJS的模块化；
  + 支持ES6的Tree Shaking；
  + 支持Go、JavaScript的API；
  + 支持TypeScript、JSX等语法编译；
  + 支持SourceMap；
  + 支持代码压缩；
  + 支持扩展其他插件；
+ ESBuild的构建速度和其他构建工具速度对比：

![在这里插入图片描述](https://qiniu.waite.wang/202311221724703.png)

+ ESBuild为什么这么快呢？
  + 使用Go语言编写的，可以直接转换成机器代码，而无需经过字节码；
  + ESBuild可以充分利用CPU的多内核，尽可能让它们饱和运行；
  + ESBuild的所有内容都是从零开始编写的，而不是使用第三方，所以从一开始就可以考虑各种性能问题；

#### **依赖处理**

Vite 通过在一开始将应用中的模块区分为 **依赖** 和 **源码** 两类，改进了开发服务器启动时间。**依赖** 大多为在开发时不会变动的纯 JavaScript。一些较大的依赖（例如有上百个模块的组件库）处理的代价也很高。

+ **依赖解析**

以 Vite 官方 demo 为例，当我们请求 **localhost:3000** 时，Vite 默认返回 **localhost:3000/index.html** 的代码。而后发送请求 **src/main.js**。

main.js 代码如下：

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'

createApp(App).mount('#app')
```

![img](https://qiniu.waite.wang/202311222321963.png)

可以观察到浏览器请求 vue.js 时， 请求路径是 **@modules/vue.js**。在 Vite 中约定若 path 的请求路径满足 **/^\/@modules\//** 格式时，被认为是一个 node_modules 模块。

平时开发中，webpack & rollup(rollup有对应插件) 等打包工具会帮我们找到模块的路径，但浏览器只能通过相对路径去寻找，而如果是直接使用模块名比如：**import vue from 'vue'**，浏览器就会报错，这个时候就需要一个三方包进行处理。Vite 对ESM形式的 js 文件模块使用了 ES Module Lexer 处理。Lexer 会找到代码中以 import 语法导入的模块并以数组形式返回。Vite 通过该数组的值获取判断是否为一个 node_modules 模块。若是则进行对应改写成 @modules/:id 的写法。

重写完路径后，浏览器会发送 path 为 /@modules/:id 的对应请求，接下来会被 Vite 客户端做一层拦截来解析模块的真实位置。

首先正则匹配请求路径，如果是/@modules开头就进行后续处理，否则就跳过。若是，会设置响应类型为js，读取真实模块路径内容，返回给客户端。

客户端注入本质上是创建一个script标签（type='module'），然后将其插入到head中，这样客户端在解析html是就可以执行代码了

```js
export const moduleRE = /^\/@modules\//
// plugin for resolving /@modules/:id requests.
app.use(async (ctx, next) => {
  if (!moduleRE.test(ctx.path)) {
    return next()
  }
  // path maybe contain encode chars
  const id = decodeURIComponent(ctx.path.replace(moduleRE, ''))
  ctx.type = 'js'
  const serve = async (id: string, file: string, type: string) => {
    // 在代码中做一个缓存，下次访问相同路径直接从 map 中获取 304 返回
    moduleIdToFileMap.set(id, file)
    moduleFileToIdMap.set(file, ctx.path)
    debug(`(${type}) ${id} -> ${getDebugPath(root, file)}`)
    await ctx.read(file)
    return next()
  }
 }
  // 兼容 alias 情况
  const importerFilePath = importer ? resolver.requestToFile(importer) : root
  const nodeModulePath = resolveNodeModuleFile(importerFilePath, id)
  // 如果是个 node_modules 的模块，读取文件。
  if (nodeModulePath) {
   return serve(id, nodeModulePath, 'node_modules')
  }
})
```

+ **依赖预构建**

依赖预构建主要有两个目的：

+ **CommonJS 和 UMD 兼容性:** 开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。
+ **性能：** Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

Vite使用esbuild在初次启动开发服务器前把检测到的依赖进行预构建。Vite 基于ESM，在使用某些模块时，由于模块依赖了另一些模块，依赖的模块又基于另一些模块。会出现页面初始化时一次发送数百个模块请求的情况。

以 lodash-es 为例，代码中以 import { debounce } from 'lodash' 导入一个命名函数时候，并不是只下载包含这个函数的文件，而是有一个依赖图。

![img](https://qiniu.waite.wang/202311222322996.png)

Vite 为了优化请求数量和速度，利用esbuild在启动的时候预先把debounce用到的所有内部模块全部打包成一个bundle，这样就浏览器在请求debounce时，便只需要发送一次请求了

##### **静态资源加载**

当请求的路径符合 imageRE, mediaRE, fontsRE 或 JSON 格式，会被认为是一个静态资源。静态资源将处理成ESM模块返回。

```js
// src/node/utils/pathUtils.ts
const imageRE = /\.(png|jpe?g|gif|svg|ico|webp)(\?.*)?$/
const mediaRE = /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/
const fontsRE = /\.(woff2?|eot|ttf|otf)(\?.*)?$/i
export const isStaticAsset = (file: string) => {
  return imageRE.test(file) || mediaRE.test(file) || fontsRE.test(file)
}

// src/node/server/serverPluginAssets.ts
app.use(async (ctx, next) => {
  if (isStaticAsset(ctx.path) && isImportRequest(ctx)) {
    ctx.type = 'js'
    ctx.body = export default ${JSON.stringify(ctx.path)} // 输出是path
    return
  }
  return next()
})

export const jsonPlugin: ServerPlugin = ({ app }) => {
  app.use(async (ctx, next) => {
    await next()
    // handle .json imports
    // note ctx.body could be null if upstream set status to 304
    if (ctx.path.endsWith('.json') && isImportRequest(ctx) && ctx.body) {
      ctx.type = 'js'
      ctx.body = dataToEsm(JSON.parse((await readBody(ctx.body))!), {
        namedExports: true,
        preferConst: true
      })
    }
  })
}
```

##### **vue文件缓存**

当 Vite 遇到一个 .vue 后缀的文件时。由于 .vue 模板文件的特殊性，它被拆分成 template, css, script 模块三个模块进行分别处理。最后会对 script, template, css 发送多个请求获取

![img](https://qiniu.waite.wang/202311222324430.png)

##### **js/ts处理**

Vite使用esbuild将ts转译到js，约是tsc速度的20～30倍，同时HMR更新反应到浏览器的时间会小于50ms。但是，由于esbuild转换ts到js对于类型操作仅仅是擦除，所以完全保证不了类型正确，因此需要额外校验类型，比如使用tsc --noEmit。

将ts转换成js后，浏览器便可以利用ESM直接拿到js资源。

#### 热更新原理

Vite 的热加载原理，其实就是在客户端与服务端建立了一个 websocket 连接，当代码被修改时，服务端发送消息通知客户端去请求修改模块的代码，完成热更新。

+ 服务端：服务端做的就是监听代码文件的改变，在合适的时机向客户端发送 websocket 信息通知客户端去请求新的模块代码。
+ 客户端：Vite 中客户端的 websocket 相关代码在处理 html 中时被写入代码中。可以看到在处理 html 时，vite/client 的相关代码已经被插入。

```js
export const clientPublicPath = `/vite/client`
const devInjectionCode = `\n<script type="module">import "${clientPublicPath}"</script>\n`
async function rewriteHtml(importer: string, html: string) {
  return injectScriptToHtml(html, devInjectionCode)
}
```

当request.path 路径是 /vite/client 时，请求获取已经提前写好的关于 websocket 的代码。因此在客户端中我们创建了一个 websocket 服务并与服务端建立了连接。

Vite 会接受到来自客户端的消息。通过不同的消息触发一些事件。做到浏览器端的即时热模块更换（热更新）。包括 connect、vue-reload、vue-rerender 等事件，分别触发组件vue 的重新加载，render等。

```js
// Listen for messages
socket.addEventListener('message', async ({ data }) => {
  const payload = JSON.parse(data) as HMRPayload | MultiUpdatePayload
  if (payload.type === 'multi') {
    payload.updates.forEach(handleMessage)
  } else {
    handleMessage(payload)
  }
})

async function handleMessage(payload: HMRPayload) {
  const { path, changeSrcPath, timestamp } = payload as UpdatePayload
  console.log(path)
  switch (payload.type) {
    case 'connected':
      console.log(`[vite] connected.`)
      break
    case 'vue-reload':
      queueUpdate(
        import(`${path}?t=${timestamp}`)
          .catch((err) => warnFailedFetch(err, path))
          .then((m) => () => {
            __VUE_HMR_RUNTIME__.reload(path, m.default)
            console.log(`[vite] ${path} reloaded.`)
          })
      )
      break
    case 'vue-rerender':
      const templatePath = `${path}?type=template`
      import(`${templatePath}&t=${timestamp}`).then((m) => {
        __VUE_HMR_RUNTIME__.rerender(path, m.render)
        console.log(`[vite] ${path} template updated.`)
      })
      break
    case 'style-update':
      // check if this is referenced in html via <link>
      const el = document.querySelector(`link[href*='${path}']`)
      if (el) {
        el.setAttribute(
          'href',
          `${path}${path.includes('?') ? '&' : '?'}t=${timestamp}`
        )
        break
      }
      const importQuery = path.includes('?') ? '&import' : '?import'
      await import(`${path}${importQuery}&t=${timestamp}`)
      console.log(`[vite] ${path} updated.`)
      break
    case 'js-update':
      queueUpdate(updateModule(path, changeSrcPath, timestamp))
      break
    case 'custom':
      const cbs = customUpdateMap.get(payload.id)
      if (cbs) {
        cbs.forEach((cb) => cb(payload.customData))
      }
      break
    case 'full-reload':
      if (path.endsWith('.html')) {
        // if html file is edited, only reload the page if the browser is
        // currently on that page.
        const pagePath = location.pathname
        if (
          pagePath === path ||
          (pagePath.endsWith('/') && pagePath + 'index.html' === path)
        ) {
          location.reload()
        }
        return
      } else {
       location.reload()
      }
  }
}
```

## Webpack-Babel

### 什么是 Babel?

> <https://babeljs.io/>

> + Babel 是一个 JavaScript 编译器，可以将 ECMAScript 2015+ 代码转换为向后兼容的 JavaScript 版本，以便在当前和旧版浏览器或环境中运行。它还支持将 JSX 转换为普通 JavaScript 代码。Babel 是一个非常流行的工具，许多现代的 JavaScript 应用程序都使用它来构建和部署。
> + 事实上，在开发中我们很少直接去接触babel，但是babel对于前端开发来说，目前是不可缺少的一部分：
> + 开发中，我们想要使用ES6+的语法，想要使用TypeScript，开发React项目，它们都是离不开Babel的；所以，学习Babel对于我们理解代码从编写到线上的转变过程至关重要；

+ 以下是一个使用 Babel 的示例：
+ 假设我们有一个使用箭头函数和 const 声明的简单 JavaScript 模块：

```javascript
const greet = (name) => {
  console.log(`Hello, ${name}!`);
}

export default greet;
```

+ 如果我们想要在旧版浏览器中运行它，我们可以使用 Babel 将其转换为 ES5：

```javascript
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
exports.default = void 0;

var greet = function greet(name) {
  console.log("Hello, ".concat(name, "!"));
};

var _default = greet;
exports.default = _default;
```

+ 这个转换过的代码可以在大多数浏览器中运行，即使它们不支持箭头函数或 const 声明。

### Babel 命令行使用

+ babel本身可以作为一个独立的工具（和postcss一样），不和webpack等构建工具配置来单独使用。
+ 如果我们希望在命令行尝试使用babel，需要安装如下库：
  + @babel/core：babel的核心代码，必须安装；
  + @babel/cli：可以让我们在命令行使用babel；

```less
npm install @babel/cli @babel/core -D
```

+ 使用babel来处理我们的源代码：
  + src：是源文件的目录；
  + –out-dir：指定要输出的文件夹dist；
  + --out-file: 指定要输出的文件dist；

```less
npx babel src --out-dir dist
npx babel src --out-file dist
```

### 插件

+ Babel 的插件是用于转换 JavaScript 代码的小型程序，可以添加到 Babel 配置中。Babel 插件可以执行各种任务，例如：
  + 转换语法：将新的 ECMAScript 特性转换为向后兼容的代码。
  + 转换 API：将使用新 API 的代码转换为旧 API。
  + 转换 JSX：将 JSX 转换为普通的 JavaScript 代码。

+ 以下是一些常见的 Babel 插件：
  + @babel/plugin-transform-arrow-functions: 将箭头函数转换为普通函数。
  + @babel/plugin-transform-block-scoping: 将 let 和 const 声明转换为 var 声明。
  + @babel/plugin-transform-classes: 将类转换为 ES5 构造函数。
  + @babel/plugin-transform-destructuring: 将解构赋值转换为普通赋值。
  + @babel/plugin-transform-object-assign: 将 Object.assign() 转换为 ES5 兼容的代码。
  + @babel/plugin-transform-react-jsx: 将 JSX 转换为普通的 JavaScript 代码。
  + @babel/plugin-transform-runtime: 避免在每个文件中重复使用 Babel 运行时代码。

+ 如何使用?
  + 比如我们需要转换箭头函数, const 转成 var，那么我们就可以使用箭头函数转换相关的插件：

```less
npm install @babel/plugin-transform-arrow-functions -D 
npm install @babel/plugin-transform-block-scoping -D

npx babel src --out-dir dist --plugins=@babel/plugin-transform-block-scoping ,@babel/plugin-transform-arrow-functions
```

### 预设 preset

Babel 的预设（preset）是一组预先配置的转换规则，用于将特定版本的 JavaScript 代码转换为向后兼容的旧版本。以下是一些常用的 Babel 预设：

1. @babel/preset-env: 根据目标环境自动确定需要的转换规则。它根据你在 .babelrc 或 babel.config.js 文件中的配置来确定需要转换的 JavaScript 特性。
2. @babel/preset-react: 用于转换 React JSX 语法的预设。它可以将 JSX 转换为普通的 JavaScript 代码。
3. @babel/preset-typescript: 用于转换 TypeScript 代码的预设。它可以将 TypeScript 的类型注解和其他特定语法转换为普通的 JavaScript 代码。
4. @babel/preset-flow: 用于转换 Flow 类型注解的预设。它可以将 Flow 的类型注解转换为普通的 JavaScript 代码。

这些预设可以根据你的项目需求进行选择和配置。你可以在 .babelrc 或 babel.config.js 文件中指定所需的预设，例如：

```javascript
module.exports = {
  presets: [
    "@babel/preset-env"
  ]
}
```

也可以

```less
npm install @babel/preset-env -D 
npx babel src --out-dir dist --presets=@babel/preset-env
```

### 原理

#### 底层原理

+ babel是如何做到将我们的一段代码（ES6、TypeScript、React）转成另外一段代码（ES5）的呢？
  + 从一种源代码（原生语言）转换成另一种源代码（目标语言），这是什么的工作呢？
  + 就是编译器，事实上我们可以将babel看成就是一个编译器。
  + Babel编译器的作用就是将我们的源代码，转换成浏览器可以直接识别的另外一段源代码；
+ Babel也拥有编译器的工作流程：
  + 解析（Parsing）：Babel首先将输入的JavaScript代码解析成抽象语法树（Abstract Syntax Tree，AST）。AST是一个用于表示代码结构的树状数据结构，它能够准确地描述代码的语法和语义。
  + 转换（Transformation）：在AST的基础上，Babel会应用一系列的插件和预设来进行代码转换。这些插件和预设可以执行各种转换操作，例如语法转换、代码优化、添加兼容性处理等。每个插件都负责处理AST中的特定节点，并根据需要进行修改或替换。
  + 生成（Generation）：转换完成后，Babel会将修改后的AST重新生成为JavaScript代码。这些生成的代码可以是与输入代码相同的版本，也可以是经过转换后的新代码。

+ <https://github.com/jamiebuilds/the-super-tiny-compiler>: 非常简单的编译器实现，旨在教授编译器原理和实践。

#### 执行原理

> Babel的执行阶段

![image-20231116205304599](https://qiniu.waite.wang/202311162053777.png)

![image-20231116205317664](https://qiniu.waite.wang/202311162053832.png)

1. 词法分析（Lexing）：将输入的源代码字符串转换为一个令牌（Token）序列。每个令牌代表源代码中的一个语法单元，例如标识符、运算符、括号等。
2. 语法分析（Parsing）：将令牌序列转换为抽象语法树（AST）。AST是一个用于描述代码结构的树状数据结构，它能够准确地描述源代码的语法和语义。
3. 转换（Transformation）：在AST的基础上，应用一系列的转换规则，以修改和优化AST。这些规则可以执行各种操作，例如语法转换、代码优化、添加兼容性处理等。
4. 代码生成（Code Generation）：将修改后的AST转换为目标语言的代码。在"The Super Tiny Compiler"中，目标语言是JavaScript。
5. 输出：输出生成的目标代码。

### babel-loader

+ babel-loader是一个用于在Webpack构建过程中将JavaScript代码转换的加载器（loader）。它是与Babel配合使用的常用工具之一。
+ 通过配置Webpack的规则，使用babel-loader可以将指定的JavaScript文件传递给Babel进行转换。Babel会根据配置的插件和预设，将源代码转换为目标浏览器或环境所支持的语法。
+ 指定使用的插件

![image-20231116220536704](https://qiniu.waite.wang/202311162205301.png)

+ presets 预设:  如果我们一个个去安装使用插件，那么需要手动来管理大量的babel插件，我们可以直接给webpack提供一个 preset，webpack会根据我们的预设来加载对应的插件列表，并且将其传递给babel。以下使用 `@babel/preset-env`

  + env
  + react
  + TypeScript

+ 使用 babel-loader 的一般步骤如下：

  1. 安装 babel-loader 和相关的Babel插件和预设：

  ```less
  npm install --save-dev babel-loader @babel/core @babel/preset-env webpack
  ```

  2. webpack.config.js，在其中配置 babel-loader：

  ```javascript
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: __dirname + '/dist'
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
      ]
    }
  };
  ```

### 配置文件

+ 我们可以将babel的配置信息放到一个独立的文件中，babel给我们提供了两种配置文件的编写：
  + babel.config.json（或者.js，.cjs，.mjs）文件；
  + babelrc.json（或者.babelrc，.js，.cjs，.mjs）文件；
+ 它们两个有什么区别呢？目前很多的项目都采用了多包管理的方式（babel本身、element-plus、umi等）；
  + .babelrc.json：早期使用较多的配置方式，但是对于配置Monorepos项目是比较麻烦的；
  + babel.config.json（babel7）：可以直接作用于Monorepos项目的子包，更加推荐；
  + 补充: *Monorepo* 是一种项目代码管理方式,指单个仓库中管理多个项目,有助于简化代码共享、版本控制、构建和部署等方面的复杂性,并提供更好的可重用性和协作性, 类似 `@babel/preset-env` 这种写法大概率就是 Monorepo
+ 以下为 `babel.config.js`

```javascript
module.exports = {
  presets: [
    "@babel/preset-env"
  ]
}
```

+ 这样在 `webpack.config.js` 中只需要

```javascript
{
  test: /\.js$/,
  loader: "babel-loader"
}
```

### 在 Webpack 中使用 Vue

```less
npm install vue
```

```javascript
import { createApp } from 'vue';

createApp({
  template: "<h2>{{title}} - {{message}}</h2>",
  components: {

  },
  data() {
    return {
      title: "Hello World",
      message: "哈哈哈"
    }
  }
}).mount("#app");
```

> 此时重新 `build` 运行后不显示, 报错如下
>
> `runtime-core.esm-bundler.js:38 [Vue warn]: Component provided template option but runtime compilation is not supported in this build of Vue. Configure your bundler to alias "vue" to "vue/dist/vue.esm-bundler.js" at <App>` -> `runtime-core.esm-bundler.js:38`

> 这个错误是因为 Vue 3 默认不包含模板编译器，所以当你试图在组件中使用 `template` 选项时，你会看到这个警告。
>
> 要解决这个问题，你需要在 webpack 配置中添加一个别名，将 "vue" 指向 "vue/dist/vue.esm-bundler.js"。这个版本的 Vue 包含了模板编译器。

```javascript
// 在 main.js 中更改应用, 重新部署即可
import { createApp } from 'vue/dist/vue.esm-bundler.js';
```

> Vue打包后不同版本解析
>
> + vue(.runtime).global(.prod).js 是用于直接在浏览器中引入的全局版本，可以通过 `<script>` 标签来使用。
    + 我们之前通过CDN引入和下载的Vue版本就是这个版本；
    + 会暴露一个全局的Vue来使用；
> + vue(.runtime).esm-browser(.prod).js 是用于原生 ES 模块导入的版本，在支持 ES 模块的浏览器中可以使用 `<script type='module'>` 来引入。
> + vue(.runtime).esm-bundler.js 是用于构建工具（如webpack、rollup、parcel等）的版本，默认情况下会使用 vue.runtime.esm-bundler.js。如果需要解析模板（template），则需要手动指定 vue.esm-bundler.js。
> + vue.cjs(.prod).js 是用于服务器端渲染的版本，在 Node.js 中可以使用 require() 来引入。
>   + `require` 是 Node.js 中用于导入模块的函数。它是 CommonJS 模块系统的一部分，这是 Node.js 的默认模块系统。以下是一个 `require` 的基本用法示例：`const fs = require('fs');`在这个例子中，我们导入了 Node.js 的内置 `fs`（文件系统）模块。
>   + `require` 函数也可以用来导入你自己的模块。例如，如果你有一个名为 `myModule.js` 的文件，你可以这样导入它：`const myModule = require('./myModule.js');` 注意，当导入自己的模块时，你需要提供模块的相对路径（以 `./` 开头）。
>   + 然而，`require` 并不是 ECMAScript（JavaScript 的标准化规范）的一部分，因此它在浏览器环境中通常不可用。在浏览器环境中，你通常会使用 ECMAScript 的 `import` 和 `export` 语句来导入和导出模块。

+ 接下来我们把 main.js 中的 vue 代码抽离为单文件组件(SFC)

```vue
<!-- /vue/App.vue -->
<template>
  <h2>我是Vue渲染出来的</h2>
  <h2>{{title}}</h2>
  <hello-world></hello-world>
</template>

<script>
  import HelloWorld from './HelloWorld.vue';

  export default {
    components: {
      HelloWorld
    },
    data() {
      return {
        title: "Hello World",
        message: "哈哈哈"
      }
    },
    methods: {

    }
  }
</script>

<style scoped>
  h2 {
    color: red;
  }
</style>
```

+ 重新打包, 报错：我们需要合适的Loader来处理文件。

```less
npm install vue-loader -D
```

+ 配置 `webpack.config.js`

```javascript
{
  test: /\.vue$/,
  loader: "vue-loader"
}
```

+ 重新 build 仍然报错 `Error:vue-loader requires @vue/compiler present the dependency tree`, 打包依然会报错，这是因为我们必须添加@vue/compiler-sfc来对template进行解析：

  ```less
  npm install @vue/compiler-sfc -D
  ```

+ 重新打包即可支持App.vue的写法

### 补充

+ 当然此时控制台还有如下报错 `You are running the csm-bundler bu1ld of Vue, It is recommended to contigure your bundler to expl1citly roplace featur flag globals with boolean literals to get proper tree-shaking in the fina bundle, See http://link,yuejs.org/feature-flags for more details.`

+ 在官方解释如下: <https://github.com/vuejs/core/tree/main/packages/vue#bundler-build-feature-flags>

![image-20231117234618957](https://qiniu.waite.wang/202311172346161.png)

+ Bundler Build Feature Flags是构建工具（Bundler）中的一种特性标志，用于控制Vue框架的不同特性的开启和关闭。从Vue 3.0.0-rc.3版本开始，esm-bundler构建工具现在公开了全局特性标志，可以在编译时进行覆盖。其中两个重要的特性标志包括：
  + VUE_OPTIONS_API：启用/禁用Options API支持，默认为true。
  + VUE_PROD_DEVTOOLS：启用/禁用生产环境下的devtools支持，默认为false。
+ 在没有配置这些特性标志的情况下，构建工具仍然可以正常工作，但强烈建议正确配置它们以便在最终的打包文件中实现正确的树摇效果。要配置这些特性标志：
  + <https://webpack.js.org/plugins/define-plugin/>

```javascript
module.exports = {
    plugins: [
        new DefinePlugin({
          BASE_URL: "'./'",
          __VUE_OPTIONS_API__: true,
          __VUE_PROD_DEVTOOLS__: false
        })
   ]
}
```

> 开启Bundler Build Feature Flags的好处在于能够更好地控制Vue框架的特性和功能，从而有效地减少最终打包文件的大小。通过正确配置特性标志，可以实现树摇（tree-shaking）效果，即只包含应用程序实际使用的代码，而不包含未使用的代码。这将有助于提高应用程序的性能和加载速度，并减少资源消耗。此外，通过禁用不需要的特性，还可以减少应用程序的复杂性，并提高代码的可维护性。因此，建议开发人员在使用Vue框架时正确配置Bundler Build Feature Flags，以获得更好的开发和部署体验。

## Webpack-dev-server

> `devServer` 是指开发服务器，通常用于前端开发环境。在前端开发中，开发者通常需要一个本地服务器来运行他们的应用程序，以便进行测试和调试。Webpack是一个流行的前端构建工具，而`devServer`就是Webpack提供的一个功能，用于在开发过程中提供一个简单的服务器。
>
> `devServer` 可以帮助开发者在本地启动一个服务器，监视文件的变化，并在文件发生更改时自动重新加载页面，以提高开发效率。此外，它还支持一些其他功能，比如模块热替换（Hot Module Replacement），允许在不刷新整个页面的情况下更新部分模块。
>
> 在使用Webpack配置文件时，你可以配置 `devServer` 的各种选项，以满足你的开发需求。这包括设置服务器的端口、指定静态资源的路径、配置代理等。

+ 目前我们开发的代码，为了运行需要有两个操作：
  + 操作一：npm run build，编译相关的代码；
  + 操作二：通过live server或者直接通过浏览器，打开index.html代码，查看效果；
+ 这个过程经常操作会影响我们的开发效率，我们希望可以做到，当文件发生变化时，可以自动的完成编译和展示；
+ 为了完成自动编译，webpack提供了几种可选的方式：
  + `webpack watch mode`：在文件发生变化时，Webpack会自动重新编译代码。
  + `webpack-dev-server`（常用）：提供了一个开发服务器，可以在本地运行你的应用程序，并且在文件发生变化时自动重新加载页面。
  + `webpack-dev-middleware`：结合其他服务器框架使用，将Webpack与服务器集成，实现在文件发生变化时自动重新编译

### Webpack watch

+ webpack给我们提供了watch模式：

  + 在该模式下，webpack依赖图中的所有文件，只要有一个发生了更新，那么代码将被重新编译；
  + 我们不需要手动去运行 npm run build指令了；

+ 开启watch模式有两种方式：

  + 通过在命令行中使用--watch参数来开启watch模式。例如，运行`webpack --watch`命令即可开启watch模式。

  ```json
  // package.json
  "scripts": {
      "build": "webpack --watch"
  }
  ```

  + 在Webpack配置文件中添加``watch: true`选项来开启watch模式。例如，在Webpack配置文件中添加以下代码：

  ```javascript
  // webpack.config.js
  
  module.exports = {
    watch: true
  }
  ```

  ### webpack-dev-server

  + 上面的方式可以监听到文件的变化，但是事实上它本身是没有自动刷新浏览器的功能的：
    + 当然，目前我们可以在 VSCode 中使用 live-server 来完成这样的功能；
    + 但是，我们希望在不适用live-server的情况下，可以具备live reloading（实时重新加载）的功能；
  + 安装webpack-dev-server

  ```less
  npm install webpack-dev-server -D
  ```

  + 修改配置文件

  ```json
  // package.json
  
  "scripts": {
    "build": "webpack",
    "serve": "webpack serve"
  }
  ```

  + 运行 `npm run serve`, 在本地配置一个服务器, 使用 `webpack cli` 解析/ 启动本地服务

  ![image-20231119190420271](https://qiniu.waite.wang/202311191904772.png)

> 在运行 `npm run serve` 命令时，如果没有指定输出目录，webpack会默认将打包后的文件输出到内存中，而不是硬盘上的某个目录中。因此即使没有指定输出目录，该命令仍然可以正常运行。这种方式称为内存编译，可以提高开发效率，因为不需要每次修改代码后都重新编译和写入磁盘。 使用 `memfs` 这一个库实现;

### 认识模块热替换（HMR）

+ 什么是HMR呢？
  + HMR的全称是Hot Module Replacement，翻译为模块热替换；
  + 模块热替换是指在 应用程序运行过程中，**替换、添加、删除模块，而无需重新刷新整个页面**；
+ HMR通过如下几种方式，来提高开发的速度：
  + 不重新加载整个页面，这样可以保留某些应用程序的状态不丢失；
  + 只更新需要变化的内容，节省开发的时间；
  + 修改了css、js源代码，会立即在浏览器更新，相当于直接在浏览器的 devtools 中直接修改样式；
+ 如何使用HMR呢？
  + 默认情况下，webpack-dev-server已经支持HMR，我们只需要开启即可；
  + 在不开启HMR的情况下，当我们修改了源代码之后，整个页面会自动刷新，使用的是live reloading；

#### 开启 HMR

+ 修改webpack的配置：

```javascript
// webpack.config.js
module.exports = {
  // target 用来指定打包后的代码在哪个环境下运行
  target: "web",
  devServer: {
    // 1. static: 用来指定静态资源的根目录
    // 如果有的资源没有在 Webpack 中加载, 那么就会去 static 中查找加载
    static: "./public",
    // 2. hot: 是否开启热更新
    hot: true
  }
}
```

![image-20231119201428778](https://qiniu.waite.wang/202311192014822.png)

+ 指定哪些模块发生更新时，进行HMR；

```javascript
if(module.hot) {
  // module.hot.accept(moduleName, callback)
  module.hot.accept("./js/element", () => {
    console.log("element模块发生了变化");
  })
}
```

#### 框架 的 HMR

+ 大多数主流框架（如React、Vue和Angular）都对模块热替换（HMR）提供了内置的支持，以便在开发过程中实现更快的热更新。

+ 具体而言，这些框架通常会提供开发服务器或开发工具，用于在开发过程中启用HMR功能。通过使用这些工具，你可以在修改代码时实时查看更新后的效果，而无需手动刷新页面。
+ 以下是一些常见的框架的HMR支持方式：
  + React：React 框架通常使用Webpack的 `react-hot-loader` 插件来实现HMR功能。你可以在Webpack配置文件中配置该插件，然后在开发服务器中启用HMR。
  + Vue：Vue 框架内置了对HMR的支持。你可以使用 `vue-loader` 和 `vue-style-loader` 等相关插件，以及在Webpack配置文件中配置HMR选项，来启用Vue的HMR功能。
  + Angular：Angular 框架使用Webpack的 `@angular-builders/custom-webpack` 插件来实现HMR功能。你可以在Angular项目的配置文件中进行相应的配置，以启用HMR。
+ 请注意，每个框架的具体配置方式可能会有所不同。建议查阅相应框架的官方文档或社区资源，以获取更详细的关于HMR的配置和使用说明。

#### HMR的原理

HMR（Hot Module Replacement）的原理是通过在应用程序运行时，通过开发服务器向客户端发送更新的模块代码，然后使用热更新运行时（Hot Update Runtime）来替换旧的模块代码，从而实现模块的热替换，而无需重新加载整个页面。

+ webpack-dev-server会创建两个服务：提供静态资源的服务（express）和Socket服务（net.Socket）；
  + HMR Socket Server，是一个socket的长连接：
    + 长连接有一个最好的好处是建立连接后双方可以通信（服务器可以直接发送文件到客户端）；
    + 当服务器监听到对应的模块发生变化时，会生成两个文件.json（manifest文件）和.js文件（update chunk）；
    + 通过长连接，可以直接将这两个文件主动发送给客户端（浏览器）；
    + 浏览器拿到两个新的文件后，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新；
  + Webpack-dev-server使用Node.js内置的net模块提供WebSocket服务。该服务与静态资源服务配合使用，用于与客户端进行实时通信。当客户端连接到WebSocket服务时，Webpack-dev-server会将更新的模块代码发送到客户端，并触发模块热替换（HMR）功能。客户端接收到更新后，会通过WebSocket与Webpack-dev-server建立连接，并将更新的模块代码应用到正在运行的应用程序中。
+ express server负责直接提供静态资源的服务（打包后的资源直接被浏览器请求和解析）；
  + Webpack-dev-server使用express框架提供静态资源服务。该服务可以将Webpack打包后的静态资源文件（如HTML、CSS、JavaScript等）提供给浏览器访问。同时，该服务还支持一些特殊的路由，如/__webpack_hmr，用于与客户端建立WebSocket通信。

具体而言，HMR的原理可以分为以下几个步骤：

1. 构建过程中的注入：在Webpack构建过程中，会将特殊的HMR运行时代码注入到应用程序中的每个模块中。这些HMR运行时代码负责与开发服务器建立连接，并接收来自服务器的更新通知。
2. 开发服务器的更新通知：开发服务器会监视文件的变化，并在文件发生更改时，向连接的客户端发送更新通知。这些更新通知包含了被修改的模块的更新代码。
3. 客户端接收更新：当客户端接收到更新通知时，它会根据更新代码进行处理。这些更新代码会被热更新运行时处理，并将其应用于相应的模块。
4. 模块的热替换：热更新运行时会将新的模块代码与旧的模块代码进行比较，并尽可能地将新代码应用于正在运行的应用程序。如果新代码可以被成功替换，应用程序会保持运行状态，同时显示更新后的效果。

总结起来，HMR利用了Webpack的构建能力和热更新运行时，使得在开发过程中可以实时地修改代码并查看更新后的效果，从而提高开发效率。

![image-20231119203330239](https://qiniu.waite.wang/202311192033908.png)

### devServer 配置信息

```javascript
// webpack.config.js
module.exports = {
  // target 用来指定打包后的代码在哪个环境下运行
  target: "web",
  devServer: {
    // 1. contentBase: 用来指定静态资源的根目录
    static: "./public",
    // 2. hot: 是否开启热更新
    hot: true,
    // 3. host: 指定服务器的ip地址, 默认是localhost
    host: "0.0.0.0",
    port: 7777,
    // 4. open: 是否自动打开浏览器
    open: true,
    // 5. compress: 是否启动gzip压缩
    // compress: true,
    // 6. proxy: 用来配置代理
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  }
}
```

#### hotOnly、host 配置

+ host设置主机地址：
  + 默认值是localhost；
  + 如果希望其他地方也可以访问，可以设置为 0.0.0.0；
+ localhost 和 0.0.0.0 的区别：
  + `localhost`：`localhost`是一个主机名，表示本地计算机或设备自身。它通常映射到回环地址（loopback address）``127.0.0.1`，也可以是IPv6的`::1`。当应用程序绑定到`localhost`时，它只能通过本地计算机或设备上的回环接口进行访问。这意味着只有本地计算机或设备上的进程可以访问该应用程序，其他计算机或设备无法直接访问。
  + `0.0.0.0`：`0.0.0.0`是一个特殊的IP地址，表示任意主机或所有主机。当应用程序绑定到`0.0.0.0`时，它将监听所有可用的网络接口，包括本地计算机上的回环接口和其他网络接口。这意味着其他计算机或设备可以通过网络访问该应用程序，前提是网络连接和防火墙允许。

+ 简而言之，`localhost`指的是本地计算机或设备自身，只能通过本地访问。而`0.0.0.0`表示任意主机或所有主机，可以通过网络访问。在开发过程中，通常将应用程序绑定到localhost以进行本地开发和测试，而将其绑定到`0.0.0.0`可以使其在局域网或公共网络上可访问。

#### port、open、compress

+ port设置监听的端口，默认情况下是8080
  + 这个选项用于指定Webpack开发服务器的端口号。通过设置port选项，你可以指定应用程序在开发服务器上监听的端口。
+ open是否打开浏览器：
  + 默认值是false，设置为true会打开浏览器；
  + 这个选项用于指定是否在启动Webpack开发服务器后自动打开浏览器。通过设置open选项为true，开发服务器将在启动后自动打开默认浏览器，并加载应用程序
  + 也可以设置为类似于 Google Chrome等值；
+ compress是否为静态文件开启gzip compression：
  + 默认值是false，可以设置为true；
  + 这个选项用于指定是否启用gzip压缩。通过设置compress选项为true，开发服务器将对传输到浏览器的资源进行gzip压缩，以减小文件大小，提高传输速度。

```javascript
module.exports = {
  // ...
  devServer: {
    port: 8080, // 指定端口号为8080
    open: true, // 自动打开浏览器
    compress: true, // 启用gzip压缩
  },
};
```

#### Proxy

> <https://webpack.docschina.org/configuration/dev-server#devserverproxy>

+ proxy是我们开发中非常常用的一个配置选项，它的目的设置代理来解决跨域访问的问题：
  + 比如我们的一个api请求是 <http://localhost:3000但是本地启动服务器的域名是> <http://localhost:8000，这> 个时候发送网络请求就会出现跨域的问题；
  + 那么我们可以将请求先发送到一个代理服务器，代理服务器和API服务器没有跨域的问题，就可以解决我们的跨域问题了
+ 代理（Proxy）是一种常见的网络应用程序架构，它可以将客户端的请求转发到另一个服务器进行处理。在开发环境中，我们通常会将应用程序和API服务分开部署，这时就需要使用代理将客户端的API请求转发到后端API服务器上。

+ 在 Webpack 的 devServer 中，可以使用proxy选项来配置代理设置。proxy选项可以是一个对象，也可以是一个函数。对象方式的proxy选项可以指定一个或多个代理规则，每个规则包含了要转发的请求路径和目标服务器地址。例如：

```javascript
module.exports = {
  // ...
  devServer: {
    proxy: {
      '/api': {
        target: 'http://localhost:3000', // 目标服务器地址
        changeOrigin: true, // 改变请求头中的Origin字段
        pathRewrite: {
          '^/api': '', // 将/api前缀替换为空
        },
      },
    },
  },
};
```

+ 在这个示例中，我们将所有以`/api`开头的请求转发到`http://localhost:3000`服务器上。同时，我们还设置了`changeOrigin`选项为`true`，以改变请求头中的`Origin`字段，并使用`pathRewrite`选项将请求路径中的`/api`前缀替换为空。
+ 当 `changeOrigin`设置为 true 时，代理服务器会将请求头中的 Origin 字段替换为目标服务器的地址，这样目标服务器就可以正确识别请求来源。否则，目标服务器可能会拒绝请求或返回错误的响应。
+ 除了对象方式的`proxy`选项外，还可以使用函数方式的`proxy`选项来进行更灵活的配置。例如：

```javascript
module.exports = {
  // ...
  devServer: {
    proxy: (req, res, proxyOptions) => {
      const target = 'http://localhost:3000';
      if (req.url.startsWith('/api')) {
        return {
          target,
          changeOrigin: true,
          pathRewrite: {
            '^/api': '',
          },
        };
      }
    },
  },
};
```

+ 在这个示例中，我们使用函数方式的`proxy`选项来动态配置代理规则。如果请求路径以`/api`开头，则将其转发到`http://localhost:3000`服务器上，并使用相应的选项进行配置。

+ 默认情况下，将不接受在 HTTPS 上运行且证书无效的后端服务器。 如果需要，可以这样修改配置：

```javascript
module.exports = {
  //...
  devServer: {
    proxy: {
      '/api': {
        target: 'https://other-server.example.com',
        secure: false,
      },
    },
  },
};
```

#### historyApiFallback

+ `historyApiFallback`是 `webpack-dev-server`的一个配置项，用于控制当使用 HTML5 History API 时，如果找不到对应的资源应该返回什么页面。
+ 当浏览器使用 HTML5 History API 进行前端路由跳转时，例如从`/home` 跳转到 `/about`，浏览器会向服务器发送一个 GET 请求，但是服务器上并没有对应的 `/about` 路径和资源，此时会返回 404 错误。为了避免这种情况，`historyApiFallback` 可以设置一个默认的页面，用于代替 404 错误页面。

+ 例如，设置 `historyApiFallback`: true 后，当访问一个不存在的路由时，`webpack-dev-server` 会返回一个默认的 HTML 页面，通常是 `index.html`。这个页面会包含前端路由所需的 JavaScript 和 CSS 资源，从而保证前端路由跳转的正常运行。

+ 需要注意的是，在生产环境中，historyApiFallback 应该由服务器来处理，而不是由前端框架或工具来处理。
+ <https://webpack.docschina.org/configuration/dev-server/#devserverhistoryapifallback>

```javascript
module.exports = {
  // ...其他配置项
  devServer: {
    port: 8080,
    proxy: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true
      }
    },
    historyApiFallback: true
  }
}
```

+ 要将 404 错误跳转到一个名为 `404.html`的页面，你可以通过 `historyApiFallback`的 `rewrites`选项来实现。以下是一个示例配置：

```javascript
module.exports = {
  //...
  devServer: {
    historyApiFallback: {
      rewrites: [
        { from: /^\/$/, to: '/views/landing.html' },
        { from: /^\/subpage/, to: '/views/subpage.html' },
        { from: /./, to: '/views/404.html' },
      ],
    },
  },
};
```

+ 在上述配置中，我们使用了 `rewrites`数组来定义重写规则。第一个规则 `{ from: /^\/$/, to: '/index.html' }` 将根路径 `/`重写到 `index.html` 页面，这样确保了默认路径的正确加载。第二个规则 `{ from: /./, to: '/404.html' }`将所有其他路径都重写到 `404.html` 页面，实现了将 404 错误跳转到指定页面的效果。

### resolve模块解析

> <https://webpack.docschina.org/configuration/resolve>

+ `resolve`是 `webpack` 中的一个配置选项，用于配置模块解析的规则。
  + 在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；
  + resolve可以帮助webpack从每个 require/import 语句中，找到需要引入到合适的模块代码；
  + webpack 使用 [enhanced-resolve](https://github.com/webpack/enhanced-resolve) 来解析文件路径；

1. extensions
`extensions`用于配置在导入模块时可以省略的文件扩展名。例如，配置了 `extensions: ['.js', '.jsx']` 后，当导入模块时可以省略文件扩展名，如 `import MyComponent from './MyComponent'`，`webpack` 会自动尝试解析 `MyComponent.js` 或 `MyComponent.jsx`。

2. alias
`alias`用于创建模块的别名，可以简化模块导入的路径。例如，配置了 `alias: { '@': path.resolve(__dirname, 'src') }`后，可以使用` import MyComponent from '@/components/MyComponent' `来导入位于 `src/components/MyComponent` 的模块。

3. modules
`modules`用于配置 `webpack`在解析模块时搜索的目录。默认情况下，`webpack`只会搜索 `node_modules` 目录。通过配置 `modules`，可以告诉 `webpack`在其他目录中查找模块。例如，配置了 `modules: ['src', 'node_modules']` 后，`webpack`会先在 `src`目录中查找模块，然后再在 `node_modules`目录中查找。

4. mainFields
`mainFields`用于配置在导入模块时，`webpack`优先使用的字段。当导入一个模块时，它可能在 `package.json` 文件中定义了多个入口字段（如 main, module, browser 等）。通过配置 `mainFields`，可以告诉 webpack 使用哪个字段作为模块的主入口。例如，配置了 `mainFields: ['browser', 'module', 'main']` 后，webpack 会优先使用 `browser` 字段，然后是 `module`字段，最后是 `main`字段。

```javascript
module.exports = {
  // ...其他配置项
  resolve: {
    extensions: ['.js', '.jsx'],
    alias: {
      // alias：创建了一个别名 @，指向项目根目录下的 src 目录。
      '@': path.resolve(__dirname, 'src')
    },
    modules: ['src', 'node_modules'],
    mainFields: ['browser', 'module', 'main']
  }
}
```

+ webpack能解析三种文件路径：
  + 绝对路径
    + 由于已经获得文件的绝对路径，因此不需要再做进一步解析。
  + 相对路径
    + 在这种情况下，使用 import 或 require 的资源文件所处的目录，被认为是上下文目录；
    + 在 import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；
  + 模块路径
    + 在 resolve.modules中指定的所有目录检索模块；
      + 默认值是 [‘node_modules’]，所以默认会从node_modules中查找文件；
    + 我们可以通过设置别名的方式来替换初识模块路径，具体后面讲解alias的配置；

### 区分开发/ 生产环境

+ 目前我们所有的webpack配置信息都是放到一个配置文件中的：webpack.config.js
  + 当配置越来越多时，这个文件会变得越来越不容易维护；
  + 并且某些配置是在开发环境需要使用的，某些配置是在生成环境需要使用的，当然某些配置是在开发和生成环 境都会使用的；
  + 所以，我们最好对配置进行划分，方便我们维护和管理；
+ 方案一：编写两个不同的配置文件，开发和生成时，分别加载不同的配置文件即可；

```json
// package.json
"scripts": {
  "build": "webpack --config ./config/webpack.prod.config.js",
  "serve": "webpack serve --config ./config/webpack.dev.config.js"
}
```

![image-20231119222920881](https://qiniu.waite.wang/202311192229409.png)

```javascript
const { merge } = require('webpack-merge');

const commonConfig = require('./webpack.comm.config');

module.exports = merge(commonConfig, {
  mode: "development",
  devtool: "source-map",
  devServer: {
    contentBase: "./public",
    hot: true,
    // host: "0.0.0.0",
    port: 7777,
    open: true,
    // compress: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  },
})

```

+ 方式二：使用相同的一个入口配置文件，通过设置参数来区分它们；

```javascript
const path = require('path');

module.exports = (env, argv) => {
  const isDev = argv.mode === 'development';

  return {
    mode: argv.mode,
    entry: './src/index.js',
    output: {
      filename: isDev ? 'bundle.js' : 'bundle.[contenthash].js',
      path: path.resolve(__dirname, 'dist')
    },
    devtool: isDev ? 'eval-source-map' : 'source-map',
    optimization: {
      minimize: !isDev
    },
    // ...其他配置项
  };
};
```

+ 在命令行中，可以通过 `--mode` 参数来指定 webpack 的构建模式。例如：

```less
webpack --mode development
```
