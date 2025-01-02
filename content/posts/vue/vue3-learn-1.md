---
title: "邂逅 Vue3 开发(一)"
date: 2023-09-30T16:04:23+08:00
categories: ["Web"]
tags: ["Vue3"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

> 本文章来源于王红元老师（coderwhy）的 Vue3 + Ts 课程第二章节 - Vue3 基本指令附上链接：https//ke.qq.com/course/3453141
permalink: /archives/vue3-ji-ben-zhi-ling-er


# 邂逅Vue3开发

## 认识 Vue

### 什么是 Vue

+ Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。
  + 全程是Vue.js或者Vuejs；
  + 什么是渐进式框架呢？表示我们可以在项目中一点点来引入和使用Vue，而不一定需要全部使用Vue来开发整个 项目；

### Vue3带来的变化

+ 源码通过monorepo的形式来管理源代码：
  + Mono：单个
  + Repo：repository仓库
  + 主要是将许多项目的代码存储在同一个 repository 中；
  + 这样做的目的是多个包本身相互独立，可以有自己的功能逻辑、单元测试等，同时又在同一个仓库下方便管理；
  + 而且模块划分的更加清晰，可维护性、可扩展性更强；
+ 源码使用TypeScript来进行重写：
  + 在Vue2.x的时候，Vue使用 Flow 来进行类型检测；
  + 在Vue3.x的时候，Vue的源码全部使用 TypeScript 来进行重构，并且 Vue 本身对 TypeScript 支持也更好了；

#### 性能方面

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

#### 新的API

+ 由Options API 到 Composition API：
  + 在 Vue2.x 的时候，我们会通过 Options API 来描述组件对象；
  + Options API 包括data、props、methods、computed、生命周期等等这些选项；
  + 存在比较大的问题是多个逻辑可能是在不同的地方：
    + 比如created中会使用某一个method来修改data的数据，代码的内聚性非常差；
  + Composition API可以将 相关联的代码 放到同一处 进行处理，而不需要在多个Options之间寻找；
+ Hooks函数增加代码的复用性：
  + 在Vue2.x的时候，我们通常通过mixins在多个组件之间共享逻辑；但是有一个很大的缺陷就是 mixins也是由一大堆的Options组成的，并且多个mixins会存在命名冲突的问题；
  + 在Vue3.x中，我们可以通过Hook函数，来将一部分独立的逻辑抽取出去，并且它们还可以做到是响应式的；

## 如何使用 Vue

1. 方式一：在页面中通过CDN的方式来引入； 
2. 方式二：下载Vue的JavaScript文件，并且自己手动引入； 
3. 方式三：通过npm包管理工具安装使用它； 
4. 方式四：直接通过Vue CLI创建项目，并且使用它；

### CDN 引入

```html
<script src="https://unpkg.com/vue@next"></script>
```

### 下载和引入

+ 下载Vue的源码，可以直接打开CDN的链接： 
  + 打开链接，复制其中所有的代码； 
  + 创建一个新的文件，比如vue.js，将代码复制到其中；

```html
<script src="../js/vue.js"></script>
```

## 声明式编程和命令式编程

- 原生开发和Vue开发的模式和特点,我们会发现是完全不同的,这里其实涉及到两种不同的编程范式命令式编程和声明式编程
- 命令式编程关注的是“ how to do”,声明式编程关注的是" what to do",由框架(机器)完成"how"的过程

## MVVM模型

+ MVC和MVVM都是一种软件的体系结构
  + MVC是 Model-View-Controller的简称,是在前期被使用非常框架的架构模式,比如iS、前端
  + MVVM是 Model-View- ViewMode的简称,是目前非常流行的架构模式
+ 通常情况下,我们也经常称vue是一个MVVM的框架
  + vue官方其实有说明,vue虽然并没有完全遵守MVVM的模型,但是整个设计是受到它的启发的

![](https://qiniu.waite.wang/202310161331268.png)

## template属性

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

## data属性

+ data属性是传入一个函数,并且该函数需要返回一个对象
  + 在Vue2x的时候,也可以传入一个对象(虽然官方推荐是一个函数);
  + 在Vue3x的时候,必须传入一个函数,否则就会直接在浏览器中报错

+ data中返回的对象会被vue的响应式系统劫持,之后对该对象的修改或者访问都会在劫持中被处理
  + 所以我们在 template中通过{ counter} 访问 counter,可以从对象中获取到数据
  + 所以我们修改 counter的值时, template中的{ counter)也会发生改变;

## methods属性（重点）

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
  - 事实上Vue的源码当中就是对methods中的所有函数进行了遍历，并且通过 bind 绑定了 this

> 具体看 https://mp.weixin.qq.com/s/hYm0JgBI25grNG_2sCRlTA

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

## 如何阅读Vue源码

+ 需要的环境 npm，yarn
+ 操作步骤
  1. 安装yarn npm install yarn -g 
  2. 在项目中配置yarn yarn install
  3. 在package.json中的dev后加上--sourcemap
  4. 打包项目 yarn dev （在vue/dist文件夹下有两个文件，vue.global.js和vue.global.js.map）
  5. 在vue/examplex新建自己的文件夹以及测试demo
  6. 在demo中打下断点—debugger
  7. 在浏览器中打开调试面板，选择其中的source面板，查看执行对应的源码