---
title: "系统学习 Vue 1"
date: 2023-01-02T21:05:54+08:00
lastmod: 2023-01-02T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue"]
author: "Waite Wang"
showToc: false
TocOpen: true
draft: false
hidemeta: false
description: "Vue 学习笔记-Vue基础"
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
## 邂逅 Vue

### 认识 Vue

#### 什么是 Vue

+ Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。
  + 全程是Vue.js或者Vuejs；
  + 什么是渐进式框架呢？表示我们可以在项目中一点点来引入和使用Vue，而不一定需要全部使用Vue来开发整个 项目；

#### Vue3带来的变化

+ 源码通过monorepo的形式来管理源代码：
  + Mono：单个
  + Repo：repository仓库
  + 主要是将许多项目的代码存储在同一个 repository 中；
  + 这样做的目的是多个包本身相互独立，可以有自己的功能逻辑、单元测试等，同时又在同一个仓库下方便管理；
  + 而且模块划分的更加清晰，可维护性、可扩展性更强；
+ 源码使用TypeScript来进行重写：
  + 在Vue2.x的时候，Vue使用 Flow 来进行类型检测；
  + 在Vue3.x的时候，Vue的源码全部使用 TypeScript 来进行重构，并且 Vue 本身对 TypeScript 支持也更好了；

##### 性能方面

+ 使用Proxy进行数据劫持
  + 在 Vue2.x 的时候，Vue2 是使用 `Object.defineProperty` 来劫持数据的 getter 和 setter 方法的；
  + 这种方式一致存在一个缺陷就是当给对象添加或者删除属性时，是无法劫持和监听的；
  + 所以在 Vue2.x 的时候，不得不提供一些特殊的API，比如 `$set` 或 `$delete` ，事实上都是一些 hack 方法，也增加了 开发者学习新的API的成本；
  + 而在 Vue3.x 开始，Vue 使用 Proxy 来实现数据的劫持
+ 删除了一些不必要的API：
  + 移除了实例上的 `$on` , `$off`  和  `$once`；
  + 移除了一些特性：如filter、内联模板等；
+ 包括编译方面的优化：
  + 生成Block Tree、Slot编译优化、diff算法优化；

##### 新的API

+ 由Options API 到 Composition API：
  + 在 Vue2.x 的时候，我们会通过 Options API 来描述组件对象；
  + Options API 包括data、props、methods、computed、生命周期等等这些选项；
  + 存在比较大的问题是多个逻辑可能是在不同的地方：
    + 比如created中会使用某一个method来修改data的数据，代码的内聚性非常差；
  + Composition API可以将 相关联的代码 放到同一处 进行处理，而不需要在多个Options之间寻找；
+ Hooks函数增加代码的复用性：
  + 在Vue2.x的时候，我们通常通过mixins在多个组件之间共享逻辑；但是有一个很大的缺陷就是 mixins也是由一大堆的Options组成的，并且多个mixins会存在命名冲突的问题；
  + 在Vue3.x中，我们可以通过Hook函数，来将一部分独立的逻辑抽取出去，并且它们还可以做到是响应式的；

### 如何使用 Vue

1. 方式一：在页面中通过CDN的方式来引入；
2. 方式二：下载Vue的JavaScript文件，并且自己手动引入；
3. 方式三：通过npm包管理工具安装使用它；
4. 方式四：直接通过Vue CLI创建项目，并且使用它；

#### CDN 引入

```html
<script src="https://unpkg.com/vue@next"></script>
```

#### 下载和引入

+ 下载Vue的源码，可以直接打开CDN的链接：
  + 打开链接，复制其中所有的代码；
  + 创建一个新的文件，比如vue.js，将代码复制到其中；

```html
<script src="../js/vue.js"></script>
```

### 声明式编程和命令式编程

+ 原生开发和Vue开发的模式和特点,我们会发现是完全不同的,这里其实涉及到两种不同的编程范式命令式编程和声明式编程
+ 命令式编程关注的是“ how to do”,声明式编程关注的是" what to do",由框架(机器)完成"how"的过程

### MVVM模型

+ MVC和MVVM都是一种软件的体系结构
  + MVC是 Model-View-Controller的简称,是在前期被使用非常框架的架构模式,比如iS、前端
  + MVVM是 Model-View- ViewMode的简称,是目前非常流行的架构模式
+ 通常情况下,我们也经常称vue是一个MVVM的框架
  + vue官方其实有说明,vue虽然并没有完全遵守MVVM的模型,但是整个设计是受到它的启发的

![](https://qiniu.waite.wang/202310161331268.png)

### template属性

+ 在使用 createApp的时候,我们传入了一个对象,接下来我们详细解析一下之前传入的属性分别代表什么含义。
  + template属性:表示的是Vue需要帮助我们渲染的模板信息
  + 目前我们看到它里面有很多的HTML标签,这些标签会替换掉我们挂载到的元素(比如id为app的dⅳv)的innerHTML
  + 模板中有一些奇怪的语法,比如{},比如@ )click,这些都是模板特有的语法
+ 但是这个模板的写法有点过于别扭了,并且IDE很有可能没有任何提示,阻碍我们编程的效率
+ vue提供了两种方式:
+ 方式一:使用 script标签,并且标记它的类型为 X-template;

```vue
<body>
   <div id="app">hhhh</div>
 
   <script type="x-template" id="why">
     <div>
       <h2>{{message}}</h2>
       <h2>{{counter}}</h2>
       <button @click='increment'>+1</button>
       <button @click='decrement'>-1</button>
     </div>
   </script>
 
   <script src="../js//Vue.js"></script>
   <script>
     Vue.createApp({
       template: '#why',
       data: function(){
         return{
           message:"Hello World",
           counter: 100
         }
       },
       methods: {
         increment(){
           this.counter++
         },
         decrement(){
           this.counter--
         }
       }
     }).mount("#app")
   </script>
 </body>
```

+ 方式二:使用任意标签(通常使用 template标签,因为不会被浏览器渲染),设置id;v template元素是一种用于保存客户端内容的机制,该内容再加载页面时不会被呈现,但随后可以在运行时使用 JavaScript 实例化

```vue
 <body>
   <div id="app"></div>
   <template id="why">
     <div>
       <h2>{{message}}</h2>
       <h2>{{counter}}</h2>
       <button @click='increment'>+1</button>
       <button @click='decrement'>-1</button>
     </div>
   </template>
 
   <script src="../js//Vue.js"></script>
   <script>
     Vue.createApp({
       template: '#why',
       data: function(){
         return{
           message:"Hello World",
           counter: 100
         }
       },
       methods: {
         increment(){
           this.counter++
         },
         decrement(){
           this.counter--
         }
       }
     }).mount("#app")
   </script>
 </body>
```

### data属性

+ data属性是传入一个函数,并且该函数需要返回一个对象
  + 在Vue2x的时候,也可以传入一个对象(虽然官方推荐是一个函数);
  + 在Vue3x的时候,必须传入一个函数,否则就会直接在浏览器中报错

+ data中返回的对象会被vue的响应式系统劫持,之后对该对象的修改或者访问都会在劫持中被处理
  + 所以我们在 template中通过{ counter} 访问 counter,可以从对象中获取到数据
  + 所以我们修改 counter的值时, template中的{ counter)也会发生改变;

### methods属性（重点）

+ methods属性是一个对象,通常我们会在这个对象中定义很多的方法
  + 这些方法可以被绑定到 template模板中;
  + 在该方法中,我们可以使用this关键字来直接访问到data中返回的对象的属性;
+ 问题：官方文档有这个描述，即不能使用箭头函数
+ 为什么不能使用箭头函数(VUE3.0)？
+ 我们在methods中要使用data返回对象中的数据：
  + 那么这个this是必须有值的，并且应该可以通过this获取到data返回对象中的数据。
+ 那么我们这个this能不能是window呢？
  + 不可以是window，因为window中我们无法获取到data返回对象中的数据；
  + 但是如果我们使用箭头函数，那么这个this就会是window了；

+ 为什么是window呢？
  + 这里涉及到箭头函数使用this的查找规则，它会在自己的上层作用于中来查找this；
  + 最终刚好找到的是script作用于中的this，所以就是window；

+ this到底是如何查找和绑定的呢？
  + [前端面试之彻底搞懂this指向](https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA)

+ **不使用箭头函数的情况下，this到底指向什么**
  + 事实上Vue的源码当中就是对methods中的所有函数进行了遍历，并且通过 bind 绑定了 this

> 具体看 <https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA>

```javascript
// window 隐式绑定
foo();
const obj = {
  bar: foo
};
obj.bar();

/*
  Window{}
  {bar: f()}
*/

const foo2 = () => {
  console.log(this);
}
const obj2 = {
  bar: foo2
};

obj2.bar();
/*
  Window{}
  Window{}
*/
```

### 编写DOM元素的模板方式

1. 方式一：template模板的方式：使用 `<template>` 标签编写模板。这种方式是之前经常使用的方式。

```html
<template>
  <div>
    <h1>{{ message }}</h1>
    <p>{{ description }}</p>
  </div>
</template>
```

2. 方式二：render函数的方式：使用h函数来编写渲染的内容。在这种方式中，h函数可以直接返回一个虚拟节点（Vnode节点）。

```javascript
export default {
  render(h) {
    return h('div', [
      h('h1', this.message),
      h('p', this.description)
    ])
  }
}
```

3. 方式三：通过.vue文件中的template来编写模板：在.vue文件中使用`<template>`标签编写模板。这种方式需要通过特定的代码来对模板进行解析：

+ 对于方式三，即.vue文件中的template，可以通过vue-loader对其进行编译和处理。
+ 对于方式一，即template模板，我们必须通过源码中的一部分代码来进行编译。

```html
<template>
  <div>
    <h1>{{ message }}</h1>
    <p>{{ description }}</p>
  </div>
</template>
```

因此，Vue在让我们选择版本时，提供了"运行时+编译器"和"仅运行时"两种选项：

+ **运行时+编译器**：这个版本包含了Vue的完整功能，包括对template模板的编译代码。在使用这个版本时，我们可以直接在代码中使用template模板，并且Vue会在运行时将其编译成渲染函数。这使得我们可以在开发过程中更方便地编写和调试模板。然而，由于包含了编译代码，这个版本的文件体积相对较大。
+ **仅运行时**：这个版本不包含对template模板的编译代码。它更小巧，文件体积更小。在使用这个版本时，我们需要使用render函数来手动编写渲染逻辑，而不是直接使用template模板。这意味着我们需要更多的代码来实现相同的功能，但也带来了更好的性能和更小的文件大小。
+ 选择哪个版本取决于项目的需求和优化目标。如果我们需要更完整的功能，并且对文件大小没有太大的担忧，可以选择运行时+编译器版本。如果我们对文件大小敏感，并且愿意手动编写渲染逻辑，可以选择仅运行时版本。

### VSCode对SFC文件的支持

> Vue 的单文件组件 (即 *.vue 文件，英文 Single-File Component，简称 SFC) 是一种特殊的文件格式，使我们能够将一个 Vue 组件的模板、逻辑与样式封装在单个文件中。下面是一个单文件组件的示例：

> 为什么要使用 SFC​
> 使用 SFC 必须使用构建工具，但作为回报带来了以下优点：
>
> + 使用熟悉的 HTML、CSS 和 JavaScript 语法编写模块化的组件
> + 让本来就强相关的关注点自然内聚
> + 预编译模板，避免运行时的编译开销
> + 组件作用域的 CSS
> + 在使用组合式 API 时语法更简单
> + 通过交叉分析模板和逻辑代码能进行更多编译时优化
> + 更好的 IDE 支持，提供自动补全和对模板中表达式的类型检查
> + 开箱即用的模块热更新 (HMR) 支持
> + SFC 是 Vue 框架提供的一个功能，并且在下列场景中都是官方推荐的项目组织方式：
>
> + 单页面应用 (SPA)
> + 静态站点生成 (SSG)
> + 任何值得引入构建步骤以获得更好的开发体验 (DX) 的项目
>
> <https://cn.vuejs.org/guide/scaling-up/sfc.html>

1. 插件一：Vetur，从Vue2开发就一直在使用的VSCode支持Vue的插件；
2. 插件二：Volar，官方推荐的插件（后续会基于Volar开发官方的VSCode插件）；

### 如何阅读Vue源码

+ 需要的环境 npm，yarn
+ 操作步骤
  1. 安装yarn npm install yarn -g
  2. 在项目中配置yarn yarn install
  3. 在package.json中的dev后加上--sourcemap
  4. 打包项目 yarn dev （在vue/dist文件夹下有两个文件，vue.global.js和vue.global.js.map）
  5. 在vue/examplex新建自己的文件夹以及测试demo
  6. 在demo中打下断点—debugger
  7. 在浏览器中打开调试面板，选择其中的source面板，查看执行对应的源码

## Vue3 基本指令

### VSCode 代码片段

+ 我们在前面练习Vue的过程中，有些代码片段是需要经常写的，我们再VSCode中我们可以生成一个代码片段，方便我们快速生成。
+ VSCode中的代码片段有固定的格式，所以我们一般会借助于一个在线工具来完成。
+ 具体的步骤如下：
  + 第一步，复制自己需要生成代码片段的代码；
  + 第二步，<https://snippet-generator.app/在该网站中生成代码片段；>
  + 第三步，在VSCode中配置代码片段；
+ 直接 Tab trigger 即可自动填充

![image-20231016233051010](https://qiniu.waite.wang/202310162330091.png)

### 模板语法

+ React的开发模式[了解]
  + React使用的jsx,所以对应的代码都是编写的类似于js的一种语法
  + 之后通过Babe将js编译成 React. create Element函数调用

```html
function () {
  return <div></div>
}
```

+ vue也支持 jsx 的开发模式:
  + 但是大多数情况下,使用基于HTML的模板语法
  + 在模板中,允许开发者以声明式的方式将 DOM 和底层组件实例的数据绑定在-起;口在底层的实现中,vue将模板编译成虚拟DOM渲染函数

```vue
<template>
  <div @click v-bind v-once> {{}} </div>
</template>
```

### Mustache 语法 双大括号语法

+ 如果我们希望把数据显示到模板（template）中，使用最多的语法是 “Mustache”语法 (双大括号) 的文本插值。
  + 并且我们前端提到过，data返回的对象是有添加到Vue的响应式系统中；
  + 当data中的数据发生改变时，对应的内容也会发生更新。
  + 当然，Mustache中不仅仅可以是data中的属性，也可以是一个JavaScript的表达式。
+ mustache的使用
     1. 基本使用
       2. 表达式
       3. 函数
       4. 三元运算符

```vue
<template id="my-app">
  <!-- 1.mustache的基本使用 -->
  <h2>{{message}} - {{message}}</h2>
  <!-- 2.是一个表达式 -->
  <h2>{{counter * 10}}</h2>
  <h2>{{ message.split(" ").reverse().join(" ") }}</h2>
  <!-- 3.也可以调用函数 -->
  <!-- 可以使用computed(计算属性) -->
  <h2>{{getReverseMessage()}}</h2>
  <!-- 4.三元运算符 -->
  <h2>{{ isShow ? "哈哈哈": "" }}</h2>
  <button @click="toggle">切换</button>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        message: "Hello World",
        counter: 100,
        isShow: true
      }
    },
    methods: {
      getReverseMessage() {
        return this.message.split(" ").reverse().join(" ");
      },
      toggle() {
        this.isShow = !this.isShow;
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

> 以下为错误写法

```html
<!-- 错误用法 -->
var name = "abc" ;

<h2>{{var name = "abc"}}</h2>
<h2>{{ if(isShow) {  return "哈哈哈" } }}</h2>
```

### 不常用指令

#### v-once指令

+ v-once用于指定元素或者组件只渲染一次
  + 当数据发生变化时,元素或者组件以及其所有的子元素将视为静态内容并且跳过;
  
  + 该指令可以用于性能优化;
  
    ```vue
    <h2 v-once>{{counter}}</h2>
    <button @click="increment">+1</button>
    ```
  
+ 如果是子节点的化，也只能渲染一次

  ```vue
  <div v-once>
       <h2>{{counter}}</h2>
       <h2>{{message}}</h2>
   </div>
   <button @click="increment">+1</button>
  ```

> 完整代码

```vue
<div id="app"></div>

<template id="my-app">
  <h2>{{counter}}</h2>
  <div v-once>
    <h2>{{counter}}</h2>
    <h2>{{message}}</h2>
  </div>
  <button @click="increment">+1</button>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        counter: 100,
        message: "abc"
      }
    },
    methods: {
      increment() {
        this.counter++;
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

#### v-html

+ 默认情况下，如果我们展示的内容本身是 html 的，那么vue并不会对其进行特殊的解析。
  + 如果我们希望这个内容被Vue可以解析出来，那么可以使用 v-html 来展示

```vue
<template id="my-app">
  <div>{{msg}}</div>
  <div v-html="msg"></div>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        msg: '<span style="color:red; background: blue;">哈哈哈</span>'
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

![image-20231018083558572](https://qiniu.waite.wang/202310180836759.png)

#### v-text

+ 用于更新元素的 textContent

```html
 <h2 v-text="message"></h2>
 <!-- 等价于 -->
 <h2>{{message}}</h2>
```

#### v-pre

+ v-pre用于跳过元素和它的子元素的编译过程，显示原始的Mustache标签：
  + 跳过不需要编译的节点，加快编译的速度

```html
 <template id="my-app">
   <h2 v-pre>{{message}}</h2>
 </template>
<!-- {{message}} -->
```

#### v-cloak

+ 用于隐藏尚未完成编译的 DOM 模板。
  + **无需传入**
  + **详细信息**
+ **该指令只在没有构建步骤的环境下需要使用。**
  + 当使用直接在 DOM 中书写的模板时，可能会出现一种叫做“未编译模板闪现”的情况：用户可能先看到的是还没编译完成的双大括号标签，直到挂载的组件将它们替换为实际渲染的内容。
  + `v-cloak` 会保留在所绑定的元素上，直到相关组件实例被挂载后才移除。配合像 `[v-cloak] { display: none }` 这样的 CSS 规则，它可以在组件编译完毕前隐藏原始模板。

```css
[v-cloak] {
  display: none;
}
```

```html
<div v-cloak>
  {{ message }}
</div>
```

>直到编译完成前，`<div>` 将不可见。

### v-bind

动态的绑定一个或多个 attribute，也可以是组件的 prop。

+ **缩写：**`:` 或者 `.` (当使用 `.prop` 修饰符)

+ **期望：**`any (带参数) | Object (不带参数)`

+ **参数：**`attrOrProp (可选的)`

+ **修饰符**

  + `.camel` - 将短横线命名的 attribute 转变为驼峰式命名。
  + `.prop` - 强制绑定为 DOM property。`3.2+`
  + `.attr` - 强制绑定为 DOM attribute。`3.2+`

+ **用途**

  当用于绑定 `class` 或 `style` attribute，`v-bind` 支持额外的值类型如数组或对象。详见下方的指南链接。

  在处理绑定时，Vue 默认会利用 `in` 操作符来检查该元素上是否定义了和绑定的 key 同名的 DOM property。如果存在同名的 property，则 Vue 会将它作为 DOM property 赋值，而不是作为 attribute 设置。这个行为在大多数情况都符合期望的绑定值类型，但是你也可以显式用 `.prop` 和 `.attr` 修饰符来强制绑定方式。有时这是必要的，特别是在和[自定义元素](https://cn.vuejs.org/guide/extras/web-components.html#passing-dom-properties)打交道时。

  当用于组件 props 绑定时，所绑定的 props 必须在子组件中已被正确声明。

  当不带参数使用时，可以用于绑定一个包含了多个 attribute 名称-绑定值对的对象。
  
+ **用法**

  + 动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

> 小知识: vue3 是允许template中有多个根元素

```html
<!-- vue2 template模板中只能有一个根元素 -->
<!-- vue3 是允许template中有多个根元素 -->
<template id="my-app">
    <!-- 1.v-bind的基本使用 -->
    <img v-bind:src="imgUrl" alt="">
    <a v-bind:href="link">百度一下</a>

    <!-- 2.v-bind提供一个语法糖 : -->
    <img :src="imgUrl" alt="">
    <img src="imgUrl" alt="">
</template>
```

#### 基本使用

```vue
<template id="my-app">
    <!-- 1.v-bind的基本使用 -->
    <img v-bind:src="imgUrl" alt="">
    <a v-bind:href="link">百度一下</a>

    <!-- 2.v-bind提供一个语法糖 : -->
    <img :src="imgUrl" alt="">
    <!-- 以下报错 -->
    <img src="imgUrl" alt="">
</template>

<script>
    const App = {
        template: '#my-app',
        data() {
            return {
                imgUrl: "https://avatars.githubusercontent.com/u/10335230?s=60&v=4",
                link: "https://www.baidu.com"
            }
        }
    }
    Vue.createApp(App).mount('#app');
</script>
```

#### 绑定 class

+ 在开发中,有时候我们的元素 class也是动态的,比如
  + 当数据为某个状态时,字体显示红色。
  + 当数据另一个状态时,字体显示黑色
+ 绑定class有两种方式：
  + 对象语法
  + 数组语法

##### 对象语法

+ 对象语法：我们可以传给 :class (v-bind:class 的简写) 一个对象，以动态地切换 class

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :class="className">哈哈哈哈</div>
    <!-- 对象语法: {'active': boolean} -->
    <div :class="{'active': isActive}">呵呵呵呵</div>
    <button @click="toggle">切换</button>

    <!-- 也可以有多个键值对 -->
    <div :class="{active: isActive, title: true}">呵呵呵呵</div>

    <!-- 默认的class和动态的class结合 -->
    <div class="abc cba" :class="{active: isActive, title: true}">
      呵呵呵呵
    </div>

    <!-- 将对象放到一个单独的属性中 -->
    <div class="abc cba" :class="classObj">呵呵呵呵</div>

    <!-- 将返回的对象放到一个methods(computed)方法中 -->
    <div class="abc cba" :class="getClassObj()">呵呵呵呵</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: "#my-app",
      data() {
        return { 
          className: "why",
          isActive: true,
          title: "abc",
          classObj: {
            active: true,
            title: true
          },
        };
      },
      methods: {
        toggle() {
          this.isActive = !this.isActive;
          this.classObj.active = !this.classObj.active;
        },
        getClassObj() {
          return this.classObj;
        }
      },
    };

    Vue.createApp(App).mount("#app");
  </script>

  <style>
    .active {
      color: red;
    }
  </style>
</body>
```

##### 数组语法

+ 绑定class – 数组语法
  + 数组语法：我们可以把一个数组传给 :class，以应用一个 class 列表

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :class="['abc', title]">哈哈哈哈</div>
   <!-- class="abc cba active" -->
    <div :class="['abc', title, isActive ? 'active': '']">哈哈哈哈</div>
 <!-- 可以嵌套对象语法 -->
    <div :class="['abc', title, {active: isActive}]">哈哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          title: "cba",
          isActive: true
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 绑定 style

+ 我们可以利用 `v-bind:style` 来绑定一些CSS内联样式
  + 这次因为某些样式我们需要根据数据动态来决定
  + 比如某段文字的颜色，大小等等
+ CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名
+ 绑定class有两种方式
  + 对象语法
  + 数组语法

> CSS property 名短横线分隔 (kebab-case，记得用引号括起来)

##### 对象语法

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- :style="{cssPropertyName: cssPropertyValue}" -->
    <div :style="{color: finalColor, 'font-size': '30px'}">哈哈哈哈</div>
    <div :style="{color: finalColor, fontSize: '30px'}">哈哈哈哈</div>
    <div :style="{color: finalColor, fontSize: finalFontSize + 'px'}">哈哈哈哈</div>

    <!-- 绑定一个data中的属性值, 并且是一个对象 -->
    <div :style="finalStyleObj">呵呵呵呵</div>
    <!-- 调用一个方法 -->
    <div :style="getFinalStyleObj()">呵呵呵呵</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          finalColor: 'red',
          finalFontSize: 50,
          finalStyleObj: {
            'font-size': '50px',
            fontWeight: 700,
            backgroundColor: 'red'
          }
        }
      },
      methods: {
        getFinalStyleObj() {
          return {
            'font-size': '50px',
            fontWeight: 700,
            backgroundColor: 'red'
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

##### 数组语法

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <div :style="[style1Obj, style2Obj]">哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          style1Obj: {
            color: 'red',
            fontSize: '30px'
          },
          style2Obj: {
            textDecoration: "underline"
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body
```

#### 动态绑定属性

+ 在某些情况下，我们属性的名称可能也不是固定的
  + 前端我们无论绑定src、href、class、style，属性名称都是固定的
  + 如果属性名称不是固定的，我们可以使用 :[属性名]=“值” 的格式来定义
  + 这种绑定的方式，我们称之为 `动态绑定属性`；如下:

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- <div cba="kobe">哈哈哈</div> -->
    <div :[name]="value">哈哈哈</div>
    <img :src="" alt="">
    <a :href=""></a>
    <div :class></div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          name: "cba",
          value: "kobe"
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 属性直接绑定一个对象

+ 如果我们希望将一个对象的所有属性，绑定到元素上的所有属性，应该怎么做呢？
  + 非常简单，我们可以直接使用 v-bind 绑定一个 对象
+ 案例：info对象会被拆解成div的各个属性

```html
<body>
  <div id="app"></div>

  <template id="my-app">
      <!-- <div name="why" age="18" height="1.88">哈哈哈哈</div> -->
    <div v-bind="info">哈哈哈哈</div>
    <div :="info">哈哈哈哈</div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

### v-on

+ v-on绑定事件

  + 前面我们绑定了元素的内容和属性，在前端开发中另外一个非常重要的特性就是交互。
  + 在前端开发中，我们需要经常和用户进行各种各样的交互
    + 这个时候，我们就必须监听用户发生的事件，比如点击、拖拽、键盘事件等等
    + 在Vue中如何监听事件呢？使用v-on指令。

+ v-on的用法

  + 缩写：@
  + 预期：Function | Inline Statement | Object
  + 参数：event p 修饰符：
    + .stop - 调用 event.stopPropagation()。
    + .prevent - 调用 event.preventDefault()。
    + .capture - 添加事件侦听器时使用 capture 模式。
    + .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
    + .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
    + .once - 只触发一次回调。
    + .left - 只当点击鼠标左键时触发。
    + .right - 只当点击鼠标右键时触发。
    + .middle - 只当点击鼠标中键时触发。
    + .passive - { passive: true } 模式添加侦听器

  + 用法：绑定事件监听

> event 事件可以参考 <https://developer.mozilla.org/en-US/docs/Web/Events>

#### 基本使用

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- 完整写法: v-on:监听的事件="methods中方法" -->
    <button v-on:click="btn1Click">按钮1</button>
    <div class="area" v-on:mousemove="mouseMove">div</div>
    <!-- 语法糖 -->
    <button @click="btn1Click">按钮1</button>
    <!-- 绑定一个表达式: inline statement -->
    <button @click="counter++">{{counter}}</button>
    <!-- 绑定一个对象 -->
    <div class="area" v-on="{click: btn1Click, mousemove: mouseMove}"></div>
    <div class="area" @="{click: btn1Click, mousemove: mouseMove}"></div>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World",
          counter: 100
        }
      },
      methods: {
        btn1Click() {
          console.log("按钮1发生了点击");
        },
        mouseMove() {
          console.log("鼠标移动");
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 参数传递

+ 当通过methods中定义方法，以供 `@click`调用时，需要注意参数问题：
+ 情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。
  + 但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去
+ 情况二：如果需要同时传入某个参数，同时需要 `event` 时，可以通过 `$event` 传入事件。

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- 默认传入event对象, 可以在方法中获取 -->
    <button @click="btn1Click">按钮1</button>
    <!-- $event可以获取到事件发生时的事件对象 -->
    <button @click="btn2Click($event, 'coderwhy', 18)">按钮2</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World"
        }
      },
      methods: {
        btn1Click(event) {
          console.log(event);
        },
        btn2Click(event, name, age) {
          console.log(name, age, event);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

![image-20231019004604619](https://qiniu.waite.wang/202310190046946.png)

#### v-on **的修饰符**

+ v-on支持修饰符，修饰符相当于对事件进行了一些特殊的处理：
  + .stop - 调用 event.stopPropagation()。
  + .prevent - 调用 event.preventDefault()。
  + .capture - 添加事件侦听器时使用 capture 模式。
  + .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  + .{keyAlias} - 仅当事件是从特定键触发时才触发回调。
  + .once - 只触发一次回调。
  + .left - 只当点击鼠标左键时触发。
  + .right - 只当点击鼠标右键时触发。
  + .middle - 只当点击鼠标中键时触发。
  + .passive - { passive: true } 模式添加侦听器

> `stopPropagation` 是一个事件修饰符，用于阻止事件冒泡。在 Vue.js 中，当一个元素上的事件被触发时，它会先执行该元素上的事件处理函数，然后再冒泡到该元素的父元素，继续执行父元素的事件处理函数。使用 `stopPropagation` 可以阻止事件继续冒泡到父元素。在给元素绑定事件时，可以使用 `@click.stop` 来阻止 `click` 事件冒泡到父元素。

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <div @click="divClick">
      <button @click="btnClick">按钮</button>
      <button @click.stop="btnClick">按钮</button>
    </div>
    <input type="text" @keyup.enter="enterKeyup">
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          message: "Hello World"
        }
      },
      methods: {
        divClick() {
          console.log("divClick");
        },
        btnClick() {
          console.log('btnClick');
        },
        enterKeyup(event) {
          console.log("keyup", event.target.value);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

![image-20231019005251770](https://qiniu.waite.wang/202310190052768.png)

### 条件渲染

+ 在某些情况下，我们需要根据当前的条件决定某些元素或组件是否渲染，这个时候我们就需要进行条件判断了。
  + Vue提供了下面的指令来进行条件判断：
    + v-if
    + v-else
    + v-else-if
    + v-show

#### 基本使用

+ v-if、v-else、v-else-if用于根据条件来渲染某一块的内容：
  + 这些内容只有在条件为true时，才会被渲染出来；
  + 这三个指令与JavaScript的条件语句if、else、else if类似；

```html
 <template id="my-app">
     <input type="text" v-model="score">
     <h2 v-if="score > 90">优秀</h2>
     <h2 v-else-if="score > 60">良好</h2>
     <h2 v-else>不及格</h2>
 </template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        score: 95
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

#### template 和  v-if  结合使用

+ v-if的渲染原理：
  + v-if是惰性的；
  + 当条件为false时，其判断的内容完全不会被渲染或者会被销毁掉；
  + 当条件为true时，才会真正渲染条件块中的内容;
+ template元素
  + 因为v-if是一个指令，所以必须将其添加到一个元素上：
    + 但是如果我们希望切换的是多个元素呢？
    + 此时我们渲染div，但是我们并不希望div这种元素被渲染；
    + 这个时候，我们可以选择使用template；
  + template元素可以当做不可见的包裹元素，并且在v-if上使用，但是最终template不会被渲染出来：
    + 有点类似于小程序中的block

```html
<body>  
  <div id="app"></div>

  <template id="my-app">
    <template v-if="isShowHa">
      <h2>哈哈哈哈</h2>
      <h2>哈哈哈哈</h2>
      <h2>哈哈哈哈</h2>
    </template>

    <template v-else>
      <h2>呵呵呵呵</h2>
      <h2>呵呵呵呵</h2>
      <h2>呵呵呵呵</h2>
    </template>

    <button @click="isShowHa = !isShowHa">切换</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          isShowHa: true
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### v-show

+ v-show和v-if的用法看起来是一致的，也是根据一个条件决定是否显示元素或者组件

```html
 <template id="my-app">
     <h2 v-show="isShow">哈哈哈哈</h2>
 </template>
```

#### v-show和v-if的区别

+ 首先，在用法上的区别：
  + v-show是不支持template；
  + v-show不可以和v-else一起使用；
+ 其次，本质的区别：
  + v-show元素无论是否需要显示到浏览器上，它的DOM实际都是有渲染的，只是通过CSS的display属性来进行 切换；
  + v-if当条件为false时，其对应的元素压根不会被渲染到DOM中；
+ 开发中如何进行选择呢？
  + 如果我们的元素需要在显示和隐藏之间频繁的切换，那么使用v-show；
  + 如果不会频繁的发生切换，那么使用v-if；

```html
<template id="my-app">
  <h2 v-if="isShow">哈哈哈哈</h2>
  <h2 v-show="isShow">呵呵呵呵</h2>
</template>
```

![image-20231019222841008](https://qiniu.waite.wang/202310192228448.png)

### 列表渲染

+ 在真实开发中，我们往往会从服务器拿到一组数据，并且需要对其进行渲染。
  + 这个时候我们可以使用v-for来完成；
  + v-for类似于JavaScript的for循环，可以用于遍历一组数据；

#### 基本使用

+ n v-for的基本格式是 "item in 数组"：
  + 数组通常是来自data或者prop，也可以是其他方式；
  + item是我们给每项元素起的一个别名，这个别名可以自定来定义；
+ 我们知道，在遍历一个数组的时候会经常需要拿到数组的索引：
  + 如果我们需要索引，可以使用格式： "(item, index) in 数组"；
  + 注意上面的顺序：数组元素项item是在前面的，索引项index是在后面的；

+ v-for支持的类型
  + v-for也支持遍历对象，并且支持有一二三个参数：
    + 一个参数： "value in object";
    + 二个参数： "(value, key) in object";
    + 三个参数： "(value, key, index) in object";
  + v-for同时也支持数字的遍历：
    + 每一个item都是一个数字；

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <h2>电影列表</h2>
    <ul>
      <!-- 遍历数组 -->
      <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <h2>个人信息</h2>
    <ul>
      <!-- 遍历对象 -->
      <li v-for="(value, key, index) in info">{{value}}-{{key}}-{{index}}</li>
    </ul>
    <h2>遍历数字</h2>
    <ul>
      <li v-for="(num, index) in 10">{{num}}-{{index}}</li>
    </ul>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          movies: [
            "星际穿越",
            "盗梦空间",
            "大话西游",
            "教父",
            "少年派"
          ],
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### template元素使用

+ 类似于v-if，你可以使用 template 元素来循环渲染一段包含多个元素的内容：
  + 我们使用template来对多个元素进行包裹，而不是使用div来完成；

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <ul>
      <template v-for="(value, key) in info">
        <li>{{key}}</li>
        <li>{{value}}</li>
        <li class="divider"></li>
      </template>
    </ul>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: {
            name: "why",
            age: 18,
            height: 1.88
          }
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 数组更新检测

+ Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：
  + push()
  + pop()
  + shift()
  + unshift()
  + splice()
  + sort()
  + reverse()
+ 替换数组的方法
  + 上面的方法会直接修改原来的数组，但是某些方法不会替换原来的数组，而是会生成新的数组，比如 filter()、 concat() 和 slice()。

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <h2>电影列表</h2>
    <ul>
      <li v-for="(movie, index) in movies">{{index+1}}.{{movie}}</li>
    </ul>
    <input type="text" v-model="newMovie">
    <button @click="addMovie">添加电影</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          newMovie: "",
          movies: [
            "星际穿越",
            "盗梦空间",
            "大话西游",
            "教父",
            "少年派"
          ]
        }
      },
      methods: {
        addMovie() {
          this.movies.push(this.newMovie);
          this.newMovie = "";

          // filter 是过滤的意思, 下文中的代码的意思是: 过滤掉长度小于等于2的电影
          // this.movies = this.movies.filter(item => item.length > 2);
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### v-for 中的 key 是什么作用？

> <https://cn.vuejs.org/api/built-in-special-attributes.html#key>

+ 在使用v-for进行列表渲染时，我们通常会给元素或者组件绑定一个key属性。

+ 这个key属性有什么作用呢？我们先来看一下官方的解释：

  + key属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes；

  + 如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法；

  + 而使用key时，它会基于key的变化重新排列元素顺序，并且会移除/销毁key不存在的元素；

+ 官方的解释对于初学者来说并不好理解，比如下面的问题：
  + 什么是新旧nodes，什么是VNode？
  + 没有key的时候，如何尝试修改和复用的？
  + 有key的时候，如何基于key重新排列的？

##### 认识 VNode

+ 我们先来解释一下VNode的概念：
  + 因为目前我们还没有比较完整的学习组件的概念，所以目前我们先理解HTML元素创建出来的VNode；
  + VNode的全称是Virtual Node，也就是虚拟节点；
  + 事实上，无论是组件还是元素，它们最终在Vue中表示出来的都是一个个VNode；
  + VNode的本质是一个JavaScript的对象；可以用于描述某一个标签/ 元素 的样子
  + 好处: 多平台的渲染, 跨平台(主要好处)

![image-20231020162812819](https://qiniu.waite.wang/202310201628396.png)

##### 虚拟 DOM

+ 如果我们不只是一个简单的div，而是有一大堆的元素，那么它们应该会形成一个 VNode Tree
+ 虚拟 DOM 与 真实 DOM 不一定一一对应

![image-20231020163154944](https://qiniu.waite.wang/202310201631261.png)

##### 插入 F 的案例

+ 我们先来看一个案例：这个案例是当我点击按钮时会在中间插入一个f；

+ 我们可以确定的是，这次更新对于ul和button是不需要进行更新，需要更新的是我们 li 的列表：

  + 在Vue中，对于相同父元素的子元素节点并不会重新渲染整个列 表；

  + 因为对于列表中 a、b、c、d它们都是没有变化的；

  + 在操作真实DOM的时候，我们只需要在中间插入一个 f 的 li 即可；

```vue
<body>
  <div id="app"></div>

  <template id="my-app">
    <ul>
      <li v-for="item in letters" :key="item">{{item}}</li>
    </ul>
    <button @click="insertF">插入F元素</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          letters: ['a', 'b', 'c', 'd']
        }
      },
      methods: {
        insertF() {
          this.letters.splice(2, 0, 'f')
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

+ 那么Vue中对于列表的更新究竟是如何操作的呢？
  + Vue事实上会对于有key和没有key会调用两个不同的方法；

###### Vue源码对于key的判断

+ 有key，那么就使用 patchKeyedChildren方法；
+ 没有key，那么久使用 patchUnkeyedChildren方法；

![image-20231020164733530](https://qiniu.waite.wang/202310201647317.png)

> diff算法: diff 算法是指生成更新补丁的方式,主要应用于虚拟 DOM 树变化后,更新真实 DOM。所以 diff 算法一定存在这样一个过程:触发更新 → 生成补丁 → 应用补丁。

+ 没有key的操作过程

![image-20231022182341619](https://qiniu.waite.wang/202310221823690.png)

+ 我们会发现上面的diff算法效率并不高：
  + c和d来说它们事实上并不需要有任何的改动；
  + 但是因为我们的c被f所使用了，所有后续所有的内容都要一次进行改动，并且最后进行新增；

![image-20231022182012027](https://qiniu.waite.wang/202310221820519.png)

+ 有key的操作过程-diff算法
  + 第一步的操作是从头开始进行遍历、比较：
    + a和b是一致的会继续进行比较；
    + c和f因为key不一致，所以就会break跳出循环
  + 第二步的操作是从尾部开始进行遍历、比较
  + 第三步是如果旧节点遍历完毕，但是依然有新的节点，那么就新增节点：
  + 第四步是如果新的节点遍历完毕，但是依然有旧的节点，那么就移除旧节点：
  + 第五步是最特色的情况，中间还有很多未知的或者乱序的节点：

![image-20231022182232738](https://qiniu.waite.wang/202310221822567.png)

![image-20231022182140133](https://qiniu.waite.wang/202310221821946.png)

![image-20231022182241976](https://qiniu.waite.wang/202310221822262.png)

![image-20231022182251535](https://qiniu.waite.wang/202310221822079.png)

![image-20231022182300781](https://qiniu.waite.wang/202310221823627.png)

###### 有无key的结论

+ 有无key的结论
  + 所以我们可以发现，Vue在进行diff算法的时候，会尽量利用我们的key来进行优化操作：
    + 在没有key的时候我们的效率是非常低效的；
    + 在进行插入或者重置顺序的时候，保持相同的key可以让diff算法更加的高效；

## Vue 的 Options API

### Computed

#### 认识计算属性

我们知道，在模板中可以直接通过插值语法显示一些data中的数据。

但是在某些情况，我们可能需要对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示；

+ 比如我们需要对多个data数据进行运算、三元运算符来决定结果、数据进行某种转化后显示；
+ 在模板中使用表达式，可以非常方便的实现，但是设计它们的初衷是用于简单的运算；
+ 在模板中放入太多的逻辑会让模板过重和难以维护；
+ 并且如果多个地方都使用到，那么会有大量重复的代码；

我们有没有什么方法可以将逻辑抽离出去呢？

+ 可以，其中一种方式就是将逻辑抽取到一个method中，放到methods的options中；
+ 但是，这种做法有一个直观的弊端，就是所有的data使用过程都会变成了一个方法的调用；
+ 另外一种方式就是使用计算属性computed；

什么是计算属性呢？

+ <https://cn.vuejs.org/api/reactivity-core.html#computed>
+ 官方并没有给出直接的概念解释；
+ 而是说：对于任何包含响应式数据的复杂逻辑，你都应该使用**计算属性**；
+ 计算属性将被混入到组件实例中。所有 getter 和 setter 的 `this` 上下文自动地绑定为组件实例；

#### 基本使用

计算属性的用法：

+ **选项：** computed
+ **类型：**`{ [key: string]: Function | { get: Function, set: Function } }`

我们来看三个案例：

+ 我们有两个变量：firstName 和 lastName，希望它们拼接之后在界面上显示；
+ 我们有一个分数：score
  + 当score大于60的时候，在界面上显示及格；
  + 当score小于60的时候，在界面上显示不及格；
+ 我们有一个变量message，记录一段文字：比如Hello World
  + 某些情况下我们是直接显示这段文字；
  + 某些情况下我们需要对这段文字进行反转；
+ 我们可以有三种实现思路：
  + 思路一：在模板语法中直接使用表达式；
  + 思路二：使用method对逻辑进行抽取；
  + 思路三：使用计算属性computed；

##### 在模板语法中直接使用表达式

+ 缺点一：模板中存在大量的复杂逻辑，不便于维护（模板中表达式的初衷是用于简单的计算）；
+ 缺点二：当有多次一样的逻辑时，存在重复的代码；
+ 缺点三：多次使用的时候，很多运算也需要多次执行，没有缓存；

```html
<template id="my-app">
    <!-- 1.实现思路一: -->
    <h2>{{ firstName + lastName }}</h2>
    <h2>{{ score >= 60 ? "及格": "不及格" }}</h2>
    <h2>{{ message.split("").reverse().join(" ") }}</h2>
</template>
```

##### 使用method对逻辑进行抽取

+ 缺点一：我们事实上先显示的是一个结果，但是都变成了一种方法的调用；
+ 缺点二：多次使用方法的时候，没有缓存，也需要多次计算；

```html
<template id="my-app">
  <h2>{{getFullName()}}</h2>
  <h2>{{getResult()}}</h2>
  <h2>{{getReverseMessage()}}</h2>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        firstName: "Kobe",
        lastName: "Bryant",
        score: 80,
        message: "Hello World"
      }
    },
    methods: {
      getFullName() {
        return this.firstName + " " + this.lastName;
      },
      getResult() {
        return this.score >= 60 ? "及格": "不及格";
      },
      getReverseMessage() {
        return this.message.split(" ").reverse().join(" ");
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

##### computed 实现

+ 注意：计算属性看起来像是一个函数，但是我们在使用的时候不需要加()，这个后面讲setter和getter时会讲到；
+ 我们会发现无论是直观上，还是效果上计算属性都是更好的选择；
+ 并且计算属性是有缓存的；

```html
<template id="my-app">
  <h2>{{fullName}}</h2>
  <h2>{{result}}</h2>
  <h2>{{reverseMessage}}</h2>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        firstName: "Kobe",
        lastName: "Bryant",
        score: 80,
        message: "Hello World"
      }
    },
    computed: {
      // 定义了一个计算属性叫fullname
      fullName() {
        return this.firstName + " " + this.lastName;
      },
      result() {
        return this.score >= 60 ? "及格": "不及格";
      },
      reverseMessage() {
        return this.message.split(" ").reverse().join(" ");
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

#### 计算属性 vs methods

在上面的实现思路中，我们会发现计算属性和methods的实现看起来是差别是不大的，而且我们多次提到计算属性有缓存。

接下来我们来看一下同一个计算多次使用，计算属性和methods的差异：

```html
<template id="my-app">
  <button @click="changeFirstName">修改firstName</button>

  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>

  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        firstName: "Kobe",
        lastName: "Bryant"
      }
    },
    computed: {
      // 计算属性是有缓存的, 当我们多次使用计算属性时, 计算属性中的运算只会执行一次.
      // 计算属性会随着依赖的数据(firstName)的改变, 而进行重新计算.
      fullName() {
        console.log("computed的fullName中的计算");
        return this.firstName + " " + this.lastName;
      }
    },
    methods: {
      getFullName() {
        console.log("methods的getFullName中的计算");
        return this.firstName + " " + this.lastName;
      },
      changeFirstName() {
        this.firstName = "Coder"
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

> 打印结果如下：
>
> + 我们会发现methods在多次使用时，会调用多次；
> + 而计算属性虽然使用了多次，但是计算的过程只调用了一次；
> + 这是因为计算属性会基于它们的依赖关系进行缓存；
> + 在数据不发生变化时，计算属性是不需要重新计算的；
> + 但是如果依赖的数据发生变化，在使用时，计算属性依然会重新进行计算；如下:

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    
    <input type="text" v-model="score">
    <!-- 1.使用methods -->
    <h2>{{getResult()}}</h2>
    <h2>{{getResult()}}</h2>
    <h2>{{getResult()}}</h2>

    <!-- 2.使用computed -->
    <h2>{{result}}</h2>
    <h2>{{result}}</h2>
    <h2>{{result}}</h2>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          score: 90
        }
      },
      computed: {
        result() {
          console.log("调用了计算属性result的getter");
          return this.score >= 60 ? "及格": "不及格";
        }
      },
      methods: {
        getResult() {
          console.log("调用了getResult方法");
          return this.score >= 60 ? "及格": "不及格";
        }
      }
    }
    
    Vue.createApp(App).mount('#app');
  </script>
</body>
```

> 当 score 变化, console 输出如下:

```
调用了getResult方法
调用了getResult方法
调用了getResult方法
调用了计算属性result的getter
```

#### 计算属性的 setter 和 getter

计算属性在大多数情况下，只需要一个getter方法即可，所以我们会将计算属性直接写成一个函数。

但是，如果我们确实想设置计算属性的值呢？

+ 这个时候我们也可以给计算属性设置一个setter的方法；

```html
<template id="my-app">
  <button @click="changeFullName">修改fullName</button>
  <h2>{{fullName}}</h2>
</template>

<script src="../js/vue.js"></script>
<script>
  const App = {
    template: '#my-app',
    data() {
      return {
        firstName: "Kobe",
        lastName: "Bryant"
      }
    },
    computed: {

      // fullName 的 getter方法
      fullName() {
        return this.firstName + " " + this.lastName;
      },
      
      // fullName的getter和setter方法
      fullName: {
        get: function() {
          return this.firstName + " " + this.lastName;
        },
        set: function(newValue) {
          console.log(newValue);
          const names = newValue.split(" ");
          this.firstName = names[0];
          this.lastName = names[1];
        }
      }
    },
    methods: {
      changeFullName() {
        this.fullName = "Coder Why";
      }
    }
  }

  Vue.createApp(App).mount('#app');
</script>
```

> 以下为内部判断

![图片](https://qiniu.waite.wang/202310251728011.jpeg)

### 侦听器 watch

> 用于声明在数据更改时调用的侦听回调。`watch` 选项期望接受一个对象，其中键是需要侦听的响应式组件实例属性 (例如，通过 `data` 或 `computed` 声明的属性)——值是相应的回调函数。该回调函数接受被侦听源的新值和旧值。

+ 什么是侦听器？
  + 开发中我们在data返回的对象中定义了数据，这个数据通过插值语法等方式绑定到template中;
  + 当数据变化时，template会自动进行更新来显示最新的数据;
  + 但是在某些情况下，我们希望在代码逻辑中监听某个数据的变化，这个时候就需要用侦听器watch来完成了;
+ 用法如下：
  + 选项：watch
  + 类型: `{[key: string]: string | Function | Object | Array}`

#### 简单案例

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    您的问题: <input type="text" v-model="question">
    <button @click="queryAnswer">查找答案</button>

    <p>您的问题是: {{ question }}</p>
    <p>答案是: {{ anwser }}</p>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          // 侦听question的变化时, 去进行一些逻辑的处理(JavaScript, 网络请求)
          question: "Hello World",
          anwser: ""
        }
      },
      watch: {
        // question侦听的data中的属性的名称
        // newValue变化后的新值
        // oldValue变化前的旧值
        question: function(newValue, oldValue) {
          console.log("新值: ", newValue, "旧值", oldValue);
          this.queryAnswer();
        }
      },
      methods: {
        queryAnswer() {
          // console.log(`你的问题${this.question}的答案是哈哈哈哈哈`);
          this.anwser = `你的问题${this.question}的答案是哈哈哈哈哈`;
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

#### 配置选项

+ `watch` 默认是浅层的：被侦听的属性，仅在被赋新值时，才会触发回调函数——而嵌套属性的变化不会触发。如果想侦听所有嵌套的变更，你需要深层侦听器：
+ 以下为不使用深度监听, 当 info.name 在方法中被赋值改变时, 页面会改变, 但是watch不会侦听到, 理由如上

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <h2>{{info.name}}</h2>
    <button @click="changeInfo">改变info</button>
    <!-- 页面会改变, 但是watch不会侦听到 -->
    <button @click="changeInfoName">改变info.name</button>
    <button @click="changeInfoNbaName">改变info.nba.name</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: { name: "why", age: 18, nba: {name: 'kobe'} }
        }
      },
      watch: {
        // 默认情况下我们的侦听器只会针对监听的数据本身的改变(内部发生的改变是不能侦听)
        info(newInfo, oldInfo) {
          console.log("newValue:", newInfo, "oldValue:", oldInfo);
        }
      },
      methods: {
        changeInfo() {
          this.info = {name: "kobe"};
        },
        changeInfoName() {
          this.info.name = "kobe";
        },
        changeInfoNbaName() {
          this.info.nba.name = "james";
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

+ 将 watch 中更改如下, 不管多深都会侦听到

```javascript
watch: {
  // 深度侦听/立即执行(一定会执行一次)
  info: {
    handler: function(newInfo, oldInfo) {
      console.log("newValue:", newInfo, "oldValue:", oldInfo);
    },
    deep: true, // 深度侦听
    // immediate: true // 立即执行
  }
}
```

+ `immediate: true` 立即执行,  这个时候无论后面数据是否有变化，侦听的函数都会有限执行一次；即当刷新页面时会立刻执行一次, 回调函数的初次执行就发生在 `created` 钩子之前。Vue 此时已经处理了 `data`、`computed` 和 `methods` 选项，所以这些属性在第一次调用时就是可用的。

```
newValue: Proxy(Object) {name: 'why', age: 18, nba: {…}} oldValue: undefined
```

> 注意: 引用对象 or watch 不能侦听到旧值, 可以利用 计算属性 缓存旧值, 或者自己深拷贝一份作为保存

#### 其他方式

> <https://cn.vuejs.org/api/options-state.html#watch>

```javascript
export default {
  data() {
    return {
      a: 1,
      b: 2,
      c: {
        d: 4
      },
      e: 5,
      f: 6
    }
  },
  watch: {
    // 侦听根级属性
    a(val, oldVal) {
      console.log(`new: ${val}, old: ${oldVal}`)
    },
    // 字符串方法名称
    b: 'someMethod',
    // 该回调将会在被侦听的对象的属性改变时调动，无论其被嵌套多深
    c: {
      handler(val, oldVal) {
        console.log('c changed')
      },
      deep: true
    },
    // 侦听单个嵌套属性：
    'c.d': function (val, oldVal) {
      // do something
    },
    // 该回调将会在侦听开始之后立即调用
    e: {
      handler(val, oldVal) {
        console.log('e changed')
      },
      immediate: true
    },
    // 你可以传入回调数组，它们将会被逐一调用
    f: [
      'handle1',
      function handle2(val, oldVal) {
        console.log('handle2 triggered')
      },
      {
        handler: function handle3(val, oldVal) {
          console.log('handle3 triggered')
        }
        /* ... */
      }
    ]
  },
  methods: {
    someMethod() {
      console.log('b changed')
    },
    handle1() {
      console.log('handle 1 triggered')
    }
  },
  created() {
    this.a = 3 // => new: 3, old: 1
  }
}
```

##### $watch 的API

+ <https://cn.vuejs.org/api/component-instance.html#watch>

+ 我们可以在created的生命周期（后续会讲到）中，使用 this.$watchs 来侦听；
+ + 第一个参数是要侦听的源；
  + 第二个参数是侦听的回调函数callback；
  + 第三个参数是额外的其他选项，比如deep、immediate；

```javascript
created() {
  const unwatch = this.$watch("info", function(newInfo, oldInfo) {
    console.log(newInfo, oldInfo);
  }, {
    deep: true,
    immediate: true
  })
  // unwatch()
}
```

### 阶段案例

> 现在我们来做一个相对综合一点的练习：书籍购物车

+ css

```css
table {
  border: 1px solid #e9e9e9;
  border-collapse: collapse;
  border-spacing: 0;
}

th, td {
  padding: 8px 16px;
  border: 1px solid #e9e9e9;
  text-align: left;
}

th {
  background-color: #f7f7f7;
  color: #5c6b77;
  font-weight: 600;
}

.counter {
  margin: 0 5px;
}
```

+ index

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div id="app"></div>

    <template id="my-app">
      <template v-if="books.length > 0">
        <table>
          <thead>
            <th>序号</th>
            <th>书籍名称</th>
            <th>出版日期</th>
            <th>价格</th>
            <th>购买数量</th>
            <th>操作</th>
          </thead>
          <tbody>
            <tr v-for="(book, index) in books">
              <td>{{index + 1}}</td>
              <td>{{book.name}}</td>
              <td>{{book.date}}</td>
              <td>{{formatPrice(book.price)}}</td>
              <td>
                <button :disabled="book.count <= 1" @click="decrement(index)">
                  -
                </button>
                <span class="counter">{{book.count}}</span>
                <button @click="increment(index)">+</button>
              </td>
              <td>
                <button @click="removeBook(index)">移除</button>
              </td>
            </tr>
          </tbody>
        </table>
        <h2>总价格: {{formatPrice(totalPrice)}}</h2>
      </template>
      <template v-else>
        <h2>购物车为空~</h2>
      </template>
    </template>

    <script src="../js/vue.js"></script>
    <script src="./index.js"></script>

    <script>
      Vue.createApp({
        template: "#my-app",
        data() {
          return {
            books: [
              {
                id: 1,
                name: "《算法导论》",
                date: "2006-9",
                price: 85.0,
                count: 1,
              },
              {
                id: 2,
                name: "《UNIX编程艺术》",
                date: "2006-2",
                price: 59.0,
                count: 1,
              },
              {
                id: 3,
                name: "《编程珠玑》",
                date: "2008-10",
                price: 39.0,
                count: 1,
              },
              {
                id: 4,
                name: "《代码大全》",
                date: "2006-3",
                price: 128.0,
                count: 1,
              },
            ],
          };
        },
        computed: {
          totalPrice() {
            let finalPrice = 0;
            for (let book of this.books) {
              finalPrice += book.count * book.price;
            }
            return finalPrice;
          },
          // Vue3不支持过滤器了, 推荐两种做法: 使用计算属性/使用全局的方法
          filterBooks() {
            return this.books.map((item) => {
              const newItem = Object.assign({}, item);
              newItem.price = "¥" + item.price;
              return newItem;
            });
          },
        },
        methods: {
          increment(index) {
            // 通过索引值获取到对象
            this.books[index].count++;
          },
          decrement(index) {
            this.books[index].count--;
          },
          removeBook(index) {
            this.books.splice(index, 1);
          },
          formatPrice(price) {
            return "¥" + price;
          },
        },
      }).mount("#app");
    </script>
  </body>
</html>
```

### v-model

+ `v-model` 可以在组件上使用以实现双向绑定。

+ 首先让我们回忆一下 `v-model` 在原生元素上的用法：

  template

  ```javascript
  <input v-model="searchText" />
  ```

+ 在代码背后，模板编译器会对 `v-model` 进行更冗长的等价展开。因此上面的代码其实等价于下面这段：

  ```javascript
  <input
    :value="searchText"
    @input="searchText = $event.target.value"
  />
  ```

+ 而当使用在一个组件上时，`v-model` 会被展开为如下的形式：

  ```javascript
  <CustomInput
    :model-value="searchText"
    @update:model-value="newValue => searchText = newValue"
  />
  ```

+ 要让这个例子实际工作起来，`<CustomInput>` 组件内部需要做两件事：

  1. 将内部原生 `<input>` 元素的 `value` attribute 绑定到 `modelValue` prop
  2. 当原生的 `input` 事件触发时，触发一个携带了新值的 `update:modelValue` 自定义事件

#### 内部实现

![图片](https://qiniu.waite.wang/202310311437450.jpeg)

#### 绑定其他表单

> 具体可以看: <https://cn.vuejs.org/guide/essentials/forms.html#modifiers>

> 在 HTML 中，`<label>` 标签的 `for` 属性被用来关联 `<label>` 标签和表单控件（如 `<input>`、`<textarea>`、`<select>` 等）。`for` 属性的值应该是你想要关联的表单控件的 `id`。 当 `<label>` 被点击时，与其关联的表单控件会获得焦点。

```html
<body>
  <div id="app"></div>

  <template id="my-app">
    <!-- 1.绑定textarea -->
    <label for="intro">
      自我介绍
      <textarea name="intro" id="intro" cols="30" rows="10" v-model="intro"></textarea>
    </label>
    <h2>intro: {{intro}}</h2>

    <!-- 2.checkbox -->
    <!-- 2.1.单选框 -->
    <label for="agree">
      <input id="agree" type="checkbox" v-model="isAgree"> 同意协议
    </label>
    <h2>isAgree: {{isAgree}}</h2>

    <!-- 2.2.多选框 -->
    <span>你的爱好: </span>
    <label for="basketball">
      <input id="basketball" type="checkbox" v-model="hobbies" value="basketball"> 篮球
    </label>
    <label for="football">
      <input id="football" type="checkbox" v-model="hobbies" value="football"> 足球
    </label>
    <label for="tennis">
      <input id="tennis" type="checkbox" v-model="hobbies" value="tennis"> 网球
    </label>
    <h2>hobbies: {{hobbies}}</h2>

    <!-- 3.radio -->
    <span>你的爱好: </span>
    <label for="male">
      <input id="male" type="radio" v-model="gender" value="male">男
    </label>
    <label for="female">
      <input id="female" type="radio" v-model="gender" value="female">女
    </label>
    <h2>gender: {{gender}}</h2>

    <!-- 4.select -->
    <span>喜欢的水果: </span>
    <select v-model="fruit" multiple size="2">
      <option value="apple">苹果</option>
      <option value="orange">橘子</option>
      <option value="banana">香蕉</option>
    </select>
    <h2>fruit: {{fruit}}</h2>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          intro: "Hello World",
          isAgree: false,
          hobbies: ["basketball"],
          gender: "",
          fruit: "orange"
        }
      },
      methods: {
        commitForm() {
          axios
        }
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
```

![image-20231031200224648](https://qiniu.waite.wang/202310312002330.png)

目前我们在前面的案例中大部分的值都是在template中固定好的：

+ 比如gender的两个输入框值male、female；
+ 比如hobbies的三个输入框值basketball、football、tennis；

在真实开发中，我们的数据可能是来自服务器的，那么我们就可以先将值请求下来，绑定到data返回的对象中，再通过v-bind来进行值的绑定，这个过程就是值绑定。

#### 修饰符

##### 内置修饰符

###### `.lazy`

默认情况下，`v-model` 会在每次 `input` 事件后更新数据 ([IME 拼字阶段的状态](https://cn.vuejs.org/guide/essentials/forms.html#vmodel-ime-tip)例外)。你可以添加 `lazy` 修饰符来改为在每次 `change` 事件后更新数据：

```html
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />
```

###### [`.number`](https://cn.vuejs.org/guide/essentials/forms.html#number)

如果你想让用户输入自动转换为数字，你可以在 `v-model` 后添加 `.number` 修饰符来管理输入：

另外，在我们进行逻辑判断时，如果是一个string类型，在可以转化的情况下会进行隐式转换的：

+ 下面的score在进行判断的过程中会进行隐式转化的；

```javascript
const score = "100";if (score > 90) {  console.log("优秀");}
```

```html
<input v-model.number="age" />
```

如果该值无法被 `parseFloat()` 处理，那么将返回原始值。

`number` 修饰符会在输入框有 `type="number"` 时自动启用。

###### [`.trim`](https://cn.vuejs.org/guide/essentials/forms.html#trim)

如果你想要默认自动去除用户输入内容中两端的空格，你可以在 `v-model` 后添加 `.trim` 修饰符：

```html
<input v-model.trim="msg" />
```

##### 自定义的修饰符

在某些场景下，你可能想要一个自定义组件的 `v-model` 支持自定义的修饰符。

我们来创建一个自定义的修饰符 `capitalize`，它会自动将 `v-model` 绑定输入的字符串值第一个字母转为大写：

```html
<MyComponent v-model.capitalize="myText" />
```

组件的 `v-model` 上所添加的修饰符，可以通过 `modelModifiers` prop 在组件内访问到。在下面的组件中，我们声明了 `modelModifiers` 这个 prop，它的默认值是一个空对象：

```vue
<script>
export default {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:modelValue'],
  created() {
    console.log(this.modelModifiers) // { capitalize: true }
  }
}
</script>

<template>
  <input
    type="text"
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```

注意这里组件的 `modelModifiers` prop 包含了 `capitalize` 且其值为 `true`，因为它在模板中的 `v-model` 绑定 `v-model.capitalize="myText"` 上被使用了。

有了这个 prop，我们就可以检查 `modelModifiers` 对象的键，并编写一个处理函数来改变抛出的值。在下面的代码里，我们就是在每次 `<input />` 元素触发 `input` 事件时将值的首字母大写：

```vue
<script>
export default {
  props: {
    modelValue: String,
    modelModifiers: {
      default: () => ({})
    }
  },
  emits: ['update:modelValue'],
  methods: {
    emitValue(e) {
      let value = e.target.value
      if (this.modelModifiers.capitalize) {
        value = value.charAt(0).toUpperCase() + value.slice(1)
      }
      this.$emit('update:modelValue', value)
    }
  }
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>
```

#### 多个 `v-model` 绑定

我们可以在单个组件实例上创建多个 `v-model` 双向绑定。

组件上的每一个 `v-model` 都会同步不同的 prop，而无需额外的选项：

```html
<UserName
  v-model:first-name="first"
  v-model:last-name="last"
/>
```

```html
<script>
export default {
  props: {
    firstName: String,
    lastName: String
  },
  emits: ['update:firstName', 'update:lastName']
}
</script>

<template>
  <input
    type="text"
    :value="firstName"
    @input="$emit('update:firstName', $event.target.value)"
  />
  <input
    type="text"
    :value="lastName"
    @input="$emit('update:lastName', $event.target.value)"
  />
</template>
```

## 生命周期

### 什么是生命周期?

什么是生命周期呢？

+ 每个组件都可能会经历从创建、挂载、更新、卸载等一系列的过程；
+ 在这个过程中的某一个阶段，用于可能会想要添加一些属于自己的代码逻辑（比如组件创建完后就请求一些服 务器数据）；

生命周期函数：

+ 生命周期函数是一些钩子函数，在某个时间会被Vue源码内部进行回调；
+ 通过对生命周期函数的回调，我们可以知道目前组件正在经历什么阶段；
+ 那么我们就可以在该生命周期中编写属于自己的逻辑代码了；

#### [注册周期钩子](https://cn.vuejs.org/guide/essentials/lifecycle.html#registering-lifecycle-hooks)

举例来说，`mounted` 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码：

```javascript
export default {
  mounted() {
    console.log(`the component is now mounted.`)
  }
}
```

还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 [`mounted`](https://cn.vuejs.org/api/options-lifecycle.html#mounted)、[`updated`](https://cn.vuejs.org/api/options-lifecycle.html#updated) 和 [`unmounted`](https://cn.vuejs.org/api/options-lifecycle.html#unmounted)。

所有生命周期钩子函数的 `this` 上下文都会自动指向当前调用它的组件实例。注意：避免用箭头函数来定义生命周期钩子，因为如果这样的话你将无法在函数中通过 `this` 获取组件实例。

### 组件的生命周期

> <https://cn.vuejs.org/api/options-lifecycle.html#options-lifecycle>

1. `beforeCreate( )`——准备创建
2. `created( )`——创建完成
3. `beforeMount( )`—挂载之前
4. `mounted( )`——挂载完成
5. `beforeUpdate( )`——更新之前
6. `updated( )`——更新完成
7. `activated( )`——当组件在 keep-alive 内被切换的时候它的 monnted( ) 被取代为activated
8. `deactivated( )`——当组件在 keep-alive 内被切换的时候它的 unmonnted( ) 被取代为deactivated
9. `beforeUnmount( )`—卸载之前
10. `unmounted( )`——卸载完成
11. `errorCaptured`——返回子孙组件中的错误
12. `renderTracked`——虚拟 DOM 重新渲染时调用。接收 `debugger event` 作为参数。告诉你哪个操作跟踪了组件以及该操作的目标对象和键。
13. `renderTiggered`——虚拟 DOM 重新渲染被触发时调用。接收 `debugger event` 作为参数。告诉你是什么操作触发了重新渲染，以及该操作的目标对象和键

![在这里插入图片描述](https://qiniu.waite.wang/202312082224668.png)

```vue
<template>
  <div>
    <h2 ref="title">{{message}}</h2>
    <button @click="changeMessage">修改message</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: "Hello Home"
      }
    },
    methods: {
      changeMessage() {
        this.message = "你好啊, 李银河"
      }
    },
    beforeCreate() {
      console.log("home beforeCreate");
    },
    created() {
      console.log("home created");
    },
    beforeMount() {
      console.log("home beforeMount");
    },
    mounted() {
      console.log("home mounted");
    },
    beforeUnmount() {
      console.log("home beforeUnmount");
    },
    unmounted() {
      console.log("home unmounted");
    },
    beforeUpdate() {
      console.log(this.$refs.title.innerHTML);
      console.log("home beforeUpdate");
    },
    updated() {
      console.log(this.$refs.title.innerHTML);
      console.log("home updated");
    }
  }
</script>
```

## Mixins，Extends以及组合式函数

### 认识 Mixin

> 在 Vue 2 中，mixins 是创建可重用组件逻辑的主要方式。尽管在 Vue 3 中保留了 mixins 支持，但对于组件间的逻辑复用，[使用组合式 API 的组合式函数](https://cn.vuejs.org/guide/reusability/composables.html)是现在更推荐的方式。

+ 目前我们是使用组件化的方式在开发整个Vue的应用程序，但是组件和组件之间有时候会存在相同的代码逻辑，我们希望对相同的代码逻辑进行抽取。
+ 在Vue2和Vue3中都支持的一种方式就是使用Mixin来完成：
  + Mixin提供了一种非常灵活的方式，来分发Vue组件中的可复用功能；
  + 一个Mixin对象可以包含任何组件选项；
  + 当组件使用Mixin对象时，所有Mixin对象的选项将被 混合 进入该组件本身的选项中；

#### 基本使用

`mixins` 选项接受一个 mixin 对象数组。这些 mixin 对象可以像普通的实例对象一样包含实例选项，它们将使用一定的选项合并逻辑与最终的选项进行合并。举例来说，如果你的 mixin 包含了一个 `created` 钩子，而组件自身也有一个，那么这两个函数都会被调用。

Mixin 钩子的调用顺序与提供它们的选项顺序相同，且会在组件自身的钩子前被调用。

```typescript
interface ComponentOptions {
  mixins?: ComponentOptions[]
}
```

```vue
<!-- App.vue -->
<template>
  <div>
    <h2>{{message}}</h2>
    <button @click="foo">按钮</button>
  </div>
</template>

<script>
  import { demoMixin } from './mixins/demoMixin';

  export default {
    mixins: [demoMixin],
    data() {
      return {
        title: "Hello World"
      }
    },
    methods: {

    }
  }
</script>

<style scoped>

</style>
```

```javascript
// ./mixins/demoMixin.js
export const demoMixin = {
  data() {
    return {
      message: "Hello DemoMixin"
    }
  },
  methods: {
    foo() {
      console.log("demo mixin foo");
    }
  },
  created() {
    console.log("执行了demo mixin created");
  }
}
```

#### Mixin的合并规则

+ 如果Mixin对象中的选项和组件对象中的选项发生了冲突，那么Vue会如何操作呢？
  + 这里分成不同的情况来进行处理；
+ 情况一：如果是data函数的返回值对象
  + 返回值对象默认情况下会进行合并；
  + 如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据；
+ 情况二：如何生命周期钩子函数
  + 生命周期的钩子函数会被合并到数组中，都会被调用；
+ 情况三：值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。
  + 比如都有methods选项，并且都定义了方法，那么它们都会生效；
  + 但是如果对象的key相同，那么会取组件对象的键值对；

#### 全局混入 Mixin

如果组件中的某些选项，是所有的组件都需要拥有的，那么这个时候我们可以使用全局的mixin：

+ 全局的Mixin可以使用 应用app的方法 mixin 来完成注册；
+ 一旦注册，那么全局混入的选项将会影响每一个组件；

```javascript
const app = createApp(App);

app.mixin({
  data() {
    return {}
  },
  methods: {

  },
  created() {
    console.log("全局的created生命周期");
  }
});

app.mount("#app");
```

### externds

+ 另外一个类似与Mixin的方式是通过extends属性
  + 允许声明拓展另外一个组件，类似与Mixins；

使一个组件可以继承另一个组件的组件选项。

从实现角度来看，`extends` 几乎和 `mixins` 相同。通过 `extends` 指定的组件将会当作第一个 mixin 来处理。

然而，`extends` 和 `mixins` 表达的是不同的目标。`mixins` 选项基本用于组合功能，而 `extends` 则一般更关注继承关系。

同 `mixins` 一样，所有选项 (`setup()` 除外) 都将使用相关的策略进行合并。

```javascript
const CompA = { ... }

const CompB = {
  extends: CompA,
  ...
}
```

```vue
<template>
  <div>
    Home Page
    <h2>{{title}}</h2>
    <button @click="bar">按钮</button>
  </div>
</template>

<script>
  import BasePage from './BasePage.vue';

  export default {
    extends: [BasePage],
    data() {
      return {
        content: "Hello Home"
      }
    }
  }
</script>

<style scoped>

</style>
```

> `extends` 是为选项式 API 设计的，不会处理 `setup()` 钩子的合并。
>
> 在组合式 API 中，逻辑复用的首选模式是“组合”而不是“继承”。如果一个组件中的逻辑需要复用，考虑将相关逻辑提取到[组合式函数](https://cn.vuejs.org/guide/reusability/composables.html#composables)中。
>
> 如果你仍然想要通过组合式 API 来“继承”一个组件，可以在继承组件的 `setup()` 中调用基类组件的 `setup()`：
>
> ```javascript
> import Base from './Base.js'
> export default {
>   extends: Base,
>   setup(props, ctx) {
>     return {
>       ...Base.setup(props, ctx),
>       // 本地绑定
>     }
>   }
> }
> ```
>
>

### 组合式函数

> <https://cn.vuejs.org/guide/reusability/composables.html>

#### [什么是“组合式函数”？](https://cn.vuejs.org/guide/reusability/composables.html#what-is-a-composable)

在 Vue 应用的概念中，“组合式函数”(Composables) 是一个利用 Vue 的组合式 API 来封装和复用**有状态逻辑**的函数。

当构建前端应用时，我们常常需要复用公共任务的逻辑。例如为了在不同地方格式化时间，我们可能会抽取一个可复用的日期格式化函数。这个函数封装了**无状态的逻辑**：它在接收一些输入后立刻返回所期望的输出。复用无状态逻辑的库有很多，比如你可能已经用过的 [lodash](https://lodash.com/) 或是 [date-fns](https://date-fns.org/)。

相比之下，有状态逻辑负责管理会随时间而变化的状态。一个简单的例子是跟踪当前鼠标在页面中的位置。在实际应用中，也可能是像触摸手势或与数据库的连接状态这样的更复杂的逻辑。

#### 鼠标跟踪器示例

如果我们要直接在组件中使用组合式 API 实现鼠标跟踪功能，它会是这样的：

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const x = ref(0)
const y = ref(0)

function update(event) {
  x.value = event.pageX
  y.value = event.pageY
}

onMounted(() => window.addEventListener('mousemove', update))
onUnmounted(() => window.removeEventListener('mousemove', update))
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

但是，如果我们想在多个组件中复用这个相同的逻辑呢？我们可以把这个逻辑以一个组合式函数的形式提取到外部文件中：

```javascript
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

下面是它在组件中使用的方式：

```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

如你所见，核心逻辑完全一致，我们做的只是把它移到一个外部函数中去，并返回需要暴露的状态。和在组件中一样，你也可以在组合式函数中使用所有的[组合式 API](https://cn.vuejs.org/api/#composition-api)。现在，`useMouse()` 的功能可以在任何组件中轻易复用了。

更酷的是，你还可以嵌套多个组合式函数：一个组合式函数可以调用一个或多个其他的组合式函数。这使得我们可以像使用多个组件组合成整个应用一样，用多个较小且逻辑独立的单元来组合形成复杂的逻辑。实际上，这正是为什么我们决定将实现了这一设计模式的 API 集合命名为组合式 API。

举例来说，我们可以将添加和清除 DOM 事件监听器的逻辑也封装进一个组合式函数中：

```javascript
// event.js
import { onMounted, onUnmounted } from 'vue'

export function useEventListener(target, event, callback) {
  // 如果你想的话，
  // 也可以用字符串形式的 CSS 选择器来寻找目标 DOM 元素
  onMounted(() => target.addEventListener(event, callback))
  onUnmounted(() => target.removeEventListener(event, callback))
}
```

有了它，之前的 `useMouse()` 组合式函数可以被简化为：

```javascript
// mouse.js
import { ref } from 'vue'
import { useEventListener } from './event'

export function useMouse() {
  const x = ref(0)
  const y = ref(0)

  useEventListener(window, 'mousemove', (event) => {
    x.value = event.pageX
    y.value = event.pageY
  })

  return { x, y }
}
```

#### [异步状态示例](https://cn.vuejs.org/guide/reusability/composables.html#async-state-example)

`useMouse()` 组合式函数没有接收任何参数，因此让我们再来看一个需要接收一个参数的组合式函数示例。在做异步数据请求时，我们常常需要处理不同的状态：加载中、加载成功和加载失败。

```vue
<script setup>
import { ref } from 'vue'

const data = ref(null)
const error = ref(null)

fetch('...')
  .then((res) => res.json())
  .then((json) => (data.value = json))
  .catch((err) => (error.value = err))
</script>

<template>
  <div v-if="error">Oops! Error encountered: {{ error.message }}</div>
  <div v-else-if="data">
    Data loaded:
    <pre>{{ data }}</pre>
  </div>
  <div v-else>Loading...</div>
</template>
```

如果在每个需要获取数据的组件中都要重复这种模式，那就太繁琐了。让我们把它抽取成一个组合式函数：

```javascript
// fetch.js
import { ref } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  fetch(url)
    .then((res) => res.json())
    .then((json) => (data.value = json))
    .catch((err) => (error.value = err))

  return { data, error }
}
```

现在我们在组件里只需要：

```vue
<script setup>
import { useFetch } from './fetch.js'

const { data, error } = useFetch('...')
</script>
```

#### [接收响应式状态](https://cn.vuejs.org/guide/reusability/composables.html#accepting-reactive-state)

`useFetch()` 接收一个静态 URL 字符串作为输入——因此它只会执行一次 fetch 并且就此结束。如果我们想要在 URL 改变时重新 fetch 呢？为了实现这一点，我们需要将响应式状态传入组合式函数，并让它基于传入的状态来创建执行操作的侦听器。

举例来说，`useFetch()` 应该能够接收一个 ref：

```javascript
const url = ref('/initial-url')

const { data, error } = useFetch(url)

// 这将会重新触发 fetch
url.value = '/new-url'
```

或者接收一个 getter 函数：

```javascript
// 当 props.id 改变时重新 fetch
const { data, error } = useFetch(() => `/posts/${props.id}`)
```

我们可以用 [`watchEffect()`](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 和 [`toValue()`](https://cn.vuejs.org/api/reactivity-utilities.html#tovalue) API 来重构我们现有的实现：

```javascript
// fetch.js
import { ref, watchEffect, toValue } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  const fetchData = () => {
    // reset state before fetching..
    data.value = null
    error.value = null

    fetch(toValue(url))
      .then((res) => res.json())
      .then((json) => (data.value = json))
      .catch((err) => (error.value = err))
  }

  watchEffect(() => {
    fetchData()
  })

  return { data, error }
}
```

`toValue()` 是一个在 3.3 版本中新增的 API。它的设计目的是将 ref 或 getter 规范化为值。如果参数是 ref，它会返回 ref 的值；如果参数是函数，它会调用函数并返回其返回值。否则，它会原样返回参数。它的工作方式类似于 [`unref()`](https://cn.vuejs.org/api/reactivity-utilities.html#unref)，但对函数有特殊处理。

注意 `toValue(url)` 是在 `watchEffect` 回调函数的**内部**调用的。这确保了在 `toValue()` 规范化期间访问的任何响应式依赖项都会被侦听器跟踪。

这个版本的 `useFetch()` 现在能接收静态 URL 字符串、ref 和 getter，使其更加灵活。watch effect 会立即运行，并且会跟踪 `toValue(url)` 期间访问的任何依赖项。如果没有跟踪到依赖项（例如 url 已经是字符串），则 effect 只会运行一次；否则，它将在跟踪到的任何依赖项更改时重新运行。

### [约定和最佳实践](https://cn.vuejs.org/guide/reusability/composables.html#conventions-and-best-practices)

#### 命名

组合式函数约定用驼峰命名法命名，并以“use”作为开头。

#### 输入参数

即便不依赖于 ref 或 getter 的响应性，组合式函数也可以接收它们作为参数。如果你正在编写一个可能被其他开发者使用的组合式函数，最好处理一下输入参数是 ref 或 getter 而非原始值的情况。可以利用 [`toValue()`](https://cn.vuejs.org/api/reactivity-utilities.html#tovalue) 工具函数来实现：

```javascript
import { toValue } from 'vue'

function useFeature(maybeRefOrGetter) {
  // 如果 maybeRefOrGetter 是一个 ref 或 getter，
  // 将返回它的规范化值。
  // 否则原样返回。
  const value = toValue(maybeRefOrGetter)
}
```

如果你的组合式函数在输入参数是 ref 或 getter 的情况下创建了响应式 effect，为了让它能够被正确追踪，请确保要么使用 `watch()` 显式地监视 ref 或 getter，要么在 `watchEffect()` 中调用 `toValue()`。

[前面讨论过的 useFetch() 实现](https://cn.vuejs.org/guide/reusability/composables.html#accepting-reactive-state)提供了一个接受 ref、getter 或普通值作为输入参数的组合式函数的具体示例。

#### [返回值](https://cn.vuejs.org/guide/reusability/composables.html#return-values)

你可能已经注意到了，我们一直在组合式函数中使用 `ref()` 而不是 `reactive()`。我们推荐的约定是组合式函数始终返回一个包含多个 ref 的普通的非响应式对象，这样该对象在组件中被解构为 ref 之后仍可以保持响应性：

```javascript
// x 和 y 是两个 ref
const { x, y } = useMouse()
```

从组合式函数返回一个响应式对象会导致在对象解构过程中丢失与组合式函数内状态的响应性连接。与之相反，ref 则可以维持这一响应性连接。

如果你更希望以对象属性的形式来使用组合式函数中返回的状态，你可以将返回的对象用 `reactive()` 包装一次，这样其中的 ref 会被自动解包，例如：

```javascript
const mouse = reactive(useMouse())
// mouse.x 链接到了原来的 x ref
console.log(mouse.x)
```

```vue
Mouse position is at: {{ mouse.x }}, {{ mouse.y }}
```

#### 副作用[](https://cn.vuejs.org/guide/reusability/composables.html#side-effects)

在组合式函数中的确可以执行副作用 (例如：添加 DOM 事件监听器或者请求数据)，但请注意以下规则：

+ 如果你的应用用到了[服务端渲染](https://cn.vuejs.org/guide/scaling-up/ssr.html) (SSR)，请确保在组件挂载后才调用的生命周期钩子中执行 DOM 相关的副作用，例如：`onMounted()`。这些钩子仅会在浏览器中被调用，因此可以确保能访问到 DOM。
+ 确保在 `onUnmounted()` 时清理副作用。举例来说，如果一个组合式函数设置了一个事件监听器，它就应该在 `onUnmounted()` 中被移除 (就像我们在 `useMouse()` 示例中看到的一样)。当然也可以像之前的 `useEventListener()` 示例那样，使用一个组合式函数来自动帮你做这些事。

#### 使用限制[](https://cn.vuejs.org/guide/reusability/composables.html#usage-restrictions)

组合式函数只能在 `<script setup>` 或 `setup()` 钩子中被调用。在这些上下文中，它们也只能被**同步**调用。在某些情况下，你也可以在像 `onMounted()` 这样的生命周期钩子中调用它们。

这些限制很重要，因为这些是 Vue 用于确定当前活跃的组件实例的上下文。访问活跃的组件实例很有必要，这样才能：

1. 将生命周期钩子注册到该组件实例上
2. 将计算属性和监听器注册到该组件实例上，以便在该组件被卸载时停止监听，避免内存泄漏。

> `<script setup>` 是唯一在调用 await 之后仍可调用组合式函数的地方。编译器会在异步操作之后自动为你恢复当前的组件实例。

#### [通过抽取组合式函数改善代码结构](https://cn.vuejs.org/guide/reusability/composables.html#extracting-composables-for-code-organization)

抽取组合式函数不仅是为了复用，也是为了代码组织。随着组件复杂度的增高，你可能会最终发现组件多得难以查询和理解。组合式 API 会给予你足够的灵活性，让你可以基于逻辑问题将组件代码拆分成更小的函数：

```javascript
<script setup>
import { useFeatureA } from './featureA.js'
import { useFeatureB } from './featureB.js'
import { useFeatureC } from './featureC.js'

const { foo, bar } = useFeatureA()
const { baz } = useFeatureB(foo)
const { qux } = useFeatureC(baz)
</script>
```

在某种程度上，你可以将这些提取出的组合式函数看作是可以相互通信的组件范围内的服务。

#### [在选项式 API 中使用组合式函数](https://cn.vuejs.org/guide/reusability/composables.html#using-composables-in-options-api)

如果你正在使用选项式 API，组合式函数必须在 `setup()` 中调用。且其返回的绑定必须在 `setup()` 中返回，以便暴露给 `this` 及其模板：

```javascript
import { useMouse } from './mouse.js'
import { useFetch } from './fetch.js'

export default {
  setup() {
    const { x, y } = useMouse()
    const { data, error } = useFetch('...')
    return { x, y, data, error }
  },
  mounted() {
    // setup() 暴露的属性可以在通过 `this` 访问到
    console.log(this.x)
  }
  // ...其他选项
}
```

### [对比](https://cn.vuejs.org/guide/reusability/composables.html#vs-mixins)

## Composition API

### Options API的弊端

+ 在Vue2中，我们编写组件的方式是Options API：
  + Options API的一大特点就是在对应的属性中编写对应的功能模块；
  + 比如data定义数据、methods中定义方法、computed中定义计算属性、watch中监听属性改变，也包括生命 周期钩子；
+ 但是这种代码有一个很大的弊端：
  + 当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中；
  + 当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散；
  + 尤其对于那些一开始没有编写这些组件的人来说，这个组件的代码是难以阅读和理解的（阅读组件的其他人）；

+ 下面我们来看一个非常大的组件，其中的逻辑功能按照颜色进行了划分：
  + 这种碎片化的代码使用理解和维护这个复杂的组件变得异常困难，并且隐藏了潜在的逻辑问题；
  + 并且当我们处理单个逻辑关注点时，需要不断的跳到相应的代码块中；

![image-20231216000029755](https://qiniu.waite.wang/202312160000793.png)

+ 如果我们能将同一个逻辑关注 点相关的代码收集在一起会更 好。
+ 这就是Composition API想 要做的事情，以及可以帮助我 们完成的事情。
+ 也有人把Vue Composition API简称为VCA。
+ 我们无需再为了一个逻辑关注点在不同的选项块间来回滚动切换。此外，我们现在可以很轻松地将这一组代码移动到一个外部文件中，不再需要为了抽象而重新组织代码，大大降低了重构成本，这在长期维护的大型项目中非常关键。

### 认识 组合式 API (Composition API)

> <https://cn.vuejs.org/guide/extras/composition-api-faq.html#what-is-composition-api>

组合式 API (Composition API) 是一系列 API 的集合，使我们可以使用函数而不是声明选项的方式书写 Vue 组件。它是一个概括性的术语，涵盖了以下方面的 API：

+ [响应式 API](https://cn.vuejs.org/api/reactivity-core.html)：例如 `ref()` 和 `reactive()`，使我们可以直接创建响应式状态、计算属性和侦听器。
+ [生命周期钩子](https://cn.vuejs.org/api/composition-api-lifecycle.html)：例如 `onMounted()` 和 `onUnmounted()`，使我们可以在组件各个生命周期阶段添加逻辑。
+ [依赖注入](https://cn.vuejs.org/api/composition-api-dependency-injection.html)：例如 `provide()` 和 `inject()`，使我们可以在使用响应式 API 时，利用 Vue 的依赖注入系统。

组合式 API 是 Vue 3 及 [Vue 2.7](https://blog.vuejs.org/posts/vue-2-7-naruto.html) 的内置功能。对于更老的 Vue 2 版本，可以使用官方维护的插件 [`@vue/composition-api`](https://github.com/vuejs/composition-api)。在 Vue 3 中，组合式 API 基本上都会配合 [``](https://cn.vuejs.org/api/sfc-script-setup.html) 语法在单文件组件中使用。下面是一个使用组合式 API 的组件示例：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 更改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`计数器初始值为 ${count.value}。`)
})
</script>

<template>
  <button @click="increment">点击了：{{ count }} 次</button>
</template>
```

虽然这套 API 的风格是基于函数的组合，但**组合式 API 并不是函数式编程**。组合式 API 是以 Vue 中数据可变的、细粒度的响应性系统为基础的，而函数式编程通常强调数据不可变。

### setup()

> 以下的代码 均会采用选项式的写法, 组合式 Api 的写法可以参考官方文档, 但基本原理差不多, 而且 Vue3 选项式写法是基于组合式写法产生的!

`setup()` 钩子是在组件中使用组合式 API 的入口，通常只在以下情况下使用：

1. 需要在非单文件组件中使用组合式 API 时。
2. 需要在基于选项式 API 的组件中集成基于组合式 API 的代码时。

#### setup函数的参数

+ 主要有两个参数:
  + 第一个参数：props
  + 第二个参数：context
+ `setup` 函数的第一个参数是组件的 `props`。和标准的组件一致，一个 `setup` 函数的 `props` 是响应式的，并且会在传入新的 props 时同步更新。：
  + 对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义；
  + 并且在template中依然是可以正常去使用props中的属性，比如message；
  + 如果我们在setup函数中想要使用props，那么不可以通过 this 去获取

```vue
<!-- 推荐使用以下写法 -->
<script>
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    // 返回值会暴露给模板和其他的选项式 API 钩子
    return {
      count
    }
  },

  mounted() {
    console.log(this.count) // 0
  }
}
</script>

<template>
  <button @click="count++">{{ count }}</button>
</template>
```

```vue
<!-- 当然也可以使用以下写法 -->
<script>
  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        counter: 100
      }
    },
    setup(props, ....){
      .....
    }
    ...
}
</script>
```

+ `setup` 函数的第一个参数是组件的 `props`。和标准的组件一致，一个 `setup` 函数的 `props` 是响应式的，并且会在传入新的 props 时同步更新。

```javascript
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

请注意如果你解构了 `props` 对象，解构出的变量将会丢失响应性。因此我们推荐通过 `props.xxx` 的形式来使用其中的 props。

如果你确实需要解构 `props` 对象，或者需要将某个 prop 传到一个外部函数中并保持响应性，那么你可以使用 [toRefs()](https://cn.vuejs.org/api/reactivity-utilities.html#torefs) 和 [toRef()](https://cn.vuejs.org/api/reactivity-utilities.html#toref) 这两个工具函数：

```javascript
import { toRefs, toRef } from 'vue'

export default {
  setup(props) {
    // 将 `props` 转为一个其中全是 ref 的对象，然后解构
    const { title } = toRefs(props)
    // `title` 是一个追踪着 `props.title` 的 ref
    console.log(title.value)

    // 或者，将 `props` 的单个属性转为一个 ref
    const title = toRef(props, 'title')
  }
}
```

+ 另外一个参数是context，我们也称之为是一个SetupContext，它里面包含三个属性：
  + attrs：所有的非prop的attribute；
  + slots：父组件传递过来的插槽（这个在以渲染函数返回时会有作用，后面会讲到）；
  + emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过 this.$emit发出事件）；

```javascript
export default {
  setup(props, context) {
    // 透传 Attributes（非响应式的对象，等价于 $attrs）
    console.log(context.attrs)

    // 插槽（非响应式的对象，等价于 $slots）
    console.log(context.slots)

    // 触发事件（函数，等价于 $emit）
    console.log(context.emit)

    // 暴露公共属性（函数）
    console.log(context.expose)
  }
    
   // 该上下文对象是非响应式的，可以安全地解构：
    setup(props, {attrs, slots, emit}) {
      console.log(props.message);
      console.log(attrs.id, attrs.class);
      console.log(slots);
      console.log(emit);
    }
}
```

`attrs` 和 `slots` 都是有状态的对象，它们总是会随着组件自身的更新而更新。这意味着你应当避免解构它们，并始终通过 `attrs.x` 或 `slots.x` 的形式使用其中的属性。此外还需注意，和 `props` 不同，`attrs` 和 `slots` 的属性都**不是**响应式的。如果你想要基于 `attrs` 或 `slots` 的改变来执行副作用，那么你应该在 `onBeforeUpdate` 生命周期钩子中编写相关逻辑。

> 代码示例

```vue
<template>
  <HelloWorld msg="Welcome to Your Vue.js App" class="app-attr" />
</template>
<script>
import HelloWorld from './components/HelloWorld.vue';
export default {
  name: 'App',
  components: {
    HelloWorld
  }
};
</script>
<style></style>
```

```vue
<template>
  <div class="hello">
    <!-- 使用接受过来的参数 -->
    {{ msg }}
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
  // 组件接受的参数
  props: {
    msg: String
  },
  // 发射的事件这里可以标注一下
  emits:['change'],
  setup(props, context) {
    // 这样可以拿到传递过来的msg的值
    console.log(props.msg);
    // attrs
    console.log(context.attrs);
    // 发射事件
    context.emit('change');
  }
};
</script>
<style scoped></style>
```

#### setup函数的返回值

`setup` 也可以返回一个[渲染函数](https://cn.vuejs.org/guide/extras/render-function.html)，此时在渲染函数中可以直接使用在同一作用域下声明的响应式状态：

```javascript
import { h, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return () => h('div', count.value)
  }
}
```

返回一个渲染函数将会阻止我们返回其他东西。对于组件内部来说，这样没有问题，但如果我们想通过模板引用将这个组件的方法暴露给父组件，那就有问题了。

我们可以通过调用 [`expose()`](https://cn.vuejs.org/api/composition-api-setup.html#exposing-public-properties) 解决这个问题：

```javascript
import { h, ref } from 'vue'

export default {
  setup(props, { expose }) {
    const count = ref(0)
    const increment = () => ++count.value

    expose({
      increment
    })

    return () => h('div', count.value)
  }
}
```

此时父组件可以通过模板引用来访问这个 `increment` 方法。

```javascript
export default {
  props: {
    message: {
      type: String,
      required: true
    }
  },
  setup() {
    let counter = 100;

    // 局部函数
    const increment = () => {
      counter++;
      console.log(counter);
    }

    return {
      title: "Hello Home",
      counter,
      increment
    }
  }
}
```

+ setup的返回值可以在模板template中被使用
+ 也就是说可以通过setup的返回值来替代data选项

> **最后导出的一定要是个对象**

```vue
<template>
  <div class="hello">
    <!-- 使用导出的变量 -->
    <h1>{{ count }}</h1>
    <!-- 使用导出的方法 -->
    <button @click="increment">+ 1</button>
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
  setup() {
    // 定义普通的变量，可以被正常使用
    // 缺点 : 数据不是响应式的
    let count = 100;
    // 定义方法
    const increment = () => {
      count++;
      console.log(count);
    };
    // 导出
    return {
      count,
      increment
    };
  }
};
</script>
<style scoped></style>
```

![img](https://qiniu.waite.wang/202312162228196.gif)

> **因为只是定义了个变量，然后导出了，并没有使它响应式**

#### 补充: 为什么 setup 不能使用 this

在Vue 3中，`setup`函数是用来替代以前的 `data`, `computed`, `methods`等选项的。`setup()` 自身并不含对组件实例的访问权，即在 `setup()` 中访问 `this` 会是 `undefined`。你可以在选项式 API 中访问组合式 API 暴露的值，但反过来则不行。

### 定义响应式数据的两种方式

#### Reactive API

> 如果想为在setup中定义的数据提供响应式的特性，那么可以使用reactive的函数
>
> ps : 如果传入一个基本数据类型（String、Number、Boolean）会报一个警告
>
> 应用场景 : reactive API对传入的类型是有限制的，它要求我们必须传入的是一个对象或者数组类型，最好相互有关联的数据时使用

比如说想要上面的例子实现响应式, 我们可以做如下操作

```vue
<template>
  <div class="hello">
    <!-- 这样使用即可 -->
    <h1>{{ state.count }}</h1>
    <button @click="increment">+ 1</button>
  </div>
</template>
<script>
// 从vue中导入reactive
import { reactive } from 'vue';
export default {
  name: 'HelloWorld',
  setup() {
    // 使用reactive，会返回一个响应式对象
    const state = reactive({
      // 在对象中编写自己所需要的属性
      count: 100
    });
    const increment = () => {
      // 这样使用
      state.count++;
      console.log(state.count);
    };
    return {
      // 导出响应式state对象
      state,
      increment
    };
  }
};
</script>
```

##### Reactive判断的API

+ isProxy : 检查对象是否是由 reactive 或 readonly创建的 proxy
+ isReactive : 检查对象是否是由 reactive创建的响应式代理，如果该代理是 readonly 建的，但包裹了由 reactive 创建的另一个代理，它也会返回 true
+ isReadonly : 检查对象是否是由 readonly 创建的只读代理
+ toRaw : 返回 reactive 或 readonly 代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）
+ shallowReactive : 创建一个响应式代理，它跟踪其自身 property 的响应性，但不执行嵌套对象的深层响应式转换 (深层还是原生对象)，只响应第一层
+ shallowReadonly : 创建一个 proxy，使其自身的 property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）只检查第一层

```javascript
import { reactive, readonly, isProxy, isReactive, isReadonly, toRaw, shallowReactive, shallowReadonly } from 'vue';

// 创建一个响应式对象
const original = { count: 0 };
const obj = reactive(original);

// 检查对象是否是代理对象
console.log(isProxy(obj)); // true

// 检查对象是否是由 reactive 创建的响应式代理
console.log(isReactive(obj)); // true

// 检查对象是否是由 readonly 创建的只读代理
const ro = readonly(obj);
console.log(isReadonly(ro)); // true

// 返回 reactive 或 readonly 代理的原始对象
const rawObj = toRaw(obj);

// 创建一个浅层响应式代理
const shallowObj = shallowReactive({ nested: { count: 0 } });

// 创建一个浅层只读代理
const shallowRo = shallowReadonly({ nested: { count: 0 } });
```

> 以下是一些名词解释:
>
> + 代理对象：在Vue 3中，代理对象是由 reactive 或 readonly 创建的对象的代理，用于跟踪对象的属性的变化。
> + readonly：readonly 是一个函数，用于创建一个只读的代理对象，该代理对象只能读取属性值，不能修改属性值。
> + 浅层响应式：浅层响应式是指只有对象的第一层属性会被设置为响应式，而嵌套对象的属性不会被设置为响应式。
> + 浅层只读代理：浅层只读代理是指只有对象的第一层属性会被设置为只读，而嵌套对象的属性不会被设置为只读。
>
> ```javascript
> import { reactive, readonly, shallowReactive, shallowReadonly } from 'vue';
> 
> // 创建一个普通对象
> const original = { count: 0, nested: { count: 1 } };
> 
> // 创建一个响应式代理对象
> const reactiveObj = reactive(original);
> reactiveObj.count = 1; // 可以修改属性值
> reactiveObj.nested.count = 2; // 嵌套对象的属性也会被设置为响应式
> 
> // 创建一个只读代理对象
> const readonlyObj = readonly(original);
> // readonlyObj.count = 2; // 试图修改只读属性会报错
> // readonlyObj.nested.count = 2; // 嵌套对象的属性也是只读的
> 
> // 创建一个浅层响应式代理对象
> const shallowReactiveObj = shallowReactive(original);
> shallowReactiveObj.count = 2; // 可以修改属性值
> shallowReactiveObj.nested.count = 2; // 嵌套对象的属性不会被设置为响应式
> 
> // 创建一个浅层只读代理对象
> const shallowReadonlyObj = shallowReadonly(original);
> shallowReadonlyObj.count = 2; // 试图修改只读属性会报错
> shallowReadonlyObj.nested.count = 2; // 嵌套对象的属性不会被设置为只读
> ```

#### Ref API

> **Ref函数 : 定义简单类型的数据，也可以定义复杂类型的数据**
>
> **应用场景 : 定义一些简单的数据，或者从接口中获得的数据**

+ ref 会返回一个可变的响应式对象，该对象作为一个 响应式的引用 维护着它内部的值，这就是ref名称的来源
+ 它内部的值是在`ref`的 `value` 属性中被维护的
+ 不管传入的是基本类型还是引用类型，都放在`.value`中

> 使用的时候是用 .value，但是有两个注意事项:
>
> + 在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以并不需要在模板中通过 ref.value 的方式，直接使用即可
> + 在 setup 函数内部，它依然是一个 ref引用， 所以对其进行操作时，依然需要使用 ref.value的方式

##### 基本使用

```vue
<template>
  <div class="hello">
    <!-- 这样使用即可，不需要使用count.value，会自动解包，取出其中的value -->
    <h1>{{ count }}</h1>
    <button @click="increment">+ 1</button>
  </div>
</template>
<script>
// 从vue中导入ref
import { ref } from 'vue';
export default {
  name: 'HelloWorld',
  setup() {
    // 使用Ref，会返回一个响应式对象
    let count = ref(100);
    const increment = () => {
      // 这样使用,需要使用 .value
      count.value++;
      console.log(count.value);
    };
    return {
      // 直接导出count即可
      count,
      increment
    };
  }
};
</script>
```

##### Ref自动解包

+ 模板中的解包是浅层的解包，如果我们的代码是下面的方式：
+ 如果我们将`ref`放到一个`reactive`的属性当中，那么在模板中使用时，它会自动解包：

```javascript
const count = ref(1)
const obj = reactive({ count })

// ref 会被解包
console.log(obj.count === count.value) // true

// 会更新 `obj.count`
count.value++
console.log(count.value) // 2
console.log(obj.count) // 2

// 也会更新 `count` ref
obj.count++
console.log(obj.count) // 3
console.log(count.value) // 3
```

注意当访问到某个响应式数组或 `Map` 这样的原生集合类型中的 ref 元素时，**不会**执行 ref 的解包：

```javascript
const books = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// 这里需要 .value
console.log(map.get('count').value)
```

##### Ref判断的API

+ isRef : 判断值是否是一个ref对象
+ unref : 如果我们想要获取一个ref引用中的value，那么也可以通过unref方法
  + 如果参数是一个 ref，则返回内部值，否则返回参数本身
  + 这是 val = isRef(val) ? val.value : val 的语法糖函数
+ shallowRef：shallowRef 是用来创建一个浅层的 ref 对象的函数。浅层的意思是只有对象的第一层属性会被设置为响应式，而嵌套对象的属性不会被设置为响应式。这意味着对嵌套对象属性的修改不会触发响应。
+ triggerRef：triggerRef 用于手动触发与 shallowRef 相关联的副作用。当 shallowRef 关联的数据发生变化时，相关的副作用将被触发执行。

```javascript
import { ref, isRef, unref, shallowRef, triggerRef } from 'vue';

// 判断值是否是一个 ref 对象
const count = ref(0);
console.log(isRef(count)); // true

// 获取 ref 引用中的值
const value = unref(count);

// 语法糖函数示例
const val = isRef(count) ? count.value : count;

// 创建一个浅层的 ref 对象
const shallowCount = shallowRef({ value: 0 });
shallowCount.value = 1; // 可以直接修改 value 属性

// 手动触发与 shallowRef 相关联的副作用
triggerRef(shallowCount);
```

##### 自定义Ref => **customRef**

+ 创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制：
  + 它需要一个工厂函数，该函数接受 `track`和 `trigger`函数作为参数；
  + 并且应该返回一个带有 `get`和 `set` 的对象；
+ 这里我们使用一个的案例：
  + 对双向绑定的属性进行debounce(节流)的操作

```javascript
import { customRef } from 'vue';

// 自定义ref
export default function(value, delay = 300) {
  let timer = null; 
  return customRef((track, trigger) => {
    return {
      get() {
        track();
        return value;
      },
      set(newValue) {
        clearTimeout(timer);
        timer = setTimeout(() => {
          value = newValue;
          trigger();
        }, delay);
      }
    }
  })
}
```

```vue
<template>
  <div>
    <input v-model="message"/>
    <h2>{{message}}</h2>
  </div>
</template>

<script>
  import debounceRef from './hook/useDebounceRef';

  export default {
    setup() {
      const message = debounceRef("Hello World");

      return {
        message
      }
    }
  }
</script>
```

### readonly

#### 概念

> 在我们传递给其他组件数据时，往往希望其他组件使用我们传递的内容，但是不允许它们修改时，就可以使用 `readonly`了；

+ 只读代理是深层的：对任何嵌套属性的访问都将是只读的。它的 ref 解包行为与 `reactive()` 相同，但解包得到的值是只读的。
+ 要避免深层级的转换行为，请使用 [shallowReadonly()](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 作替代。
+ 在开发中常见的readonly方法会传入三个类型的参数：
  + 类型一：普通对象
  + 类型二：reactive返回的对象
  + 类型三：ref的对象
+ 在readonly的使用过程中，有如下规则 :
  + readonly返回的对象都是不允许修改的
  + 但是经过readonly处理的原来的对象是允许被修改的
    + 比如 const info = readonly(obj)，info对象是不允许被修改的
    + 当obj被修改时，readonly返回的info对象也会被修改
    + 但是不能去修改readonly返回的对象info

```vue
<template>
  <div class="hello">
    <button @click="btnClick">按钮</button>
  </div>
</template>
<script setup>
import { reactive, readonly, watchEffect } from 'vue'

const original = reactive({ count: 0 })

const copy = readonly(original)

const btnClick = () => {
  original.count++
}

watchEffect(() => {
  // 用来做响应性追踪
  console.log(copy.count)
})

// 更改源属性会触发其依赖的侦听器
original.count++

// 更改该只读副本将会失败，并会得到一个警告
copy.count++ // warning!
</script>
```

### toRefs && toRef

#### **toRefs**

> 如果使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive 返回的state对象，数据都不再是响应式的

![image-20231217170024283](https://qiniu.waite.wang/202312171700262.png)

+ 如何改成响应式呢，Vue提供了一个toRefs的函数
+ 可以将reactive返回的对象中的属性都转成ref，这样解构出来的就是响应式的了

```vue
<template>
  <div class="hello">
    <h1>{{ age }}</h1>
    <button @click="increment">+age</button>
  </div>
</template>
<script>
// 从vue中导入ref
import { reactive, ref, readonly, toRefs } from 'vue';
export default {
  name: 'HelloWorld',
  setup() {
    const info = reactive({ name: 'star', age: 18 });
    // 使用toRefs包裹需要结构的reactive对象，这样解构出来的值也是响应式的
    let { name, age } = toRefs(info);
    const increment = () => {
      info.age++;
      // 👆这样都可以修改age，都是响应式的👇
      // 相当于已经建立了链接，任何一个修改都会引起另外一个变化
      age.value++;
      console.log(age, info.age);
    };
    return {
      name,
      age,
      increment
    };
  }
};
</script>
<style scoped></style>
```

#### **toRef**

> 如果只希望转换reactive对象中的其中某个属性为ref, 那么可以使用toRef的方法
>
> **ps : 这个效率会更高点,  这种做法相当于已经在state.name和ref.value之间建立了 链接，任何一个修改都会引起另外一个变化**

```javascript
let age = toRef(info, "age");

const changeAge = () => {
  age.value++;
}
```

### computed

+ 在前面的Options API中，我们是使用computed选项来完成的；
+ 在Composition API中，我们可以在 setup 函数中使用 computed 方法来编写一个计算属性；

+ 如何使用computed呢？
  + 方式一：接收一个getter函数，并为 getter 函数返回的值，返回一个不变的 ref 对象；
  + 方式二：接收一个具有 get 和 set 的对象，返回一个可变的（可读写）ref 对象；

#### 方式一

```vue
<template>
  <!-- coderstar -->
  {{ fullName }}
  <!-- 一般 -->
  {{ scoreState }}
</template>
 
<script>
import { computed, reactive, ref } from 'vue';
export default {
  name: 'App',
  setup() {
    const names = reactive({
      firstName: 'coder',
      lastName: 'star'
    });
    // 直接使用getter函数，正常来说都这么使用
    const fullName = computed(() => names.firstName + names.lastName);
 
    const score = ref(88);
    const scoreState = computed(() => (score.value > 90 ? '优秀' : '一般'));
    
    return {
      fullName,
      scoreState
    };
  }
};
</script>
```

#### 方式二

```vue
<template>
  {{ fullName }}
  <button @click="changeName">change</button>
</template>
 
<script>
import { computed, reactive } from 'vue';
export default {
  name: 'App',
  setup() {
    const names = reactive({
      firstName: '冲啊',
      lastName: '迪迦奥特曼'
    });
    // 会返回一个ref对象
    const fullName = computed({
      set(newValue) {
        const tempNames = newValue.split(' ');
        names.firstName = tempNames[0];
        names.lastName = tempNames[1];
      },
      get() {
        return names.firstName + names.lastName;
      }
    });
    // 设置值
    const changeName = () => {
      fullName.value = fullName.value === '冲啊迪迦奥特曼' ? '神秘的 宇宙人' : '冲啊 迪迦奥特曼';
    };
 
    return {
      fullName,
      changeName
    };
  }
};
</script>
```

![img](https://qiniu.waite.wang/202312171732557.gif)

### 生命周期钩子

> <https://cn.vuejs.org/api/composition-api-lifecycle.html>

> **setup中可以直接使用导入的onX函数注册生命周期，并且同一个生命周期可以使用多次**
>
> **所有罗列在本页的 API 都应该在组件的 `setup()` 阶段被同步调用。相关细节请看[指南 - 生命周期钩子](https://cn.vuejs.org/guide/essentials/lifecycle.html)。**
>
> + 可以使用直接导入的 onX 函数注册生命周期钩子；
> + beforeCreate和create在setup中没有相对应的onX的函数
>   + 如果想要在beforeCreate和create中进行操作
>   + 可以把代码直接写入到setup中
>   + setup的执行时序比beforeCreate和create还要早

![image-20231218002757954](https://qiniu.waite.wang/image-20231218002757954.png)

```javascript
import { onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue';

// 注册生命周期钩子
export default {
  setup() {
    onBeforeMount(() => {
      console.log('Before Mount'); // 组件挂载前
    });

    onMounted(() => {
      console.log('Mounted'); // 组件挂载后
    });

    onBeforeUpdate(() => {
      console.log('Before Update'); // 组件更新前
    });

    onUpdated(() => {
      console.log('Updated'); // 组件更新后
    });

    onBeforeUnmount(() => {
      console.log('Before Unmount'); // 组件卸载前
    });

    onUnmounted(() => {
      console.log('Unmounted'); // 组件卸载后
    });

    // 同一个生命周期可以使用多次
    onMounted(() => {
      console.log('Another Mounted'); // 另一个组件挂载后
    });

    return {};
  }
};
```

### setup中使用ref获取元素或组件

> **要定义一个ref对象，绑定到元素或者组件的ref属性上即可**
>
> **只有在挂载完成后才能拿到值, 所以需要在生命周期中调用拿值**

#### 获取元素

```vue
<template>
  <!-- 1. 指定ref -->
  <h2 ref="titleRef">我是迪迦</h2>
</template>
 
<script>
import { onMounted, ref } from 'vue';
export default {
  name: 'App',
  setup() {
    // 2. 生成ref对象
    const titleRef = ref();
 
    // 4. 可以在生命周期中获取到值
    onMounted(() => {
      console.log(titleRef.value); // <h2>我是迪迦</h2>
    });
 
    return {
      // 3. 返回出去，会自动匹配到对应的ref的
      titleRef
    };
  }
};
</script>
```

#### 获取组件

```vue
<template>
  <div>我是子组件</div>
</template>
 
<script>
export default {
  name: 'home-layout',
  setup() {
    const showMessage = () => {
      console.log('home-layout function exection');
    };
    return { showMessage };
  }
};
</script>
```

```vue
<template>
  <!-- 1. 指定ref -->
  <home ref="homeCompRef" />
</template>
 
<script>
import { onMounted, ref } from 'vue';
import home from './home.vue';
export default {
  name: 'App',
  components: { home },
  setup() {
    // 2. 生成ref对象
    const homeCompRef = ref();
 
    // 4. 可以在生命周期中获取到值
    onMounted(() => {
      console.log(homeCompRef.value); // proxy对象
      console.log(homeCompRef.value.$el); // <div>我是子组件</div>
      homeCompRef.value.showMessage(); // 调用子组件方法
    });
 
    return {
      // 3. 返回出去，会自动匹配到对应的ref的
      homeCompRef
    };
  }
};
</script>
```

![image-20231217214139261](https://qiniu.waite.wang/202312172141785.png)

### 侦听数据的变化

+ 在前面的Options API中，我们可以通过watch选项来侦听data或者props的数据变化，当数据变化时执行某一些 操作。

+ 在Composition API中，我们可以使用watchEffect和watch来完成响应式数据的侦听；
  + watchEffect用于自动收集响应式数据的依赖；
  + watch需要手动指定侦听的数据源；

#### watchEffect

##### 基本使用

+ 自动收集响应式数据的依赖
+ watchEffect传入的函数会被立即执行一次，并且在执行的过程中会收集依赖
+ 只有收集的依赖发生变化时，watchEffect传入的函数才会再次执行

```vue
<template>
  <div>
    <h1>{{ name }} - {{ age }}</h1>
    <button @click="changeName">changeName</button>
    <button @click="changeAge">changeAge</button>
  </div>
</template>
<script>
import { ref, watchEffect } from 'vue';
export default {
  setup() {
    let name = ref('star');
    let age = ref(18);
 
    const changeName = () => (name.value === 'star' ? (name.value = 'xuanyu') : (name.value = 'star'));
    const changeAge = () => age.value++;
 
    watchEffect(() => {
      // 因为这里只使用了name，所以只会监听name，如果把age也写进来，那么两个都会监听
      console.log('name:', name.value);
    });
 
    return { name, age, changeName, changeAge };
  }
};
</script>
```

![img](https://qiniu.waite.wang/202312171743816.gif)

##### 停止监听

+ 如果在发生某些情况下，我们希望停止侦听，这个时候我们可以获取watchEffect的返回值函数，调用该函数即可。

```vue
<template>
  <div>
    <h1>{{ name }} - {{ age }}</h1>
    <button @click="changeName">changeName</button>
    <button @click="changeAge">changeAge</button>
  </div>
</template>
<script>
import { ref, watchEffect } from 'vue';
export default {
  setup() {
    let name = ref('star');
    let age = ref(18);
 
    const changeName = () => (name.value === 'star' ? (name.value = 'xuanyu') : (name.value = 'star'));
    // 获取返回值
    const stopWatchEffect = watchEffect(() => {
      // 自动监听age
      console.log('age:', age.value);
    });
    const changeAge = () => {
      age.value++;
      if (age.value > 22) {
        // 停止监听
        stopWatchEffect();
      }
    };
 
    return { name, age, changeName, changeAge };
  }
};
</script>
```

![img](https://qiniu.waite.wang/202312171747594.gif)

##### 清除副作用

+ 什么是清除副作用呢？
  + 比如在开发中我们需要在侦听函数中执行网络请求，但是在网络请求还没有达到的时候，我们停止了侦听器， 或者侦听器侦听函数被再次执行了
  + 那么上一次的网络请求应该被取消掉，这个时候我们就可以清除上一次的副作用；
+ 在我们给watchEffect传入的函数被回调时，其实可以获取到一个参数：onInvalidate
  + 当副作用即将重新执行 或者 侦听器被停止 时会执行该函数传入的回调函数；
  + 我们可以在传入的回调函数中，执行一些清除工作；

```vue
<template>
  <div>
    <h2>{{ name }}-{{ age }}</h2>
    <button @click="changeName">修改name</button>
    <button @click="changeAge">修改age</button>
  </div>
</template>

<script>
import { ref, watchEffect } from 'vue';

export default {
  setup() {
    // watchEffect: 自动收集响应式的依赖
    const name = ref("why");
    const age = ref(18);

    const stop = watchEffect((onInvalidate) => {
      const timer = setTimeout(() => {
        console.log("网络请求成功~");
      }, 2000)

      // 根据name和age两个变量发送网络请求
      onInvalidate(() => {
        // 在这个函数中清除额外的副作用
        // request.cancel()
        clearTimeout(timer);
        console.log("onInvalidate");
      })
      console.log("name:", name.value, "age:", age.value);
    });

    const changeName = () => name.value = name.value === "why" ? "kobe" : "why";
    const changeAge = () => age.value++;

    return {
      name,
      age,
      changeName,
      changeAge
    }
  }
}
</script> 
```

##### watchEffect的执行时机

+ 默认情况下，组件的更新会在副作用函数执行之前：
  + 如果我们希望在副作用函数中获取到元素，是否可行呢？

```vue
<template>
  <div>
    <h2 ref="title">哈哈哈</h2>
  </div>
</template>

<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      const title = ref(null);

      watchEffect(() => {
        console.log(title.value);
      })
      
      return {
        title
      }
    }
  }
</script>
```

![image-20231217214736939](https://qiniu.waite.wang/202312172147749.png)

+ 我们会发现打印结果打印了两次：
  + 这是因为setup函数在执行时就会立即执行传入的副作用函数，这个时候DOM并没有挂载，所以打印为null；
  + 而当DOM挂载时，会给`title`的`ref`对象赋值新的值，副作用函数会再次执行，打印出来对应的元素；
+ 这个时候我们需要改变副作用函数的执行时机；
  + 它的默认值是pre，它会在元素 挂载 或者 更新 之前执行；
  + 所以我们会先打印出来一个空的，当依赖的title发生改变时，就会再次执行一次，打印出元素；
+ 我们可以设置副作用函数的执行时机：
  + pre : 默认值,它会在元素 挂载 或者 更新 之前执行
  + post : 元素 挂载 或者 更新 之后执行
  + sync : 强制同步一起执行，效率很低，不推荐

```vue
<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      const title = ref(null);

      watchEffect(() => {
        console.log(title.value);
      }, {
        flush: "post"
      })

      return {
        title
      }
    }
  }
</script>
```

#### Watch

+ watch的API完全等同于组件watch选项的Property：
  + watch需要侦听特定的数据源，并在回调函数中执行副作用；
  + 默认情况下它是惰性的，只有当被侦听的源发 生变化时才会执行回调；
+ 与watchEffect的比较，watch允许我们：
  + 懒执行副作用（第一次不会直接执行）；
  + 更具体的说明当哪些状态发生变化时，触发侦听器的执行；
  + 访问侦听状态变化前后的值；

##### 侦听单个数据源

watch侦听函数的数据源有两种类型：

+ 一个getter函数：但是该getter函数必须引用可响应式的对象（比如reactive或者ref）；
+ 直接写入一个可响应式的对象，ref（如果是一个 reactive 的对象的侦听, 需要进行某些转换 ）；

```javascript
import { watch, reactive, ref, toRefs } from 'vue';

// 一个getter函数引用可响应式的对象
const state = reactive({ count: 0 });
watch(
  () => state.count, 
  (newValue, oldValue) => {
 console.log(`Count changed from ${oldValue} to ${newValue}`);
});

// 直接写入一个可响应式的对象
const count = ref(0);
watch(count, (newValue, oldValue) => {
  console.log(`Count changed from ${oldValue} to ${newValue}`);
});

// 直接写入一个可响应式的对象，需要进行某些转换
const reactiveState = reactive({ count: 0 });
const { count } = toRefs(reactiveState);
watch(count, (newValue, oldValue) => {
  console.log(`Count changed from ${oldValue} to ${newValue}`);
});
```

> 注意: `reactive`对象获取到的 `newValue`以及 `oldValue`本身都是 `reactive` 对象
>
> ```javascript
> watch(
>   info, 
>   (newInfo, oldInfo) => {
>     console.log(newInfo, oldInfo);
>   }
> )
> ```
>
> ![image-20231217231108219](https://qiniu.waite.wang/202312172311755.png)
>
> 如果希望两者都是一个普通对象, 可以使用以下写法(JavaScript中的展开运算符):
>
> ```javascript
> watch(
>   () => ({ ...info }),
>   (newInfo, oldInfo) => {
>     console.log(newInfo, oldInfo);
>   }
> )
> ```
>
> ![image-20231217231330425](https://qiniu.waite.wang/202312172313342.png)
>
> 以下是完整代码:
>
> ```vue
> <template>
>   <div>
>     <h2 ref="title">{{ info.name }}</h2>
>     <button @click="changeData">修改数据</button>
>   </div>
> </template>
> 
> <script>
> import { reactive, watch } from 'vue';
> 
> export default {
>   setup() {
>     const info = reactive({ name: "why", age: 18 });
> 
>     watch(
>       () => ({ ...info }),
>       (newInfo, oldInfo) => {
>         console.log(newInfo, oldInfo);
>       }
>     )
>     watch(
>       info,
>       (newInfo, oldInfo) => {
>         console.log(newInfo, oldInfo);
>       }
>     )
> 
>     const changeData = () => info.name = info.name === "why" ? "kobe" : "why";
>     return {
>       changeData,
>       info
>     }
>   }
> }
> </script>
> ```

##### 侦听多个数据源

当侦听多个来源时，回调函数接受两个数组，分别对应来源数组中的新值和旧值：

```javascript
watch([fooRef, barRef], ([foo, bar], [prevFoo, prevBar]) => {
  /* ... */
})
```

```vue
<template>
  <div>
    <h2 ref="title">{{ info.name }}</h2>
    <button @click="changeData">修改数据</button>
  </div>
</template>

<script>
import { ref, reactive, watch } from 'vue';

export default {
  setup() {
    const info = reactive({ name: "why", age: 18 });
    const name = ref("why");

    watch([() => ({ ...info }), name], ([newInfo, newName], [oldInfo, oldName]) => {
      console.log(newInfo, newName, oldInfo, oldName);
    })

    const changeData = () => {
      info.name = "kobe";
    }

    return {
      changeData,
      info
    }
  }
}
</script>
```

##### watch的选项

+ **deep : 是否深度监听**
+ **immediate ： 是否立即执行**

> `watch` 侦听 `reactive`时默认是深度侦听的, 但是在使用 `{...info}`展开运算符时, 是不会深度监听的, 所以我们要设置 `deep: True`
>
> immediate: 第一次会执行

```javascript
watch(
  () => {
    const obj = { ...info }
    obj.friend = { ...obj.friend }
    return obj
  },
  (newValue, oldValue) => {
    console.log(newValue, oldValue)
  },
  {
    // 如果有多层，需要加上deep
    deep: true,
    // 立即执行
    immediate: true
  }
)
```

##### 停止侦听  

```javascript
const stop = watch(source, callback)

// 当已不再需要该侦听器时：
stop()
```

##### 副作用清理

```javascript
watch(id, async (newId, oldId, onCleanup) => {
  const { response, cancel } = doAsyncWork(newId)
  // 当 `id` 变化时，`cancel` 将被调用，
  // 取消之前的未完成的请求
  onCleanup(cancel)
  data.value = await response
})
```

### provide && inject

`provide` 和 [`inject`](https://cn.vuejs.org/api/options-composition.html#inject) 通常成对一起使用，使一个祖先组件作为其后代组件的依赖注入方，无论这个组件的层级有多深都可以注入成功，只要他们处于同一条组件链上。

**provide可以传入两个参数 :**

+ **name：提供的属性名称**
+ **value：提供的属性值**

**inject可以传入两个参数 :**

+ **对应provide传过来的name值**
+ **默认值**

```vue
<template>
  <h1>APP count: {{ count }}</h1>
  <button @click="change">APP button</button>
  <demo />
</template>
 
<script>
import { ref, provide, readonly } from 'vue'
import demo from './components/demo.vue'
 
export default {
  name: 'App',
  components: {
    demo
  },
  setup() {
    let count = ref(100)
    // 第一个参数key  第二个参数值，不让子组件随便修改，用readonly包裹一下
    provide('count', readonly(count))
    const change = () => count.value++
    return {
      count,
      change
    }
  }
}
</script>
```

```vue
<template>
  <h2>demo count:{{ count }}</h2>
  <button @click="change">demo change</button>
</template>
<script>
import { ref, inject } from 'vue'
export default {
  setup() {
    // 接收   第二个参数可以给一个默认值
    let count = inject('count', '')
    // 因为设置了readOnly 所以更改不了
    const change = () => count.value++
    return {
      count,
      change
    }
  }
}
</script>
```

![img](https://qiniu.waite.wang/202312180036440.gif)

### h函数

+ Vue在生成真实的DOM之前，会将节点转换成VNode，而VNode组合在一起形成一颗树结构，就是虚拟DOM ( VDOM )
+ 事实上，编写的 template 中的HTML 最终也是使用渲染函数生成对应的VNode
+ 那么，如果想充分的利用JavaScript的编程能力，可以自己来编写 createVNode 函数，生成对应的VNode
+ **h() 函数是一个用于创建 vnode 的一个函数**
+ **其实更准备的命名是 createVNode() 函数，但是为了简便在Vue将之简化为 h() 函数**

#### 参数

```javascript
// 完整参数签名
function h(
  type: string | Component,
  props?: object | null,
  children?: Children | Slot | Slots
): VNode

// 省略 props
function h(type: string | Component, children?: Children | Slot): VNode

type Children = string | number | boolean | VNode | null | Children[]

type Slot = () => Children

type Slots = { [name: string]: Slot }
```

+ 第一个参数既可以是一个字符串 (用于原生元素) 也可以是一个 Vue 组件定义。第二个参数是要传递的 prop，第三个参数是子节点。
+ 当创建一个组件的 vnode 时，子节点必须以插槽函数进行传递。如果组件只有默认槽，可以使用单个插槽函数进行传递。否则，必须以插槽函数的对象形式来传递。
+ 为了方便阅读，当子节点不是插槽对象时，可以省略 prop 参数。

#### 基本使用

h函数可以在两个地方使用：

+ render函数选项中；
+ setup函数选项中（setup本身需要是一个函数类型，函数再返回h函数创建的VNode）；

##### 在render函数选项中

```vue
<script>
// 1. 引入h函数
import { h } from 'vue';
 
export default {
  data() {
    return {
      counter: 0
    };
  },
  // 2. 定义render选项
  render() {
    // 3. 返回自定义的h函数
    return h('div', { class: 'app-view', name: 'abc' }, [
      // 4. 定义h2
      h('h2', { className: 'title' }, this.counter),
      // 5. 定义增加按钮
      h(
        'button',
        {
          className: 'add-btn',
          onClick: () => {
            this.counter++;
          }
        },
        '加一'
      ),
      // 6. 定义减少按钮
      h(
        'button',
        {
          className: 'remove-btn',
          onClick: () => {
            this.counter--;
          }
        },
        '减一'
      )
    ]);
  }
}
</script>
```

##### 在setup函数选项中

```vue
<script>
import { h, ref } from 'vue';
 
export default {
  setup() {
    const counter = ref(0);
    const increment = () => {
      counter.value++;
    };
    const decrement = () => {
      counter.value--;
    };
 
    // 返回render函数
    return () =>
      h('div', { class: 'app-view', name: 'abc' }, [
        h('h2', { className: 'title' }, counter.value),
        h(
          'button',
          {
            onClick: increment
          },
          '+1'
        ),
        h(
          'button',
          {
            onClick: decrement
          },
          '-1'
        )
      ]);
  }
};
</script>
```

##### 在setup语法糖中

```vue
<template>
  <!-- 2. 使用一下 -->
  <star-render />
</template>
 
<script setup>
import { h, ref } from 'vue';
 
const counter = ref(0);
const increment = () => {
  counter.value++;
};
const decrement = () => {
  counter.value--;
};
 
// 1. 拿到render函数
const starRender = () =>
  h('div', { class: 'app-view', name: 'abc' }, [
    h('h2', { className: 'title' }, counter.value),
    h(
      'button',
      {
        onClick: increment
      },
      '+1'
    ),
    h(
      'button',
      {
        onClick: decrement
      },
      '-1'
    )
  ]);
</script>
```

##### 其他写法

创建原生元素：

```javascript
import { h } from 'vue'

// 除了 type 外，其他参数都是可选的
h('div')
h('div', { id: 'foo' })

// attribute 和 property 都可以用于 prop
// Vue 会自动选择正确的方式来分配它
h('div', { class: 'bar', innerHTML: 'hello' })

// class 与 style 可以像在模板中一样
// 用数组或对象的形式书写
h('div', { class: [foo, { bar }], style: { color: 'red' } })

// 事件监听器应以 onXxx 的形式书写
h('div', { onClick: () => {} })

// children 可以是一个字符串
h('div', { id: 'foo' }, 'hello')

// 没有 prop 时可以省略不写
h('div', 'hello')
h('div', [h('span', 'hello')])

// children 数组可以同时包含 vnode 和字符串
h('div', ['hello', h('span', 'hello')])
```

创建组件：

```javascript
import Foo from './Foo.vue'

// 传递 prop
h(Foo, {
  // 等价于 some-prop="hello"
  someProp: 'hello',
  // 等价于 @update="() => {}"
  onUpdate: () => {}
})

// 传递单个默认插槽
h(Foo, () => 'default slot')

// 传递具名插槽
// 注意，需要使用 `null` 来避免
// 插槽对象被当作是 prop
h(MyComponent, null, {
  default: () => 'default slot',
  foo: () => h('div', 'foo'),
  bar: () => [h('span', 'one'), h('span', 'two')]
})
```

#### 函数组件和插槽的使用

```vue
<script>
  import { h } from "vue";

  export default {
    render() {
      return h("div", null, [
        h("h2", null, "Hello World"),
        this.$slots.default ? this.$slots.default({name: "coderwhy"}): h("span", null, "我是HelloWorld的插槽默认值")
      ])
    }
  }
</script>

<style lang="scss" scoped>

</style>
```

```vue
<template>
  <starRender />
</template>

<script setup>
import { h } from 'vue';
import HelloWorld from './HelloWorld.vue';

const starRender = () =>
  h("div", null, [
    h(HelloWorld, null, {
      default: props => h("span", null, `app传入到HelloWorld中的内容: ${props.name}`)
    })
  ])
</script>
```

![image-20231218195545969](https://qiniu.waite.wang/202312181955950.png)

### Jsx

[JSX](https://facebook.github.io/jsx/) 是 JavaScript 的一个类似 XML 的扩展，有了它，我们可以用以下的方式来书写代码：

```jsx
const vnode = <div>hello</div>
```

在 JSX 表达式中，使用大括号来嵌入动态值：

```jsx
const vnode = <div id={dynamicId}>hello, {userName}</div>
```

#### 配置

##### vue-cli环境

+ `npm install @vue/babel-plugin-jsx -D`
+ `babel.config.js` 中配置

![img](https://qiniu.waite.wang/202312182036029.png)

##### vite环境

+ `npm install @vitejs/plugin-vue-jsx -D`
+ `vite.config.js` 中配置

```javascript
import { fileURLToPath, URL } from 'node:url';
 
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import jsx from '@vitejs/plugin-vue-jsx';
 
export default defineConfig({
  plugins: [vue(), jsx()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
});
```

#### 基本使用

##### 在render函数中

```vue
<!-- 1. 这里加上注明语言使用jsx -->
<script lang="jsx">
import Home from './pages/home.vue';
 
export default {
  data() {
    return {
      counter: 0
    };
  },
  render() {
    // 2. 返回jsx写法
    return (
      <div class="app-view">
        <h2>当前计数:{this.counter}</h2>
        <button onClick={this.increment}>+1</button>
        <button onClick={this.decrement}>-1</button>
      </div>
    );
  },
  methods: {
    increment() {
      this.counter++;
    },
    decrement() {
      this.counter--;
    }
  }
};
</script>
```

##### 在setup函数中

```vue
<!-- 1. 这里加上注明语言使用jsx -->
<script lang="jsx">
 
export default {
  data() {
    return {
      counter: 0
    };
  },
  render() {
    // 2. 返回jsx写法
    return (
      <div class="app-view">
        <h2>当前计数:{this.counter}</h2>
        <button onClick={this.increment}>+1</button>
        <button onClick={this.decrement}>-1</button>
      </div>
    );
  },
  methods: {
    increment() {
      this.counter++;
    },
    decrement() {
      this.counter--;
    }
  }
};
</script>
```

##### 在setup语法糖中

```vue
<template>
  <!-- 3. 使用 -->
  <star-render />
</template>
 
<!-- 1. 这里加上注明语言使用jsx -->
<script setup lang="jsx">
import { ref } from 'vue';

const counter = ref(0); 
const increment = () => counter.value++;
const decrement = () => counter.value--;
 
// 2. 拿到render函数
const starRender = () => (
  <div class="app-view">
    <h2>当前计数:{counter.value}</h2>
    <button onClick={increment}>+1</button>
    <button onClick={decrement}>-1</button>
  </div>
);
</script>
```

### script setup语法糖

> `<script setup>`是在单文件组件 (SFC) 中使用组合式 API 的编译时语法糖，当同时使用 SFC 与组合式 API 时则推荐该语法

+ **更少的样板内容，更简洁的代码**
+ **能够使用纯 Typescript 声明 prop 和抛出事件**
+ **更好的运行时性能**
+ **更好的 IDE 类型推断性能**

#### 顶层的绑定会被暴露给模板

> 当使用`<script setup>` 的时候，任何在`<script setup>` 声明的顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 能在模板中直接使用, 导入的组件也可以直接使用

```vue
<template>
  <div>{{ mes }}</div>
  <button @click="addClick">按钮</button>
</template>
 
<!-- 1. 这里加上setup属性 -->
<script setup>
import { ref } from 'vue';
 
// 定义数据后，template中可以直接使用，无需返回
const mes = ref(0);
// 定义的方法也是，直接可被使用
const addClick = () => {
  console.log('hahah');
};
</script>
```

```vue
<template>
  <!-- 2. 直接使用，不用通过compoents注册 -->
  <my-home></my-home>
</template>
 
<script setup>
// 1. 这是导入的组件
import myHome from './myHome.vue';
</script>
```

#### defineProps()

> defineProps  =>  用来接收从父组件传递过来的数据

```vue
<template>
  <my-home name="hello" :age="18"></my-home>
</template>
 
<script setup>
import myHome from './myHome.vue';
</script>
```

```vue
<template>
  <div>{{ name }} - {{ age }}</div>
</template>
 
<script setup>
// defineProps是内置组件，可以直接使用，不用导入
// 可以接收一下返回的props对象，也可以不用
const props = defineProps({
  name: {
    type: String,
    default: ''
  },
  age: {
    type: Number,
    default: 0
  }
});
console.log(props); // Proxy {name: 'hello', age: 18}
</script>
```

#### defineEmits()

> defineProps  =>  用来发射事件给父组件

```vue
<template>
  <button @click="btnClick">发送</button>
</template>
 
<script setup>
// 1. 注册一下发射的事件
const emits = defineEmits(['btnClick']);
// 2. 监听按钮的点击
const btnClick = () => {
  // 3. 发射
  emits('btnClick', '我发射了');
};
</script>
```

```vue
<template>
  <!-- 1. 监听子组件发射来的事件 -->
  <my-home @btnClick="handleClick"></my-home>
</template>
 
<script setup>
import myHome from './myHome.vue';
 
// 2. 获取子组件传递过来的值
const handleClick = (message) => {
  console.log(message); // 我发射了
};
</script>
```

#### defineExpose()

> defineExpose  =>  用来暴露数据
>
> ps : 使用 `<script setup>`的组件是默认关闭的

```vue
<script setup>
const foo = () => {
  console.log('foo');
};
// 暴露出去，才可以被访问到
defineExpose({
  foo
});
</script>
```

```vue
<template>
  <!-- 1. 定义ref -->
  <my-home ref="myHomeRef"></my-home>
</template>
 
<script setup>
import { onMounted, ref } from 'vue';
import myHome from '../../../Vue3/06_阶段六-Vue3全家桶实战/code/04_learn_composition/src/11_script_setup语法/myHome.vue';
// 2. 定义名称一样
const myHomeRef = ref();
onMounted(() => {
  // 3. 在生命周期中访问
  console.log(myHomeRef.value);
});
</script>
```

### 自定义组件

#### 指令的生命周期

+ 一个指令定义的对象，Vue提供了如下的几个钩子函数：
+ created：在绑定元素的 attribute 或事件监听器被应用之前调用；
+ beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用；
+ mounted：在绑定元素的父组件被挂载后调用；
+ beforeUpdate：在更新包含组件的 VNode 之前调用；
+ updated：在包含组件的 VNode 及其子组件的 VNode 更新后调用；
+ beforeUnmount：在卸载绑定元素的父组件之前调用；
+ unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次；

#### 指令钩子

一个指令的定义对象可以提供几种钩子函数 (都是可选的)：

```javascript
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```

指令的钩子会传递以下几种参数：

+ `el`：指令绑定到的元素。这可以用于直接操作 DOM。
+ `binding`：一个对象，包含以下属性。
  + `value`：传递给指令的值。例如在 `v-my-directive="1 + 1"` 中，值是 `2`。
  + `oldValue`：之前的值，仅在 `beforeUpdate` 和 `updated` 中可用。无论值是否更改，它都可用。
  + `arg`：传递给指令的参数 (如果有的话)。例如在 `v-my-directive:foo` 中，参数是 `"foo"`。
  + `modifiers`：一个包含修饰符的对象 (如果有的话)。例如在 `v-my-directive.foo.bar` 中，修饰符对象是 `{ foo: true, bar: true }`。
  + `instance`：使用该指令的组件实例。
  + `dir`：指令的定义对象。
+ `vnode`：代表绑定元素的底层 VNode。
+ `prevNode`：代表之前的渲染中指令所绑定元素的 VNode。仅在 `beforeUpdate` 和 `updated` 钩子中可用。

举例来说，像下面这样使用指令：

```vue
<div v-example:foo.bar="baz">
```

`binding` 参数会是一个这样的对象：

```javascript
{
  arg: 'foo',
  modifiers: { bar: true },
  value: /* `baz` 的值 */,
  oldValue: /* 上一次更新时 `baz` 的值 */
}
```

和内置指令类似，自定义指令的参数也可以是动态的。举例来说：

```vue
<div v-example:[arg]="value"></div>
```

这里指令的参数会基于组件的 `arg` 数据属性响应式地更新。

> 除了 `el` 外，其他参数都是只读的，不要更改它们。若你需要在不同的钩子间共享信息，推荐通过元素的 [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset) attribute 实现。

#### 简单使用

> **Vue中自带的指令例如v-show、v-for、v-model等等，除了使用这些指令之外，Vue 也允许我们来自定义自己的指令**
>
> **ps : 一般需要对dom元素进行底层操作时使用**

+ 自定义指令分为两种：
  + 自定义局部指令：组件中通过 directives 选项，只能在当前组件中使用；
  + 自定义全局指令：app的 directive 方法，可以在任意组件中被使用；

##### 默认实现方式

一个自定义指令由一个包含类似组件生命周期钩子的对象来定义。钩子函数会接收到指令所绑定元素作为其参数。下面是一个自定义指令的例子，当一个 input 元素被 Vue 插入到 DOM 中后，它会被自动聚焦：

```vue
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```

```vue
<template>
  <div class="app-view">
    <input type="text" ref="inputRef" />
  </div>
</template>
 
<script setup>
import { onMounted, ref } from 'vue';
 
const inputRef = ref(null);
 
onMounted(() => {
  inputRef.value.focus();
});
</script>
```

##### 使用局部指令

在 `<script setup>` 中，任何以 `v` 开头的驼峰式命名的变量都可以被用作一个自定义指令。在上面的例子中，`vFocus` 即可以在模板中以 `v-focus` 的形式使用。

在没有使用 `<script setup>` 的情况下，自定义指令需要通过 `directives` 选项注册：

```vue
<template>
  <div>
    <input type="text" v-focus>
  </div>
</template>

<script>
  export default {
    // 局部指令
    directives: {
      focus: {
        mounted(el, bindings, vnode, preVnode) {
          console.log("focus mounted");
          el.focus();
        }
      }
    }
  }
</script>
```

##### 自定义全局指令

+ `main.js`中注册

```javascript
import { createApp } from 'vue'
import App from './App.vue'
 
const app = createApp(App)
 
// 指令名称
app.directive('focus', {
  // 使用自定义指令的生命周期，挂载后访问
  mounted(el, bindings, vnode, preVnode) {
    el?.focus()
  }
})
 
app.mount('#app')
```

###### 进行抽取

+ 注册directives文件夹
+ /directives/format-time.js

```javascript
import dayjs from 'dayjs';

export default function(app) {
  app.directive("format-time", {
    created(el, bindings) {
      bindings.formatString = "YYYY-MM-DD HH:mm:ss";
      if (bindings.value) {
        bindings.formatString = bindings.value;
      }
    },
    mounted(el, bindings) {
      const textContent = el.textContent;
      let timestamp = parseInt(textContent);
      if (textContent.length === 10) {
        timestamp = timestamp * 1000
      }
      el.textContent = dayjs(timestamp).format(bindings.formatString);
    }
  })
} 
```

+ /directives/index.js

```javascript
import registerFormatTime from './format-time';

export default function registerDirectives(app) {
  registerFormatTime(app);
}
```

+ mian.js

```javascript
import registerDirectives from './directives'

registerDirectives(app);
```

##### setup

###### 函数

```vue
<template>
  <h1 v-fomat-time="timeFormatType">{{ timeStamp }}</h1>
</template>
<script>
import { ref } from 'vue'
import dayJs from 'dayjs'
export default {
  directives: {
    'fomat-time': {
      mounted(el, bindings) {
        // 默认显示时间类型
        let formatType = bindings.value
        console.log(formatType)
        // 转换成number类型
        let time = el.textContent.length === 10 ? el.textContent * 1000 : el.textContent * 1;
        // 格式化
        el.textContent = dayJs(time).format(formatType)
        setInterval(() => {
          // 定时器
          time = dayJs(new Date().valueOf()).format(formatType)
          el.textContent = time
        }, 1000)
      }
    }
  },
  setup() {
    // 设置初始时间戳
    const timeStamp = ref(new Date().valueOf())
 
    const timeFormatType = ref('YYYY-MM-DD HH:mm:ss')
 
    return {
      timeStamp,
      timeFormatType
    }
  }
}
</script>
 
<style>
h1 {
  display: inline-block;
  color: transparent;
  -webkit-background-clip: text;
  background-image: linear-gradient(to right, red, blue);
}
</style>
```

###### 语法糖

```vue
<template>
  <h1 v-fomat-time="timeFormatType">{{ timeStamp }}</h1>
</template>
<script setup>
import { ref } from 'vue';
import dayJs from 'dayjs';
 
// 设置初始时间戳
const timeStamp = ref(new Date().valueOf());
// 设置初始时间格式
const timeFormatType = ref('YYYY-MM-DD HH:mm:ss');
 
// 自定义时间格式化指令
const vFomatTime = {
  mounted(el, bindings) {
    // 获取定义的时间格式
    const { value: timeFormatType } = bindings;
    // 转换成number类型
    let time = el.textContent.length === 10 ? el.textContent * 1000 : el.textContent * 1;
    // 使用dayJs，根据时间格式来格式化时间,并赋值给el
    el.textContent = dayJs(time).format(timeFormatType);
    // 定时器，每隔一秒，重新赋值给el
    setInterval(() => {
      time = dayJs(new Date().valueOf()).format(timeFormatType);
      el.textContent = time;
    }, 1000);
  }
};
</script>
 
<style>
h1 {
  display: inline-block;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  background-image: linear-gradient(to right, red, blue);
}
</style>
```

### 内置组件

#### Teleport

> `<Teleport>` 是一个内置组件，它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。

+ 在某些情况下，希望组件不是挂载在当前组件树上的，可能是移动到Vue app之外的其他位置
  + 比如移动到body元素上，或者我们有其他的div#app之外的元素上
  + 可以通过teleport来完成
+ teleport 翻译过来是心灵传输、远距离运输的意思，有两个属性
  + to : 指定将其中的内容移动到的目标元素，可以使用选择器
  + disabled : 是否禁用 teleport 的功能

##### 基本使用

```vue
<template>
  <div class="app-view">
    <!-- 把该组件挂载到body元素上 -->
    <teleport to="body">
      <h1>Teleport</h1>
    </teleport>
  </div>
</template>
<script setup></script>
 
<style>
h1 {
  display: inline-block;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  background-image: linear-gradient(to right, red, green, pink, yellow, blue);
}
</style>
```

![image-20231219000840061](https://qiniu.waite.wang/202312190008149.png)

##### 挂载到#app之外的指定元素上

```vue
<template>
  <div class="app">
    <div id="star"></div>
    <div class="b">
        <div class="c"></div>
    </div>

  </div>

  <!-- 把该组件挂载到#star元素上 -->
  <teleport to="#star">
    <h1>Teleport</h1>
  </teleport>

  <!-- 把该组件挂载到.b元素上 -->
  <teleport to=".b">
    <h1>Teleport123</h1>
  </teleport>

  <!-- 文档上说是挂载到#app之外的元素，可是我发现自己内部的也可以指定，emmmm，优先是从内部一层层往外找的 -->
  <!-- 把该组件挂载到.c元素上... -->
  <teleport to=".c">
    <h1>Teleport123</h1>
  </teleport>
</template>
<script setup></script>
 
<style>
h1 {
  display: inline-block;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  background-image: linear-gradient(to right, red, green, pink, yellow, blue);
}
</style>
```

![image-20231219001340177](https://qiniu.waite.wang/202312190013634.png)

##### 多个Teleport

> **会合并，谁先谁在前面**

```vue
<template>
  <div id="star"></div>

  <div class="app-view">
    <!-- 把该组件挂载到#star元素上 -->
    <teleport to="#star">
      <h1>Teleport</h1>
    </teleport>
  </div>
  <!-- 把该组件挂载到#star元素上 -->
  <teleport to="#star">
    <h1>Teleport123</h1>
  </teleport>
</template>
<script setup></script>
 
<style>
h1 {
  display: inline-block;
  color: transparent;
  -webkit-background-clip: text;
  background-clip: text;
  background-image: linear-gradient(to right, red, green, pink, yellow, blue);
}
</style>
```

![image-20231219001638121](https://qiniu.waite.wang/202312190016061.png)

#### 其他

> [异步组件 defineAsyncComponent/ Suspense : 实验特性](https://ladder.waite.wang/web/vue/learn/components/%E5%BC%82%E6%AD%A5%E7%BB%84%E4%BB%B6.html)
>
> [动态组件 : component](https://ladder.waite.wang/web/vue/learn/components/%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6.html)

### Vue插件

> <https://cn.vuejs.org/guide/reusability/plugins.html#plugins>

+ 通常我们向Vue全局添加一些功能时，会采用插件的模式，它有两种编写方式：
  + 对象类型：一个对象，但是必须包含一个 `install`的函数，该函数会在安装插件时执行；
  + 函数类型：一个`function`，这个函数会在安装插件时自动执行；
+ 插件可以完成的功能没有限制，比如下面的几种都是可以的：
  + 添加全局方法或者`property`，通过把它们添加到 `config.globalProperties` 上实现；
  + 添加全局资源：指令/过滤器/过渡等；
  + 通过全局 `mixin`来添加一些组件选项；
  + 一个库，提供自己的 API，同时提供上面提到的一个或多个功能；

#### 对象类型

> 对象类型：一个对象，但是必须包含一个 install 的函数，该函数会在安装插件时执行

```javascript
app.use({
  install(app) {
    console.log('对象方式，插件被调用了', app);
  }
});
```

#### 函数类型

> 函数类型：一个function，这个函数会在安装插件时自动执行

```javascript
app.use(function(app){
  console.log('函数方式，插件被调用了', app);
})
```

#### 改写自定义指令

```javascript
import { createApp } from 'vue';
 
import App from './App.vue';
// 1. 导入指令方法
import installDirectives from './directives';
 
// 2。 注册所有指令
// installDirectives(app);
 
// 这样使用use方法注册指令，因为传入的是一个函数，所以会自动执行
// 并且会把app实例传入，这样就可以在函数内部注册指令了
createApp(App).use(installDirectives).mount('#app');
```
